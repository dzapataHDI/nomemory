One of the main reason MockNeat was created was to allow developers generate arbitrary test data for their real-world applications.

The following tutorial will quickly demonstrate how we can populate a MySQL Schema for an "imaginary" HR Application (for an "imaginary" company).

# The Schema

The SQL Schema is described in the picture bellow:

![](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/hr-schema-mysql.png)

The schema contains 5 tables:

```sql
CREATE TABLE regions (
	region_id INT (11) UNSIGNED NOT NULL,
	region_name VARCHAR(32),
	PRIMARY KEY (region_id)
	);
```

In the regions table we will keep 4 geographical locations (Europe, Americas, Middle East and Africa and Asia). The primary keys will be numbers in sequence from 0 to 3.

```sql
CREATE TABLE countries (
	country_id CHAR(2) NOT NULL,
	country_name VARCHAR(128),
	region_id INT (11) UNSIGNED NOT NULL,
	PRIMARY KEY (country_id),
	CONSTRAINT countries_regions_region_id FOREIGN KEY (region_id) REFERENCES regions(region_id)
	);
```

In the countries table we will keep the countries information. In total there are 241 countries. Each country will have a reference to one of the geographical regions defined above.

```sql
CREATE TABLE locations (
	location_id INT (11) UNSIGNED NOT NULL AUTO_INCREMENT,
	street_address VARCHAR(128),
	postal_code VARCHAR(12),
	city VARCHAR(64) NOT NULL,
	state_province VARCHAR(25),
	country_id CHAR(2) NOT NULL,
	PRIMARY KEY (location_id),
	CONSTRAINT locations_countries_country_id FOREIGN KEY (country_id) REFERENCES countries(country_id)
	);
```

For each country we can have multiple locations. Each location holds the address information where the company's  
offices are.

```sql
CREATE TABLE departments (
	department_id INT (11) UNSIGNED NOT NULL,
	department_name VARCHAR(64) NOT NULL,
	manager_id INT (11) UNSIGNED,
	location_id INT (11) UNSIGNED,
	PRIMARY KEY (department_id),
	CONSTRAINT departments_locations_location_id FOREIGN KEY (location_id) REFERENCES locations(location_id)
	);
```

The "imaginary" company is organised in departments. Each department has two foreign key pointing to a given location and a given manager. 

```sql
CREATE TABLE employees (
	employee_id INT (11) UNSIGNED NOT NULL,
	first_name VARCHAR(32),
	last_name VARCHAR(32) NOT NULL,
	email VARCHAR(64) NOT NULL,
	phone_number VARCHAR(20),
	hire_date DATE NOT NULL,
	salary DECIMAL(8, 2) NOT NULL,
	manager_id INT (11) UNSIGNED,
	department_id INT (11) UNSIGNED,
	PRIMARY KEY (employee_id),
	CONSTRAINT employees_departments_department_id FOREIGN KEY (department_id) REFERENCES departments(department_id),
	CONSTRAINT employees_employees_employee_id FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
	);

ALTER TABLE departments ADD FOREIGN KEY (manager_id) REFERENCES employees (employee_id);
```

The employees table holds all the information regarding the people who are working in this company. Each employee is associated with a department and has a manager (who is an employee as well, thus we can say the table is auto-referencing).  

The full DDL script to create the MySQL Schema can be found [here](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/hr-schema-mysql.sql).

# Generating the Data

Before continue reading it's good that you have a general knowledge of the [MockNeat](MockNeat) methods and how a [MockUnit](MockUnits) works.

## Generating `Regions`

The `regions` table is mapped to the [`Region.class`](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/model/Region.java).

We are going to define 4 regions:

```java    
private static final List<String> REGIONS = asList("Europe", "Americas", "Middle East and Africa", "Asia");
```

Using the [`reflect()`](MockNeat#reflect) method we can mock 4 `Region` objects through reflection an keep them in a `List<Region>`.

We are going to use this list later in order to generate the actual SQL Inserts.:

```java
// Generate Regions
List<Region> regions = m.reflect(Region.class) 
                        .field("id", m.longSeq()) 
                        .field("name", m.seq(REGIONS)) 
                        .list(REGIONS.size()) 
                        .val(); 
```

Explanation:
* `field()`: It is used to define how we populate the fields of the `Region` class;
* [`lonqSeq()`](MockNeat#longseq): It is used to return numbers in a sequence. Without additional configuration it will start by `0` and increment with `1`. 
* [`seq()`](MockNeat#seq): It is used to iterate over REGIONS static variable. 
* [`list()`](MockUnit#list): It is used to group all the results in a list of a given size.

## Generate Countries

The `countries` table is mapped by the [`Country.class`](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/model/Country.java).

All 241 the countries and their ISO2 codes are defined into two dictionaries: `DictType.COUNTRY_ISO_CODE_2` and `DictType.COUNTRY_NAME`.

In the same manner as for `regions` we will use [`reflect()`](MockNeat#reflect) in order to mock some `Country` objects.

We can use the [`seq()`](MockNeat#seq) to iterate over the two dictionaries and obtain a list of the 241 countries:

```java
// Generate Countries
final int totalCountries = 241;
List<Country> countries = m.reflect(Country.class)
                            .field("id", m.seq(DictType.COUNTRY_ISO_CODE_2))
                            .field("name", m.seq(DictType.COUNTRY_NAME).mapToString().replaceAll("'", "''"))
                            .field("regionId", m.from(regions).map(Region::getId))
                            .list(totalCountries)
                            .val();
```

Explanation:
* `mapToString().replaceAll("'", "''")`: Is used to escape the apostrophe;
* `m.from(regions).map(Region::getId)`: Is used to obtain a random id for a region (we need to do that in order to accomplish the constraint imposed by the foreign key).

## Generation Locations

The `locations` table is mapped by the [Location.class](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/model/Location.java).

The ids for the locations will start from `1000` and the increment will be `100`. 

For generating the street name we will use a custom `MockUnitString` written from scratch.

```java
// Generate Locations
int totalLocations = 200;
List<Location> locations = m.reflect(Location.class)
                              .field("id", m.longSeq().start(1000).increment(100))
                              .field("street", streeNameGenerator())
                              .field("city", m.cities().capitals())
                              .field("postalCode", m.regex("[A-Z]{1,3}[0-9]{1,2}[A-Z]{1,2}[0-9]{3,5}"))
                              .field("countryId", m.from(countries).map(Country::getId))
                              .map(HrSchema::cityWithCountryId)
                              .list(totalLocations)
                              .val();
```

Explanation:
* `m.regex("[A-Z]{1,3}[0-9]{1,2}[A-Z]{1,2}[0-9]{3,5}")`: this makes use of the [`regex()`](MockUnit#regex) method that allows the developer to build arbitrary text from a given 'regex'. 
* `m.from(countries).map(Country::getId)`: is used to model the foreign key relationship. We pick randomly a country from the `List<String> countries` that we've previously defined and then we "extract" the id using the `map()` method.

As you can see the streetNameGenerator is a custom made `MockUnitString`.

The method that defines this:

```java
private static final List<String> STREET_SUFFIX =
            asList("Rd", "Str", "Blvd");

private static final MockUnitString streeNameGenerator() {

        // A reference to the mock object associated with the current thread;
        MockNeat m = MockNeat.threadLocal();

        // Returns a random string from the 'STREET_SUFFIX' list;
        // Eg: "Str"
        MockUnitString streetNameSuffix = m.fromStrings(STREET_SUFFIX);

        // Returns a potential street name:
        // - Streets names have 55% chances of containing two words;
        // - Street names have 45% chances of containing one word;
        // The single word is always a noun (1 syllable, 2 syllables or 3 syllables)
        // The first word if exists is an adjective (1 syllable or 2 syllables)
        MockUnitString streetNameGenerator = m.fmt("#{word1}#{word2}")
                .param("word1", m.probabilites(String.class)
                                        .add(0.55, m.dicts().types(EN_ADJECTIVE_1SYLL, EN_ADJECTIVE_2SYLL))
                                        .add(0.45, "")
                                        // If the first word exists (55% chance) then append a space
                                        .mapToString(s -> s.equals("") ? s : s + " ")
                                        // Capitalize the two (or one words) generated
                                        .format(CAPITALIZED))
                .param("word2", m.dicts().types(EN_NOUN_1SYLL, EN_NOUN_2SYLL, EN_NOUN_3SYLL).format(CAPITALIZED));

        // Formatting street name
        // - First section is a number in the range [1, 100000)
        // - Second section is the name as generated above
        // - Last section is the suffix as obtained above
        return m.fmt("#{no} #{name} #{suffix}")
                .param("no", m.ints().range(1, 10000))
                .param("name", streetNameGenerator)
                .param( "suffix", streetNameSuffix);
}
``` 

## Generating departments

The `departments` table is mapped by [`Departments.class`](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/model/Departments.java)

The Java code to generate the list of departments is:
```java
// Departments
int totalDepartments = 16;
List<Departments> departments = m.reflect(Departments.class)
                                    .field("id", m.longSeq().start(10).increment(10))
                                    .field( "depName", m.seq(DEPARTMENTS))
                                    .field( "locationId", m.from(locations).map(Location::getId))
                                    .list(16)
                                    .val();
```

The field `managerId` from the `Departments.class` will be populated later, once we have generated the employees.

## Generating Employees

The `employees` table is mapped by the [`Employee.class`](https://github.com/nomemory/mockneat/blob/master/examples/net/andreinc/mockneat/github/hr/model/Employee.java).

The java code to generate a `List<Employee> employees`:

```java
// Employees
int totalEmployees = 1500;
LocalDate minDate = LocalDate.of(1990, 1, 1);
List<Employee> employees = m.reflect(Employee.class)
                                    .field("id", m.longSeq().start(1))
                                    .field("firstName", m.names().first())
                                    .field("lastName", m.names().last())
                                    .field("email", m.emails())
                                    .field("phoneNumber", m.regex("([0-9]{3}) [0-9]{10}"))
                                    .field("hireDate", m.localDates().past(minDate).toUtilDate())
                                    .field("salary", m.doubles().range(2000.0, 10000.0))
                                    .field("depId", m.from(departments).map(Departments::getId))
                                    .map(HrSchema::populateHireDateStr)
                                    .list(totalEmployees)
                                    .val();
```

And the `populateHireDateStr()` code is:

```java
/**
* Generates a STR_TO_DATE string that is going to be used for the MySQL INSERT
* @param employee
* @return
*/
public static Employee populateHireDateStr(Employee employee) {
        Date date = employee.getHireDate();
        SimpleDateFormat sdf = new SimpleDateFormat("dd-MMM-yyyy");
        String dateStr = sdf.format(date);
        employee.setHireDateStr("STR_TO_DATE('" + dateStr + "', '%d-%M-%Y')");
        return employee;
}
```

Before generating the employees we need to pick some random managers from their ranks and populate the `managerId` fields for both the employees and departments:

```java
// Decide who is manager
int totalNumManagers = 100;
List<Long> managersIds = m.from(employees)
                                  .map(Employee::getId)
                                  .list(totalNumManagers).val()
                                  .stream().distinct().collect(Collectors.toList());

// Set Up managers for employees
employees.forEach(
            e ->  {
                long empId = e.getId();
                long mngId = e.getId();

                // Avoid assigning the same number as
                // id and managerId
                while(mngId == empId)
                    mngId = m.from(managersIds).val();

                e.setManagerId(mngId);
            }
);

// Set Up Managers for departments
MockUnit<Long> managersIdSeq = m.seq(managersIds);
departments.forEach(d -> d.setManagerId(managersIdSeq.val()));
```

## Printing the INSERTS for the Schema

```java
        // Print everything
        System.out.println("SET FOREIGN_KEY_CHECKS = 0;");
        printSql("INSERT INTO regions VALUE (#{region.id}, '#{region.name}');", "region", regions);
        printSql("INSERT INTO countries VALUES('#{country.id}', '#{country.name}', #{country.regionId});", "country", countries);
        printSql("INSERT INTO locations VALUES(#{l.id}, '#{l.street}', '#{l.postalCode}', '#{l.city}', #{l.state}, '#{l.countryId}');", "l", locations);
        printSql("INSERT INTO departments VALUES(#{dep.id}, '#{dep.depName}', #{dep.managerId}, #{dep.locationId});","dep", departments);
        printSql("INSERT INTO employees VALUES(#{e.id}, '#{e.firstName}', '#{e.lastName}', '#{e.email}', '#{e.phoneNumber}', #{e.hireDateStr}, #{e.salary}, #{e.managerId}, #{e.depId});", "e", employees);
        System.out.println("SET FOREIGN_KEY_CHECKS = 1;");
        System.out.println("COMMIT;");
```

The `printSql()` makes use of the [AlephFormatter](https://github.com/nomemory/aleph-formatter) library. `AlephFormatter` is by default a dependency of MockNeat and the code should work by default. 

The method is implemented as this:
```java
/**
* Prints a list of objects as a SQL INSERT Statement.
* @param template The template of the SQL INSERT Statement
* @param param The name of the parameter used in the template
* @param list The list of objects we are going to use for the INSERT
*/
private static <T> void printSql(String template, String param, List<T> list) {
        list.forEach(obj -> System.out.println(
            template(template)
                    .arg(param, obj)
                    .fmt()));
}
````

## Running the example.

The full code can be found [here](https://github.com/nomemory/mockneat/tree/master/examples/net/andreinc/mockneat/github/hr).

Sample output:
```sql
INSERT INTO regions VALUE (0, 'Europe');
INSERT INTO regions VALUE (1, 'Americas');
INSERT INTO regions VALUE (2, 'Middle East and Africa');
INSERT INTO regions VALUE (3, 'Asia');
/* ... */
INSERT INTO countries VALUES('AW', 'Aruba', 2);
INSERT INTO countries VALUES('AU', 'Australia', 1);
INSERT INTO countries VALUES('AT', 'Austria', 2);
INSERT INTO countries VALUES('AZ', 'Azerbaijan', 1);
INSERT INTO countries VALUES('BS', 'Bahamas', 3);
INSERT INTO countries VALUES('BH', 'Bahrain', 3);
/* .... */
NSERT INTO locations VALUES(1000, '2175 Wows Str', 'YI9K4927', 'Bangkok HM', null, 'HM');
INSERT INTO locations VALUES(1100, '2662 Rowan Rd', 'YU41L132', 'Copenhagen IN', null, 'IN');
INSERT INTO locations VALUES(1200, '2362 Pashas Rd', 'J79M112', 'Bern CN', null, 'CN');
INSERT INTO locations VALUES(1300, '9392 Silenced Healds Rd', 'QFT35P855', 'Copenhagen BE', null, 'BE');
INSERT INTO locations VALUES(1400, '5011 Shoal Depressants Str', 'XZN97P378', 'Manama SH', null, 'SH');
INSERT INTO locations VALUES(1500, '3704 Like Infections Blvd', 'P56P23632', 'Moscow ET', null, 'ET');
INSERT INTO locations VALUES(1600, '6706 Turnstone Rd', 'JP50M342', 'Tunis GD', null, 'GD');
INSERT INTO locations VALUES(1700, '3418 Moot Alewife Rd', 'VQ50JU943', 'Bucharest SH', null, 'SH');
/* ... */
INSERT INTO departments VALUES(10, 'Customer Service', 598, 20100);
INSERT INTO departments VALUES(20, 'Customers', 912, 10900);
INSERT INTO departments VALUES(30, 'Financial', 293, 9700);
INSERT INTO departments VALUES(40, 'Human Resources', 721, 10700);
INSERT INTO departments VALUES(50, 'IT', 1148, 17400);
INSERT INTO departments VALUES(60, 'Insurance', 1210, 6800);
INSERT INTO departments VALUES(70, 'Inventory', 988, 17400);
INSERT INTO departments VALUES(80, 'Licenses', 828, 5400);
/* ... */
INSERT INTO employees VALUES(1, 'Rosaria', 'Pawluk', 'scorethu@gmail.com', '798 6709073976', STR_TO_DATE('24-Sep-1995', '%d-%M-%Y'), 4768.751828039507, 1232, 120);
INSERT INTO employees VALUES(2, 'Lucie', 'Monton', 'plusedmund@mac.com', '451 2596002039', STR_TO_DATE('05-Apr-1999', '%d-%M-%Y'), 4211.7941129205365, 229, 130);
INSERT INTO employees VALUES(3, 'Bonita', 'Wiederholt', 'gummedgrasps@comcast.net', '020 6345639483', STR_TO_DATE('30-Jan-1995', '%d-%M-%Y'), 6745.4866164397035, 1106, 40);
INSERT INTO employees VALUES(4, 'Tomika', 'Grimwood', 'blacklysquid@hotmail.co.uk', '192 5722857827', STR_TO_DATE('20-Aug-1995', '%d-%M-%Y'), 5727.020908520373, 392, 70);
INSERT INTO employees VALUES(5, 'Dina', 'Shekarchi', 'keptmoure@email.com', '840 5515414920', STR_TO_DATE('10-Nov-1997', '%d-%M-%Y'), 7745.181343097232, 531, 110);
INSERT INTO employees VALUES(6, 'Mark', 'Gramolini', 'lowsetamatha@yahoo.co.uk', '076 2244937530', STR_TO_DATE('05-May-2001', '%d-%M-%Y'), 2569.3379470850327, 499, 120);
INSERT INTO employees VALUES(7, 'Kimberley', 'Heckart', 'saidjoey@msn.com', '008 4299154405', STR_TO_DATE('29-Jun-2011', '%d-%M-%Y'), 7389.820579285557, 293, 30);
INSERT INTO employees VALUES(8, 'Marguerita', 'Hao', 'hecticmosque@verizon.net', '217 1857306714', STR_TO_DATE('21-May-1995', '%d-%M-%Y'), 2103.5185502411355, 460, 100);
```
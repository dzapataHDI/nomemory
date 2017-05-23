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

We can use the [`seq()`](MockNeat#seq) to iterate over the two dictionaries and obtain a list of the 241 countries:

```
java

// Generate Countries
final int totalCountries = 241;
List<Country> countries = m.reflect(Country.class)
                            .field("id", m.seq(DictType.COUNTRY_ISO_CODE_2))
                            .field("name", m.seq(DictType.COUNTRY_NAME).mapToString().replaceAll("'", "''"))
                            .field("regionId", m.from(regions).map(r -> r.getId()))
                            .list(totalCountries)
                            .val();
```



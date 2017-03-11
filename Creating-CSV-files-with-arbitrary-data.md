*Scenario:* 

Create a CVS file with arbitrary data that has the following structure:

`id, firstName, lastName, email, salary (euro)`

The file should contain 1000 lines.

*Solution:*

We will use:
- [`longSeq()`](MockNeat#longseq) for generating line ids;
- [`names().first()`](MockNeat#names) for generating first names;
- [`names().last()`](MockNeat#names) for generating last names;
- [`emails()`](MockNeat#emails) for generating emails;
- [`money()`](MockNeat#money) for generating salaries in EURO;

To group everything in a line we can use the [`fmt()`](MockNeat#fmt) function. This works similar with `String.format()` but enable developers to specify named parameters. It's also more efficient.

To generate exactly 1000 lines we can keep everything in a `List<String>` with the `list()` method.

The list will be then written in a file with the `consume()` method.

Code:
```java
MockNeat m = MockNeat.threadLocal();
final Path path = Paths.get("./test.csv");

m.fmt("#{id},#{first},#{last},#{email},#{salary}")
 .param("id", m.longSeq())
 .param("first", m.names().first())
 .param("last", m.names().last())
 .param("email", m.emails())
 .param("salary", m.money().locale(GERMANY).range(2000, 5000))
 .list(1000)
 .consume(list -> {
            try { Files.write(path, list, CREATE, WRITE); }
            catch (IOException e) { e.printStackTrace(); }
 });
```

The result:
```
0,Ailene,Greener,auldsoutache@gmx.com,4.995,59 €
1,Yung,Skovira,sereglady@mail.com,2.850,23 €
2,Shanelle,Hevia,topslawton@mac.com,2.980,19 €
3,Venice,Lepe,sagelyshroud@mail.com,4.611,83 €
4,Mi,Repko,nonedings@email.com,3.811,38 €
5,Leonie,Slomski,plumpcreola@aol.com,4.584,28 €
6,Elisabeth,Blasl,swartjeni@mail.com,2.839,69 €
7,Ernestine,Syphard,prestoshod@aol.com,3.471,93 €
8,Honey,Winfrey,pseudpatria@email.com,4.276,56 €
9,Dian,Holecek,primbra@att.net,3.643,66 €
10,Mitchell,Lawer,lessjoellen@yahoo.com,3.260,92 €
11,Kayla,Labbee,hobnailmastella@mail.com,2.504,99 €
12,Jann,Grafenstein,douremile@verizon.net,4.535,70 €
13,Shaunna,Uknown,taughtclifton@gmx.com,3.028,81 €
14,Paul,Gehri,auldabraham@yahoo.co.uk,4.931,44 €
15,Janie,Hesselink,tanmorton@gmx.com,4.879,61 €
16,Ena,Jordahl,drossyspanks@hotmail.com,4.651,49 €
17,Min,Peterman,nudemargeret@me.com,2.211,34 €
18,Desiree,Valasek,firmemmett@mac.com,4.037,74 €
19,Darci,Nolen,stretchdenver@mac.com,4.582,98 €
20,Irene,Pyper,sanehui@gmail.com,4.150,80 €
21,Beryl,Beydler,shybidez@att.net,4.297,74 €
22,Demetrius,Greaser,sedgedmodesta@verizon.net,3.032,22 €
23,Carli,Dowty,fleetlysole@aol.com,2.011,43 €
....
....
....
995,Maxine,Vatalaro,peakedhuey@yahoo.com,4.560,79 €
996,Carlie,Jessie,stringedsteffanie@gmx.com,4.100,61 €
997,Roxanna,Pozzi,brutehedrix@yahoo.com,4.574,65 €
998,Hedwig,Vranek,smashingstake@hotmail.com,3.146,43 €
999,Autumn,Urso,unpraisedsoots@msn.com,4.701,16 €
```

Note: You can additionaly use the [`escapeCsv()`](MockUnitString#escapeCsv) method from the [`MockUnitString`](MockUnitString) functional interface to be 100% sure your strings are correctly escaped for the CSV format.
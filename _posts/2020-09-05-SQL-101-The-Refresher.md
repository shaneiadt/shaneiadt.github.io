---
title: "SQL 101 The Refresher"
date: 2020-09-05 07:56:02
categories:
  - Blog
tags:
	- SQL
---

<p align="center">
  <img src="/images/sql-101-the-refresher/sql-what.jpg">
</p>

## But I'm a Frontend Dev

Yes, but without backend servers there would be no front-to-end :laughing:. Having some basic knowledge of relational databases and in particular SQL will always come in handy.

First things first, we need a couple of tables to re-jig that brain matter :thinking:

## Gimme Them Tables

Let's use an example of a school, teacher and student database.

If you fancy following along all you will need is a local installation of [MySQL](https://dev.mysql.com/downloads/) and a SQL editor. I enjoy using [Beekeeper Studio](https://www.beekeeperstudio.io/), it's open source and I find it straight forward to use.

**PK** = **Primary Key**
**FK** = **Foreign Key**

#### __Student Table__

| student_id (PK) 	| first_name 	| last_name 	| date_of_birth 	| school_id (FK) 	|
|------------	    |------------	|-----------	|---------------	|-----------	    |
| 1          	    | John       	| Donut     	| 1997-04-15    	| 1         	    |
| 2          	    | Alison     	| Bragg     	| 1996-02-08    	| 2         	    |
| 3          	    | Morgan     	| Lambert     	| 1994-11-19    	| 1         	    |
| 4          	    | Peter     	| Spector	  	| 1998-01-20    	| 3         	    |

#### __Teacher Table__

| teacher_id (PK)	| first_name 	| last_name 	| start_date     	| school_id (FK) 	|
|------------	    |------------	|-----------	|---------------	|-----------	    |
| 1          	    | Mary       	| Harkin     	| 1987-01-19    	| 2         	    |
| 2          	    | John       	| Anderson     	| 1981-08-06    	| 1         	    |
| 3          	    | Paul       	| Berger     	| 1988-09-22    	| 1         	    |
| 4          	    | Ann        	| Connors     	| 2000-02-07    	| 3         	    |

#### __School Table__

| school_id (PK) 	| name                  	| eircode 	| principal_id (FK) 	|
|-----------	    |-----------------------	|---------	|--------------	        |
| 1         	    | St. Mildrews         		| F903LH  	| 1            	        |
| 2         	    | Mount View Highschool 	| GH123M  	| 2            	        |
| 3         	    | Athlone College       	| 4TY21B  	| 4            	        |

## Create Them Tables

Let the creation begin :smiling_imp:

```SQL
CREATE TABLE student (
	student_id INT PRIMARY KEY,
  	first_name VARCHAR(40),
  	last_name VARCHAR(40),
  	date_of_birth DATE,
  	school_id INT
);

CREATE TABLE teacher (
	teacher_id INT PRIMARY KEY,
  	first_name VARCHAR(40),
  	last_name VARCHAR(40),
  	start_date DATE,
  	school_id INT
);

CREATE TABLE school (
	school_id INT PRIMARY KEY,
  	name VARCHAR(40),
  	eircode VARCHAR(40),
  	principal_id INT
);
```

....one more thing we will need to setup is the table relationships.

```SQL
ALTER TABLE student
ADD FOREIGN KEY(school_id)
REFERENCES school(school_id)
ON DELETE CASCADE;

ALTER TABLE teacher
ADD FOREIGN KEY(school_id)
REFERENCES school(school_id)
ON DELETE CASCADE;

ALTER TABLE school
ADD FOREIGN KEY(principal_id)
REFERENCES teacher(teacher_id)
ON DELETE SET NULL;
```

The "__ON DELETE__" key phrase ensures when a row from another table is deleted either to set the value in the referencing table to NULL or completely remove the row from the table.

## Add Some Data

Now that we have our tables created let's add some data so we can run some queries.

First let's add the school data, note we have not specfied a **principal_id** because we don't have any teacher data inserted yet.

```SQL
--Add The Schools
INSERT INTO school(school_id, name, eircode) VALUES(1, 'St. Mildrews', 'F903LH');
INSERT INTO school(school_id, name, eircode) VALUES(2, 'Mount View Highschool', 'GH123M');
INSERT INTO school(school_id, name, eircode) VALUES(3, 'Athlone College', '4TY21B');
```

```SQL
--Add The Teachers
INSERT INTO teacher VALUES(1, 'Mary', 'Harkin', '1987-01-19', 2);
INSERT INTO teacher VALUES(2, 'John', 'Anderson', '1981-08-06', 1);
INSERT INTO teacher VALUES(3, 'Paul', 'Berger', '1988-09-22', 1);
INSERT INTO teacher VALUES(4, 'Ann', 'Connors', '2000-02-07', 3);
```

```SQL
--Add The Students
INSERT INTO student VALUES(1, 'John', 'Donut', '1997-04-15', 1);
INSERT INTO student VALUES(2, 'Alison', 'Bragg', '1996-02-08', 2);
INSERT INTO student VALUES(3, 'Morgan', 'Lambert', '1994-11-19', 1);
INSERT INTO student VALUES(4, 'Peter', 'Spector', '1998-01-20', 3);
```

...and finally let's **ALTER** our school data to set a principal relationship for each school.

```SQL
UPDATE school
SET principal_id = 1
WHERE school_id = 1;

UPDATE school
SET principal_id = 2
WHERE school_id = 2;

UPDATE school
SET principal_id = 4
WHERE school_id = 3;
```
 
We now have our tables & data ready for some querying :grin:

## **S**imple**Q**uery**L**anguage

Let's start with making some straight forward queries and solving the prompts below :thumbsup:

#### **SELECT**

```SQL
--Get all students who were born before 1997
SELECT * FROM student
WHERE date_of_birth < '1997-01-01';
```

| student_id (PK) 	| first_name 	| last_name 	| date_of_birth 	| school_id (FK) 	|
|------------	    |------------	|-----------	|---------------	|-----------	    |
| 2          	    | Alison       	| Bragg     	| 1996-02-08    	| 2         	    |
| 3          	    | Morgan       	| Lambert     	| 1994-11-19    	| 1         	    |

#### **WILDCARDS**

**%** = **any number of characters**
**_** = **one character**

```SQL
--Get the school that contains 'Highschool' in it's name 
SELECT * FROM school
WHERE name LIKE '%Highschool';
```

| school_id (PK) 	| name                  	| eircode 	| principal_id (FK) 	|
|-----------	    |-----------------------	|---------	|--------------	        |
| 2         	    | Mount View Highschool 	| GH123M  	| 2            	        |

```SQL
--Get all teachers that have a four letter first name and started work in August or September
SELECT * FROM teacher
WHERE LENGTH(first_name) <= 4 
AND start_date LIKE '____-09-__' 
OR start_date LIKE '____-08-__';
```

| teacher_id (PK)	| first_name 	| last_name 	| start_date     	| school_id (FK) 	|
|------------	    |------------	|-----------	|---------------	|-----------	    |
| 2          	    | John       	| Anderson     	| 1981-08-06    	| 1         	    |
| 3          	    | Paul       	| Berger     	| 1988-09-22    	| 1         	    |

#### **UNION**

```SQL
--Get all of the first & last names alphabetically of all students & teacher
SELECT first_name, last_name from student
UNION
SELECT first_name, last_name from teacher ORDER BY first_name;
```

| first_name 	| last_name 	|
|------------	|-----------	|
| Alison       	| Bragg     	|
| Ann       	| Connors     	|
| John       	| Donut     	|
| John       	| Anderson     	|
| Mary       	| Harkin     	|
| Morgan       	| Lambert     	|
| Paul       	| Berger     	|
| Peter       	| Spector     	|

#### **NESTED QUERY**

```SQL
--Get all principals
SELECT * FROM teacher
WHERE teacher_id IN (
  SELECT school.principal_id FROM school
);
```

| teacher_id (PK)	| first_name 	| last_name 	| start_date     	| school_id (FK) 	|
|------------	    |------------	|-----------	|---------------	|-----------	    |
| 1          	    | Mary       	| Harkin     	| 1987-01-19    	| 2         	    |
| 2          	    | John       	| Anderson     	| 1981-08-06    	| 1         	    |
| 4          	    | Ann        	| Connors     	| 2000-02-07    	| 3         	    |

#### **JOIN**

```SQL
--Get all principals & their school names
SELECT CONCAT(teacher.first_name,' ',teacher.last_name) AS principal_name, school.name AS school_name
FROM teacher
JOIN school
ON teacher.teacher_id = school.principal_id;
```

| principal_name 	| school_name		     	|
|------------		|---------------			|
| Mary Harkin     	| St. Mildrews   		 	|
| John Anderson     | Mount View Highschool    	|
| Ann Connors     	| Athlone College    		|

The above examples cover most of all the basic / common queries you might run on a database. With these we could create some pretty complex queries, it just takes some practice.

If you're interested in brushing up on your SQL (or if you're a beginner) I would highly recommend watching the [FreeCodeCamp](https://www.freecodecamp.org/) 4+ hour YouTube video....it's totally **FREE**! :sunglasses:

https://www.youtube.com/watch?v=HXV3zeQKqGY
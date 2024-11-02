# SQL Syntax Essentials

```

## Creating a Database
CREATE DATABASE database_name;
CREATE DATABASE school;


USE database_name;
USE school;

CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);

Example

CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    age INT,
    grade_level VARCHAR(10)
);

#Inserting Data into a Table

INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

Example

INSERT INTO students (first_name, last_name, age, grade_level)
VALUES ('Emma', 'Johnson', 14, '8th');

#Retrieving Data with SELECT
SELECT column1, column2, ...
FROM table_name
WHERE condition;


SELECT * FROM students;


UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

Example:
UPDATE students
SET grade_level = '9th'
WHERE first_name = 'Emma';

# Deleting Data
DELETE FROM students
WHERE last_name = 'Johnson';

Example
ALTER TABLE table_name
ADD column_name datatype;

ALTER TABLE students
ADD email VARCHAR(100);
```


# Testing Your SQL Database
# Check if Database Exists:

```
SHOW DATABASES;
USE school;
SHOW TABLES;
DESCRIBE students;

# Insert Sample Data
INSERT INTO students (first_name, last_name, age, grade_level)
VALUES ('Alex', 'Smith', 15, '10th');


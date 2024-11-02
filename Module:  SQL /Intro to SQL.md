 # Intro to SQL 

 What is SQL 
 SQL (Structured Query Language) is powerful for managing and interacting with databases. Let’s go through step-by-step instructions on setting up MySQL and Workbench, as well as covering some foundational SQL syntax for beginners.

 
 ## Step 1: Installing MySQL Server

   ### Download MySQL:
        - Visit the MySQL Community Downloads page.
        - Choose your operating system (Windows, MacOS, Linux).
        -  Download the MySQL Installer.

    ### Run the Installer:
        - Open the downloaded installer and follow the setup steps.
        - During setup, you’ll be prompted to select components. Choose:
            MySQL Server (mandatory)
            MySQL Workbench (for easy GUI-based database interaction)

    ### Configure MySQL Server:
       -  Set up the configuration as prompted.
        When asked for a root password, choose a secure one and remember it—this will be your main login.
       -  Complete the installation by clicking through the steps.

       Step 2: Opening and Connecting MySQL Workbench

    Open MySQL Workbench:
        Once installation is complete, open MySQL Workbench.

    Create a New Connection:
        In Workbench, click on the + icon next to MySQL Connections to set up a new connection.
        Enter connection details:
            Connection Name: Name your connection (e.g., Local MySQL).
            Hostname: localhost (assuming you installed on your local machine).
            Username: root.
            Password: Click Store in Vault and enter the password you set during installation.
        Click Test Connection to ensure the setup is correct.

    Open Your Connection:
        After connecting successfully, double-click your connection to open it and start querying!

## Step 3: Basic SQL Syntax and Commands

Let’s get you familiar with some basic SQL commands you can start practicing right away!


1.Creating a Database
```
CREATE DATABASE sample_db;
```

2. Using a Database
```
USE sample_db;
```
3. Creating a Table
Here’s how to create a simple table called students:

```
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    age INT,
    grade VARCHAR(5)
);
```

4. Inserting Data into a Table
To add data to your table, use INSERT:

```
INSERT INTO students (name, age, grade)
VALUES ('Alice', 14, '8th'),
       ('Bob', 15, '9th');
```

5. Querying Data
To retrieve data, use SELECT:
```
SELECT * FROM students;
```

6. Updating Data
```
Here’s how to update a record:

UPDATE students
SET grade = '10th'
WHERE name = 'Bob';
```

7. Deleting Data
To remove records, use DELETE:
```
DELETE FROM students
WHERE name = 'Alice';
```







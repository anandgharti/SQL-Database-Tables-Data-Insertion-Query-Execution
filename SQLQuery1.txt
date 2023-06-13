--Name: Anand Gharti 
--Roll: THA077BEI006 
--Subj: DBMS         


-------------------------------------------QN.1------------------------------------------------
CREATE DATABASE db_Datas;

CREATE TABLE
    tbl_Employee (
        employee_name VARCHAR(255) NOT NULL,
        street VARCHAR(255) NOT NULL,
        city VARCHAR(255) NOT NULL,
        PRIMARY KEY(employee_name)
    );
 

CREATE TABLE
    tbl_Works (
        employee_name VARCHAR(255) NOT NULL,
        FOREIGN KEY (employee_name) REFERENCES tbl_Employee(employee_name),
        company_name VARCHAR(255),
        salary DECIMAL(10, 2)
    );
 
CREATE TABLE
    tbl_Company (
        company_name VARCHAR(255) NOT NULL,
        city VARCHAR(255),
        PRIMARY KEY(company_name)
    );
 
CREATE TABLE
    tbl_Manages (
        employee_name VARCHAR(255) NOT NULL,
        FOREIGN KEY (employee_name) REFERENCES tbl_Employee(employee_name),
        manager_name VARCHAR(255)
    );
 
INSERT INTO
    tbl_Employee (employee_name, street, city)
VALUES ('Alice Williams','321 Maple St','Houston'),
	   ('Sara Davis','159 Broadway','New York'), 
	   ('Mark Thompson','235 Fifth Ave','New York'),
	   ('Ashley Johnson','876 Market St','Chicago'),
	   ('Emily Williams','741 First St','Los Angeles'),
	   ('Michael Brown','902 Main St','Houston'),
	   ('Samantha Smith','111 Second St','Chicago'),
	   ('John Smith','741 First St','Los Angeles'),
	   ('Jane Doe','321 Maple St','Houston'),
	   ('Jones','22 First St','California'),
	   ('Patrick','123 Main St','New Mexico');
 
INSERT INTO
    tbl_Works (
        employee_name,
        company_name,
        salary
    )
VALUES ('Patrick','Pongyang Corporation',500000),
	   ('Sara Davis','First Bank Corporation',82500.00),
	   ('Mark Thompson','Small Bank Corporation',78000.00),
	   ('Ashley Johnson','Small Bank Corporation',92000.00),
	   ('Emily Williams','Small Bank Corporation',86500.00),
	   ('Michael Brown','Small Bank Corporation',81000.00),
	   ('Samantha Smith','Small Bank Corporation',77000.00),
	   ('John Smith','First Bank Corporation',50000),
	   ('Jane Doe','First Bank Corporation',100000);
 
INSERT INTO
    tbl_Company (company_name, city)
VALUES (
        'Small Bank Corporation', 'Chicago'), 
        ('ABC Inc', 'Los Angeles'), 
        ('Def Co', 'Houston'), 
        ('First Bank Corporation','New York'), 
        ('456 Corp', 'Chicago'), 
        ('789 Inc', 'Los Angeles'), 
        ('321 Co', 'Houston'),
        ('Pongyang Corporation','Chicago');
 
INSERT INTO
    tbl_Manages(employee_name, manager_name)
VALUES 
    ('Mark Thompson', 'Emily Williams'),
    ('John Smith', 'Jane Doe'),
    ('Alice Williams', 'Emily Williams'),
    ('Samantha Smith', 'Sara Davis'),
    ('Patrick', 'Jane Doe');

--Display All Info
SELECT *FROM tbl_Employee;
SELECT *FROM tbl_Works;
SELECT *FROM tbl_Company;
SELECT *FROM tbl_Manages;

-- Update the value of salary to 1000 where employee name= John Smith and company_name = First Bank Corporation
UPDATE tbl_Works
SET salary = 1000
WHERE
    employee_name = 'John Smith'
AND company_name = 'First Bank Corporation';

-------------------------------------------QN.2a------------------------------------------------
select employee_name,company_name from tbl_Works
where company_name = 'First Bank Corporation';


-------------------------------------------QN.2b------------------------------------------------
select employee_name,city from tbl_Employee
where employee_name in(select employee_name from tbl_Works 
						where company_name = 'First Bank Corporation');

-------------------------------------------QN.2c------------------------------------------------
select employee_name,street,city from tbl_Employee
where employee_name in(select employee_name from tbl_Works 
						where company_name = 'First Bank Corporation' AND salary>10000);

-------------------------------------------QN.2d------------------------------------------------
select tbl_Employee.employee_name from tbl_Employee,tbl_Works,tbl_Company
where tbl_Employee.city = tbl_Company.city AND 
tbl_Company.company_name = tbl_Works.company_name 
AND tbl_Works.employee_name = tbl_Employee.employee_name;

-------------------------------------------QN.2e------------------------------------------------
SELECT e.employee_name, e.street, e.city
FROM tbl_Employee e
JOIN tbl_Manages m ON e.employee_name = m.manager_name
WHERE EXISTS (
    SELECT *FROM tbl_Employee 
    WHERE street = e.street
    AND city = e.city
    AND employee_name = m.employee_name);


-------------------------------------------QN.2f------------------------------------------------
SELECT tbl_Employee.employee_name FROM tbl_Employee,tbl_Works
WHERE tbl_Employee.employee_name = tbl_Works.employee_name 
AND NOT company_name = 'First Bank Corporation';


-------------------------------------------QN.2g------------------------------------------------
SELECT tbl_Works.employee_name FROM tbl_Works
WHERE NOT tbl_Works.company_name = 'Small Bank Corporation' AND salary >
(SELECT Max(salary) FROM tbl_Works
WHERE tbl_Works.company_name = 'Small Bank Corporation');

-------------------------------------------QN.2h------------------------------------------------
SELECT tbl_Company.company_name FROM tbl_Company
WHERE NOT company_name = 'Small Bank Corporation' AND 
tbl_Company.city in (SELECT tbl_Company.city FROM tbl_Company 
WHERE company_name = 'Small Bank Corporation');

-------------------------------------------QN.2i------------------------------------------------
SELECT w.employee_name FROM tbl_Works w
WHERE w.salary > (SELECT AVG(salary) FROM tbl_Works
WHERE company_name = w.company_name);

-------------------------------------------QN.2j------------------------------------------------
SELECT company_name,COUNT(company_name) AS no_of_employee FROM tbl_Works
GROUP BY company_name
ORDER BY COUNT(company_name) DESC 
OFFSET 0 ROWS FETCH NEXT 1 ROWS ONLY;

-------------------------------------------QN.2k------------------------------------------------
SELECT company_name,SUM(salary) FROM tbl_Works
GROUP BY company_name
ORDER BY SUM(salary) ASC
OFFSET 0 ROWS FETCH NEXT 1 ROWS ONLY;


-------------------------------------------QN.2l------------------------------------------------
SELECT tbl_Works.company_name FROM tbl_Works
WHERE employee_name in
(SELECT tbl_Works.employee_name FROM tbl_Works
WHERE salary >(SELECT AVG(salary) FROM tbl_Works 
WHERE company_name = 'First Bank Corporation' ))
GROUP BY company_name HAVING NOT company_name = 'First Bank Corporation';

-------------------------------------------QN.3a------------------------------------------------
UPDATE tbl_Employee
SET city = 'NewTown'
WHERE employee_name = 'Jones';

-------------------------------------------QN.3b------------------------------------------------
UPDATE tbl_Works
SET salary = 1.1*salary
WHERE company_name = 'First Bank Corporation';

-------------------------------------------QN.3c------------------------------------------------
UPDATE tbl_Works
SET salary = 1.1*salary
WHERE tbl_Works.employee_name in
(SELECT tbl_Employee.employee_name FROM tbl_Employee
JOIN tbl_Manages ON tbl_Manages.manager_name = tbl_Employee.employee_name)
AND tbl_Works.company_name = 'First Bank Corporation';

-------------------------------------------QN.3d------------------------------------------------
UPDATE tbl_Works
SET salary = 
    CASE
        WHEN salary >= 100000 THEN salary * 1.03 
        WHEN salary < 100000 THEN salary  * 1.1
    END
WHERE tbl_Works.employee_name in
(SELECT tbl_Employee.employee_name FROM tbl_Employee
JOIN tbl_Manages ON tbl_Manages.manager_name = tbl_Employee.employee_name)
AND tbl_Works.company_name = 'First Bank Corporation';

-------------------------------------------QN.3e------------------------------------------------
DELETE FROM tbl_Works
WHERE company_name = 'Small Bank Corporation';
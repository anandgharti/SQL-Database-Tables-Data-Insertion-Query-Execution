# SQL-Database-Tables-Data-Insertion-Query-Execution

employee(employee-name, street, city)
works(employee-name, company-name, salary)
company(company-name, city)
manages (employee-name, manager-name)
//Figure 5

1. Give an SQL schema definition for the employee database. Choose an appropriate
   primary key for each relation schema, and insert any other integrity constraints (for example,
   foreign keys) you find necessary.

2. Consider the employee database of Figure 5, where the primary keys are underlined. Give
  an expression in SQL for each of the following queries:
  (a) Find the names of all employees who work for First Bank Corporation.
  (b) Find the names and cities of residence of all employees who work for First Bank Corporation.
  (c) Find the names, street addresses, and cities of residence of all employees who work for
      First Bank Corporation and earn more than $10,000.
  (d) Find all employees in the database who live in the same cities as the companies for
      which they work.
  (e) Find all employees in the database who live in the same cities and on the same streets
      as do their managers.
  (f) Find all employees in the database who do not work for First Bank Corporation.
  (g) Find all employees in the database who earn more than each employee of Small Bank
      Corporation.
  (h) Assume that the companies may be located in several cities. Find all companies located
      in every city in which Small Bank Corporation is located.
  (i) Find all employees who earn more than the average salary of all employees of their
    company.
  (j) Find the company that has the most employees.
  (k) Find the company that has the smallest payroll.
  (l) Find those companies whose employees earn a higher salary, on average, than the
      average salary at First Bank Corporation.

3. Consider the relational database of Figure 5. Give an expression in SQL for each of the
  following queries:
  (a) Modify the database so that Jones now lives in Newtown.
  (b) Give all employees of First Bank Corporation a 10 percent raise.
  (c) Give all managers of First Bank Corporation a 10 percent raise.
  (d) Give all managers of First Bank Corporation a 10 percent raise unless the salary becomes greater than $100,000; in such cases,
      give only a 3 percent raise.
  (e) Delete all tuples in the works relation for employees of Small Bank Corporation.

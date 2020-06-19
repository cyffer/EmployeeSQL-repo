# EmployeeSQL-repo
SQL homework challenge

**Data Modeling**

In this assignment inspected the CSVs,  and diagrammed ERD data model.

![ERD-SQL-Callenge](https://github.com/cyffer/EmployeeSQL-repo/blob/master/ERD-SQLchallenge.png)

**Data Engineering**

Use the information you have to create a table schema for each of the six CSV files. Remember to specify data types, primary keys, foreign keys, and other constraints.

Import each CSV file into the corresponding SQL table.

```SQL
CREATE TABLE "departments" (
    "dept_no" VARCHAR(255)   NOT NULL,
    "dept_name" VARCHAR(255)   NOT NULL,
    CONSTRAINT "pk_departments" PRIMARY KEY (
        "dept_no"
     )
);

CREATE TABLE "employees" (
    "emp_no" INT   NOT NULL,
    "birth_date" DATE   NOT NULL,
    "first_name" VARCHAR   NOT NULL,
    "last_name" VARCHAR   NOT NULL,
    "gender" VARCHAR   NOT NULL,
    "hire_date" DATE   NOT NULL,
    CONSTRAINT "pk_employees" PRIMARY KEY (
        "emp_no"
     )
);


CREATE TABLE "dept_emp" (
    "emp_no" INT   NOT NULL,
    "dept_no" VARCHAR(255)   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL
);

CREATE TABLE "dept_manager" (
    "dept_no" VARCHAR   NOT NULL,
    "emp_no" INT   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL
);

CREATE TABLE "salaries" (
    "emp_no" INT   NOT NULL,
    "salary" FLOAT   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL
);

CREATE TABLE "titles" (
    "emp_no" INT   NOT NULL,
    "title" VARCHAR   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL
);

ALTER TABLE "dept_emp" ADD CONSTRAINT "fk_dept_emp_emp_no" FOREIGN KEY("emp_no")
REFERENCES "employees" ("emp_no");

ALTER TABLE "dept_emp" ADD CONSTRAINT "fk_dept_emp_dept_no" FOREIGN KEY("dept_no")
REFERENCES "departments" ("dept_no");

ALTER TABLE "dept_manager" ADD CONSTRAINT "fk_dept_manager_dept_no" FOREIGN KEY("dept_no")
REFERENCES "departments" ("dept_no");

ALTER TABLE "dept_manager" ADD CONSTRAINT "fk_dept_manager_emp_no" FOREIGN KEY("emp_no")
REFERENCES "employees" ("emp_no");

ALTER TABLE "salaries" ADD CONSTRAINT "fk_salaries_emp_no" FOREIGN KEY("emp_no")
REFERENCES "employees" ("emp_no");

ALTER TABLE "titles" ADD CONSTRAINT "fk_titles_emp_no" FOREIGN KEY("emp_no")
REFERENCES "employees" ("emp_no");


select * from departments;
select * from dept_emp;
select * from dept_manager;
select * from employees;
select * from salaries;
select * from titles;

```



**Data Analysis**

Completed the database analysis, for the following queations:

```SQL
--1. List the following details of each employee: employee number, last name, first name, gender, and salary.

SELECT
emp.emp_no,
emp.last_name,
emp.first_name,
emp.gender,
sal.salary
FROM employees emp
JOIN salaries sal
ON emp.emp_no = sal.emp_no;




--2. List employees who were hired in 1986.

SELECT
emp.emp_no, emp.first_name, emp.last_name, emp.hire_date
FROM
employees emp
WHERE EXTRACT (YEAR FROM hire_date) = 1986;


--3. List the manager of each department with the following information:
----department number, department name, the manager's employee number,last name, first name, and start and end --employment dates.

SELECT
dep.dept_no, dep.dept_name, Dept_manager.emp_no,
employees.first_name, employees.last_name, Dept_manager.from_date, Dept_manager.to_date
FROM Departments dep
RIGHT JOIN Dept_manager
ON (dep.dept_no = Dept_manager.dept_no)
JOIN employees
ON (Dept_manager.emp_no = employees.emp_no);


--4. List the department of each employee with the following information: employee number, last name, first name, and --department name.

SELECT
empls.emp_no, empls.first_name, empls.last_name, departments.dept_name
FROM employees empls
JOIN dept_emp
ON (empls.emp_no = dept_emp.emp_no)
JOIN departments
ON (dept_emp.dept_no = departments.dept_no);


--5. List all employees whose first name is "Hercules" and last names begin with "B."

SELECT *
FROM employees
WHERE(first_name LIKE 'Hercules' AND last_name LIKE '%B%');


--6.  List all employees in the Sales department, including their employee number, last name, first name, and --department name.

SELECT
empls.emp_no, empls.first_name, empls.last_name, departments.dept_name
FROM employees empls
JOIN dept_emp
ON (empls.emp_no = dept_emp.emp_no)
JOIN departments
ON (dept_emp.dept_no = departments.dept_no)
WHERE departments.dept_name = 'Sales';



--7. List all employees in the Sales and Development departments, including their employee number, last name, first --name, and department name.

SELECT empls.emp_no, empls.first_name, empls.last_name, departments.dept_name
FROM employees empls
JOIN dept_emp
ON (empls.emp_no = dept_emp.emp_no)
JOIN departments
ON (dept_emp.dept_no = departments.dept_no)
WHERE departments.dept_name = 'Sales' OR departments.dept_name = 'Development';


--8. In descending order, list the frequency count of employee last names, i.e., how many employees share each last --name.

SELECT
last_name,
COUNT(last_name) AS "count_of_lastnames"
from employees
GROUP BY
last_name
ORDER BY name_count DESC;



```
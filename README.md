--CREATING TABLES & IMPORTING CSV FILES--
create table departments (
	dept_no varchar(15) NOT NULL,
	dept_name varchar(30) NOT NULL,
	PRIMARY KEY (dept_no)
	);
select * 
from departments;
-------------------------------------------------------------------------------
create table employees ( 
	emp_no INT, 
	birth_date DATE, 
	first_name varchar(20) NOT NULL,
	last_name varchar(20) NOT NULL,
	gender varchar(10) NOT NULL,
	hire_date DATE,
	PRIMARY KEY(emp_no)
	);
select * 
from employees;
-------------------------------------------------------------------------------
create table department_employees (	
	emp_no INT, 
	dept_no varchar(15) NOT NULL,
	from_date DATE,
	to_date DATE,
	PRIMARY KEY (emp_no, dept_no),
  	FOREIGN KEY (emp_no) REFERENCES employees(emp_no),
  	FOREIGN KEY (dept_no) REFERENCES departments(dept_no)
	);
select*
from department_employees;
-------------------------------------------------------------------------------
create table department_manager ( 
	dept_no varchar(15) NOT NULL,
	emp_no INT, 
	from_date DATE,
	to_date DATE,
	PRIMARY KEY (dept_no, emp_no),
  	FOREIGN KEY (emp_no) REFERENCES employees(emp_no),
  	FOREIGN KEY (dept_no) REFERENCES departments(dept_no)
	);
select * 
from department_manager;
-------------------------------------------------------------------------------
create table salaries ( 
	emp_no INT,
	salary INT, 
	from_date DATE,
	to_date DATE,
	PRIMARY KEY (emp_no),
  	FOREIGN KEY (emp_no) REFERENCES employees(emp_no)
	);
select *
from salaries;
-------------------------------------------------------------------------------
create table titles (
	emp_no INT, 
	title varchar(40) NOT NULL, 
	from_date DATE,
	to_date DATE,
	PRIMARY KEY (emp_no, from_date),
  	FOREIGN KEY (emp_no) REFERENCES employees(emp_no)
	);
select *
from titles;
-------------------------------------------------------------------------------
ANALYSIS

--List the following details of each employee: 
--employee number, last name, first name, gender, and salary.
SELECT employees.emp_no, employees.last_name, employees.first_name, employees.gender, salaries.salary
FROM employees
JOIN salaries
ON employees.emp_no = salaries.emp_no;

--List employees who were hired in 1986.
SELECT first_name, last_name, hire_date 
FROM employees
WHERE hire_date BETWEEN '1986-01-01' AND '1987-01-01';

--List the manager of each department with the following information: 
--department number, department name, the manager's employee number, 
--last name, first name, and start and end employment dates.
SELECT departments.dept_no, departments.dept_name, department_manager.emp_no, employees.last_name, employees.first_name, department_manager.from_date, department_manager.to_date
FROM departments
JOIN department_manager
ON departments.dept_no = department_manager.dept_no
JOIN employees
ON department_manager.emp_no = employees.emp_no;

--List the department of each employee with the following information: 
--employee number, last name, first name, and department name.
SELECT department_employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM department_employees
JOIN employees
ON department_employees.emp_no = employees.emp_no
JOIN departments
ON department_employees.dept_no = departments.dept_no;

--List all employees whose first name is "Hercules" and 
--last names begin with "B."
SELECT first_name, last_name
FROM employees
WHERE first_name = 'Hercules'
AND last_name LIKE 'B%';

--List all employees in the Sales department, including their employee 
--number, last name, first name, and department name.
SELECT department_employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM department_employees
JOIN employees
ON department_employees.emp_no = employees.emp_no
JOIN departments
ON department_employees.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales';

--List all employees in the Sales and Development departments, including 
--their employee number, last name, first name, and department name.
SELECT department_employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM department_employees
JOIN employees
ON department_employees.emp_no = employees.emp_no
JOIN departments
ON department_employees.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales' 
OR departments.dept_name = 'Development';

--In descending order, list the frequency count of employee last names, i.e., 
--how many employees share each last name.
SELECT last_name,
COUNT(last_name)
FROM employees

GROUP BY last_name
ORDER BY
COUNT(last_name) DESC;
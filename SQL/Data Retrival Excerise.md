1. Retrieve all records from the `employees` table.
    
2. Select only the `name` and `salary` columns from the `employees` table.
    
3. Retrieve unique department names from the `departments` table.
    
4. Get all employees whose salary is greater than 50,000.
    
5. Fetch all employees who joined between `2020-01-01` and `2023-01-01`.
    
6. Find employees who work in either 'Sales' or 'Marketing' departments.
    
7. Count the total number of employees in the company.
    
8. Get the highest and lowest salary in the `employees` table.
    
9. Retrieve employees sorted by their joining date in descending order.
    
10. Find the total salary expenditure for each department.

[[Aggrecastion Excerise]]

#### Answers
1. `Select * from employees;`
2. `Select name,salary from employees;`
3. `Select distinct(department) from employees;`
4. `Select * from employees where salary>50000;`
5.  [[how to access date parts]]
	`select * from payments where paymentDate between '2003-01-01' and '2004-01-01' order by paymentDate;`
6. `Select * from employees where deparment in ('Sales','Marketing');`
7. `Select Count(*) from the employees;`
8. `select min(amount) as MINIMUM_PAYMENT ,max(amount) as MAXIMUM_PAYMENT from payments;`
9. `Select * from employees order by join_date asc;`


[[Aggrecastion Excerise]]

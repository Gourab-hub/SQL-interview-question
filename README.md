# SQL-interview-question

1. Find Duplicate Records in a Table (Amazon)

SELECT column1, column2, COUNT(*) AS duplicate_count
FROM your_table
GROUP BY column1, column2
HAVING COUNT(*) > 1;

2. Retrieve the Second Highest Salary

SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);

or

SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;

3. Find Employees Without Department (Using LEFT JOIN)

SELECT e.*
FROM Employee e
LEFT JOIN Department d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;

4. Calculate Total Revenue Per Product With Product Name

SELECT p.product_name,
       SUM(s.quantity * s.price) AS total_revenue
FROM Sales s
JOIN Product p
ON s.product_id = p.product_id
GROUP BY p.product_name;

5. Get Top 3 Highest-Paid Employees

SELECT *
FROM Employee
ORDER BY salary DESC
LIMIT 3;

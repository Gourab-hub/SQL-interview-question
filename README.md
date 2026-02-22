# SQL-interview-question

1. Find Duplicate Records in a Table (Amazon)

```sql
SELECT column1, column2, COUNT(*) AS duplicate_count
FROM your_table
GROUP BY column1, column2
HAVING COUNT(*) > 1;
```
2. Retrieve the Second Highest Salary
```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```
or
```sql
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```
3. Find Employees Without Department (Using LEFT JOIN)
```sql
SELECT e.*
FROM Employee e
LEFT JOIN Department d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```
4. Calculate Total Revenue Per Product With Product Name
```sql
SELECT p.product_name,
       SUM(s.quantity * s.price) AS total_revenue
FROM Sales s
JOIN Product p
ON s.product_id = p.product_id
GROUP BY p.product_name;
```
5. Get Top 3 Highest-Paid Employees
```sql
SELECT *
FROM Employee
ORDER BY salary DESC
LIMIT 3;
```
6.total amount per customer using INNER JOIN (Interview)
```sql
SELECT c.customer_id,
       c.customer_name,
       SUM(o.amount) AS total_amount
FROM customers c
INNER JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```
```sql
customers
| customer_id | customer_name |
| ----------- | ------------- |
| 1           | Amit          |
| 2           | Neha          |
| 3           | Rohit         |
| 4           | Kiran         |

```
```sql
orders
| order_id | customer_id | amount |
| -------- | ----------- | ------ |
| 101      | 1           | 500    |
| 102      | 1           | 700    |
| 103      | 2           | 300    |
| 104      | 2           | 200    |
| 105      | 3           | 1000   |

```

```sql
output
| customer_id | customer_name | total_amount |
| ----------- | ------------- | ------------ |
| 1           | Amit          | 1200         |
| 2           | Neha          | 500          |
| 3           | Rohit         | 1000         |
```

7. Customers Who Made Purchases but Never Returned Products (Walmart)
```sql
SELECT DISTINCT c.customer_id
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
WHERE c.customer_id NOT IN (
    SELECT customer_id FROM Returns
);
```
8. Show the Count of Orders Per Customer
```sql
SELECT c.customer_id,
       c.customer_name,
       COUNT(o.order_id) AS order_count
FROM Customers c
LEFT JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```
If you also want customer name:
```sql
SELECT c.customer_id,
       c.customer_name,
       COUNT(o.order_id) AS order_count
FROM Customers c
LEFT JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```
```sql
Customers
| customer_id | customer_name |
| ----------- | ------------- |
| 101         | Amit          |
| 102         | Neha          |
| 103         | Rahul         |
| 104         | Priya         |
```
```sql
Orders
| order_id | customer_id | order_date | total_amount |
| -------- | ----------- | ---------- | ------------ |
| 1        | 101         | 2024-01-01 | 500.00       |
| 2        | 101         | 2024-01-05 | 700.00       |
| 3        | 102         | 2024-01-10 | 300.00       |
| 4        | 102         | 2024-01-12 | 900.00       |
| 5        | 103         | 2024-01-15 | 400.00       |
```
```sql
Result
| customer_id | customer_name | order_count |
| ----------- | ------------- | ----------- |
| 101         | Amit          | 2           |
| 102         | Neha          | 2           |
| 103         | Rahul         | 1           |
| 104         | Priya         | 0           |
```
9. Retrieve All Employees Who Joined in 2023
```sql
SELECT *
FROM Employee
WHERE EXTRACT(YEAR FROM hire_date) = 2023;
```
10. Calculate Average Order Value Per Customer
```sql
SELECT customer_id,
       AVG(total_amount) AS avg_order_value
FROM Orders
GROUP BY customer_id;
```
```sql
| order_id | customer_id | order_date | total_amount |
| -------- | ----------- | ---------- | ------------ |
| 1        | 101         | 2024-01-01 | 500.00       |
| 2        | 101         | 2024-01-10 | 700.00       |
| 3        | 102         | 2024-01-05 | 300.00       |
| 4        | 102         | 2024-01-20 | 900.00       |
| 5        | 103         | 2024-01-15 | 400.00       |
```
```sql
Result
| customer_id | avg_order_value |
| ----------- | --------------- |
| 101         | 600.00          |
| 102         | 600.00          |
| 103         | 400.00          |
```

11. Get the Latest Order Placed by Each Customer
```sql
SELECT customer_id,
       MAX(order_date) AS latest_order_date
FROM Orders
GROUP BY customer_id;
```
12. Find Products That Were Never Sold
```sql
SELECT p.product_id
FROM Products p
LEFT JOIN Sales s
ON p.product_id = s.product_id
WHERE s.product_id IS NULL;
```
```sql
Products
| product_id | product_name | price |
| ---------- | ------------ | ----- |
| 1          | Laptop       | 50000 |
| 2          | Mobile       | 20000 |
| 3          | Tablet       | 15000 |
| 4          | Headphone    | 3000  |
```
```sql
Sales
| sale_id | product_id | quantity | sale_date  |
| ------- | ---------- | -------- | ---------- |
| 101     | 1          | 2        | 2024-01-01 |
| 102     | 2          | 1        | 2024-01-02 |
| 103     | 1          | 1        | 2024-01-03 |
```
```sql
Result
| product_id | product_name |
| ---------- | ------------ |
| 3          | Tablet       |
| 4          | Headphone    |
```
13.Identify the Most Selling Product
```sql
SELECT product_id,
       SUM(quantity) AS total_qty
FROM Sales
GROUP BY product_id
ORDER BY total_qty DESC
LIMIT 1;
```
14. Get Total Revenue and Number of Orders Per Region

```sql
SELECT region,
       SUM(total_amount) AS total_revenue,
       COUNT(*) AS order_count
FROM Orders
GROUP BY region;
```

If region is stored in Customer table
```sql
SELECT c.region,
       SUM(o.total_amount) AS total_revenue,
       COUNT(o.order_id) AS order_count
FROM Orders o
JOIN Customers c
ON o.customer_id = c.customer_id
GROUP BY c.region;
```
```sql
Customers
| customer_id | customer_name | region |
| ----------- | ------------- | ------ |
| 1           | Amit          | East   |
| 2           | Neha          | West   |
| 3           | Rohit         | East   |
| 4           | Priya         | South  |
```
```sql
Orders
| order_id | customer_id | total_amount |
| -------- | ----------- | ------------ |
| 101      | 1           | 500          |
| 102      | 2           | 700          |
| 103      | 1           | 300          |
| 104      | 3           | 400          |
| 105      | 4           | 600          |
```
```sql
After JOIN
| order_id | customer_id | region | total_amount |
| -------- | ----------- | ------ | ------------ |
| 101      | 1           | East   | 500          |
| 102      | 2           | West   | 700          |
| 103      | 1           | East   | 300          |
| 104      | 3           | East   | 400          |
| 105      | 4           | South  | 600          |
```
```sql
Final Output (After GROUP BY)
| region | total_revenue | order_count |
| ------ | ------------- | ----------- |
| East   | 1200          | 3           |
| West   | 700           | 1           |
| South  | 600           | 1           |
```

15. Count Customers with More Than 5 Orders
```sql
SELECT COUNT(DISTINCT customer_id)
FROM Orders
GROUP BY customer_id
HAVING COUNT(*) > 5;
```
16. Retrieve Customers with Orders Above Average Order Value (PayPal)
```sql
SELECT DISTINCT customer_id
FROM Orders
WHERE total_amount > (
    SELECT AVG(total_amount)
    FROM Orders
);
```
17. Find All Employees Hired on Weekends
```sql
SELECT *
FROM Employee
WHERE DAYOFWEEK(hire_date) IN (1, 7);
```
18. Find Duplicate Records
```sql
SELECT email, COUNT(*) 
FROM employees
GROUP BY email
HAVING COUNT(*) > 1;
```
19. Remove Duplicate Records

```sql
DELETE e1
FROM employees e1
JOIN employees e2
ON e1.email = e2.email
AND e1.id > e2.id;
```
20. Find Employees With Highest Salary in Each Department

```sql
SELECT department, MAX(salary) AS max_salary
FROM employees
GROUP BY department;
```
or
```sql
SELECT e.*
FROM employees e
JOIN (
    SELECT department, MAX(salary) AS max_salary
    FROM employees
    GROUP BY department
) m
ON e.department = m.department
AND e.salary = m.max_salary;
```
```sql
Employees Table
| emp_id | name  | department | salary |
| ------ | ----- | ---------- | ------ |
| 1      | Amit  | IT         | 90000  |
| 2      | Neha  | IT         | 75000  |
| 3      | Rohit | HR         | 60000  |
| 4      | Priya | HR         | 55000  |
| 5      | Ankit | Finance    | 80000  |
| 6      | Sneha | Finance    | 70000  |
```
```sql
Step 1: Subquery Result (This becomes table m)
SELECT department, MAX(salary) AS max_salary
FROM employees
GROUP BY department;
| department | max_salary |
| ---------- | ---------- |
| IT         | 90000      |
| HR         | 60000      |
| Finance    | 80000      |
```
```sql
Output
| emp_id | name  | department | salary |
| ------ | ----- | ---------- | ------ |
| 1      | Amit  | IT         | 90000  |
| 3      | Rohit | HR         | 60000  |
| 5      | Ankit | Finance    | 80000  |
```

21. Find Employees Who Joined in Last 30 Days
```sql
SELECT *
FROM employees
WHERE join_date >= CURDATE() - INTERVAL 30 DAY;
```
22. Find Employees With No Manager
```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```
23. Find Common Records Between Two Tables
```sql
SELECT e.id
FROM employees e
INNER JOIN managers m
ON e.id = m.id;
```
24. Count Employees in Each Department
```sql
SELECT department, COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```
25.Difference Between WHERE and HAVING

WHERE → Filters rows before grouping

HAVING → Filters groups after GROUP BY
```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```
26.All Join Example

```sql
employees
| id | name  | department_id |
| -- | ----- | ------------- |
| 1  | Amit  | 1             |
| 2  | Neha  | 2             |
| 3  | Rohit | 3             |
| 4  | Kiran | NULL          |

```
```sql
departments
| id | department_name |
| -- | --------------- |
| 1  | IT              |
| 2  | HR              |
| 4  | Finance         |

```
INNER JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;
```

```sql
Output
| name | department_name |
| ---- | --------------- |
| Amit | IT              |
| Neha | HR              |
```

LEFT JOIN

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;
```
```sql
Output
| name  | department_name |
| ----- | --------------- |
| Amit  | IT              |
| Neha  | HR              |
| Rohit | NULL            |
| Kiran | NULL            |

```

RIGHT JOIN

```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.id;
```
```sql
Output
| name | department_name |
| ---- | --------------- |
| Amit | IT              |
| Neha | HR              |
| NULL | Finance         |
```
FULL OUTER JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d
ON e.department_id = d.id;
```
```sql
Output
| name  | department_name |
| ----- | --------------- |
| Amit  | IT              |
| Neha  | HR              |
| Rohit | NULL            |
| Kiran | NULL            |
| NULL  | Finance         |

```

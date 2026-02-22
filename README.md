# SQL-interview-question

1) Find Duplicate Records in a Table (Amazon)

SELECT column1, column2, COUNT(*) AS duplicate_count
FROM your_table
GROUP BY column1, column2
HAVING COUNT(*) > 1;

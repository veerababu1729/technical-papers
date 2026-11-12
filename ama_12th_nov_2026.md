# AMA Answers on 12th Nov, 2026.
- Veerababu Palepu  
Date: 12-11-2026.

**1. Anoop Vantagodi: What is the use of import abc?**  
`abc` is a Python module that stands for Abstract Base Classes. We import it to create abstract classes, which are classes that can't be used directly. They force child classes to write certain methods.
<br>  
<br>
**2. Khushi Rani: Why do we use distinct in sql?**  
`DISTINCT` removes duplicate rows from the result. If we select one column, it shows each value only once. If we select multiple columns, it removes a row only when the whole combination is repeated.  
For example, `SELECT DISTINCT city FROM students;` shows each city only once.  
              `SELECT DISTINCT city, age FROM students;` city and age combination is not repeated in the results.
<br>  
<br>
**3. Kiran B L: What is difference between union and union all?**  
`UNION` combines results of two queries and removes duplicate rows. `UNION ALL` combines them and keeps the duplicates, so it is also faster.
<br>  
<br>
**4. Kummari Amaresh: Use of UPDATE command in sql?**  
`UPDATE` changes data that is already in a table. Example: `UPDATE students SET age = 21 WHERE id = 1;`. Always use `WHERE`, otherwise all rows get updated.
<br>  
<br>
**5. Kuppireddy Sivashankara Reddy: What is the use of primary key?**  
A primary key uniquely identifies every row in a table. It can't be NULL and can't have duplicates. Example: student id. In a table there is a single primary key and it can be combination of one or more columns (composite primary key).
<br>  
<br>
**6. Manish Nalumachu: Difference between where and having?**  
`WHERE` filters rows before grouping. `HAVING` filters groups after `GROUP BY`, and it works with aggregate functions like `COUNT()` or `SUM()`.
<br>  
<br>
**7. Masarpu Jyothish: How to concatenate values?**  
In PostgreSQL we can use `||` or the `CONCAT()` function.  
Example: `SELECT first_name || ' ' || last_name FROM students;`
or `SELECT CONCAT(first_name, ' ', last_name) FROM students;`
<br>  
<br>
**8. Mohd Saad Shaikh: What is subquery?**  
A subquery is a query written inside another query. The inner query runs first and its result is used by the outer query.  
Example: `SELECT name FROM students WHERE marks > (SELECT AVG(marks) FROM students);`
<br>  
<br>
**9. Moyya Naveen: List all the logical operators in sql?**  
The main ones are `AND`, `OR` and `NOT`. Others that also work like logical operators are `IN`, `BETWEEN`, `LIKE`, `EXISTS`, `ANY`, `ALL`, and `SOME`.
<br>  
<br>
**10. Mudrakola Sai Pavan: How to delete table?**  
Use `DROP TABLE table_name;`. Note that `DELETE` only removes rows, not the table.
<br>  
<br>
**11. Neha Sunil Magadum: Difference between truncate and drop?**  
`TRUNCATE` removes all rows but the table structure stays. `DROP` removes the whole table, structure and data both.
<br>  
<br>
**12. Parameswaran R: difference between head and tail in cli?**  
`head` shows the first lines of a file (10 by default). `tail` shows the last lines (10 by default). We can change the count like `head -n 5 file.txt`.
<br>  
<br>
**13. Pavan Kumar: what is limit clause?**  
`LIMIT` restricts how many rows the query returns. Example: `SELECT * FROM students LIMIT 5;` gives only 5 rows.
<br>  
<br>
**14. Raghu B S: Difference between "like" and "="?**  
`=` checks for an exact match. `LIKE` checks for a pattern using wildcards like `%` and `_`.
<br>  
<br>
**15. Shakthivel: Use of "%" while comparing a value?**  
`%` is a wildcard used with `LIKE`. It stands for any number of characters (even zero). `'A%'` means starts with A, `'%a'` means ends with a, and `'%an%'` means contains "an".
<br>  
<br>
**16. Sunkara Prasanth: What will select clause do in sql?**  
`SELECT` is used to fetch data from a table. We tell it which columns we want, like `SELECT name, age FROM students;`, or `*` for all columns.
<br>  
<br>
**17. Varad Rajesh Barbhai: Use of group by clause in sql?**  
`GROUP BY` puts rows with the same value into groups, so we can use aggregate functions on each group.  
Example: `SELECT city, COUNT(*) FROM students GROUP BY city;` gives the number of students in each city.

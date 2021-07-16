# Using NULL

![Using%20NULL%2014e9359fee6742fd939956ecb97b40bf/Untitled.png](Using%20NULL%2014e9359fee6742fd939956ecb97b40bf/Untitled.png)

## Teachers and Departments

The school includes many departments. Most teachers work exclusively for a single department. Some teachers have no department.

[Selecting NULL values.](https://sqlzoo.net/wiki/Selecting_NULL_values.)

## NULL, INNER JOIN, LEFT JOIN, RIGHT JOIN

# 1.

List the teachers who have NULL for their department.

*Why we cannot use =*

You might think that the phrase dept=NULL would work here but it doesn't - you can use the phrase `dept IS NULL`

*That's not a proper explanation.*

No it's not, but you can read a better explanation at Wikipedia:[NULL.](http://en.wikipedia.org/wiki/Null_%28SQL%29)

```sql
SELECT name FROM teacher
 WHERE dept IS NULL
```

# 2.

Note the INNER JOIN misses the teachers with no department and the departments with no teacher.

```sql
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```

# 3.

Use a different JOIN so that all teachers are listed.

```sql
SELECT teacher.name, dept.name
  FROM teacher FULL JOIN dept
    ON (teacher.dept=dept.id)
 WHERE teacher.name IS NOT NULL
```

# 4.

Use a different JOIN so that all departments are listed.

```sql
SELECT teacher.name, dept.name
  FROM teacher FULL JOIN dept
    ON (teacher.dept=dept.id)
 WHERE dept.name IS NOT NULL
```

Using the [COALESCE](https://sqlzoo.net/wiki/COALESCE) function

# 5.

Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. **Show teacher name and mobile number or '07986 444 2266'**

```sql
SELECT name, COALESCE(mobile, '07986 444 2266')
  FROM teacher
```

# 6.

Use the COALESCE function and a LEFT JOIN to print the teacher **name** and department name. Use the string 'None' where there is no department.

```sql
SELECT teacher.name, COALESCE(dept.name, 'None') 
  FROM teacher LEFT JOIN dept ON (teacher.dept=dept.id)
 WHERE teacher.name IS NOT NULL
```

# 7.

Use COUNT to show the number of teachers and the number of mobile phones.

```sql
SELECT COUNT(name), COUNT(mobile)
  FROM teacher
```

# 8.

Use COUNT and GROUP BY **dept.name** to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.

```sql
SELECT dept.name, COUNT(teacher.name)
  FROM dept LEFT JOIN teacher ON (dept.id=teacher.dept)
GROUP BY dept.name
```

Using [CASE](https://sqlzoo.net/wiki/CASE)

# 9.

Use CASE to show the **name** of each teacher followed by 'Sci' if the teacher is in **dept** 1 or 2 and 'Art' otherwise.

```sql
SELECT name, CASE 
                 WHEN (dept=1 OR dept=2) THEN 'Sci' 
                 ELSE 'Art' 
             END
  FROM teacher
```

# 10.

Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.

```sql
SELECT name, CASE 
                  WHEN (dept=1 OR dept=2) THEN 'Sci' 
                  WHEN (dept=3) THEN 'Art'
                  ELSE 'None' 
             END
  FROM teacher
```

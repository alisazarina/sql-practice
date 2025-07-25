# SQL Query Practice
- From [sqlbolt](https://sqlbolt.com/lesson)

# Lesson 1 : SELECT queries 101
1. Find the title of each film
```
SELECT title
FROM movies;
```
2. Find the director of each film
```
SELECT director
FROM movies;
```
3. Find the title and director of each film
```
SELECT title, director
FROM movies;
```
4. Find the title and year of each film
```
SELECT title, year
FROM movies;
```
5. Find all the information about each film
```
SELECT *
FROM movies;
```

# Lesson 2 : Queries with constraints (Pt. 1)
1. Find the movie with a row id of 6
```
SELECT *
FROM movies
WHERE id = 6;
```
2. Find the movies released in the years between 2000 and 2010
```
SELECT *
FROM movies
WHERE year
BETWEEN 2000 AND 2010;
```
3. Find the movies not released in the years between 2000 and 2010
```
SELECT *
FROM movies
WHERE year
NOT BETWEEN 2000 AND 2010;
```
4. Find the first 5 Pixar movies and their release year
```
SELECT *
FROM movies
WHERE year
LIMIT 5;
```

# Lesson 3 : Queries with constraints (Pt. 2)
1. Find all the Toy Story movies
```
SELECT title
FROM movies
WHERE title
LIKE "%Toy Story%";
```
2. Find all the movies directed by John Lasseter
```
SELECT title
FROM movies
WHERE director = "%John Lasseter%";
```
3. Find all the movies (and director) not directed by John Lasseter
```
SELECT title
FROM movies
WHERE director != "%John Lasseter%";
```
4. Find all the WALL-* movies
```
SELECT title
FROM movies
WHERE title LIKE "WALL-%"
```

# Lesson 4 : Filtering and sorting Query results
1. List all directors of Pixar movies (alphabetically), without duplicates
```
SELECT DISTINCT director
FROM movies
ORDER BY director ASC;
```
2. List the last four Pixar movies released (ordered from most recent to least)
```
SELECT DISTINCT title
FROM movies
ORDER BY year DESC
LIMIT 4;
```
3. List the first five Pixar movies sorted alphabetically
```
SELECT title
FROM movies
ORDER BY title ASC
LIMIT 5;
```
4. List the next five Pixar movies sorted alphabetically
```
SELECT title
FROM movies
ORDER BY title
LIMIT 5 OFFSET 5;
```

# Lesson 5 : Review Simple SELECT Queries
1. List all the Canadian cities and their populations
```
SELECT city, population
FROM north_american_cities
WHERE country = "Canada";
```
2. Order all the cities in the United States by their latitude from north to south
```
SELECT city
FROM north_american_cities
WHERE country = "United States"
ORDER BY latitude DESC;
```
3. List all the cities west of Chicago, ordered from west to east
```
SELECT city
FROM north_american_cities
WHERE longitude < -87.629798
ORDER BY longitude;
```
4. List the two largest cities in Mexico (by population)
```
SELECT city
FROM north_american_cities
WHERE country = "Mexico"
ORDER BY population DESC
LIMIT 2;
```
5. List the third and fourth largest cities (by population) in the United States and their population
```
SELECT city
FROM north_american_cities
WHERE country = "United States"
ORDER BY population DESC
LIMIT 2 OFFSET 2;
```

# Lesson 6 : Multi-table queries with JOINs
- Mentions **Database Normalization** (minimizes duplicate data in any single table).
- Allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car).
- Template (using **INNER JOIN**):
```
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```
## Exercises
1. Find the domestic and international sales for each movie
```
SELECT title, domestic_sales, international_sales
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```
2. Show the sales numbers for each movie that did better internationally rather than domestically
```
SELECT title, domestic_sales, international_sales
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id
WHERE international_sales > domestic_sales;
```
3. List all the movies by their ratings in descending order
```
SELECT title, rating
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id
ORDER BY rating DESC;
```

# Lesson 7 : OUTER JOINs
- Depending on how you want to analyze the data, the **INNER JOIN** we used last lesson might not be sufficient because the resulting table only contains data that belongs in both of the tables.
- If the two tables have asymmetric data, which can easily happen when data is entered in different stages, then we would have to use a **LEFT JOIN**, **RIGHT JOIN** or **FULL JOIN** instead to ensure that the data you need is not left out of the results.
- Template Select Query with **LEFT/RIGHT/FULL JOINs** on multiple tables:
```
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```
- Like the **INNER JOIN** these three new joins have to specify which column to join the data on.
- When joining table A to table B, a **LEFT JOIN** simply includes rows from A regardless of whether a matching row is found in B.
- The **RIGHT JOIN** is the same, but reversed, keeping rows in B regardless of whether a match is found in A.
- Finally, a **FULL JOIN** simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table.

## Exercises
1. Find the list of all buildings that have employees
```
SELECT DISTINCT building FROM employees;
```
2. Find the list of all buildings and their capacity
```
SELECT * FROM buildings;
```
3. List all buildings and the distinct employee roles in each building (including empty buildings)
```
SELECT DISTINCT building_name, role
FROM buildings
LEFT JOIN employees
    ON building_name = employees.building;
```

# Lesson 8 : A short note on NULLs
- An alternative to **NULL** values in your database is to have data-type appropriate default values, like 0 for numerical data, empty strings for text data, etc.
- But if your database needs to store incomplete data, then **NULL** values can be appropriate if the default values will skew later analysis (for example, when taking averages of numerical data).
  
- Sometimes, it's also not possible to avoid **NULL** values, as we saw in the last lesson when outer-joining two tables with asymmetric data. In these cases, you can test a column for NULL values in a **WHERE** clause by using either the **IS NULL** or **IS NOT NULL** constraint.
- Template of **Complete Select Query**:
```
Select query with constraints on NULL values
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

## Exercises
1. Find the name and role of all employees who have not been assigned to a building
```
SELECT name, role FROM employees WHERE building IS NULL;
```
2. Find the names of the buildings that hold no employees
```
SELECT DISTINCT building_name
FROM buildings
LEFT JOIN employees
    ON building_name = employees.building
WHERE employees.building IS NULL;
```

# Lesson 9 : Queries with expressions
- Example query with expressions:
```
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```
- The use of expressions can save time and extra post-processing of the result data, but can also make the query harder to read, so we recommend that when expressions are used in the SELECT part of the query, that they are also given a descriptive alias using the AS keyword.

- Select query with expression aliases
```
SELECT col_expression AS expr_description, …
FROM mytable;
```

- Regular columns and even tables can also have aliases to make them easier to reference in the output and as a part of simplifying more complex queries.
- Example query with both column and table name aliases:
```
SELECT column AS better_column_name, …
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales
  ON mywidgets.id = widget_sales.widget_id;
```
1. List all movies and their combined sales in millions of dollars
```
SELECT DISTINCT
    title,
    (domestic_sales + international_sales) / 1000000 AS sales
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```
2. List all movies and their ratings in percent
```
SELECT DISTINCT
    title,
    (rating * 10) AS rate_percent
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```
3. List all movies that were released on even number years
```
SELECT title FROM movies WHERE year % 2 = 0;
```

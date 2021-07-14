# SELECT within SELECT tutorial

![SELECT%20within%20SELECT%20tutorial%20311673a392634289b36acf6358bae086/Untitled.png](SELECT%20within%20SELECT%20tutorial%20311673a392634289b36acf6358bae086/Untitled.png)

```
world(name, continent, area, population, gdp)
```

Guide: [Using nested SELECT](https://sqlzoo.net/wiki/Using_nested_SELECT)

## Bigger than Russia

**List each country name where the population is larger than that of 'Russia'.**

```sql
SELECT name FROM world
 WHERE population >
   (SELECT population FROM world
     WHERE name='Russia')
```

## Richer than UK

**Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.**

*Per Capita GDP*

```sql
SELECT name FROM world
 WHERE continent = 'Europe'
   AND (gdp/population) > 
       (SELECT gdp/population FROM world
         WHERE name = 'United Kingdom')
```

## Neighbors of Argentina and Australia

**List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.**

```sql
SELECT name, continent FROM world
 WHERE continent = 
       (SELECT continent FROM world
         WHERE name = 'Australia')
   OR continent = 
       (SELECT continent FROM world
         WHERE name = 'Argentina')
ORDER BY name
```

## Between Canada and Poland

**Which country has a population that is more than Canada but less than Poland? Show the name and the population.**

```sql
SELECT name, population FROM world
 WHERE population > 
       (SELECT population FROM world
        WHERE name = 'Canada')
   AND population <
       (SELECT population FROM world
        WHERE name = 'Poland')
```

## Percentages of Germany

Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.

**Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.**

The format should be Name, Percentage for example:

![SELECT%20within%20SELECT%20tutorial%20311673a392634289b36acf6358bae086/Untitled%201.png](SELECT%20within%20SELECT%20tutorial%20311673a392634289b36acf6358bae086/Untitled%201.png)

*Decimal places*

You can use the function [ROUND](https://sqlzoo.net/wiki/ROUND) to remove the decimal places.

*Percent symbol %*

You can use the function [CONCAT](https://sqlzoo.net/wiki/CONCAT) to add the percentage symbol.

```sql
SELECT name, 
       CONCAT(CAST(ROUND(100*population/(SELECT population 
                                           FROM world 
                                          WHERE name = 'Germany'), 0) AS INT), '%') 
FROM world 
WHERE continent = 'Europe'
```

## Bigger than every country in Europe

**Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)**

```sql
SELECT name FROM world
WHERE gdp > ALL(SELECT gdp
                  FROM world
                 WHERE gdp > 0
                   AND continent='Europe');
```

## Largest in each continent

**Find the largest country (by area) in each continent, show the continent, the name and the area:**

```sql
SELECT continent, name, area FROM world x
 WHERE area >= ALL(SELECT area FROM world y
                    WHERE y.continent = x.continent
                      AND area > 0);
```

## First country of each continent (alphabetically)

**List each continent and the name of the country that comes first alphabetically.**

```sql
SELECT continent, name FROM world x 
 WHERE name = ALL(SELECT MIN(name) FROM world y
                   WHERE y.continent = x.continent)
```

## Difficult Questions That Utilize Techniques Not Covered In Prior Sections

**Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.**

```sql
SELECT name, continent, population FROM world
 WHERE name IN (SELECT name
                  FROM world
                 WHERE population <= 25000000)
   AND continent NOT IN (SELECT continent
                           FROM world
                          WHERE population > 25000000)
```

## Three times

**Some countries have populations more than three times that of any of their neighbors (in the same continent). Give the countries and continents.**

```sql
SELECT name, continent FROM world x
 WHERE population/3 > ALL(SELECT population FROM world y
                          WHERE y.continent = x.continent
                            AND y.name <> x.name)
```

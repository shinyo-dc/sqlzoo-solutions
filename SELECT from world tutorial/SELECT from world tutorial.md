# SELECT from world tutorial

## Introduction

[Read the notes about this table.](https://sqlzoo.net/wiki/Read_the_notes_about_this_table.) Observe the result of running this SQL command to show the name, continent and population of all countries.

```sql
SELECT name, continent, population FROM world
```

## Large Countries

[How to use WHERE to filter records.](https://sqlzoo.net/wiki/WHERE_filters) Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.

```sql
SELECT name
FROM world
  WHERE population >= 200000000;
```

## Per capita GDP

Give the `name` and the **per capita GDP** for those countries with a `population` of at least 200 million.

```sql
SELECT name, gdp/population
FROM world
  WHERE population >= 200000000;
```

## South America In millions

Show the `name` and `population` in millions for the countries of the `continent` 'South America'. Divide the population by 1000000 to get population in millions.

```sql
SELECT name, population/1000000
FROM world
  WHERE continent = 'South America';
```

## France, Germany, Italy

Show the `name` and `population` for France, Germany, Italy

```sql
SELECT name, population
FROM world
  WHERE name IN ('France', 'Germany', 'Italy');
```

## United

Show the countries which have a `name` that includes the word 'United'

```sql
SELECT name
FROM world
  WHERE name LIKE '%United%';
```

## Two ways to be big

Two ways to be big: A country is **big** if it has an area of more than 3 million sq km or it has a population of more than 250 million.

**Show the countries that are big by area or big by population. Show name, population and area.**

```sql
SELECT name, population, area
FROM world
  WHERE population > 250000000 or area > 3000000;
```

## One or the other (but not both)

**Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.**

- Australia has a big area but a small population, it should be **included**.
- Indonesia has a big population but a small area, it should be **included**.
- China has a big population **and** big area, it should be **excluded**.
- United Kingdom has a small population and a small area, it should be **excluded**.

```sql
SELECT name, population, area
FROM world
  WHERE (population > 250000000 and area <= 3000000) 
         or (population <= 250000000 and area > 3000000);
```

## Trillion dollar economies

Show the `name` and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.

**Show per-capita GDP for the trillion dollar countries to the nearest $1000.**

```sql
SELECT name, ROUND(gdp/population, -3)
FROM world
  WHERE gdp >= 1000000000000
```

## Name and capital have the same length

Greece has capital Athens.

Each of the strings 'Greece', and 'Athens' has 6 characters.

**Show the name and capital where the name and the capital have the same number of characters.**

- You can use the [LENGTH](https://sqlzoo.net/wiki/LENGTH) function to find the number of characters in a string

```sql
SELECT name, capital
  FROM world
 WHERE LEN(name) = LEN(capital)
```

## Matching name and capital

The capital of Sweden is Stockholm. Both words start with the letter 'S'.

**Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.**

- You can use the function [LEFT](https://sqlzoo.net/wiki/LEFT) to isolate the first character.
- You can use `<>` as the **NOT EQUALS** operator.

```sql
SELECT name, capital
FROM world
WHERE
  (LEFT(name,1) = LEFT(capital,1)) and (name <> capital)
```

## All the vowels

**Equatorial Guinea** and **Dominican Republic** have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name.

**Find the country that has all the vowels and no spaces in its name.**

- You can use the phrase `name NOT LIKE '%a%'` to exclude characters from your results.
- The query shown misses countries like Bahamas and Belarus because they contain at least one 'a'

```sql
SELECT name
FROM world
WHERE name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
  AND name NOT LIKE '% %'
```

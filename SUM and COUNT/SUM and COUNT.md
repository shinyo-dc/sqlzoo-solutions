# SUM and COUNT

### **World Country Profile: Aggregate functions**

This tutorial is about aggregate functions such as COUNT, SUM and AVG. An aggregate function takes many values and delivers just one value. For example the function SUM would aggregate the values 2, 4 and 5 to deliver the single value 11.

![SUM%20and%20COUNT%20edaf22584b2f4a4783661d005d4e43fc/Untitled.png](SUM%20and%20COUNT%20edaf22584b2f4a4783661d005d4e43fc/Untitled.png)

## You might want to look at these examples first

[Using SUM, Count, MAX, DISTINCT and ORDER BY](https://sqlzoo.net/wiki/Using_SUM,_Count,_MAX,_DISTINCT_and_ORDER_BY).

## Total world population

Show the total **population** of the world.

```
world(name,continent,area,population,gdp)
```

```sql
SELECT SUM(population)
FROM world
```

## List of continents

List all the continents - just once each.

```sql
SELECT DISTINCT(continent)
FROM world
```

## GDP of Africa

Give the total GDP of Africa

```sql
SELECT SUM(gdp) FROM world
 WHERE continent = 'Africa'
```

## Count the big countries

How many countries have an **area** of at least 1000000

```sql
SELECT count(name) FROM world
WHERE area >= 1000000
```

## Baltic states population

What is the total **population** of ('Estonia', 'Latvia', 'Lithuania')

```sql
SELECT SUM(population) FROM world
 WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```

## Using GROUP BY and HAVING

You may want to look at these examples: [Using GROUP BY and HAVING.](https://sqlzoo.net/wiki/Using_GROUP_BY_and_HAVING.)

## Counting the countries of each continent

For each **continent** show the **continent** and number of countries.

```sql
SELECT continent, COUNT(name) FROM world
GROUP BY continent
```

## Counting big countries in each continent

For each **continent** show the **continent** and number of countries with populations of at least 10 million.

```sql
SELECT continent, count(NAME) FROM world
WHERE population >= 10000000
GROUP BY continent
```

## Counting big continents

List the continents that **have** a total population of at least 100 million.

```sql
SELECT continent FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

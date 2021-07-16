# Self JOIN

## Edinburgh Buses

[Details of the database](https://sqlzoo.net/wiki/Edinburgh_Buses.) Looking at the data

```
stops(id, name)
route(num,company,pos,stop)
```

# 1.

How many **stops** are in the database.

```sql
SELECT COUNT(*) FROM stops
```

# 2.

Find the **id** value for the stop 'Craiglockhart'

```sql
SELECT id FROM stops 
 WHERE name='Craiglockhart'
```

# 3.

Give the **id** and the **name** for the **stops** on the '4' 'LRT' service.

```sql
SELECT id,name FROM stops JOIN route ON stops.id=route.stop
 WHERE company='LRT' AND num='4'
```

## Routes and stops

# 4.

The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these **stops** have a count of 2. Add a HAVING clause to restrict the output to these two routes.

```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
  HAVING COUNT(*)=2
```

# 5.

Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.

```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=(SELECT id FROM stops WHERE name='Craiglockhart') 
  AND b.stop=(SELECT id FROM stops WHERE name='London Road')
```

# 6.

The query shown is similar to the previous one, however by joining two copies of the **stops** table we can refer to **stops** by **name** rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'

```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND stopb.name='London Road'
```

## [Using a self join](https://sqlzoo.net/wiki/Using_a_self_join)

# 7.

Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')

```sql
SELECT DISTINCT r1.company, r1.num
  FROM route r1 JOIN route r2 ON 
    (r1.company=r2.company) AND (r1.num=r2.num)
    JOIN stops stopr1 ON (r1.stop=stopr1.id)
    JOIN stops stopr2 ON (r2.stop=stopr2.id)
WHERE stopr1.id=115 AND stopr2.id=137
```

# 8.

Give a list of the services which connect the **stops** 'Craiglockhart' and 'Tollcross'

```sql
SELECT DISTINCT r1.company, r1.num
  FROM route r1 JOIN route r2 ON 
    (r1.company=r2.company) AND (r1.num=r2.num)
    JOIN stops stopr1 ON (r1.stop=stopr1.id)
    JOIN stops stopr2 ON (r2.stop=stopr2.id)
WHERE stopr1.name='Craiglockhart' AND stopr2.name='Tollcross'
```

# 9.

Give a distinct list of the **stops** which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.

```sql
SELECT stopr2.name, r1.company, r1.num
  FROM route r1 JOIN route r2 ON 
    (r1.company=r2.company) AND (r1.num=r2.num)
    JOIN stops stopr1 ON (r1.stop=stopr1.id)
    JOIN stops stopr2 ON (r2.stop=stopr2.id)
WHERE stopr1.name='Craiglockhart'
  AND r1.company='LRT'
```

# 10.

Find the routes involving two buses that can go from **Craiglockhart** to **Lochend**.Show the bus no. and company for the first bus, the name of the stop for the transfer,and the bus no. and company for the second bus.

*Hint*

Self-join twice to find buses that visit Craiglockhart and Lochend, then join those on matching stops.

```sql
SELECT anum, acomp, sb, cnum, ccomp
  FROM (SELECT a.num as anum, a.company as acomp, stopa.name as sa, stopb.name as sb
          FROM route a JOIN route b ON
           (a.company=b.company AND a.num=b.num)
          JOIN stops stopb ON (b.stop=stopb.id)
          JOIN stops stopa ON (a.stop=stopa.id)
         WHERE stopa.name='Craiglockhart') t1
INNER JOIN
         (SELECT c.num as cnum, c.company as ccomp, stopc.name as sc, stopd.name as sd
            FROM route c JOIN route d ON
                (c.company=d.company AND c.num=d.num)
            JOIN stops stopc ON (c.stop=stopc.id)
            JOIN stops stopd ON (d.stop=stopd.id)
           WHERE stopd.name='Lochend') t2
  ON t1.sb=t2.sc
```

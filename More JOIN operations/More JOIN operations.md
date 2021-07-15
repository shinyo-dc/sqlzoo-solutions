# More JOIN operations

This tutorial introduces the notion of a join. The database consists of three tables `movie` , `actor` and `casting` .

![More%20JOIN%20operations%20a4ade009f0b2498aae9de2eaf84bbf89/Untitled.png](More%20JOIN%20operations%20a4ade009f0b2498aae9de2eaf84bbf89/Untitled.png)

![https://sqlzoo.net/w/images/5/50/Movie2-er.png](https://sqlzoo.net/w/images/5/50/Movie2-er.png)

[More details about the database.](https://sqlzoo.net/wiki/More_details_about_the_database.)

## 1962 movies

List the films where the **yr** is 1962 [Show **id**, **title**]

```sql
SELECT id, title
 FROM movie
 WHERE yr=1962
```

## When was Citizen Kane released?

Give year of 'Citizen Kane'.

```sql
SELECT yr
  FROM movie
 WHERE title='Citizen Kane'
```

## Star Trek movies

List all of the Star Trek movies, include the **id**, **title** and **yr** (all of these movies include the words Star Trek in the title). Order results by year.

```sql
SELECT id, title, yr
  FROM movie
 WHERE title LIKE '%Star Trek%'
 ORDER BY yr
```

## id for actor Glenn Close

What **id** number does the actor 'Glenn Close' have?

```sql
SELECT actor.id
  FROM movie JOIN actor ON (movie.id=actor.id)
 WHERE name='Glenn Close'
```

## id for Casablanca

What is the **id** of the film 'Casablanca'

```sql
SELECT id
  FROM movie
 WHERE title='Casablanca'
```

## Cast list for Casablanca

Obtain the cast list for 'Casablanca'.

*what is a cast list?*

The cast list is the names of the actors who were in the movie.

Use **movieid=11768**, (or whatever value you got from the previous question)

```sql
SELECT name
  FROM actor JOIN casting on actor.id=casting.actorid
 WHERE movieid=(SELECT id 
                  FROM movie 
                 WHERE title='Casablanca')
```

## Alien cast list

Obtain the cast list for the film 'Alien'

```sql
SELECT name
  FROM actor JOIN casting on actor.id=casting.actorid
 WHERE movieid=(SELECT id
                  FROM movie
                 WHERE title='Alien')
```

## Harrison Ford movies

List the films in which 'Harrison Ford' has appeared

```sql
SELECT title 
  FROM movie JOIN casting ON movie.id=casting.movieid
             JOIN actor ON casting.actorid=actor.id
 WHERE actor.name='Harrison Ford'
```

## Harrison Ford as a supporting actor

List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the **ord** field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

```sql
SELECT title
  FROM movie JOIN casting ON movie.id=casting.movieid
             JOIN actor ON casting.actorid=actor.id
 WHERE actor.name='Harrison Ford'
   AND casting.ord<>1
```

## Lead actors in 1962 movies

List the films together with the leading star for all 1962 films.

```sql
SELECT title, name
  FROM movie JOIN casting ON movie.id=casting.movieid
             JOIN actor ON casting.actorid=actor.id
 WHERE yr=1962 
   AND casting.ord=1
```

## Busy years for Rock Hudson

Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT yr, COUNT(title)
 FROM movie JOIN casting ON movie.id=casting.movieid
            JOIN actor ON casting.actorid=actor.id
 WHERE actor.name='Rock Hudson'
GROUP BY yr
 HAVING COUNT(title) > 2
```

## Lead actor in Julie Andrews movies

List the film title and the leading actor for all of the films 'Julie Andrews' played in.

*Did you get "Little Miss Marker twice"?*

```sql
SELECT title, name
  FROM movie JOIN casting ON (movie.id=casting.movieid
                                AND casting.ord=1)
             JOIN actor ON casting.actorid=actor.id
 WHERE movieid IN (SELECT movieid FROM casting
                    WHERE actorid IN (SELECT id FROM actor
                                       WHERE name='Julie Andrews'))
```

## Actors with 15 leading roles

Obtain a list, in alphabetical order, of actors who've had at least 15 **starring** roles.

```sql
SELECT name
  FROM movie JOIN casting ON movie.id=casting.movieid
             JOIN actor ON casting.actorid=actor.id
 WHERE casting.ord=1
 GROUP BY name
 HAVING COUNT(title)>=15
```

## 14.

List the films released in the year 1978 ordered by the number of actors in the cast, then by title

```sql
SELECT title, COUNT(name)
  FROM movie JOIN casting ON movie.id=casting.movieid
             JOIN actor ON casting.actorid=actor.id
 WHERE yr=1978
 GROUP BY title
 ORDER BY COUNT(name) DESC, title
```

## 15.

List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT name
  FROM movie JOIN casting ON movie.id=casting.movieid
             JOIN actor ON casting.actorid=actor.id
 WHERE movieid IN (SELECT movieid FROM casting 
                    WHERE actorid IN (SELECT id FROM actor
                                       WHERE actor.name='Art Garfunkel'))
   AND actor.name<>'Art Garfunkel'
```

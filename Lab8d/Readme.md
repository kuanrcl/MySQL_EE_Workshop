# Query Optimizer

## Using EXPLAIN

```
SELECT ci.ID, ci.Name, ci.District,
       co.Name AS Country, ci.Population
  FROM world.city ci
       INNER JOIN
         (SELECT Code, Name
            FROM world.country
           WHERE Continent = 'Europe'
           ORDER BY SurfaceArea
           LIMIT 10
         ) co ON co.Code = ci.CountryCode
 ORDER BY ci.Population DESC
 LIMIT 5;
 
 EXPLAIN FORMAT=TRADITIONAL
       SELECT ci.ID, ci.Name, ci.District,
              co.Name AS Country, ci.Population
         FROM world.city ci
              INNER JOIN
                (SELECT Code, Name
                   FROM world.country
                  WHERE Continent = 'Europe'
                  ORDER BY SurfaceArea
                  LIMIT 10
                ) co ON co.Code = ci.CountryCode
        ORDER BY ci.Population DESC
        LIMIT 5\G
  
  EXPLAIN FORMAT=JSON
       SELECT ci.ID, ci.Name, ci.District,
              co.Name AS Country, ci.Population
         FROM world.city ci
              INNER JOIN
                (SELECT Code, Name
                   FROM world.country
                  WHERE Continent = 'Europe'
                  ORDER BY SurfaceArea
                  LIMIT 10
                ) co ON co.Code = ci.CountryCode
        ORDER BY ci.Population DESC
        LIMIT 5\G
 
 EXPLAIN FORMAT=TREE
       SELECT ci.ID, ci.Name, ci.District,
              co.Name AS Country, ci.Population
         FROM world.city ci
              INNER JOIN
                (SELECT Code, Name
                   FROM world.country
                  WHERE Continent = 'Europe'
                  ORDER BY SurfaceArea
                  LIMIT 10
                ) co ON co.Code = ci.CountryCode
        ORDER BY ci.Population DESC
        LIMIT 5\G
 ```
 
 ![EER](img/world_eer.png)


  
        
   
 

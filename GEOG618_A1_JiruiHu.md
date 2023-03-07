# GEOG618
## Question 1
Select location:
1. South park woods south door (45)
2. Park Manor public school (720)
3. Woolwoch memorial centre (32)
4. Former riverside public school (536)
### 2.1.1
```sql
WITH results AS (
SELECT seq, edge AS gid, cost AS seconds FROM pgr_dijkstra(
     '
       SELECT gid as id, 
        source, 
        target, 
        length AS cost 
      FROM ways 
' ,
  45 ,
  536 , 
    directed:= false)
)
select
 results.*,
 st_astext(the_geom) as route_readable
 from results
 left join ways
 using (gid)
 order by seq;
 ```
<img width="545" alt="1678153574137" src="https://user-images.githubusercontent.com/98428762/223297856-892b69e9-f494-4f2a-ae3e-2b2fae0eb0e5.png">

[2.1.1.csv](https://github.com/BuGaoSN/GEOG618/files/10904557/2.1.1.csv)

## Question 4

```sql
SELECT id, lon, lat, st_distance(ST_SetSRID(ST_MakePoint(lon,lat),4326), St_setSRID(st_makepoint(-80.5652469954505,43.59640273366601),4326)) as distance
FROM ways_vertices_pgr
ORDER BY distance
ASC LIMIT 1;
```


### Question 2

```sql
WITH results AS (
SELECT seq, edge AS gid, cost AS seconds FROM pgr_dijkstra(
    '
      SELECT gid AS id,
        source,
        target,
        length_m AS cost
      FROM ways
    ',
ARRAY[45,720],
32,
directed := false)
)
select
 results.*,
 st_astext(the_geom) as route_readable
 from results
 left join ways
 using (gid)
 order by seq;
 ```
<img width="456" alt="1678153854532" src="https://user-images.githubusercontent.com/98428762/223298546-2b42ab0e-26f2-4073-9679-c1bb337f7b01.png">

[2.1.2 result](https://github.com/BuGaoSN/GEOG618/files/10904435/data-1678153964325.csv)



### 2.1.3
```sql
WITH results AS (
SELECT seq, edge AS gid, cost AS seconds FROM pgr_dijkstra(
    '
      SELECT gid AS id,
        source,
        target,
        length_m / 1.3 AS cost
      FROM ways
    ',
536,
ARRAY[720,32],
directed := false)
)
select
 results.*,
 st_astext(the_geom) as route_readable
 from results
 left join ways
 using (gid)
 order by seq;
 ```
 
<img width="529" alt="1678154358622" src="https://user-images.githubusercontent.com/98428762/223299798-7bb1ecc5-e7be-40ac-a8ce-993d81a30883.png">

[2.1.3 result](https://github.com/BuGaoSN/GEOG618/files/10904495/2.1.3_result.csv)

### 3.1.1

```sql
WITH results AS (
SELECT seq, edge AS gid, cost AS seconds FROM pgr_dijkstra(
 '
    SELECT gid AS id,
      source,
      target,
      cost,
      reverse_cost
    FROM ways
  ',
720,
32,
directed := true)
)
select
 results.*,
 st_astext(the_geom) as route_readable
 from results
 left join ways
 using (gid)
 order by seq;
 ```

<img width="511" alt="1678154834713" src="https://user-images.githubusercontent.com/98428762/223300958-6fbb4085-99ad-421b-af80-152748ee50a3.png">

[3.1.1 result](https://github.com/BuGaoSN/GEOG618/files/10904522/3.1.1.result.csv)

### 3.1.2
```sql
WITH results AS (
SELECT seq, edge AS gid, cost AS seconds FROM pgr_dijkstra(
  '
  SELECT gid AS id,
    source,
    target,
    cost,
    reverse_cost
  FROM ways
  ',
32,
720,
directed := true)
)
select
 results.*,
 st_astext(the_geom) as route_readable
 from results
 left join ways
 using (gid)
 order by seq;
 ```

<img width="539" alt="1678155182581" src="https://user-images.githubusercontent.com/98428762/223301771-9d6e0751-cb91-47c9-ac4a-bf9b11e8717a.png">

[3.1.2 result.csv](https://github.com/BuGaoSN/GEOG618/files/10904531/3.1.2.result.csv)

### 3.1.3
```sql
WITH results AS (
SELECT seq, edge AS gid, cost AS seconds FROM pgr_dijkstra(
  '
    SELECT gid AS id,
      source,
      target,
      cost_s / 3600 * 100 AS cost,
      reverse_cost_s / 3600 * 100 AS reverse_cost
    FROM ways
  ',
45,
32)
)
select
 results.*,
 st_astext(the_geom) as route_readable
 from results
 left join ways
 using (gid)
 order by seq;
 ```

<img width="443" alt="1678155428269" src="https://user-images.githubusercontent.com/98428762/223302286-8f8a2d3d-7f87-4f46-adf3-eedbdeeb1c29.png">

[3.1.3 result.csv](https://github.com/BuGaoSN/GEOG618/files/10904545/3.1.3.result.csv)

## Question 2
The OpenStreetMap (OSM) is a global database that contains geospatial data in a simple structure of nodes, ways, and relationships. These elements can be used to represent various features, such as points of interest, roads, railways, administrative borders, and more.

Unlike the basic geometric objects used in GIS, such as points, lines, and polygons, OSM's elements represent topological features. In other words, the topological features are defined based on their components' sequential arrangement in a graph, while geographic features are defined based on their coordinates.

For example, a node in the OSM model represents a point-like entity with an ID identifier, latitude, and longitude, such as a bus stop or a gas station. A way is a sequence of connected nodes that form a linear feature, such as a road, and a relationship is an ordered list of nodes, ways, or other relationships that share a common attribute or purpose. Relationships can be used to model complex features such as roads for a bus route or a lake and its islands.

In comparison, GIS features typically have a one-to-one relationship. For example, a point represents a single location, a line represents a single path, and a polygon represents a single area.

Additionally, the relationships between OSM's elements and geographic features are different. A node can comprise one or more ways, and a way can contain multiple nodes, resulting in one-to-many, many-to-one, or many-to-many relationships. In contrast, GIS features often have a one-to-one relationship.

Overall, OSM's topological features provide a more flexible and versatile approach to representing geospatial data than the basic geometric objects used in GIS. The ability to define complex relationships between elements and represent topological features through sequential arrangement provides a powerful tool for modeling and analyzing complex geospatial phenomena.

## Question 3
A database view is a subset of a database that represents a specific portion of data obtained from one or more database tables. These views are named queries stored in the database and can be used to simplify complex queries, improve data security, ensure data consistency, improve performance, and simplify application development.

Views can simplify complex queries by providing an intuitive interface to the underlying data and hiding the complexity of table structures and relationships. They can also be used to restrict access to sensitive data by limiting user access to certain columns and rows. Views help enforce data consistency by providing a standardized way of accessing and updating data across multiple tables. Views can improve performance by precomputing complex queries and storing the results, which reduces the processing required to execute complex queries and improves response times. Lastly, views can simplify application development by providing a consistent and predictable way of accessing data, reducing the amount of code needed to build applications and improving overall performance.

## Question 4

As shown in the figure, I selected the Woolwich Memorial Centre in Elmira as the reference point with coordinates "-80.5652469954505,43.59640273366601".

<img width="952" alt="1678158478409" src="https://user-images.githubusercontent.com/98428762/223310314-905ccaf5-b1bb-48a6-a083-6f16ffca726d.png">

```sql
SELECT id, lon, lat, st_distance(ST_SetSRID(ST_MakePoint(lon,lat),4326), St_setSRID(st_makepoint(-80.5652469954505,43.59640273366601),4326)) as distance
FROM ways_vertices_pgr
ORDER BY distance
ASC LIMIT 1;
```

<img width="492" alt="1678158798055" src="https://user-images.githubusercontent.com/98428762/223311069-f80a3dc1-e69e-4c44-84ff-2a7973cc0bd8.png">

<img width="438" alt="1678158892435" src="https://user-images.githubusercontent.com/98428762/223311332-5a0de55e-39a9-4cfe-a36f-18ccccd7faac.png">

This code uses the function ST_MakePoint(lon, lat) to generate the geometry of the point with longitude and latitude values from two different sources - the ways_vertices_pgr table and a newly selected point. The spatial reference system identifier is set to 4326 using the ST_SetSRID function, which corresponds to the WGS 84 coordinate system used by GPS.

Using the ST_Distance(... , ...) calculates the distance between the given point and each node in the table and sorts the results in ascending order of distance using the ORDER BY clause.

Finally, LIMIT 1 is used to return only the nearest node.

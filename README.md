# GEOG618
## Question 1
Select location:
1. South park woods south door (45)
2. Park Manor public school (720)
3. Woolwoch memorial centre (32)
4. Former riverside public school (536)
#### 2.1.1
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

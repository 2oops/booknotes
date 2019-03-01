# Commands

1. 设置某一属性值为特征值（表头）
   ```sql
   MATCH (variable:Label {propertyKey1: propertyValue1})
   RETURN variable.propertyKey2 AS alias2
   ```

2. 不连续的表头命名

   ```sql
   MATCH (p:Person {born: 1965})
   RETURN p.name AS name, p.born AS `birth year`
   ```

3. ```sql
   ()          // a node
   ()--()      // 2 nodes have some type of relationship
   ()-->()     // the first node has a relationship to the second node
   ()<--()     // the second node has a relationship to the first node
   ```

4. ```sql
   match (n)-[director]-(m) delete director //删除关系
   ```

5. ```sql
   match (n:person{name:'zhangsan'}),(m:person{name:'lisi'}) create (n)-[r:Friend]->(m) return n,m,r;  //创建关系
   ```

6. ```sql
   MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie {title: 'The Matrix'})
   RETURN p, rel, m
   ```

7. ```sql
   MATCH (node1)-[:Friend |:fdasf]->(node2)
   RETURN node1
    //same as
   MATCH (p)-[rel:ACTED_IN]->(m {title:'The Matrix'})
   RETURN p,m
   ```

8. ```sql
   MATCH (p:Person {name: 'Tom Hanks'})-[:ACTED_IN|:DIRECTED]->(m:Movie)
   RETURN p.name, m.title //Querying by multiple relationships
   ```

9. ```sql
   MATCH (p:Person)-->(m:Movie {title: 'The Matrix'})
   RETURN p, m // 	匿名关系查询
   
   MATCH (p:Person)--(m:Movie {title: 'The Matrix'})
   RETURN p, m
   
   MATCH (m:Movie)<--(p:Person {name: 'Keanu Reeves'})
   RETURN p, m
   ```

10. ```sql
    MATCH (p:Person)-[rel]->(:Movie {title:'The Matrix'})
    RETURN p.name, type(rel) //返回关系类型
    ```

11. ```sql
    MATCH (p:Person)-[:REVIEWED {rating: 65}]->(:Movie {title: 'The Da Vinci Code'})
    RETURN p.name //添加关系属性
    ```

12. ```sql
    MATCH  (p:Person)-[:FOLLOWS]->(:Person {name:'Angela Scope'})
    RETURN p//匹配所有节点
    ```

13. ```sql
    MATCH  path = (:Person)-[:FOLLOWS]->(:Person)-[:FOLLOWS]->(:Person {name:'Jessica Thompson'})
    RETURN  path  //返回符合要求的路径
    ```

14. ```sql
    MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person)
    RETURN a.name, m.title, d.name  //优化查询
    ```

15. ```sql
    MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
    WHERE m.released = 2008 OR m.released = 2009
    RETURN p, m //使用where过滤查询
    ```

16. ```sql
    MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
    WHERE m.released >= 2003 AND m.released <= 2004
    RETURN p.name, m.title, m.released //确定范围查询
    
    MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
    WHERE 2003 <= m.released <= 2004
    RETURN p.name, m.title, m.released
    ```

17. ```sql
    MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
    WHERE p.name='Jack Nicholson' AND exists(m.tagline)
    RETURN m.title, m.tagline  //查询是否存在某个属性
    ```

18. ```sql
    MATCH (p:Person)-[:ACTED_IN]->()
    WHERE p.name STARTS WITH 'Michael'
    RETURN p.name //字符串模糊查询  starts with, ends with, contains
    ```

19. ```sql
    MATCH (p:Person)
    WHERE p.name =~'Tom.*'
    RETURN p.name  //正则查询
    ```

20. ```sql
    MATCH (p:Person)-[:WROTE]->(m:Movie) // pattern 查询
    RETURN p.name, m.title
    
    MATCH (p:Person)-[:WROTE]->(m:Movie)
    WHERE NOT exists( (p)-[:DIRECTED]->() )
    RETURN p.name, m.title
    
    MATCH (gene:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(other:Person)
    WHERE gene.name= 'Gene Hackman'
    AND exists( (other)-[:DIRECTED]->() )
    RETURN  gene, other, m
    ```

21. ```sql
    MATCH (p:Person) // IN
    WHERE p.born IN [1965, 1970]
    RETURN p.name as name, p.born as yearBorn
    
    MATCH (p:Person)-[r:ACTED_IN]->(m:Movie) // IN AND
    WHERE  'Neo' IN r.roles AND m.title='The Matrix'
    RETURN p.name
    ```

22. ```sql
    MATCH p = shortestPath((m1:Movie)-[*]-(m2:Movie))
    WHERE m1.title = 'A Few Good Men' AND
          m2.title = 'The Matrix'
    RETURN  p  // 最短路径
    ```

23. ```sql
    MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
    WHERE p.name ='Tom Cruise'
    RETURN collect(m.title) AS `movies for Tom Cruise` // 收集结果
    
    MATCH (actor:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(director:Person)
    RETURN actor.name, director.name, count(m) AS collaborations, collect(m.title) AS movies  
    //计算结果
    
    MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
    WITH  a, count(a) AS numMovies, collect(m.title) as movies
    WHERE numMovies > 1 AND numMovies < 4
    RETURN a.name, numMovies, movies  //可添加的过程  with,where
    ```

24. ```sql
    MATCH (p:Person)-[:DIRECTED | :ACTED_IN]->(m:Movie)
    WHERE p.name = 'Tom Hanks'
    RETURN m.released, collect(DISTINCT m.title) AS movies  //distinct 控制返回值
    
    MATCH (p:Person)-[:DIRECTED | :ACTED_IN]->(m:Movie)
    WHERE p.name = 'Tom Hanks'
    RETURN m.released, collect(DISTINCT m.title) AS movies ORDER BY m.released DESC // 排序
    ```

25. 
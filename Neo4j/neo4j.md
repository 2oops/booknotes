# Neo4j

* `Neo4j`数据库有一个.id文件保持对未使用记录的跟踪,用来回收未使用记录占用的空间,节点和关系存储文件只关心图的基本存储结构而不是属性数据,这两种记录都是固定大小,从而达到高性能遍历的关键设计决策.几点记录和关系记录都是相当轻量级的,由指向联系和属性列表的指针构成

* 一个节点的所有属性被记录到一个单向链表上面.只有指向下一个属性的指针,没有指向上一个属性的指针

* 两个节点之间的所有关系被记录到一个双向链表上面,既有指向上一个关系的指针,也有指向下一个关系的指针

* 节点存储文件和关系存储文件都是固定长度,只关心结构,不关心属性数据,属性存储文件也是固定长度,只关心数据,不关心结构.当长度不足时,会去申请动态存储,将超出的数据长度存放在动态存储里面,并将地址存放在属性存储文件中.查找的时候进行拼接

* ```sql
  MATCH (customer:Customer)-[:BOUGHT]->(:Basket)
   <-[:IN]-(product:Product)
  RETURN product, count(product)
  ORDER BY count(product) DESC LIMIT 5
  // customers who bought a basket that had a product in it
  // we return the node representing the product(s) and the count of how many product nodes     // matched, then order by the number of nodes that matched in a descending fashion, limiting to // the top five
  ```

* ```sql
  MATCH (customer:Customer {name: ‘Alice’})-[:BOUGHT]
   ->(:Basket)<-[:IN]-(product:Product)
  RETURN product, count(product)
  ORDER BY count(product) DESC LIMIT 5 // ego-centric 以自我为中心
  ```

* ```
  MATCH (customer:Customer {name: ‘Alice’})-[:FRIEND*1..2]->(friend:Customer)
  WHERE customer <> friend
  WITH DISTINCT friend
  MATCH (friend)-[:BOUGHT]->(:Basket)<-[:IN]-(product:Product)
  RETURN product, count(product)
  ORDER BY count(product) DESC LIMIT 5
  ```

* These companies have all  recognized the value in – and necessity of – finding and leveraging connections between data  for a variety of uses that ultimately provide customers with better user experiences(这些公司都认识到 - 并且必须 - 寻找和利用数据之间的连接，以实现各种用途，最终为客户提供更好的用户体验)

* 
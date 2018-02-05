---
title: neo4j官方开发文档阅读记录
categories: Learning
---

> 重新选择neo-4j官方的与python交互包，尝试了py2neo包后发现对neo4j了解还不够，很多操作只是浅尝辄止，所以，决定阅读neo4j的官方开发驱动包，并尝试学习Cypher语言，这对以后对人物关系的挖掘是有帮助的．

### 1.概念
neo-4j由两部分组成:relationship,label和property,label或者relationship中包含property,label与label之间形成关系.

### 2.语法
<font color='red'>2.1 Node语法</font>
Cypher语言用()代表一个节点
```
()
(matrix)
(matrix:Movie)
(matrix:Movie{title:"The Matrix",released:1997})
```

<font color='red'>2.2 Relationship语法</font>
```
-->
-[role]->
-[:ACTED_IN]->
# 关系的类型
-[role:ACTED_IN]->
# 关系的属性值,属性值可以是数组
-[role:ACTED_IN {role:["Neo"]}]->

```

<font color='red'>2.3 Pattern语法</font>
语法中有Node和Relationship
```
(keanu:Person:Actor {name:"Keanu Reeves"})
-[role:ACTED_IN {roles:["Neo"]}]->
(matrix:Movie {title:"The matrix"})
```

保存结点关系path
```
# acted_in中保存的就是path,有很多函数可以对path操作:nodes(path),rels(paht),len(path)
acted_in = (:Person)-[:ACTED_IN]->(:Movie)
```
创建数据及及结点关系
```
CREATE (:MOVIE {title:"The Matrix",released:1997})
CREATE (p:Person { name:"Keanu Reeves", born:1964 })
RETURN p
# 创建关系
CREATE (a:Person { name:"Tom Hanks",
  born:1956 })-[r:ACTED_IN { roles: ["Forrest"]}]->(m:Movie { title:"Forrest Gump",released:1994 })
CREATE (d:Person { name:"Robert Zemeckis", born:1951 })-[:DIRECTED]->(m)
RETURN a,d,r,m
```

匹配pattern
```
MATCH(m:Movie)
RETURN m
MATCH(p:Person {name:"duncan"})
RETURN p
# 匹配关系
MATCH (p:Person { name:"Tom Hanks" })-[r:ACTED_IN]->(m:Movie)
RETURN m.title, r.roles
```

添加节点并添加关系
```
MATCH (p:Person { name:"Tom Hanks" })
CREATE (m:Movie { title:"Cloud Atlas",released:2012 })
CREATE (p)-[r:ACTED_IN { roles: ['Zachry']}]->(m)
RETURN p,r,m
```
更新结点属性,但不确定图中是否存在一个结点时(这样做的代价是开销很大),总之,使用MERGE,它没有找到就会创建.
```
MERGE (m:Movie { title:"Cloud Atlas" })
ON CREATE SET m.released = 2012
RETURN m
```

<font color='red'>2.4 where语法</font>
以下两种写法相同
```
MATCH (m:Movie)
WHERE m.title = "The Matrix"
RETURN m
```
```
MATCH (m:Movie { title: "The Matrix" })
RETURN m
```

```
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE p.name =~ "K.+" OR m.released > 2000 OR "Neo" IN r.roles
RETURN p,r,m
```

where子句可以用关系来判断
```
MATCH (p:Person)-[:ACTED_IN]->(m)
WHERE NOT (p)-[:DIRECTED]->()
RETURN p,m
```

使用别名返回值
```
MATCH (p:Person)
RETURN p, p.name AS name, upper(p.name), coalesce(p.nickname,"n/a") AS nickname, { name: p.name,
  label:head(labels(p))} AS person
```

聚合函数
```
MATCH (:Person)
RETURN count(*) AS people
```

排序和分页
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
RETURN a,count(*) AS appearances
ORDER BY appearances DESC LIMIT 10;
```

聚集
```
MATCH (m:Movie)<-[:ACTED_IN]-(a:Person)
RETURN m.title AS movie, collect(a.name) AS cast, count(*) AS actors
```

合并两个结果
```
MATCH (actor:Person)-[r:ACTED_IN]->(movie:Movie)
RETURN actor.name AS name, type(r) AS acted_in, movie.title AS title
UNION
MATCH (director:Person)-[r:DIRECTED]->(movie:Movie)
RETURN director.name AS name, type(r) AS acted_in, movie.title AS title
```

with语法保留中间结果
```
MATCH (person:Person)-[:ACTED_IN]->(m:Movie)
WITH person, count(*) AS appearances, collect(m.title) AS movies
WHERE appearances > 1
RETURN person.name, appearances, movies
```

增加约束
```
CREATE CONSTRAINT ON (movie:Movie) ASSERT movie.title IS UNIQUE
```

创建索引
```
CREATE INDEX ON :Actor(name)
```

<font color='red'>2.4 Cypher操作</font>

更新操作
```
MATCH (n {name: 'John'})-[:FRIEND]-(friend)
WITH n, count(friend) AS friendsCount
SET n.friendCount = friendsCount
RETURN n.friendsCount
```
<font color="red">2.5 Cypher语法</font>
<font color='red'>2.5.1 CASE语法</font>
```
MATCH (n)
RETURN
CASE n.eyes
WHEN 'blue'
THEN 1
WHEN 'brown'
THEN 2
ELSE 3 END AS result
```
```
MATCH (n)
RETURN
CASE
WHEN n.eyes = 'blue'
THEN 1
WHEN n.age < 40
THEN 2
ELSE 3 END AS result
```
<font color='red'>2.5.2 带参数查询</font>
```
MATCH (n:Person { name: $name })
RETURN n
```
<font color='red'>2.5.3 定义正则表达式</font>
```
MATCH (n:Person)
# regex在之前定义
WHERE n.name =~ $regex
RETURN n.name
```
<font color='red'>2.5.4</font>
用json数据创建结点
```
{
  "props" : {
 
"name" : "Andres",
 
"position" : "Developer"
  }
}

CREATE ($props)
```

用json数据批量创建结点
```
{
  "props" : [ {
 
"awesome" : true,
 
"name" : "Andres",
 
"position" : "Developer"
  }, {
 
"children" : 3,
 
"name" : "Michael",
 
"position" : "Developer"
  } ]
}

UNWIND $props AS properties
CREATE (n:Person)
SET n = properties
RETURN n
```
<font color='red'>2.5.5 查询关系(限定跳数)</font>
a到b的跳数少于7跳
```
(a)-[*..7]->(b)
```
<font color='red'>2.5.6 Match</font>
匹配关系
```
# 不分方向
--
# 带有具体关系
-[r]-
# 指向关系
-->
# 带有具体关系
-[r]->
```

两点之间最短长度的路径
```
MATCH (martin:Person { name: 'Martin Sheen' }),(oliver:Person { name: 'Oliver Stone' }), p =
shortestPath((martin)-[*..15]-(oliver))
RETURN p
```

<font color='red'>2.5.7 直接从CSV文件中批量插入结点数据</font>
```
# CSV文件内容:
"1","ABBA","1992"
"2","Roxette","1986"
"3","Europe","1979"
"4","The Cardigans","1992"

# query
LOAD CSV FROM '{csv-dir}/artists.csv' AS line
CREATE (:Artist { name: line[1], year: toInt(line[2])})
```
当CSV文件包含大量数据时,使用USING PERIODIC COMMIT
```
USING PERIODIC COMMIT
LOAD CSV FROM '{csv-dir}/artists.csv' AS line
CREATE (:Artist { name: line[1], year: toInt(line[2])})
```

<font color='red'>2.5.7 Set</font>
```
# 更新属性
MATCH (peter { name: 'Peter' })
SET peter += { hungry: TRUE , position: 'Entrepreneur' }
```
```
# 给结点增加标签
MATCH (n { name: 'Stefan' })
SET n :German
RETURN n
```
<font color='red'>2.5.8 Delete</font>
```
# 删除单节点
MATCH (n:Useless)
DELETE n

# 删除一个结点及其所有关系
MATCH (n { name: 'Andres' })
DETACH DELETE n
```
<font color='red'>2.5.9 Remove</font>
Remove和Delete不同之处在于,Delete用来删除结点,而Remove用来移除结点的属性和标签.
```
# 移除结点的age属性
MATCH (n { name: 'Peter' })
REMOVE n:German
RETURN n
```
<font color='red'>2.5.10 FOREACH</font>
```
MATCH p =(begin)-[*]->(END )
WHERE begin.name = 'A' AND END .name = 'D'
FOREACH (n IN nodes(p)| SET n.marked = TRUE )
```
### 3.neo4j-python
安装驱动
```
pip install neo4j-driver==1.1.0
```
带参更新数据
```
tx.run( "CREATE (person:Person {name: {name}, title: {title}})",parameters( "name", "Arthur", "title", "king" ) );
```
```
result = session.run("MATCH (weapon:Weapon) WHERE weapon.name CONTAINS {term} "
 
"RETURN weapon.name", {"term": search_term})
```

保存结果
```
session = driver.session()
result = session.run("MATCH (knight:Person:Knight) WHERE knight.castle = {castle} "
 
"RETURN knight.name AS name", {"castle": "Camelot"})
retained_result = list(result)
session.close()
for record in retained_result:
	print("%s is a knight of Camelot" % record["name"])
```


---
title: python与neo-4j交互(对py2neo包做的笔记)
categories: Note
---

1.连接数据库(三种方式相等)
```
graph_1 = Graph()
graph_2 = Graph(host="localhost")
graph_3 = Graph("http://localhost:7474/db/data")
```

2.事务操作
a)直接返回结果
```
graph.data("MATCH (a:Person) RETURN a.name, a.born LIMIT 4")
```
b)以pandas格式返回结果
```
DataFrame(graph.data("MATCH (a:Person) RETURN a.name, a.born LIMIT 4"))
```

事务操作样例
```
from py2neo import Graph, Node, Relationship
g = Graph()
tx = g.begin()
a = Node("Person", name="Alice")
tx.create(a)
b = Node("Person", name="Bob")
ab = Relationship(a, "KNOWS", b)
tx.create(ab)
tx.commit()
g.exists(ab)
```

3.匹配关系
查找alice的所有朋友
```
for rel in graph.match(start_node=alice,rel_type="FRIEND"):
	print(rel.end_node()['name'])
```

4.带参数查询
```
from py2neo import Graph
g = Graph()
# evaluate()返回结果的第一个值
g.run(""MATCH (a) WHERE a.email={x} RETURN a.name",x="bob@acme.com").evaluate()
g.run(""MATCH (a) WHERE a.email={x} RETURN a.name",x="bob@acme.com").data()
```
5.NodeSelector使用,可以使用Cypher语言的where部分
```
from py2neo import Graph,NodeSelector
graph = Graph()
selector = NodeSelector(graph)
slected = selector.select("Person",name="Keanu Reeves")
list(selected)
selected = selector.select("Person").where("_.name =~ 'J.*'","1960 <= _.born < 1970")
list(selected)
```

6.删除操作
```
# 删除所有的
graph.delete_all()
```



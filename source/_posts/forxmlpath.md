---
title: sql高级语法-for xml path
date: 2018-11-01 22:50:21
categories: "数据库"
tags:
    - 数据库
    - sqlserver
description: 

---

# **for xml path**

over()函数

补充：平时写sql
```
select a,b,c count(*) as 'countNum' from table

```
这样会报错
要么将abc也加入到 group by 子句里，
要么使用over函数

select a,b,c count(*) over(partition by a) as 'countNum' from table

a  b   count
1  1    2
1  sd   2
2  222  1
这样既显示了数量  又同时显示了记录  group by就不好搞了
row_number over()
rank over()
lead over()   lag over()

## for xml path :这个在sql server中的作用主要是把行数据转列。在mysql中有group_concat
比如：

stuid|name|hobby
---|:--:|---:
1|tuwei|爬山
2|tuwei|看书
3|tuwei|发呆

现在要转换成一条记录：
stuid|name|hobby
---|:--:|---:
1|tuwei|爬山，看书，发呆

```sql
SELECT B.sName,LEFT(StuList,LEN(StuList)-1) as hobby FROM (
    SELECT 
    sName,
    (SELECT hobby+',' FROM student 
      WHERE sName=A.sName 
      FOR XML PATH('')) AS StuList
      
    FROM student A 
    GROUP BY sName
) B 
```


## PIVOT 和UNPIVOT: 行列转换函数

stuid|name|subject|score
---|:--:|---:
1|tuwei|语文|3
2|tuwei|数学|4
3|tuwei|英语|5
4|zhang|语文|35
5|zhang|数学|43
6|zhang|英语|54

```
select * from stuScore as p 
Pivot
(
    sum(score) for
    p.Subject IN([语文],[数学],[英语])
) AS T
```

行转列
stuid|name|语文|数学|英语
---|:--:|:---:|:---:|:---:
1|tuwei|3|4|5
2|zhang|35|43|54


列转行  转回上面的
```
select P.name, p.Subject, P.score
from
(
    select * from stuScore2
) T
UNPIVOT
(
    Score FOR Subject IN
    (语文，数学，英语)
)p
```


---
title: sql高级语法
date: 2018-11-01 22:50:21
categories: "数据库"
tags:
    - 数据库
    - sqlserver
description: 

---

# **with 语法**
用来实现递归查询，比如当要查询部门时，查询当前部门及下属部门就要采用递归的查询

Id  |Pid  |DeptName 
:--:|:--:|:--:
1    |0   |总部
2    |1    |研发部
3     |1    |测试部
4     |1   |质量部
5     |2    |小组1
6    |2     |小组2
7      |3   |测试1
8    |3     |测试2
9      |5     |前端组
10    |5    |美工

要实现的效果：


Id          |Pid         |DeptName                    |lvl
:--:|:--:|:--:|:--:
2       |1           |研发部                                     |0
5       |2           |小组1                                      |1
6        |2           |小组2                                      |1
9   |5           |前端组                                     |2
10      |5           |美工                                       |2

查询sql:

```sql
with cte as
(
    select Id,Pid,DeptName,0 as lvl from Department
    where Id = 2
    union all
    select d.Id,d.Pid,d.DeptName,lvl+1 from cte c inner join   Department d
    on c.Id = d.Pid
)
select * from cte
```

执行过程：
    递归CTE最少包含两个查询(也被称为成员)。第一个查询为定点成员，定点成员只是一个返回有效表的查询，用于递归的基础或定位点。第二个查询被称为递归成员，使该查询称为递归成员的是对CTE名称的递归引用是触发

首先定点成员 ：
```
select Id,Pid,DeptName,0 as lvl from Department where Id = 2
```
查询出来的结果集为ID=2的一条数据
接下来此条数据作为第二个查询的基础 也就是cte
```
select d.Id,d.Pid,d.DeptName,lvl+1 from cte c inner join   Department d on c.Id = d.Pid
```
此时查出的结果集为 id为2的这一条数据和Department表作连接

Id          |Pid         |DeptName                    |lvl
:--:|:--:|:--:
2       |1           |研发部                                     |0
5       |2           |小组1                                      |1
6        |2           |小组2                                      |1

同理 将得到的记录（上面的id=5和id=6的2条数据作为 cte，id为2的是条件，不是本次的结果）再作为基础，再与Department表作连接 得到

Id          |Pid         |DeptName                    |lvl
:--:|:--:|:--:|:--:
2       |1           |研发部                                     |0
5       |2           |小组1                                      |1
6        |2           |小组2                                      |1
9       |5           |前端组                                     |2
10      |5           |美工                                       |2

依此类推  最后一次讲上面id=9和id=10的记录作为cte ，再与Department表作连接  ， 此次的结果为空 ，递归结束。
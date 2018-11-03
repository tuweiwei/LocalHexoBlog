---

title: Maven
date: 2018-8-25 22:50:21
categories: "工具"
tags:
    - maven
    - 工具
description: maven学习

---


# Dependency Scope
compile:默认的范围  编译测试 运行 都有效



provided： 编译测试 有效 在最后运行的时候不会被加入  比如Servlet ,在最后的运行时就不需要被加入 因为web容器提供了此API

runtime:   测试运行时有效 比如jdbc 因为编译的时候只需要java提供的API即可  但在测试和运行的时候就需要jdbc具体实现


test ：   测试范围有效   junit


system:  同provided  编译和测试    与本机系统相关  可移植性差


import：   导入的范围， 使用在dependencyManagement中，表示从其他pom中 导入到自己的dependency的配置

# 传递依赖
A依赖于B   B依赖于C  那么A也会依赖于C 也就是A会同时引入B和C的 JAR包
# 排除依赖
上面 如果A只想依赖于B 那么排除C就要用到
```
<exclusions>
    <exclusion>
       <g>
       <a>
       <v>
    <exclusino>
</exclusions>
```
# 依赖冲突
1 短路优先 （注： maven对于同一jar 只会引入一个版本）
  A -> B -> C - > X.JAR
  A -> D -> X.JAR
2 路径相同  先申明的优先
# 聚合和继承
一般用于父子模块   集体编译
如 根工程中：
```
<packaging>pom</packaging>
<modules>
    <module>模块1</module>
    <module>模块2</module>
    <module>模块3</module>
</modules>
```

继承：子模块指定  parent标签 共用父模块 依赖






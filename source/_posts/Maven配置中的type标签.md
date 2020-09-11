---
title: Maven配置中的type标签
date: 2020/09/11
categories: Java
tags: Maven
---

此处的type标签指的是配置文件`pom.xml`中dependency标签的子标签，如下：

~~~xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
    <type>bundle</type>
</dependency>
~~~

并且这个标签只出现在部分dependency中，网络资料没有一个准确的解释。

查找Maven的文档，其对于`<type>`的描述只出现在一个用例中，具体如下：

~~~xml
<dependency>
    <groupId>group-c</groupId>
    <artifactId>artifact-b</artifactId>
    <!-- This is not a jar dependency, so we must specify type. -->
    <type>war</type>
</dependency>
<dependency>
    <groupId>group-a</groupId>
    <artifactId>artifact-b</artifactId>
    <!-- This is not a jar dependency, so we must specify type. -->
    <type>bar</type>
</dependency>
~~~

下方有注释：

> NOTE: In two of these dependency references, we had to specify the <type/> element. This is because the minimal set of information for matching a dependency reference against a dependencyManagement section is actually {groupId, artifactId, type, classifier}. In many cases, these dependencies will refer to jar artifacts with no classifier. This allows us to shorthand the identity set to {groupId, artifactId}, since the default for the type field is jar, and the default classifier is null.

大致翻译：

> 注：在使用其中两个依赖引用时，我们必须指定<type/>元素。这是因为根据dependencyManagement标签匹配依赖引用的最小信息集实际上是{groupId，artifactId，type，classifier}。在许多情况下，这些依赖项将引用没有classifier的jar构件。这使得我们可以将identity设置为{groupId，artifactId}，因为type字段的默认值是jar，而默认的classifier是null。

我的理解是dependency的坐标（标识）实际上是由{groupId，artifactId，type，classifier}四项组成的（我们平时使用的一般是{groupId，artifactId，version}），

没有显式指定则type为jar，classifier是null。

而遇到type不是jar的dependency则需要显式指定。

问题：在实际使用时，譬如log4j:log4j的引入，maven repo提供的坐标如下：

~~~xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
    <type>bundle</type>
</dependency>
~~~

然而添加到pom.xml时（加到`<dependencies>`下）却无法正常使用，必须去除type标签

而单独以上例log4j:log4j的情况，虽然maven repo提供的坐标中包含type标签，但其实利用{groupId，artifactId，version}这三项就已经能够唯一确定依赖的坐标了，加上type标签后反而不能正常使用。

胡乱猜测与使用的Java版本有关，本文使用的环境为JDK8，很有可能此标签是为了满足高版本 JDK特性做出的适配，而低版本并不能识别。

由于能力有限，暂时还未碰到type发挥实际作用的地方，以后若有新的信息会再进行补充。
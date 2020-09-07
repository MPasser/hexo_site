---
title: markdown语法学习
date: 2019/05/01
categories: 其他
tags: markdown
---
# 关于标题
<pre>
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
####### 七级标题
</pre>

效果如下：

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
####### 七级标题

所以最多支持六级标题。



# 插入列表
<pre>
- 生物
  - 植物
    - 白菜
      - 毛白菜
      - 圆白菜
  - 动物
    - 哺乳类
    - 灵长类
  - 微生物
    - 细菌
    - 真菌
- 静物
</pre>

效果如下：

- 生物
  - 植物
    - 白菜
      - 毛白菜
      - 圆白菜
  - 动物
    - 哺乳类
    - 灵长类
  - 微生物
    - 细菌
    - 真菌
- 静物



# 插入超链接
<pre>
[百度 (https://www.baidu.com)](https://www.baidu.com)
</pre>

效果如下：

[百度 (https://www.baidu.com)](https://www.baidu.com)



# 插入图片
<pre>
![百度LOGO](https://www.baidu.com/img/baidu_jgylogo3.gif)
</pre>

效果如下：

![百度LOGO](https://www.baidu.com/img/baidu_jgylogo3.gif)



# 如何引用文字
<pre>
> 苟利国家生死以，岂因福祸避趋之。--林则徐
</pre>

效果如下：

> 苟利国家生死以，岂因福祸避趋之。--林则徐



# 行内修饰文字
<pre>
普通文字，**强调文字** ，*斜体文字* ，~~删除的文字~~ ，&lt;span style='color : yellow' &gt;style加持的文字&lt;/span&gt;
</pre>

效果如下：

普通文字，**强调文字** ，*斜体文字* ，~~删除的文字~~ ，<span style='color : yellow' >style加持的文字</span>



# 插入代码
<pre>
```java
public class Test{
    public static void main(String[] args){
        System.out.println("Hello World");
    }
}
```
</pre>

效果如下：

```java
public class Test{
    public static void main(String[] args){
        System.out.println("Hello World");
    }
}
```



# 插入公式
<pre>
```math
E = mc^2
```
</pre>

效果如下：

```math
E = mc^2
```



# 分割线
<pre>
no
---
辣鸡编辑器。
***
液！
</pre>

效果如下：

no
---
辣鸡编辑器。
***
液！

其中---的上一行还会被当做一级标题。



# 预定义
<pre>
&lt;pre&gt;
这句话
是预定义格式的效果
&lt;/pre&gt;
</pre>

效果如下：

<pre>
这句话
是预定义格式的效果
</pre>



# 有序列表
<pre>
1. one
2. e
3. 
</pre>

效果如下：

1. one
2. e
3. 



# 表格
<pre>
name | 价格 |  数量  
-|-|-
香蕉 | $1 | 5 |
苹果 | $1 | 6 |
草莓 | $1 | 7 |  

name | 价格 |  数量  
:-:|-|-:
香蕉 | $1 | 5 |
苹果 | $1 | 6 |
草莓 | $1 | 7 |
</pre>

效果如下：

name | 价格 |  数量  
-|-|-
香蕉 | $1 | 5 |
苹果 | $1 | 6 |
草莓 | $1 | 7 |  

name | 价格 |  数量  
:-:|-|-:
香蕉 | $1 | 5 |
苹果 | $1 | 6 |
草莓 | $1 | 7 |
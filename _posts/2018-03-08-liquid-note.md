---
layout: post
title: Liquid 语法
tags: 编程语言
---

![liquid]({{site.baseurl}} /images/posts/liquid.jpg)

## 基本

liquid 代码可以被分类为`对象`，`标签`和`过滤`代码

### 对象

对象，表示页面中将要**显示的内容**，对象和变量使用双花括号表示

| Input              | Output       |
| ------------------ | ------------ |
| `{{ page.title }}` | Introduction |

上面的实例中，liquid 指向对象`page.title`，对象中包含文本为`Introduction`

### 标签

标签，创建模板的**逻辑和控制流**，标签使用花括号和百分号表示

可以分配变量，创建条件和循环

标签分为：`控制流标签`，`迭代型标签`，`变量赋值型标签`

**Input**

**Output**

### 过滤

过滤，可以变更liquid对象的输出内容，过滤包含在一个输出代码，中间使用`|`分隔

| Input                                     | Output             |
| ----------------------------------------- | ------------------ |
| `{{ "/my/fancy/url" | append: ".html" }}` | /my/fancy/url.html |

### 运算

- 逻辑运算符和比较运算符

| Symbol | Introduction             |
| ------ | ------------------------ |
| ==     | equals                   |
| !=     | does not equals          |
| >      | greater than             |
| <      | less than                |
| \>=    | greater than or equal to |
| <=     | less than or equal to    |
| or     | logical or               |
| and    | logical and              |

- `contains`可以查询一个字符串中是否存在某个子串，也可以查询一个字符串数组中是否包含某个子串，不可以查询一个对象列表中是否存在某个对象
- liquid 中的所有值都是真实的，除了`nil`和`false`

### 类型

- 使用`assign`和`captrure`标签初始化liquid变量


- 字符串 String，使用单引号或者双引号声明一个字符串
- 数值 Number，包含了整型和浮点型数值
- 布尔型 Boolean
- Nil，一个特别的空值
- 数组，
- - 可以包含任何类型的变量的列表
  - 使用迭代类型标签可以遍历一个数组
  - 可以使用`[]`访问到数组中的任意一个元素
  - 可以使用过滤符号`split`将一个字符串切分成一个数组
- 使用连字符"-"结合`\{\%`标签，剥离标签左右的空白

## 标签

### 控制流

- `if`只有当条件语句结果是`true`的时候，才会执行`if`块中的代码

- `unless`可以看成是`if`的对立，只有后面的条件为`false`时，才会执行`unless`块的代码

- 更多的判断条件可以使用`elsif`，`else`，或者使用`case/when`来实现开关式条件语句

### 迭代

- 迭代标签可以将代码循环执行，`for`，`break`，`continue`

- `limit`可以控制迭代循环特定次数

- `offset`可以制定迭代循环的索引初始值

- 定义循环范围，可以使用变量或者固定值定义

- `reversed`可以你想循环

- `tablerow`可以生成一个表格中的一行，但是必须包含在`<table>`标签中

- `tablecol`可以定义表格的列，但是必须包含在`<table>`标签中

### 变量

- 变量标签能创建新的 liquid 变量
- `assign`可以创建一个新的变量，使用`"`可以将创建变量保存为一个字符串
- `capture`可以创建一个字符串变量，值为标签中间字符
- `increment`标签可以创建一个数值型变量，每一次声明将会增加变量值，初始值是0
- `decrement`的用法同`increment`，只是初始值是 -1

## 过滤

经过方法过滤后返回



---

**原文链接** :[Liquid语法](https://www.jianshu.com/p/4224b8ea0ec0)
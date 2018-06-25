---
layout: post
title: YAML 语法
tags: 编程语言
---

![yaml]({{site.baseurl}}/images/posts/yuml.jpeg)

### 简介

YAML 语言，实质上是一种通用的数据串**行化格式**。

### 基本语法规则

- 大小写敏感
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- `#`表示注释

### 数据结构

- 对象，键值对的集合，又称为映射，哈希，字典
- 数组，一组按次序排列的值，又称为序列，列表
- 纯量，单个的、不可再分的值

### 对象

- 对象的一组键值对，使用冒号结构表示

```yaml
animal: pets
```

转化为 javascript 如下

```json
{animal: 'pets'}
```

- 也允许另一种写法，将所有键值对写成一个行内对象

```yaml
hash: {name: Steve, foo: bar}
```

转化为 javascript 如下

```json
{hash: {name: 'Steve', foo: 'bar'}}
```

### 数组

- 一组**连词线**开头的行，构成一个数组

```yaml
- cat
- dog
- fish
```

转为 JS 如下

```js
['cat', 'dog', 'fish']
```

- 数据结构的子成员是一个数组，则可以在该项下面缩进一个空格

### 复合结构

- 对象和数组可以结合使用，形成复合结构

### 纯量

- 纯量是最基本的、不可再分的值

```
- 字符串
- 布尔值
- 整数
- 浮点数
- Null，用`～`表示
- 时间
- 日期
```

- YAML允许使用两个感叹号，强制转换数据类型

### 字符串

- 字符串默认不使用引号表示
- 如果字符串中包含空格或特殊字符，需要放在引号中用
- 单引号和双引号都可以使用，双引号不会对特殊字符转义
- 单引号之中如果还有单引号，必须连续使用两个单引号转义
- 字符串可以写成多行，从第二行开始，必须有一个**单空格**缩进，换行符会被转为空格
- 多行字符串可以使用`|`保留换行符，也可以使用`>`折叠换行
- `+`表示保留文字块末尾的换行，`-`表示删除字符串末尾的换行
- 字符串之中可以插入HTML标记

### 引用

- 锚点`&`和别名`*`，可以用来引用
- `&`用来建立锚点，`<<`表示合并到当前数据，`*`用来引用锚点

### 函数和正则表达式的转换

这是 [JS-YAML](https://github.com/nodeca/js-yaml) 库特有的功能，可以把函数和正则表达式转为字符串

```yaml
# example.yml
fn: function () { return 1 }
reg: /test/
```

解析为 JS 如下

```js
var yaml = require('js-yaml')
var fs = require('fs')

try {
    var doc = yaml.load(
    fs.readFileSymc('./example.yml', 'utf8')
    );
    console.log(doc);
} catch (e) {
    console.log(e)
}
```



---

**原文链接** ：[YAML 语言教程](http://www.ruanyifeng.com/blog/2016/07/yaml.html)
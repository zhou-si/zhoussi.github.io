---
layout: post
title: JavaScript 中的面向对象和原型
tag: 编程语言
---

![JsPrpto]({{site.baseurl}}/images/posts/jsProto.png)

### 创建一个对象，新建属性和方法
```js
var box = new Object();
box.name = 'Jack';
box.age = 100;
box.run = function() {
    return this.name + this.age + 'running...';
}

console.log(box.run());     // Jack100running...
```

### 重复实例化，代码冗余 --》 工厂模式

```js
function CreatObject(name, age) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.run = function(){
        return this.name + this.age + 'running...';
    };

    return obj;
}
var box = new CreatObject('Jack', 100);
console.log(box.run());     // Jack100running...
```

### 无法确定实例所属对象 --》 构造函数

```js
function Box(name, age) {
    this.name = name;
    this.age = age;
    this.run = function() {
        return this.name + this.age + 'running...';
    }
}

var box = new Box('Jack', 100);
console.log(box.run());     // Jack100running...
```

### 构造函数中的方法引用不唯一 --》 原型模式

```js
function Box() {};

Box.prototype.name = 'Jack';
Box.prototype.age = 100;
Box.prototype.run = function() {
    return this.name + this.age + 'running....';
}
var box = new Box();
console.log(box.run());     // Jack100running...
```

### 为了体现封装 --》 原型创建的字面量方式

```js

function Box() {};  // 此时 constructor 是指向 Box 的

var boxA = new Box();
console.log(boxA.constructor == Box);       // true
console.log(boxA.constructor == Object);    // false

// 字面量方式，相当于再次 `Box.prototype = {...};`，重写的原型会切断之前的原型；
// 新对象的 constructor 自然指向新对象 Object
Box.prototype = {    
    name: 'Jack',
    age: 100,
    run: function() {
        return this.name + this.age + 'running....';
    }
};

var box = new Box();
console.log(box.run());     // Jack100running...

// 字面量方式，constructor 会指向 Object
console.log(box.constructor == Box);    // false
console.log(box.constructor == Object);    // true
```

###  为了解决传参和共享 --》 构造函数+原型模式

```js

 function Box(name, age) {  // 不共享的使用构造函数
     this.name = name;
     this.age = age;
 }
 Box.prototype = {          // 共享的使用原型模式
     constructor: Box,      // 字面量模式切断原型，直接强制指向 Box，解决问题
     run: function() {
        return this.name + this.age + 'running...';
     }
 };

 var box = new Box('Jack', 100);
 console.log(box.run());    // Jack100running...
```

### 构造函数+原型模式 很怪异 --》 动态原型模式

```js

function Box(name, age) {
    this.name = name;
    this.age = age;

    if(typeof this.run != 'function') {     // 仅在第一次调用的时候初始化
        Box.prototype.run = function() {    // 注意，不可以使用字面量的方式重写原型，会切断实例和新原型之间的联系
            return this.name + this.age + 'running...';
        };
    
        /* Box.prototype = {                
            constructor: Box,
            run: function() {               // 外界box会读取不到哦
                return this.name + this.age + 'running...';
            }
        }; */
    }
}

var box = new Box('Jack', 100);
console.log(box.run());     // Jack100running...
```



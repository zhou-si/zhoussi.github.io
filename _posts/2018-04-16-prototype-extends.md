---
layout: post
title: JavaScipt 中的继承
tag: JavaScript
---



### JS 继承机制的设计思想

Brendan Eich，

- 借鉴 C++ 和 Java ，把 new 命令引入了 JS，用来从原型对象生成一个实例对象。
- 他做了简化，在 JS 中，new 命令后面跟的不是类，而是构造函数。

new 构造函数缺点：

- 构造函数，生成实例对象，无法共享属性和方法；
- 每个实例对象，都有自己的属性和方法的副本，资源浪费。

引入 prototype 属性，

- BE 为构造函数设置了一个 `prototype` 属性；
- `prototype` 属性包含一个对象（`prototype对象`）；
- 所有实例对象需要共享的属性和方法，都放在 `prototype对象`里面；
- 不需要共享的属性和方法，就放在构造函数里面。
- 实例对象一旦创建，将**自动引用**`prototype对象`的属性和方法。

JS 继承机制的设计思想，

- 由于所有的实例对象共享同一个` prototype 对象`，从外界看起来，`prototype对象`就好像是实例对象的原型，而实例对象则好像“继承”了`prototype`对象一样。

### JS 面向对象--封装

#### 1. 生成实例对象的原始模式

```js
var cat = {
    name:  '',
    color: ''
}

// ------------------------

var cat1 = {};
cat1.name = "Kitty";
cat1.color = "yellow";

var cat2 = {};
cat1.name = "Lucy";
cat1.color = "black";
```

缺点，

1. 多生成几个实例，代码重复，写起来很麻烦；
2. 实例与原型之间，没有任何办法，可以看出有什么联系。

#### 2. 原始模式的改进

解决了代码重复，

```js
function Cat(name, color) {
    return {
        name: name,
        color: color
    }
}

var cat1 = Cat("Kitty", "yellow");
var cat2 = Cat("Lucy", "black");
```

缺点，

- cat1 和 cat2 之间仍然没有内在联系，不能反映出它们是同一个原型对象的实例。

#### 3. 构造函数模式

为了解决从源性对象生成实例的问题，JS 提供了一个构造函数（Constructor）模式。

构造函数，

- 实际就是一个普通函数；
- 函数名大写以区分普通函数；
- 内部使用`this`变量；
- 对构造函数使用`new`运算符，生成实例，`this`变量会绑定在实例对象上。

```js
function Cat(name, color) {
    this.name = name;
    this.color = color;
}

var cat1 = new Cat("Kitty", "yellow");
var cat2 = new Cat("Lucy", "black");
```

至此，

- 代码重复解决了；
- 实例对象都自动含有一个`constructor属性`，指向它们的构造函数。
- JS 还提供了`instanceof`运算符，验证源性对象与实例之间的关系。

```js
console.log(cat1.constructor == Cat);	// true
console.log(cat2.constructor == Cat);	// true

console.log(cat1 instanceof Cat);	// true
console.log(cat2 instanceof Cat);	// true
```

缺点，

- 构造函数中的方法每次实例化对象，都必须为重复的内容，多占用一些内存。
- 不能共享。

#### 4. prototype 模式

JS 规定，

- 每一个构造函数都有一个`prototype`属性，指向另一个对象；
- 这个对象的所有属性和方法，都会被构造函数的实例继承。

```js
function Cat(name, color) {
    this.name = name;
    this.color = color;
}
Cat.prototype.eat = function() {
    alert("eating mouse...");
}

var cat1 = new Cat("Kitty", "yellow");
var cat2 = new Cat("Lucy", "black");

cat1.eat();	// eating mouse...
cat2.eat();	// eating mouse...
```

至此，

- 代码重复解决了；
- 实例对象之间的联系明确了；
- 属性和方法共享问题解决了。

#### 5. prototype 模式的验证方法

- `isPrototypeOf()`，用来判断，某个`prototype`对象和某个实例之间的关系;
- `hasOwnProperty()`，用来判断，某个属性到底是本地属性，还是继承自`prototype`对象的属性；
- `in`运算符，用来判断，某个实例是否含有某个属性，不管是不是本地属性；它还可以用来用来便利某个对象的所有属性。

```js
// isPrototypeOf
console.log(Cat.prototype.isPrototypeOf(cat1));	// true
console.log(Cat.prototype.isPrototypeOf(cat2));	// true
// hasOwnProperty
console.log(cat1.hasOwnProperty("name"));	// true
console.log(cat2.hasOwnProperty("type"));	// false
// in
console.log("name" in cat1);	// true
console.log("type" in cat1);	// false
// 遍历属性
for(var prop in cat1) {
    console.log("cat1["+prop+"]="+cat1[prop]);
}
```

### JS 面向对象--继承：构造函数的继承

对象之间的“继承”的 5 种方法，

```js
// 两个构造函数

// 动物对象的构造函数
function Animal() {
    this.species = "动物"；
}

// 猫对象的构造函数
function Cat(name, color) {
    this.name = name;
    this.color = color;
}
```

怎样才能使`猫继`承`动物`呢？？？

#### 1. 构造函数绑定

最简单的方法，使用`call`或`apply`方法，将父对象的构造函数绑定到子对象上，即在子对象构造函数中加一行：

```js
function Cat(name, color) {
    Animal.apply(this, arguments);	// 父对象的构造函数绑定到子对象上
    
    this.name = name;
    this.color = color;
}

var cat1 = new Cat("Kitty", "yellow");
alert(cat1.species);	// 动物
```

#### 2. prototype 模式

最常见的方法，使用`prototype`属性。

如果`猫`的 prototype 对象，指向一个 Animal 实例，那么所有猫的实例，就能继承 Animal 了。

```js
Cat.prototype = new Animal();		// 猫的 prototype 指向 Animal 的实例
Cat.prototype.constructor = Cat;	// 别忘了把构造函数的指向恢复成 Cat 哦

var cat1 = new Cat;
alert(cat1.species);	// 动物
```

编程时务必注意，

- 如果替换了 `prototype` 对象；
- 下一步必然是为了新的`prototype`对象加上`contructor`属性，并将这个属性只会原来的构造函数。

```js
o.prototype = {};	// o 的 prototype对象 被新的对象{} 替换了
o.prototype.constructor = o; // 必须将新的 prototype 对象加上 constructor 属性，指回原来的构造函数
```

#### 3. 直接继承 prototype

这是对第二种方法的改进，

- 由于 Animal 对象中，不变的属性都可以直接写入 `Animal.prototype`；
- 所以可以让`Cat()`跳过`Animal()`，直接继承`Animal.prototype`。

```js
// 改写 Animal 对象
function Animal() {};
Animal.prototype.species = "动物";

Cat.prototype = Animal.prototype;	// 将 Cat的prototype对象直接指向Animal的prototype对象，就完成了继承
Cat.prototype.constructor = Cat;	// 这一步，实际上把 Animal.prototype对象的constructor属性也改掉了

var cat1 = new Cat("Kitty", "yellow");
alert(cat1.species);	// 动物
```

与第二种比，

- 优点：效率比较高，不用执行和建立Animal实例了，省内存；
- 缺点：`Cat.prototype`和`Animal.prototype`现在指向了同一个对象，任何对`Cat.prototype`的修改，都会反映到`Animal.prototype`

#### 4. 利用空对象作为中介

怎么解决上面的缺点？

可以用空对象作为中介。

```js
var F = function() {};			// 空对象
F.prototype = Animal.prototype;	// 空对象的原型指向动物的原型
Cat.prototype = new F();		// 原型指向新的实例对象，空对象几乎不占内存哦
Cat.prototype.constructor = Cat;// 此时，修改Cat的prototype对象，就不会影响到Animal的prototype对象

alert(Animal.prototype.constructor);	// Animal
```

将上面的方法，封装成一个函数，便于使用。

```js
function extend(Child, Parent) {
    var F = function() {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
    
    Child.uber = Parent.prototype;	// 为子对象设一个uber属性，这个属性直接指父对象的prototype属性，等于在子对象可以直接调用父对象的方法。只是为了实现继承的完备性，纯属备用性质。
}

// 使用的时候，方法如下
extend(Cat, Animal);
var cat1 = new Cat("Kitty", "yellow");
alert(cat1.species);	// 动物
```

#### 5. 拷贝继承

上面是采用 prototype对象，实现继承。

换一种思路，纯粹采用”拷贝“方法实现继承，把父对象的所有属性和方法，拷贝进子对象，不也能够实现继承吗？

- 首先，还是把 Animal的所有不变属性，都放到它的 prototype对象上。

```js
function Animal() {};
Animal.prototype.species = "动物";
```

- 然后，再写一个函数，实现属性拷贝的目的。

```js
function extend2(Child, Parent) {
    var p = Parent.prototype;
    var c = Child.prototype;
    
    for(var i in p) {
        c[i] = p[i];	// 将父对象的属性一一拷贝给Child对象的prototype对象
    }
    
    c.uber = p;
    
}

// 使用的时候
extend2(Cat, Animal);
var cat1 = new Cat("Kitty", "yellow");
alert(cat1.species);	// 动物
```

### JS 面向对象--继承： 非构造函数的继承

#### 1. 什么是”非构造函数“的继承

```js
var Chinese = {
    nation: '中国'
};

var Doctor = {
    career: '医生'
}

// 怎么样才能让 Doctor 去继承 Chinese 呢 ??
// 两个都是普通对象，不是构造函数，无法使用构造函数方法实现”继承“
```

#### 2. object() 方法

json格式的发明人Douglas Crockford，提出了一个 `object()`函数，可以实现

```js
function object(o) {
    function F() {}
    F.prototype = o;	// object()函数，其实只做一件事，就是把子对象的prototype属性指向父对象，从而使得子对象与父对象连在一起
    return new F();
}


// 使用的时候：
// 第一步,先在父对象的基础上，生成子对象；
var Doctor = object(Chinese);
// 然后，再加上子对象本身的属性：
Doctor.career = '医生'；

// 这时，子对象已经继承了父对象的属性了
alert(Doctory.nation);	// 中国
```

#### 3. 浅拷贝

```js
// 浅拷贝

function extendCopy(p) {
    var c = {};
    
    for (var i in p) {
        c[i] = p[i];
    }
    
    c.uber = p;
    
    return c;
}

// 使用
var Doctor = extendCopy(Chinese);
Doctor.career = '医生';
alert(Doctor.nation);	// 中国
```

缺点，

- 如果父对象的属性等于数组或另一个对象，那么，实际上子对象获得的只是一个内存地址，而不是真正拷贝，因此存在父对象被篡改的可能。
- `extendCopy()`只是拷贝基本类型的数据，我们把这种拷贝叫做**浅拷贝**。早期jQuery实现继承的方式。

#### 4. 深拷贝

所谓 **深拷贝**，就是能够实现真正意义上的数组和对象的拷贝。只要递归调用 **浅拷贝**就行了。

```js
function deepCopy(p, c) {
    var c = c || {};
    for (var i in p) {
        if(typeof p[i] === 'object') {
            c[i] = (p[i].constructor === Array) ? [] : {};
            deepCopy(p[i], c[i]);
        }else {
            c[i] = p[i];
        }
    }
    return c;
}

// 使用
var Doctor = deepCopy(Chinese);	
```

因为是值的深度拷贝，所以父对象不会收到影响。jQurey库目前使用的继承方法。
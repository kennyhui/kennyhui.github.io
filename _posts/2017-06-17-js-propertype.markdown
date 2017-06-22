---
layout:     post
title:      "Javascript原型链"
subtitle:   "原型链"
date:        2017-06-17 13:00:00
author:     ""
header-img: "img/post-bg-2015.jpg"
tags:
   - javascript
---


## 前言

在写复杂的 JavaScript 应用之前，充分理解原型链继承的工作方式是每个 JavaScript 程序员必修的功课。

JavaScript中并没有类（class）；Js是基于原型（prototype-based）来实现的面向对象（OOP）的编程范式的，但并不是所有的对象都拥有prototype这一属性

```
var a = {},b = 'shi';
var fn = function(){};
a.prototype // undefined
b.prototype // undefined
fn.prototype  // Object {constructor: function}

```

prototype属性只有 function对象定义时才有，函数本身也是对象。想要明白原型问题我们先明白几个基本概念。

## 一、function、Function、Object和{}

function是JavaScript的关键字，用于定义函数变量，一般有2种定义方式

```
function f1(){  
   console.log('This is function f1!');
}
typeof(f1);  //=> 'function'
var f2 = function(){  
  console.log('This is function f2!');
}
typeof(f2);  //=> 'function'

```


Object，它用于生成对象类型，其简写形式为{}，其实和function和Function的关系类似。

```
var o1 = new Object();  
typeof(o1);      //=> 'object'
var o2 = {};  
typeof(o2);     //=> 'object'
typeof(Object); //=> 'function'

```
二、prototype 和 __proto__

prototype属性只有函数类型对象才有，上面说的很清楚了。而__proto__是所有JavaScript对象都内置的属性[[Prototype]]，而这个属性指向构造函数（类似父类）的prototype属性，从而继承属性. 通俗一点讲，prototype中定义的属性和方法都是留给自己的“后代”用的，因此，子类完全可以访问prototype中的属性和方法。举个例子：

```
var Person = function(){};  
Person.prototype.type = 'Person';  
Person.prototype.maxAge = 100;
var p = new Person();  
console.log(p.maxAge);  
p.name = 'rainy';
// 修正Person.prototype.constructor为Person本身
Person.prototype.constructor === Person;  //=> true  
p.__proto__ === Person.prototype;         //=> true  
console.log(p.prototype);                 //=> undefined


```

Person是一个函数类型的变量，因此自带了prototype属性，prototype属性中的constructor又指向Person本身；通过new关键字生成的Person类的实例p1，通过__proto__属性指向了Person的原型。


|注意：
遵循ECMAScript标准，someObject.[[Prototype]] 符号是用于指派 someObject 的原型。这个等同于 JavaScript 的 __proto__ 属性。从 ECMAScript 6 开始, [[Prototype]] 可以用Object.getPrototypeOf()和Object.setPrototypeOf()访问器来访问。

三、原型链原型链是基于 __proto__ 的。JavaScript 对象有一个指向一个原型对象的链。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依此层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。


举例说明：


```
// Node
var Obj = function(){};  
var o = new Obj();  
o.__proto__ === Obj.prototype;  //=> true  
o.__proto__.constructor === Obj; //=> true
Obj.__proto__ === Function.prototype; //=> true  
Obj.__proto__.constructor === Function; //=> true
Function.__proto__ === Function.prototype; //=> true  
Object.__proto__ === Object.prototype;     //=> false  
Object.__proto__ === Function.prototype;   //=> true
Function.__proto__.constructor === Function;//=> true  
Function.__proto__.__proto__;               //=> {}  
Function.__proto__.__proto__ === o.__proto__.__proto__; //=> true  
o.__proto__.__proto__.__proto__ === null;   //=> true

```


来张图更直观一些


到了这里，其实有个问题大家没注意，new关键词到底起了什么作用呢？

其实开头的图已经给出答案：new关键词的作用就是完成上图所示实例与父类原型之间关系的串接，并创建一个新的对象.简单来说，new就是把父类的prototype赋值给实例的__proto__


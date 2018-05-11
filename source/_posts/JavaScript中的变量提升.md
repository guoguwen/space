---
title: JavaScript中的变量提升
date: 2018-05-11 09:51:58
header-img: "/img/featured-image.png"
cdn: "header-off"
tags:
    - JavaScript
---

今天又重新复习了一遍ES6,就重新再温故而知新重新研究一下变量提升
## 名词解释（变量提升）

##### hoisting(变量提升)

函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部。
JavaScript 只有声明的变量会提升，初始化的不会。

``` bash
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
上面代码的原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。 
```
按照解释的变量的声明总会被解释器提升到方法体的最顶部，那么上段代码变为
``` bash
var tmp = new Date();

function f() {
  var tmp;
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
这样理解起来就不难了
```

再来一道
``` bash
function foo(a) {  
    var a;  
    return a;  
}  
function bar(a) {  
    var a = 'bye';  
    return a;  
}  
[foo('hello'), bar('hello')]//输出结果为：hello，bye 

该这样解析
var a;  
function foo(a) {  
    return a;  
}  
function bar(a) {  
    a = 'bye';  
    return a;  
}  
[foo('hello'), bar('hello')]//输出结果为：hello，bye   
```

```bash
经典面试题
function Foo() {
getName = function () { alert (1); };
return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}
//请写出以下输出结果：
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```


"use strict" 的目的是指定代码在严格条件下执行。

严格模式下你不能使用未声明的变量。

消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
消除代码运行的一些不安全之处，保证代码运行的安全；
提高编译器效率，增加运行速度；
为未来新版本的Javascript做好铺垫。

"严格模式"体现了Javascript更合理、更安全、更严谨的发展方向，包括IE 10在内的主流浏览器，都已经支持它，许多大项目已经开始全面拥抱它。

另一方面，同样的代码，在"严格模式"中，可能会有不一样的运行结果；一些在"正常模式"下可以运行的语句，在"严格模式"下将不能运行。

## 参考文档

#####  [参考文档](http://www.jb51.net/article/79461.htm)
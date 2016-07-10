title: "闭包是什么"
date: 2015-03-22 10:37:07
tags:
---

一直以来，对闭包的概念比较模糊。趁着天气晴朗，再梳理一遍。

-----

**1.作用域？**

我们总会在程序中定义一些函数和变量，用它构建我们的系统。但对解释器来说，它又是如何以及从哪里找到这些数据呢？我们以javascript为例来解释。
大家可能都知道：变量和执行上下文有关。如：
```JavaScript
var a = 10; // 全局上下文中的变量
(function () {
  var b = 20; // 函数上下文中的局部变量
  alert(b);   // 20
})();
alert(a); // 10
alert(b); // "b" is not defined
```
1.变量对象
既然变量和执行上下文有关，那它就该知道数据存储在哪里以及如何获取。这种机制就称作变量对象：
>A variable object (in abbreviated form — VO) is a special object related with an execution context and which stores:
variables (var, VariableDeclaration);
function declarations (FunctionDeclaration, in abbreviated form FD);
and function formal parameters
declared in the context.
简单理解：它与执行上下文有关，存储很多内容，包括变量、函数等。
先放一放，我们接着向下看。

2.无块级作用域
ECMAScript标准中指出独立的作用域只有通过"函数代码"，看例子：
```JavaScript
for(var k in {a: 1, b: 2}) {
	alert(k);
}
alert(k); // 尽管循环已经结束，但是变量“k”仍然在作用域中
```
3.作用域链
就是变量对象构成的一个链表，从里向外查找的一个过程。如上面例子有2层：函数级变量对象 -> 全局变量对象。
全局对象是一个在进入任何执行上下文前就创建出来的对象，生命随着程序结束而终止。
至于js中`var obj = new Object;`，我自己的理解是：若从变量查找角度看，与普通变量比，只是相对复杂一点的数据结构，它们是等同。

4.再补充：变量对象
处理上下文代码，会有2个阶段，如下：
```JavaScript
function test(a, b) {
	alert(c);
	var c = 10;
	function d() {}
	var e = function _e() {};
}
test(10);
```
a、进入执行上下文
  此时变量对象数据是：{a:10,b:undefined,c:undefined,d:reference to FunctionDeclaration,e:undefined}
b、执行代码
  第一步：弹出 undefined，而不是 is not defined
  第二部：变量对象数据被更改 {c:10} ......

5.基于上面的铺垫，js作用域的定义：
  - 没有块级作用域
  - 函数中声明的变量，在整个函数中都有定义【原因参见4】
  - 作用域链
  - 局部变量比全局变量优先【作用域链查找顺序决定】
  - 未用var声明属于全局变量【准确说是全局属性】

**2.闭包给你的感受？**

说到感受，先上一个例子：
```JavaScript
function f1(n){
	return function f2(){
  		alert(n);
	}
}
var t1 = f1(100);
t1();  // 100
var t2 = f1(200);
t2();  // 200
```
按着第一部分我们对js变量作用域的理解，t()执行应该访问不到n，才是正确的呀？这就是闭包。

函数是一些可执行的代码，这些代码在函数被定义后就确定了，不会在执行时发生变化，所以一个函数只有一个实例。闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例。所谓引用环境是指在程序执行中的某个点所有处于活跃状态的约束所组成的集合。其中的约束是指一个变量的名字和其所代表的对象之间的联系。

为什么要把引用环境与函数组合起来呢？这主要是因为在支持嵌套作用域的语言中，有时不能简单直接地确定函数的引用环境。这样的语言一般具有这样的特性：
- 函数是一阶值（First-class value），即函数可以作为另一个函数的返回值或参数，还可以作为一个变量的值。
- 函数可以嵌套定义，即在一个函数内部可以定义另一个函数。

在这样的语言中，如果按照作用域规则在执行时确定一个函数的引用环境，那么这个引用环境可能和函数定义时不同。

要想使这两段程序正常执行，一个简单的办法是在函数定义时捕获当时的引用环境，并与函数代码组合成一个整体。当把这个整体当作函数调用时，先把其中的引用环境覆盖到当前的引用环境上，然后执行具体代码，并在调用结束后恢复原来的引用环境。

这种由引用环境与函数代码组成的实体就是闭包。

**3.其它语言的表现形式？**

顺道看看，不同语言在闭包上的表现形式

- python
```python
def addx(x):
	def adder (y): return x + y
 	return adder
add8 = addx(8)
print add8(100)
```
- ruby
Ruby 使用 Block 来定义闭包，Block 在 Ruby 中十分重要，几乎到处都可以看到它的身影，下面的代码就展示了一个 Block：
```ruby
sum = 0
10.times{|n| sum += n}
print sum
```
- scheme
```Scheme
(define (addx x)
	(lambda (y) (+ y x)))
(define add8 (addx 8))
(add8 100)
```
- java
Java对lambda支持，以及和闭包关系，先挖个坑
http://rensanning.iteye.com/blog/2039106

**4.留个思考题：闭包的作用？**

- - -

参考：
- [javascript 执行环境，变量对象，作用域链](http://segmentfault.com/blog/f2e/1190000000533094)
- [变量对象【推荐】](https://github.com/goddyZhao/Translation/blob/master/JavaScript/%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1%EF%BC%88Variable%20object%EF%BC%89.md)
- [深入理解js变量作用域](http://www.cnblogs.com/rainman/archive/2009/04/28/1445687.html#m2)
- [闭包的概念](http://www.ibm.com/developerworks/cn/linux/l-cn-closure/)
- [理解Javascript的闭包](http://coolshell.cn/articles/6731.html)

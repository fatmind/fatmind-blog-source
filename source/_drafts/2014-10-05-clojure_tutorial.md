title: clojure tutorial
date: 2014-10-06 01:19:38
tags:
---

```
推荐文章：
1.徐明明clojure入门文章 http://xumingming.sinaapp.com/302/clojure-functional-programming-for-the-jvm-clojure-tutorial/
2.开发工具sublimeText http://dev.clojure.org/display/doc/Getting+Started+With+Sublime+Text+2
3.leiningen安装
http://www.cnblogs.com/darkluck99/archive/2012/02/20/2360216.html
```

随记笔记：

1.函数式编程中，函数是一等公民，体现在 ？
-函数无副作用
-可赋值给变量
-可作为参数或返回值

2.什么是高阶函数 ？
-此函数可以接受一个函数作为参数
-作用是 ？

3.函数语言优势在 ？
-在并发情况下，数据不可变，因此不需要锁

4.clojure特性 ？
- 基于JVM，跨平台，且有java丰富类库支持
- 每个操作被实现成以下三种形式的一种: 函数(function), 宏(macro)或者special form.
- 序列操组
  -什么可以认为是序列 ？
- 如何安全共享可修改数据
  -xxx
- 延迟计算
  -xxx
- 代码处理过程
  -读入期，编译期以及运行期。在读入期，读入期会读取clojure源代码并且把代码转变成数据结构，基本上来说就是一个包含列表的列表的列表。在编译期，这些数据结构被转化成java的bytecode。在运行期这些java bytecode被执行。函数只有在运行期才会执行。而宏在编译期就被展开成实际对应的代码了。
- binding
 -Clojure里面是不支持变量的，它跟变量有点像。def全局；let局部，会临时覆盖全局，但不影响其值，当退出当前binding空间后继续使用上一层定义。
 -binding范围有：全局、线程内、函数内
- 集合
  -不可变。创建后，不能删除也不能修改。
  -异源。运行不同类型数据。
- 宏
  -xxx
- 函数
  -defn 暂时还未看到特殊概念
- 与Java互操作
  -xxx
  -异常捕获
- 语法
  -圆括号，前置表达式
  -注释;，字符串"，关键字:name，正则#""，逗号认为是空白（提供代码可读性）
  -linked list、vector（类似数组）、set、map
  -高级：引用、创建对象、匿名函数、宏
  -Lisp代码比其它语言的代码有更多的小括号的一个原因是Lisp里面不使用其它语言使用的大括号，比如在java里面，方法代码是被包含在大括号里面的，而在lisp代码里面是包含在小括号里面的
- 命名规范
  -全部小写单词，多个单词之间中横线连接

5.一些观点
- Lisp方言有一个非常简洁的语法 — 有些人觉得很美的语法。数据和代码的表达形式是一样的，一个列表的列表很自然地在内存里面表达成一个tree。(a b c)表示一个对函数a的调用，而参数是b和c。通常情况下就是这样了，除了一些特殊情况 — 到底有多少特殊情况取决于你所使用的方言。
- 我们把这些特殊情况称为语法糖。语法糖越多代码写起来越简介，但是同时我们也要学习更多的东西以读懂这些代码。这需要找到一个平衡点。很多语法糖都有对应的函数可以调用。到底语法糖是多了还是少了还是你们自己来判断吧。



---

1.变量定义
- Clojure里面是不支持变量的，而是叫binding；本质也是value与symbol建立关系，并且有作用域
- 关键字区分：
	- def，当前ns。
	- let，创建局限于一个当前form的bindings。注意：如果这些表达式里面有调用别的函数，那么这个函数是无法利用let创建的这个binding的
	- binding，创建的本地binding会暂时地覆盖已经存在的全局binding. 这个binding可以在创建这个binding的form以及这个form里面调用的函数里面都能看到
```
(function-name arg1 arg2 arg3)
左括号被移到了最前面；逗号和分号不需要了. 我们称这种语法叫： “form”. 体会代码：
(def v 0)
(defn f [] (println v))
(let [v 10] (f))           ; 0 没有影响f函数，只会影响(let..)内显式出现的v值
(binding [v 100] (f))      ; 100 影响f函数，是ThreadLocal级别的
```

2.与Java互操作
- import导入对Java引用，(import '(java.util))
- 调用方式：
	- 静态方法：(. Math pow 2 4)
	- 对象：(doto (new java.util.HashMap) (. put "a" 1) )
- 不是非常理解clojure对java/new对象，如何去交互？
  -仅调用其方法，模拟类似doto去解决
  -若对象有field，会存储数据；可以def建立binding吗？如何继承java接口 ？
 http://techbehindtech.com/2010/10/11/calling-java-from-clojure/
  -函数迭代调用 ->

3.函数
- Clojure中函数也是list列表：第1个元素是函数名，后面是函数参数的列表。体现：代码即数据。
- 定义函数：(defn f “注释” [n] (* n n n))，一些特征点：
  -(string? "hello")，谓词函数
- 子函数/匿名函数
  ```
  (defn x2y2 [x y]
  	(defn f [x] (* x x))
  	(+ (f x) (f y))
  )
  (x2y2 3 4) ; 25 子函数，和普通函数没有区别，可能只是作用域范围的差异
  (some #(= % "Moe") stooges) ; -> true 匿名函数
  ```
- 可变参数：
  ```
  (defn f [a b & c] (list a b c))
  (= (f 1 2 3 4)  '(1 2 (3 4)))
  ```
- 函数重载：依赖参数个数，复杂使用defmulti
- 函数作为参数/返回值
```
(defn exp [a f1 b f2 c] (f2 (f1 a b) c))
(exp 5 - 2 + 3) ; 6
```

4.宏
- 宏是用来给语言添加新的结构、新的元素。它们是在读入期（而不是编译期）就会实际代码替换的一个机制
http://www.cnblogs.com/fxjwind/archive/2013/02/27/2934851.html
- 对于函数来说，它们的所有的参数都会被evaluate的, 而宏则会自动判断哪些参数需要evaluate。 这对于实现像(if condition then-expr else-expr)这样的结构是非常重要的
- 元编程：强烈推荐 ‘ruby元编程’ 这本书
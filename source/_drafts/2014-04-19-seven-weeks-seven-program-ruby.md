1.信息比较

|  		 | 说明 |
|--------|--------|
| 编译型  |相对可执行代码生成时机。在执行前已编译为可执行代码|
| 解释型  |在运行时生成可执行代码|
| 静态语言  |在编译时确定变量的数据类型，大多数语言要求必须声明数据类型，少部分编译器可自己推导|
| 动态语言  |在运行时确定变量的数据类型，通常即值的类型|
>强/弱类型，之间的区分不是那么清晰，可笼统的认为是 ‘describe the manner in which restrictions on how operations involving values of different data types can be intermixed’ 的严格程度，比如：int a，a=1.5，在c和java；'5'+5，在java和ruby、python；在不同的语言中表现是不同的  
详情请参见伟大的wiki，http://en.wikipedia.org/wiki/Strong_typing，In general, these terms do not have a precise definition.

2.ruby中一切是对象  

3.鸭子类型，如: arr = ['10', 100], arr[i].to_i  

4.字符串和数组等  
a、在python中，字符串也是序列，支持 ‘切片、索引’ 操作，同理也支持。另外有很多语法糖。  
b、其它数据类型：  
\- 数组：[]是Array类的方法；语法糖：arr[-1]，arr[1..2]  
\- 散列表：hashmap结构
>引入‘symbol’概念，类似Java常量池，相同object_id；普通 “string”.obejct_id 执行两次，结果是一样的吗 ？

5.函数  
a、每个函数都有返回值  
b、且函数也是个对象

6.代码块  
如：animals = ['dog', 'cat']; animals.each {|a| puts a}
```ruby
class Fixnum
	def my_times
		i = self
		while i > 0
			i = i - 1
			yield
		end
	end	
end
4.my_times {puts "hello"}
```
a、代码块，类似匿名函数或lambda概念，f = lambda x : x * 2  
b、yield  
c、见上面代码，给已有类新增方法，即 ‘元编程’  
d、代码块可作一等参数（first-class parameter）



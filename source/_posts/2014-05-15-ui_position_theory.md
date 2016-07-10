title: 页面布局原理
date: 2014-05-15 10:30:06
tags:
---

**一、css定位**  

1.在研究定位之前，必须理解 ‘文档流、盒模型’  
- 文档流  
整个html是从上到下、从左向右依次排列，称为文档流。包含两个类：块框（如：div、p），行内框（如：span、strong等）。块框从上到下，行内框从左向右。  
- 盒模型  
在html总一切都是‘框’，遵循盒模型。  
![](/img/html_boxmodel.gif)  
提醒：在 CSS 中，width 和 height 指的是内容区域的宽度和高度。增加内边距、边框和外边距不会影响内容区域的尺寸，但是会增加元素框的总尺寸。  
- 父子关系  
尽管在文档结构中是父子关系，但其width/height并无限制关系，比如：parent.width=500px，child.width=1000px。  
父子关系更多体现在‘相对（参考）’，如：parent.width=500px，child.width=100%即500px；position: absolute 相对于最近的已定位的祖先元素。

2.CSS定位梳理  
撇开原始的文档流不谈，css定义主要有三种：position-relative，position-absolute，float。  
- position: relative  
a. 相对自己原有的位置偏移  
b. 仍占据原始位置  
c. 新位置会覆盖其它元素，可设置z-index
- position：absolute  
a绝对定位的元素的位置相对于最近的已定位祖先元素，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。==已定位祖先元素含义：明确通过position定位；最初包含块：html根==  
a. 从文档流中删除，释放原始位置  
b. 新位置会覆盖页面上其它元素，可设置z-index  
c. example:  
属于祖先的子元素，当祖先设置（position: relative;left: 50px;），子元素都会整体向左移，基于父元素的去计算，如：子元素 left:20px，实际到边缘距离是70px  
- float  
a. 浮动框不在文档流中，可以向左或右移动，直到它的外边缘碰到包含框或另一个浮动框的边框。包含框即父元素，因此是相对于自己的父元素位置浮动  
b. 不在文档流中，自然释放原始位置  
c. 若包含框太窄，无法容纳水平排列的三个元素，后面的块向下移动，直到有足够空间；若块高度不同，向下移动时可能会被卡住。  
d. 文字环绕  
不管是否包含在block元素中，文字都会环绕（原理未知）  
e. clean概念  
表示框的哪些边不应该挨着浮动框。只需要处理紧挨float的元素，当它占住自己在文档流中的位置，则后续元素不受到float的影响  

3.margin负数效果  
```html
<div id="id1" style="float: left;"></div>  
<div id="id2" style="float: left; margin-left: -190px; width: 100%"></div>
```
id1、id2都是向左浮动，且id2宽带和parent一样宽，当浮动到左侧时，因为已经有id1，所以按float规则被挤到id1下面。但可通过设置margin做位置移动，实现并排。可此时右边没有元素，所以设置margin-left:负数，实现margin-right效果.

---

**二、android ui 布局**

1.从属性上看  
同样有width、height、margin等  

2.LinearLayout、RelativeLayout对比  
待补充

3.weight概念  
Either expand children with weight to take up available space or shrink them if they extend beyond our current bounds。代码参见：LinearLayout 804行。逻辑：  
- 若所有child均未设置weight，则从头到尾（We have no limit, so make all weighted views as tall as the largest child）在其定义允许内最大空间，如：fill_parent  
- 若定义weight  
a. 其计算是基于**‘剩余空间’**，正负均可能
int delta = heightSize – mTotalLength，heightSize=LineLayout高度，mTotalLength=其所有child高度和  
b. 根据每个child的weight值，计算其
```java
int share = (int) (childExtra * delta / weightSum)
weightSum -= childExtra;
delta -= share;
int childHeight = child.getMeasuredHeight() + share;
```
- 出个题目巩固下  
```xml
<LinearLayout>
    <Button
        android:layout_width="150dp"
        android:layout_height="wrap_content"
        android:text="bindService"
        android:id="@+id/bind_service" />

    <Button
        android:layout_width="150dp"
        android:layout_height="fill_parent"
        android:layout_weight="1"
        android:text="unbindService"
        android:id="@+id/unbind_service" />

    <Button
        android:layout_width="150dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="printBroadcast"
        android:id="@+id/print_broadcast" />

</LinearLayout>
```
---

**三、参考资料**  
1.http://learn.hicc.me/html-css/box-model-positioning.html  
2.http://www.w3school.com.cn/css/css_positioning.asp  


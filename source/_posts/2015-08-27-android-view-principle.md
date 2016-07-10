title: android之view
date: 2015-08-27 23:35:18
tags:
---

*写android有一段时间了，慢慢梳理下相关的一些知识点，形成体系*

**1.View与ViewGroup**
https://www.zybuluo.com/stepbystep/note/65426

1.1 页面结构
一个页面展示的信息，是由不同的组件，按照某种布局组合在一起。不管在web、桌面、app中，都是一样的。常用的组件会被抽象出来，如：TextView、img等。
在android同样，页面结构：
![](http://pic002.cnblogs.com/images/2010/152755/2010121322475720.png)

1.2 类体系
![](http://7u2m19.com1.z0.glb.clouddn.com/4-view&viewgroup.jpg)

1.3 属性
抽象的组件，必须能有可定制的属性，如：width、gravity、padding等。剩下的就是各种布局之间的差异，多实践而已。https://www.zybuluo.com/stepbystep/note/65426。

**2.View如何渲染？**

2.1 要不要灵活适配？
现实的世界，有各种屏幕尺寸，很难基于原点去声明自己的位置和高宽。从使用者角度，会极大降低复杂度。付出代价就是计算。

2.2 如何计算？
计算：高宽（measure） -> 相对parent距离（layout） -> 画（draw）
推荐此篇文章，多看几遍：http://blog.csdn.net/luoshengyang/article/details/8372924

**3.如何自定义？**
已经写的很清楚，推荐大家 http://blog.csdn.net/guolin_blog/article/details/17357967

**4.ScrollView与ListView方案之一**

```java
public class ListViewForScrollView extends ListView {
	@Override
	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2,
				MeasureSpec.AT_MOST);
		super.onMeasure(widthMeasureSpec, expandSpec);
	}
}
```
参考：
1.android单位 http://www.imyukin.com/?p=277




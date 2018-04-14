title: 如何让Markdown图片居中
date: 2015-07-15 15:13:57
tags: Mac 
------


<!--more-->

一般在MarkDown中通过

	 '![](url)' 
	 
添加图片看到的都是这样的：

![](http://7xkfbb.com1.z0.glb.clouddn.com/15-7-16/95341796.jpg)

但是如果我想让图片居中该怎么做呢？查了点资料之后发现可以这样：

```html
<div style="text-align: center">
<img src="url"/>
</div>
```


把这几行代码直接加到markdown里面就可以了，url换成图片的地址，就像这样：


<div style="text-align: center">
	<img src="http://7xkfbb.com1.z0.glb.clouddn.com/15-7-16/95341796.jpg"/>
</div>
	
相当于手动嵌入了HTML代码
	
**或者**  

```html
<center>
![](http://7xkfbb.com1.z0.glb.clouddn.com/15-7-16/95341796.jpg)
</center>
```


<center>

![](http://7xkfbb.com1.z0.glb.clouddn.com/15-7-16/95341796.jpg)

</center>



> 此图片只是为了纪念今天风云微博的优衣库事件～～
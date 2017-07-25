#lazyload的使用
######lazyload依赖jquery，因此要先引入jquery再引入jquery-lazyload.js
img标签的src指向一个图片占位符，真实的地址(必须)放在data-original属性上；给懒加载图像一个特定的class(如lazy)，以方便进行图像插件捆绑；
######代码：
	html代码
	````
		<img class="lazy" src="images/placeholder.gif" data-original="img/example.jpg" alt="" width="640" height="480">
	````
	JS代码
	````
		$(function(){
			$("img.lazy").lazyload();
		});
	````   
这将使所有的class为lazy的img图片被延迟加载。
#####注意： 这里必须设置图片的width和height，否则插件可能无法正常工作。  
lazyload()方法接收一个对象，可以通过设置对象的属性来对懒加载做相应的控制。
##用图片提前占位 - placeholder
图片的占位符除了可以在img标签中通过src属性来指定外，也可以在lazyload中通过指定placeholder值为某一图片路径，此图片用来占据将要加载的图片的位置，待图片加载时占位图则会隐藏
######代码
	$("img.lazy").lazyload({
		placeholder: "img/grey.gif"
	});
##设置临界点 - threshold
默认情况下图片会在出现在屏幕时加载，如果想要提前加载图片，可以设置threshold
######代码 - 在距离屏幕200像素时提前加载图片
	$("img.lazy").lazyload({
		threshold: 200
	});
##设置事件来触发加载 - event
通过设置event属性，可以使用jQuery事件（如click和mouseover)，也可以使用自定义事件，如sporty、foobar默认情况下要等到用户向下滚动并且图像出现在视口中时触发。  
######代码 - 只有当用户点击它们时才加载图片: 
	$("img.lazy").lazyload({
		event: "click"
	});
##使用特效 - effect
默认情况下插件等待图像完全加载并调用show()。可以通过设置effect属性来选择想要的效果。
###### 代码 - 使用fadeIn(淡入效果)
	$("img.lazy").lazyload({
		effect: "fadeIn"
	});
##图片在容器里面 - container
可以将插件作用在可滚动的容器上，将容器定义为jQuery对象并作为参数传递到lazyload()方法中的container属性中；
######代码
	$("img.lazy").lazyload({
		container: $("container")
	});
##设置查找图片张数 - failure_limit
lazyload的实现原理是，在页面滚动时，会搜索未加载的图片，如果图片在可视范围内就加载，默认情况下当找到第一张不在可见区域的图片时就会停止搜索。
######代码
	$("img.lazy").lazyload({
		failure_limit: 10
	});
将failure_limit设置为10，令插件在找到10个不在可视区域的图片时才停止搜索。
##加载隐藏的图片 - skip_invisible
为了提升性能，lazyload默认忽略了隐藏图片，如果想要加载隐藏图片，需要将skip_invisible设置为false
######代码
	$("img.lazy").lazyload({
		skip_invisible: false
	});




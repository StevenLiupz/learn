###变量声明
------
######Sass中的变量以$开头：
	$变量名: 变量值;
####普通变量与默认变量
普通变量定义之后可以在全局范围内使用；  
默认变量仅需要在值后面加上!default即可；  
默认变量一般用来设置默认值，然后根据需求来覆盖，覆盖的方式：只需要在默认变量`之前`重新声明变量即可。  
默认变量的价值在进行`组件化开发`的时候会非常有用。
####全局变量与局部变量
在`选择器、函数、混合宏...`的外面定义的变量为全局变量，即定义在元素外部的变量。  
定义在元素内部的变量就是局部变量，局部变量只会在局部范围内覆盖全局变量。

###伪类嵌套
伪类嵌套需要借助`&`符号一起配合使用。如：	


	.clearfix {
		&::before,&::after {
			content: "";
			display: table;
 		}
		&::after {
			clear: both;
			overflow: hidden;
		}
	}
###混合宏
####声明混合宏
#####1. 不带参数的混合宏
在Sass中，使用`@mixin`来声明一个混合宏。

	@mixin border-radius {
		border-radius: 5px;
	}
其中`@mixin`是用来声明混合宏的关键词，`border-radius`是混合宏的名称。
#####2. 带参数的混合宏
Sass中可以在混合宏名称后面通过`()`来传入参数.

	@mixin border-radius($radius: 5px) {
		border-radius: $radius;
	}
#####3. 复杂的混合宏
Sass中的混合宏还提供更为复杂的，你可以在大括号里面写上带有`逻辑关系`
	
	@mixin box-shadow($shadow...) {
	  @if length($shadow) >= 1 {
	    @include prefixer(box-shadow, $shadow);
	  } @else{
	    $shadow:0 0 4px rgba(0,0,0,.3);
	    @include prefixer(box-shadow, $shadow);
	  }
	}
这个 box-shadow 的混合宏，带有多个参数，这个时候可以使用“ … ”来替代。简单的解释一下，当 $shadow 的参数数量值大于或等于“ 1 ”时，表示有多个阴影值，反之调用默认的参数值“ 0 0 4px rgba(0,0,0,.3) ”。
####调用混合宏
混合宏通过`@include`来调用声明号的混合宏。
####混合宏的参数
#####1. 传一个不带值的参数
在混合宏中可以传一个不带任何值的参数，如：

	@mixin border-radius($radius){
	  -webkit-border-radius: $radius;
	  border-radius: $radius;
	}
在混合宏`border-radius`中定义了一个不带任何值的参数`$radius`，在调用的时候可以给这个混合宏传一个参数值。

	.box {
	  @include border-radius(3px);
	}
#####2. 传一个带值的参数
在混合宏中可以给混合宏的参数传一个默认值。

	@mixin border-radius($radius:3px){
	  -webkit-border-radius: $radius;
	  border-radius: $radius;
	}
假设页面中很多地方的圆角都是3px，那么调用默认的混合宏即可，但是页面中有些元素的圆角值不一样，那么可以随机给混合宏传值，如：

	.box {
	  @include border-radius(50%);
	}
#####3. 传多个参数
有一个特别的参数`...`。当混合宏的`参数过多`之时，可以使用该参数来代替。
#### 混合宏的不足之处
混合宏在实际编码中对于`可复用重复代码块`有很方便之处，但最大的不足之处是会`生成冗余的代码块`，在调用相同的混合宏时并不能智能的将相同的样式代码块合并在一起。
###继承
在Sass中通过关键词`@extend`来继承已存在的类样式块，从而实现代码的继承。
 
	//SCSS
	.btn {
	  border: 1px solid #ccc;
	  padding: 6px 10px;
	  font-size: 14px;
	}
	
	.btn-primary {
	  background-color: #f36;
	  color: #fff;
	  @extend .btn;
	}
	
	.btn-second {
	  background-color: orange;
	  color: #fff;
	  @extend .btn;
	}
编译出来之后：

	//CSS
	.btn, .btn-primary, .btn-second {
	  border: 1px solid #ccc;
	  padding: 6px 10px;
	  font-size: 14px;
	}
	
	.btn-primary {
	  background-color: #f36;
	  color: #fff;
	}
	
	.btn-second {
	  background-clor: orange;
	  color: #fff;
	}
Sass中的继承，可以继承样式块中所有样式代码，而且`编译出来的css会将选择器合并在一起`，形成组合选择器。
###占位符 %placeholder
Sass中的占位符可以取代以前css中的基类造成的代码冗余的情形。因为%placeholder声明的代码，如果不被@extend调用的话，不会产生任何代码。  
而通过 @extend 调用的占位符，编译出来的代码会将相同的代码合并在一起。

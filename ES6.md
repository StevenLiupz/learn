##1. 概述
模块加载方案： CommonJS和AMD。前者用于服务器，后者用于浏览器，二者都是只能在运行时确定模块的依赖关系，是"运行时加载"，而ES6模块是编译时加载。
###严格模式
-----
ES6的模块自动采用严格模式，不管有没有在模块头部加上"use strict"，严格模式主要有以下限制：  

	1. 变量必须声明后再使用；	
	2. 函数的参数不能有同名属性，否则报错；
	3. 不能使用with语句；
	4. 不能对只读属性赋值，否则报错；
	5. 不能删除变量(delete prop)，只能删除属性(delete global[prop])；
	6. 不能删除不可删除的属性，否则报错；
	7. 不能使用arguments.callee和arguments.caller;
	8. 禁止this指向全局对象(ES6中顶层的this指向的是undefined，因此不要在顶层使用this)；
###export和import
-----
export命令用于规定模块的对外接口，import用于导入其他模块提供的功能。  
#####export和export default
如果将export语句放在块级作用域中，就会报错。  
使用export时对应的import语句要使用{}，使用export default时，对应的import语句的不需要使用{}

	//使用export，对应的import要使用{}
	export function fn(){}
	import {fn} from '../myModule.js'
	//使用export default，对应的import不需要{}
	export default function fn(){}
	import myFn from '../myModule.js'
#####import
import命令具有提升效果，会提升到整个模块的头部，首先执行。  
由于import是静态执行，所以不能使用表达式和变量这些只有在运行时才能得到结果的语法结构，也不能放在块级作用域中。  
	
	import {'f' + 'oo'} from 'my-module'; // 报错
	let module = 'my-module'
	import {foo} from module; // 报错
#####import()
用import加载模块，是在编译时加载的，只能在模块的顶层使用，无法再运行时加载模块。因此使用import(spec)方法在运行时加载模块，参数spec指定所要加载模块的位置，最终import()返回一个Promise对象。
######import()的使用场景
	1. 按需加载 - 在需要的时候再加载某个模块
	2. 条件加载 - 根据不同的情况加载不同的模块
	3. 动态的模块路径 - import()方法运行模块的路径动态生成
######注意点：
import()方法加载模块成功以后，这个模块会作为一个对象，当做then方法的参数

	import('../myModule.js').then(({fn, myFn})=>{...})
#####跨模块常量
const声明的常量只在当前代码块有效，如果想设置跨模块的常量，可以通过export实现
	export const A = 1;
##let和const命令
###let变量
	1. let声明的变量，只在let所在的代码块内有效，只存在其所在的块级作用域中，一个{}就是一个块级作用域；
	2. 不存在变量提升；
	3. 暂时性死区-先声明后使用，否则会报错；(在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”)
	4. 不允许重复声明，let不允许在相同作用域内重复声明同一个变量；  
###const常量
const声明一个只读的常量，一旦声明，常量的值就不能改变。这里的不能改变并不是值不能改变，而是const常量指向的那个内存地址不能改变。

##变量的解构赋值
###1. 数组的解构赋值
	let [a,b,c] = [1,2,3];
本质上这种写法属于"模式匹配"，只要=号两边的模式相同，左边的变量就会被赋予对应的值。
####默认值
解构赋值允许指定默认值。

	let [x, y = 'b'] = ['a', null]; // x='a', y=null
	let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
######注意：  
ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。如上，如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。

###2. 对象的解构赋值
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。  

	let { bar, foo } = { foo: "aaa", bar: "bbb" };
	foo // "aaa"
	bar // "bbb"
	
	let { baz } = { foo: "aaa", bar: "bbb" };
	baz // undefined
如果变量名与属性名不一致，必须写成下面这样:

	let { foo: baz } = { foo: "aaa", bar: "bbb" };
	baz // "aaa"
	foo // error: foo is not defined
上面代码中，foo是匹配的模式，baz才是变量。真正被赋值的是变量baz，而不是模式foo。
####默认值
	var {x = 3} = {x: undefined};
	x // 3
	
	var {x = 3} = {x: null};
	x // null
对象的解构也可以指定默认值，默认值生效的条件是，对象的属性值严格等于`undefined`。
### 3. 解构赋值的用途
####（1）交换变量的值
	let x = 1;
	let y = 2;
	
	[x, y] = [y, x];
上面代码交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。
####（2）从函数返回多个值
	// 返回一个数组
	function example() {
	  return [1, 2, 3];
	}
	let [a, b, c] = example();
	
	// 返回一个对象
	function example() {
	  return {
	    foo: 1,
	    bar: 2
	  };
	}
	let { foo, bar } = example();
函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。
####（3）函数参数的定义
	// 参数是一组有次序的值
	function f([x, y, z]) { ... }
	f([1, 2, 3]);
	
	// 参数是一组无次序的值
	function f({x, y, z}) { ... }
	f({z: 3, y: 2, x: 1});
####（4）提取JSON数据
	let jsonData = {
	  id: 42,
	  status: "OK",
	  data: [867, 5309]
	};
	
	let { id, status, data: number } = jsonData;
	
	console.log(id, status, number);
	// 42, "OK", [867, 5309]
####（5）函数参数的默认值
	jQuery.ajax = function (url, {
	  async = true,
	  beforeSend = function () {},
	  cache = true,
	  complete = function () {},
	  crossDomain = false,
	  global = true,

	  // ... more config
	}) {
	  // ... do stuff
	};
指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。
####（6）输入模块的指定方法
	const { SourceMapConsumer, SourceNode } = require("source-map");
加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。
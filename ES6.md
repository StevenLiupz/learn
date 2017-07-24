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
	1. let声明的变量，只在let所在的代码块内有效；
	2. 不存在变量提升；
	3. 暂时性死区-先声明后使用，否则会报错；
	4. 不允许重复声明==重复使用使用；  
###const常量
const声明一个只读的常量，一旦声明，常量的值就不能改变。这里的不能改变并不是值不能改变，而是const常量指向的那个内存地址不能改变。

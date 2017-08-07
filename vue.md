## 计算属性 - computed
计算属性是用来声明式的描述一个值依赖了其他的值。当你在模板里把数据绑定到一个计算属性上时，Vue会在其依赖的任何值导致该计算属性改变时更新DOM。  
computed相当于method，返回function内return的值渲染到DOM上。  
######相比于method，即使多个{{}}里面使用了computed计算属性，conputed内的function也只执行一次，仅当function内涉及到Vue实例绑定的data的值的改变，function才会重新执行。而如果多个{{}}里面调用了method里面的方法，就会执行多次。
### 观察 watch
当想要在数据变化响应时，执行异步操作或开销较大的操作，可以使用watch进行监听。

## 条件渲染 v-if
### `v-if` vs `v-show`
	1. v-if 是动态的向DOM树你添加或删除DOM元素，v-show 是通过设置DOM元素的display样式属性控制显隐；
	2. v-if 是惰性的，如果在初始渲染时条件为假，则什么也不做，直到条件第一变为真时才会开始渲染条件块；
	3. v-show 则不管初始条件是什么，元素总是被渲染，并且只是简单的基于CSS进行切换；
	4. v-if 有更高的切换消耗，v-show 有更高的初始渲染开销；因此如果需要进行频繁地切换则使用v-show较好，如果在运行时条件不大可能改变，则使用v-if较好；
#####注意： 如果v-show作用的元素，css文件中display:none，通过v-show进行设置不能显示该元素
原因： v-show控制显隐，是通过js代码去修改元素的element style，如果value为false，设置display: none;如果value为true，设置display: ''；于是value为true时，只能将element style中的display效果清除，并不能覆盖css中的display效果
### `v-if` & `key` 管理可复用的元素
Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。因此，当我们需要让一个元素独立开来不影响其他元素，可以使用key来进行管理。如：

	<template v-if="loginType === 'username'">
	  <label>Username</label>
	  <input placeholder="Enter your username" key="username-input">
	</template>
	<template v-else>
	  <label>Email</label>
	  <input placeholder="Enter your email address" key="email-input">
	</template>

## 列表渲染 v-for
### key
v-for渲染列表时，给每一项提供一个唯一的key属性，以便跟踪每个节点的身份。注意与v-if中key的区别，这里是属性，要用v-bind绑定。

	<div v-for="item in items" :key="item.id">
	  <!-- 内容 -->
	</div>

## 事件修饰符
vue提供的事件修饰符`.stop .prevent .capture .self .once`，通过.表示的指令后缀来调用修饰符。

	<!-- 阻止单击事件冒泡 -->
	<a v-on:click.stop="doThis"></a>

	<!-- 提交事件不再重载页面 -->
	<form v-on:submit.prevent="onSubmit"></form>

	<!-- 修饰符可以串联  -->
	<a v-on:click.stop.prevent="doThat"></a>

	<!-- 只有修饰符 -->
	<form v-on:submit.prevent></form>

	<!-- 添加事件侦听器时使用事件捕获模式 -->
	<div v-on:click.capture="doThis">...</div>

	<!-- 只当事件在该元素本身（比如不是子元素）触发时触发回调 -->
	<div v-on:click.self="doThat">...</div>
## 组件
### data必须是一个函数
在组件中 `data` 必须是一个函数。

	Vue.component('myComponent',{
		template: '<span>name</span>',
		data: function(){
			return {counter: 0} //返回一个对象	
		}
	})

### 组件中的通信
#### 父子组件通信
在vue中，父子组件的关系可以总结为props doen,events up，父组件通过props向下传递数据给子组件，子组件通过events给父组件发送信息。
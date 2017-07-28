##动态路由匹配
我们经常需要把某种模式匹配到的所有路由全部映射到同一个组件，基于这种情况，我们可以在vue-router的路由路径中使用『动态路径参数』来实现。

	const router = new VueRouter({
		routes: [
			//动态路径参数 以冒号开头
			{path: '/user/:id', component: User}
		]
	})
现在，像/user/foo和/user/bar这样的路径都将映射到相同的路由。  
一个『动态路径参数』使用冒号:标记，当匹配到一个路由时，参数值会被设置到this.$route.params，可以在每个组件内使用。  
在一个路由中设置多段『路径参数』，对应的值都会被设置到$route.params中，如：   
 
	模式								匹配路径				$route.params
	------------------------------------------------------------------------------------------
	/user/:username					/user/Steven			{username: "Steven"}
	/user/:username/post/:post_id	/user/Steven/post/123	{username: "Steven", post_id: 123}
除了$route.params外，$route对象还提供了$route.query(如果url中有查询参数key/value对象)、$route.hash等。
###响应路由参数的变化
当使用路由参数时，例如从/user/foo导航到/user/bar时，原来的组件实例会被复用，因为两个路由都渲染的同一个组件。比起销毁再创建，复用显得更加高效，但这也意味着组件的声明周期钩子不会再被调用。  
######复用组件时，想对路由参数的变化做出响应的话，可以通过watch来监听$route对象：
	const User = {
		template: '...',
		watch: {
			//'$route' (to,from){...}
			'要监听的$route属性': (to, from) => {//对路由变化做出响应}
		}
	}
##导航
###声明式导航
<router-link :to="..."></router-link>  
to的值可以是一个字符串或者是描述目标位置的对象。

	//命名的路由
	<router-link :to="{name: 'use', params: '{userId: 123}'}">user</router-link>
	//带查询参数，下面的结果为	register/plan=private
	<router-link :to="{path: 'register', query: {plan: 'private'}}">Register</router-link>
#### active-class
设置链接激活时使用的css类名，默认值是"router-link-active"，可以通过路由的构造选项linkActiveClass来全局配置。  

	const router = new VueRouter({
		linkActiveClass: 'myLinkClass' //设置链接激活时的class
	})
###编程式导航
####router.push(...)  
该方法的参数可以是一个字符串路径，或者一个描述地址的对象。

	//命名的路由
	router.push(nam: 'user', params: {userId: 123})
	//带查询参数，变成 /register?plan=private
	router.push({ path: 'register', query: { plan: 'private' }})
####router.go(n)
该方法的参数是一个整数，意思是在history记录中向前或向后退多少步。
##命名路由
在创建Router实例的时候，在routes配置中给某个路由设置名称。

	const router = new VueRouter({
		routes: [
			{
				path: '/user/userId',
				name: 'user',
				component: 'User'
			}
		]
	})
##命名视图
有时候想同时(同级)展示多个视图，而不是嵌套展示，例如创建一个布局，有sideBar和main两个视图，这个时候就可以使用命名视图了，在页面中拥有多个单独命名的视图，而不是只有一个单独的出口。

	<router-view class="view1"></router-view>
	<router-view class="view2" name="a"></router-view>
	<router-view class="view3" name="b"></router-view>
一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件，确保正确使用components配置。

	const router = new VueRouter({
		routes: [
			{
				path: '/',
				components: {
					default: Foo,	//如果router-view没有设置名字，name默认为default
					a: bar,
					b: baz
				}
			}
		]
	})
##重定向-redirect
『重定向』的意思是，当用户访问/a时，url将会被替换成/b，然后匹配路由/b。
重定向的目标可以是一个路径字符串 命名的路由，或是一个方法，动态返回重定向的目标。

	const router = new VueRouter({
		routes: [
			{path: '/a', redirect: '/b'},
			{path: '/c', redirect: {name: 'foo'}},
			{path: '/d', redirect: to=>{
				//方法接收目标路由作为参数
				//return 重定向的字符串路径/路径对象
			}}
		]
	})
##别名-alias
『别名』的功能让你可以自由地将UI结构映射到任意的url，而不是受限于配置的嵌套路由结构。
/a的别=别名是/b，意味着当用户访问/b时，url会保持为/b，但是路由匹配则为/a，就像用户访问/a一样。

	const router = new VueRouter({
		routes: [
			{path: '/a', component: A, alias: '/b'}
		]
	})
##导航钩子
vue-router提供的导航钩子主要用来拦截导航，让它完成调整或取消。
###全局钩子
使用router.beforeEach注册一个全局的befor钩子。

	const router = new VueRouter({...})
	router.beforEach((to,form,next)=>{...})
当一个导航触发时，全局的before钩子按照创建顺序调用。钩子是异步解析执行，此时导航在所有钩子resolve完之前一直处于等待中。  
每个钩子方法接收3个参数：

	to： router，即将要进入的目标路由对象，相当于this.$route。
	from： router，当前导航主要离开的路由。
	next： Function，一定要调用这个方法来resolve这个钩子，执行效果是依赖next方法的调用参数。
		next()： 进行管道中的下一个钩子；如果全部钩子执行完了，则导航的状态是confirmed(确认的)。
		next(false)： 中断当前的导航，如果浏览器的url变了，那么url的地址会重置到from路由对应的地址。
		next('/')或next({path: '/'})： 跳转到一个不同的地址，当前的导航被中断，然后进行一个新的导航。
######确保要调用next()方法，否则钩子就不会被resolve。
同样可以注册一个全局的after钩子，不过after钩子没有next方法，不能改变导航。
	
	router.afterEach(route => {...})
###某个路由独享的钩子
在路由配置上(routes中)直接定义beforeEach钩子。
###组件内的钩子
在路由组件内直接定义钩子

	const Foo = {
	  template: `...`,
	  beforeRouteEnter (to, from, next) {
	    // 在渲染该组件的对应路由被 confirm 前调用
	    // 不！能！获取组件实例 `this`
	    // 因为当钩子执行前，组件实例还没被创建
	  },
	  beforeRouteUpdate (to, from, next) {
	    // 在当前路由改变，但是该组件被复用时调用
	    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
	    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
	    // 可以访问组件实例 `this`
	  },
	  beforeRouteLeave (to, from, next) {
	    // 导航离开该组件的对应路由时调用
	    // 可以访问组件实例 `this`
	  }
	}
beforeRouteEnter 钩子 不能 访问 this，因为钩子在导航确认前被调用,因此即将登场的新组件还没被创建。
###数据获取
有时候进入某个路由后，需要从服务器获取数据，如在渲染用户信息时，需要从服务器获取用户的数据，可以有两种方式实现：
####导航完成后获取
先完成导航，然后在接下来的组件生命周期钩子(created)中获取数据，在数据获取期间显示『加载中』之类的指示。
####导航完成前获取
导航完成前在路由的beforeEnter钩子中获取数据，在数据获取成功后执行导航。
##滚动行为
当页面切回之前的路由时，想要页面滚到顶部或者是保持原先的滚动位置，就像重新加载页面那样，vue-router可以自定义路由切换时页面如何滚动。
######注意：这个功能只在HTML5 history模式下可用。
当创建一个Router实例时，可以提供一个scrollBehavior方法：

	const router = new VueRouter({
	  routes: [...],
	  scrollBehavior (to, from, savedPosition) {
	    // return 期望滚动到哪个的位置
	  }
	})
scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
##路由懒加载
当打包构建应用时，js包会变得非常强大，影响页面加载。如果能将不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。  
结合 Vue 的 异步组件 和 Webpack 的 code splitting feature, 轻松实现路由组件的懒加载。  
我们要做的就是把路由对应的组件定义成异步组件：

	const Foo = resolve => {
		//require.ensure是webpack的特殊语法，用来设置代码分块code-split point
		require.ensure(['./Foo.vue'],()=>{
			resolve(require('./Foo.vue'))
		})
	}
这里还有另一种代码分块的语法，使用AMD风格的require：
	
	const Foo = resolve => require(['./Foo.vue'],resolve)
不需要改变任何路由配置，跟之前一样使用Foo：

	const Router = new VueRouter({
		routes: [
			{path: '/foo', component: Foo}
		]
	})
###把组件按组分块
有时候我们想把某个路由下的所有组件都打包在同个异步chunk中，只需要给chunk命名，提供require.ensure的第三个参数作为chunk的名称：

	const Foo = r => require.ensure([], () => r(require('./Foo.vue')), 'group-foo')
	const Bar = r => require.ensure([], () => r(require('./Bar.vue')), 'group-foo')
	const Baz = r => require.ensure([], () => r(require('./Baz.vue')), 'group-foo')
Webpack 将相同 chunk 下的所有异步模块打包到一个异步块里面 —— 这也意味着我们无须明确列出 require.ensure 的依赖（传空数组就行）
##路由信息对象

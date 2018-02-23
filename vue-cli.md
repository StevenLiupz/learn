##vue-cli
#####1. 安装vue-cli
	`npm install -g vue-cli`
#####2. 生成项目 
	`vue init webpack Vue-project`
	其中，webpack是模板名称，Vue-project是自定义的项目名称
	执行上诉命令时会需要输入一些配置信息：
	Project name(Vue-project) sell		#项目名称
	Project Description sell project  	#项目描述
	Autor Steven	#作者
	Use build standalone	#打包方式(遇到Use build直接回车即可)
	Install vue-router		#是否安装vue-router，y
	Use ESLint to lint your code 	#是否使用ESLint规范代码，建议不要写y
	Setup unit tests with Karma·Mocha？ #是否需要单元测试，n
	Setup e2c tests with Nightwatch？	#是否需要依托于HTTP的REST API，n
#####3. 安装依赖
	`npm install`
#####4. 载入vue-router和vue-resource
	'npm install vue-router vue-resource --save'
#####5. 启动项目
	`npm run dev`
	注意： 这里是默认启动的是本地的8080端口，因此要确保8080端口未被占用。
	如果8080端口被占用，需要修改配置文件config/index.js
	`
	build{
		assetsPublicPath: './'	//这里是路径前缀，将默认的'/'改为'./'，因为打包之后，外部引入js和css文件时，如果路径以'/'开头，本地是无法找到对应文件的(服务器上没问题)
	},
	dev{
		port: 6666	//默认是8080端口，这里将其改为不常用的端口号
	}`
#####6. 打包上线
	`npm run build`
	自己的项目文件都需要放到src文件夹下，项目开发完成之后输入以上命令进行打包
#####7. 参考文献
基于vue-cli快速构建项目: [https://www.jianshu.com/p/2769efeaa10a](https://www.jianshu.com/p/2769efeaa10a "基于vue-cli快速构建项目")  
Vue2.0入坑教程（上）： [https://www.jianshu.com/p/1626b8643676](https://www.jianshu.com/p/1626b8643676 "Vue2.0入坑教程（上）")  
Vue2.0入坑教程（中）： [https://www.jianshu.com/p/2b661d01eaf8](https://www.jianshu.com/p/2b661d01eaf8 "Vue2.0入坑教程（中）")  
Vue2.0入坑教程（下）： [https://www.jianshu.com/p/ec436222c608](https://www.jianshu.com/p/ec436222c608 "Vue2.0入坑教程（下）")
#####创建好目录后，各个文件的作用：
	01. build  			#脚本目录
	02. config    		#配置目录
	03. node_modules	#依赖的node工具包目录
	04. src				#源码目录
	05. src/assets		#资源目录
	06. src/components	#组件目录
	07. src/App.vue		#页面级Vue组件
	08. src/main.js		#页面入口js文件
	09. static			#静态文件目录
		在static目录下有一个.gitkeep文件，作用是即使static是一个空文件夹，也能git提交到远程服务器，因为默认git会忽略空文件夹
	10. test			#测试文件目录
	11. .eslintrc.js	#ES语法检查配置
	12. index.html		#入口页面
	13. package.json	#项目描述文件
	
## Vue项目中的具体文件说明
### main.js文件
	import Vue from 'vue'
	import App from './App' //引入组件(后缀名.vue可以省略)
	import router from './router' //引入路由
	 
	Vue.config.productionTip = false  //阻止vue在启动时生成生产提示
	
	/* eslint-disable no-new */
	new Vue({
	  el: '#app', //最后效果是将所有视图放在id为app的div元素中
	  router,	//使用路由
	  template: '<App/>', //告知页面这个组件用这样的标签来包裹着，并写进#app中
	  components: { App } //注册组件，即上面引入的App文件
	})
######注意：在vue中，诸如.vue .js .json这些后缀名的文件可以省略后缀名，这些配置是在build/webpack.base.conf.js文件中 

### App.vue文件
	<template>
	  <div id="app">
	    <img src="./assets/logo.png"> 
	    <!-- 路由出口-路由匹配到的组件将渲染在这里 -->
	    <router-view></router-view>
	  </div>
	</template>
	
	<script>
	  export default {
	    name: 'app'
	  }
	</script>
	
	<style>
	  #app {
	    font-family: 'Avenir', Helvetica, Arial, sans-serif;
	    -webkit-font-smoothing: antialiased;
	    -moz-osx-font-smoothing: grayscale;
	    text-align: center;
	    color: #2c3e50;
	    margin-top: 60px;
	  }
	</style>

### index.js文件
	import Vue from 'vue'
	import Router from 'vue-router'
	import Hello from '@/components/Hello'
	
	Vue.use(Router)	//显示声明要使用路由
	
	export default new Router({
	  routes: [
	    {
	      path: '/',
	      name: 'Hello',  //name参数只是用来做识别用的，没有什么特殊作用
	      component: Hello	//引入组件	
	    },
		{
		  path: '/about',
		  name: 'about',
		  component: 'about'
		}
	  ]
	})
######路由的配置应该一目了然，给不同的path分配不同的页面(即组件)，name参数不重要，只是用来做识别用的。
######嵌套路由：
	{
      path: '/blog',
      name: 'blog',
      component: Blog,
      children: [
        {
          path: '/',
          component: page1
        },
        {
          path: 'info',
          component: page2
        }
      ]
    }
访问http://localhost:8080/#/blog/的时候会往路由容器中放置page1的内容，访问http://localhost:8080/#/blog/info的时候会往路由容器中放置page2的内容。



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
	Install vue-router		#是否安装vue-router
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
	10. test			#测试文件目录
	11. .eslintrc.js	#ES语法检查配置
	12. index.html		#入口页面
	13. package.json	#项目描述文件
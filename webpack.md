## webpack打包工具

### webpack基本使用

- ```shell
  安装webpack
  npm i webpack webpack-cli -d 
  
  
  ```

- ```javascript
  //创建webpack.config.js配置文件 写入以下代码
  module.exports = {
  	//编译模式
  	mode: 'development' // development和production
  }
  ```

- ```javascript
  //在package.json下 配置
  {
  "scripts": {
      "dev": "webpack",//这一行是配置的
      "test": "echo \"Error: no test specified\" && exit 1"
    },
  }
  ```

- ```shell
  运行
  npm run dev
  ```

- 默认的打包入口文件是`src -> index.js`

- 默认的打包输出文件是`dist -> main.js`

- 可以在webpack.config.js中改配置信息

  - ```javascript
    const path = require ('path')
    
    entry: path.join(__dirname, './src/index.js'),
    	output: {
    		path: path.join(__dirname, './dist/'),//输出文件的存放路径
    		filename: 'bundle.js', //输出文件名称
    	},
    ```

- 自动打包

  - ```shell
    npm i webpack-dev-sever
    #然后在package.json中把这个东西改了
     "scripts": {
        "dev": "webpack-dev-server",
        "test": "echo \"Error: no test specified\" && exit 1"
      },
    #将index.html中引用路径改了
    <script src="/bundle.js"></script> #改成根路径之后要在服务器打开才能看结果否则是not found
     #因为webpack-dev-sever会启动一个实时打包的http服务器
    #启动
    npm run dev 
    
    ```

- 配置html-webpack-plugin生成预览页面

  - ```shell
    #下载插件
    npm i html-webpack-plugin
    #修改配置信息
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    const htmlPlugin = new HtmlWebpackPlugin({
    	template: './src/index.html',
    	filename: 'index.html'
    })
    #这里是导出exports
    plugins: [htmlPlugin]
    
    ```

  - ```javascript
    //一些自动打包的参数设置----这个是在package.json中配置的
    "dev": "webpack-dev-server --open --host 127.0.0.1 --port 8080"
    //open  自动打开浏览器
    //host  配置ip
    //port  配置端口号
    
    ```

### webpack中的加载器

1. 通过loader打包非js模块

   - webpack不能打包非js模块,所以要调用loader模块协助打包

   - 使用步骤

     - ```shell
       npm i style-loader css-loader -d
       npm i less-loader less -d
       npm i sass-loader node-sass -d
       npm i postcss-loader autoprefixer -d #自动兼容前缀
       npm i url-loader file-loader -d #打包样式表中的图片和字体文件
       npm i babel-loader @babel/core @babel/runtime -d #打包js文件
       npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -d
       ```

     - ```javascript
       //在webpack中配置
       module: {
       		rules: [
       			{ test: /\.css$/,use: ['style-loader', 'css-loader', 'post-loader']}, 
       			{ test: /\.less$/,use: ['style-loader', 'css-loader', 'less-loader']},
       			{ test: /\.scss$/,use: ['style-loader', 'css-loader', 'sass-loader']},
                   { test: /\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/, use: ['url-loader?limit=16940']}, //limit是指小于这个字节的才会被打包成base64文件 使加载更快
                   { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
                   
       		]
       	}
       //导入import './css/1.css'  要加根节点
       //导入import './less/1.css'  要加根节点
       //新建一个postcss.config.js然后写入  
       const autoprefixer = require('autoprefixer')
       module.exports = {
       	plugins: [autoprefixer]
       }
       //新建一个babel.config.js然后写入
       module.exports = {
       	presets: ['@babel/preset-env'],
       	plugins: ['@babel/plugin-transform-runtime', '@babel/plugin-proposal-class-properties'],
       }
       
       ```


### Vue单文件组件

#### Vue单文件组件的基本用法

1. 单文件组件的组成结构

   - template组件的模板区域

   - script    业务逻辑区

   - style      样式区域

   - ```html
     <!--基本结构-->
     <template>
     	<div>
     		<h2>不跟你多bb</h2>
     	</div>
     </template>
     
     <script>
     	export default {
     		data() {
     			return {}
     		},
     		methods: {}
     	}
     </script>
     <!-- 尽量都加上scoped属性,避免样式名冲突 -->
     <style scoped>
     	h2 {
     		color: #aa5;
     	}
     </style>
     
     ```

   - 基本步骤

     - ```shell
       npm i vue-loader vue-template-compiler -d
       #导入
       import App from './components/App.vue'
       #在webpack.config.js中配置
       const VueLoaderPlugin = require('vue-loader/lib/plugin')
       plugins: [htmlPlugin, new VueLoaderPlugin()],
       #这是规则
       { test: /\.vue$/, use: 'vue-loader' },
       ```

   - 在webpack下使用vue

     - ```shell
       npm i vue
       #在index.js下导入
       import Vue from 'vue'
       import App from './components/App.vue'
       const vm = new Vue ({
       	el: '#app',
       	data: {},
       	render: h => h(App) #以后在webpack下都尽量用这个函数
       })
       
       ```

#### webpack打包与发布

1. 在package.json下配置打包命令

   - ```json
     "build": "webpack -p"
     ```

### vue脚手架

#### vue脚手架的基本用法

1. 步骤

   - ```shell
     npm i -g @vue/cli
     #检查 就是用vue -v
     ```

   - vue创建vue项目

     - ```javascript
       //交互式命令行
       vue create my-project
       //图形化界面
       vue ui
       ```

   - 配置vue自定义

     - ```javascript
       module.exports = {
       	devServer: {
       		open: true,
       		port: 8848
       	}
       }
       也可以在package.json里面
       ```

### Element-UI

#### 基本使用

1. 基于命令行方式手动安装

   - ```shell
     npm i element-ui
     ```

   - 导入Element-UI相关资源

     - ```javascript
       // 手动配置ElementUI
       import ElementUI from 'element-ui'
       import 'element-ui/lib/theme-chalk/index.css'
       Vue.use(ElementUI)
       
       ```

2. 自动安装

   - 运行 vue ui 打开图形化界面
   - 通过vue项目管理器进入具体的项目配置面板
   - 点击插件 ->添加插件,进入插件查询面板
   - 搜索vue-cli-plugin-element并且安装
   - 配置插件,实现按需导入,从而减少打包后项目的体积


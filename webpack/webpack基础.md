# webpack

模块打包工具

模块引入导出方式
### ES module 模块引入方式  
```
import XX from XX；  
export default XX;
```
### CommonJs模块引入规范  
```
var xx=require('xx');
module.exports=xx
```
### CMD/AMD
``` 
require(['./util.js',function(util){}])
define (function(){})
```   

## webpack安装
### 全局方式
```
npm install webpack webpack-cli -g
```  
不推荐此种方式安装，不同项目可能使用不同的webpack版本，会出现两个不同版本webpack无法同时启动的情况

### 项目内安装
``` 
npm install webpack webpack-cli -D(--save -dev)
```  
- 项目下查看webpack版本
``` 
npx install
```   
命令会在当前目录的node_modules中查找
- 打包命令
```  
npx webpack
```  

## webpack配置文件 
默认webpack.config.js  
修改启动时配置文件
```  
npx webpack --config newname.js
```  
### 配置文件中属性名称  
```javascript 
var path = require('path')
module.exports = {
    entry: './src/index.js',//打包的入口文件
    mode:'development',//按照什么模式打包
    output: {//输出的打包后文件的文件名和文件路径
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
}
```  

## entry   
打包入口文件，在不配置output的情况下，默认输出的文件为main.js.  
``` entry: './src/index.js'```是 
```   entry:{
        main:'./src/index.js'
    } ``` 的简写形式  
    entry可以生成多个文件
    
    ```
    entry:{
        main:'./src/index.js',
        sub:'./src/index.js'
    } 
    ```  
    如果生成多个文件时，output的filename为固定值会报错，此时可写成``` output: {filename:[name].js}``` 生成的两个文件分别为main.js和sub.js,更多filename的配置方法可参看https://www.webpackjs.com/configuration/output/
## mode  
mode有三个模式：none、production（默认）、development    
其中production压缩代码，development不压缩
## output  
path定义的文件夹一定要用绝对路径，所以引用nodejs的path模块获取绝对路径 
### filename 
输出的文件名```filename: '[name].js'```
### path
输出路径``` path: path.resolve(__dirname, 'dist')```
### publicPath
给引入的js添加统一前缀```publicPath:'http://xxx.com'```

## devtool
映射关系，main.js与原文件中某文件某行的对应关系   

- none 无映射
- source-map:打包过程比较慢，打包后会出现main.js.map的映射文件
- inline-source-map:会以base64形式引入main.js中，报错具体到某行某列
- inline-cheap-source-map:会以base64形式引入main.js中，报错具体到某行，此模式不管第三方模块
- cheap-module-source-map：会以base64形式引入main.js中，报错具体到某行，此模式管第三方模块
- eval 打包方式最快，会在main.js中添加eval打印代码，代码复杂时打印不全面
- cheap-module-eval-source-map:会在main.js中添加eval打印代码，报错具体到某行，此模式管第三方模块

### 常用打包配置
- production ：cheap-module-source-map
- development：cheap-module-eval-source-map  

具体查看文档：https://www.webpackjs.com/configuration/devtool/


## devServer
### webpack中三种可以自动打包的方式
- 1.webpack --watch
在package.json中配置--watch，可以在文件修改的时候重新打包编译，但是不能刷新浏览器
- 2.webpackDevServer
``` 
  devServer: {
        contentBase: "./dist",//启动服务器的根目录
        open: true,//是否自动打开浏览器
    }
``` 
为项目启一个服务器，修改时自动打包文件，自动刷新浏览器。还可以配置poxy，port等项目。打包的结果不会生成dist文件，会存在内存中。  
具体查看文档：https://www.webpackjs.com/configuration/dev-server/
- 3.node server.js
老版本webpack的devServer不完善，需要自己写node文件来启动  
可以使用express或koa框架，同时安装webpak-dev-middlewave
简单的启动

```
var express = require('express')
const webpack = require('webpack')
const webpackDevMiddleWare = require('webpack-dev-middleware')
const config = require('./webpack.config.js')
const complier = webpack(config) //编译用

const app = express()
app.use(webpackDevMiddleWare(complier, {
    publicPath: config.output.publicPath
}))

app.listen(3000, () => {
    console.log('server is running')
})
```
### HotModuleReplacement(HMR)
可以在不刷新页面的情况下，显示更改的样式。还可以有js时只重新载入有修改的文件。

``` 
 devServer: {
        hot: true,//开启HMR功能
        hotOnly: true//即使没生效也不让浏览器自动刷新
    }
    
//在plugin中配置，因为是webpack自带，配置如下
const webpack=require('webpack')
plugins: [ new webpack.HotModuleReplacementPlugin() ],
```

## module中的loader
loader的加载顺序，从下到上，从右到左

### 图片 file-loader  

``` 
{
     test: /\.jpeg$/,
      use: {
      loader: 'file-loader',
      options: {
        name: '[name]_[hash].[ext]'
       }
    }
 }
```

### css文件 style-loader/css-loader    
其中css-loader负责合并css文件，style-loader负责挂在css到页面的header中

```
{
    test: /\.css$/,
    use: ['style-loader', 'css-loader']
}
```  
### sass文件 
除css要安装的loader ，额外添加sass-loader  
安装sass-loader需要同时安装node-sass  (详见文档：https://webpack.js.org/loaders/sass-loader/)

```
{
  test: /\.scss$/,
  use: [
      'style-loader',
      'css-loader',
      'sass-loader',//从下到上加载，先翻译sass文件
      'postcss-loader'//添加厂商前缀loader，见下
   ]
 }
npm install sass-loader node-sass webpack --save-dev
``` 
### 样式中添加前缀的loader  
postcss-loader 需要一个配置文件postcss.config.js  
其中至少引入autoprefixer

```  
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}
```  
此时如果还未添加，可尝试在package.json中添加browserslist配置项

```  
  "browserslist": [
        "iOS >= 6",
        "Android >= 4",
        "IE >= 9"
   ],
``` 
### iconfont
同image可以使用file-loader将文件放入打包文件夹中

```
{
   test: /\.(eot|ttf|svg|woff)$/,
   use: {
         loader: 'file-loader'
        }
}
```

### babel
内容参看https://www.babeljs.cn/setup#webpack 内容

```
npm install --save-dev babel-loader @babel/core
npm install @babel/preset-env --save-dev
{
  "presets": ["@babel/preset-env"]
}
``` 
babel-loader只是建立了babel和webpack的桥梁，并不会吧es6转换成es5
preset-env会实现转换，如果要实现低版本语法的转化，还要安装polyfill

```
npm install --save @babel/polyfill
``` 
在页面```import "@babel/polyfill"```后再打包时会打入很多没有引用到的东西，此时可在preset-env中加“useBuiltIns”配置项解决

```
 presets: [
     ['@babel/preset-env', {
         useBuiltIns: 'usage',
         corejs: 3
     }]
 ]
```  
**注意**  
此时1.要同时配置corejs版本，同时安装相应版本  2.不再需要再js中引用polyfill  ，否则会报错
在业务代码中配置此项即可，如想在库项目中配置，要使用“transform-runtime”配置。

transform-runtime方式，会以闭包形式打包代码，避免全局污染
同时要安装corejs

```
npm install --save-dev @babel/plugin-transform-runtime
npm install --save @babel/runtime
npm install --save @babel/runtime-corejs

 "plugins": [
        ["@babel/plugin-transform-runtime", {
            "absoluteRuntime": false,
            "corejs": 3}
        ]
    ]
```

## plugins
可以在webpack打包的某个节点上做某些事情

### htm-webpack-plugin
- 作用：会在打包结束后生成一个index.html文件，并把打包生成的js自动引入到此文件中。  
- 作用时间：整个打包结束后

```
const HtmlWebpackPlugin = require('html-webpack-plugin')
plugins: [
	new HtmlWebpackPlugin({ template: 'src/index.html' })
	]
```
接收一个template参数作为index.html的模版

### clean-webpack-plugin 
- 作用：重新打包时删除原有的打包生成文件夹
- 作用时间：打包前运行   
写法1 ：此写法为旧写法，插件3.0以上版本会报```TypeError: CleanWebpackPlugin is not a constructor```错误

```
const CleanWebpackPlugin  = require('clean-webpack-plugin')
plugins: [new CleanWebpackPlugin(['dist'])]
```
方法2：新写法

```  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
plugins: [new CleanWebpackPlugin()]
```

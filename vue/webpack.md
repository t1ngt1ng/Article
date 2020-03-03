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
###配置文件中属性名称  
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
### 注意
- entry   
``` entry: './src/index.js'```是 
```   entry:{
        main:'./src/index.js'
    } ``` 的简写形式 
- mode  
mode有三个模式：none、production（默认）、development    
其中production压缩代码，development不压缩
- output  
path定义的文件夹一定要用绝对路径，所以引用nodejs的path模块获取绝对路径  

## loader
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

## plugin
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

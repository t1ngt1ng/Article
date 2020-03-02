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

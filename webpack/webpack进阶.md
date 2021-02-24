# webpack
## Tree Shaking
只打包被使用的代码，有被export的代码。
只支持es Module引入方式，common.js不支持。因为前者是静态引用，后者是动态的。

开发环境下：

```
//webpack.config.js

 mode: 'development',
 devtool: 'cheap-module-eval-source-map',
 optimization: {
        usedExports: true //检查被使用的打包模块
   }
   
//package.json
  "sideEffects": ["*.css"]
```
sideEffects选项是将不希望被tree shaking的文件填入，因为配置了usedExports: true后没有导出的文件会被忽略，如@bable/polyfill（只是在window上绑定了方法，没有导出内容）

生产环境：

```
//webpack.config.js
	mode: 'production',
	devtool: 'cheap-module-source-map',
//package.json
  	"sideEffects": ["*.css"]
```
生产环境会自行配置optimization项，不需要额外配置。  
**注意**  
生产环境可以看出代码的减少，开发环境的打包文件看不出具体效果（未使用的文件不打包），因为配置的报错提示需要对应文件行数等问题，只会在打包的文件中多添加一行注释

```
//两个方法add, minus，只引入了add
/*! exports provided: add, minus */
/*! exports used: add */  //不添加optimization，没有此行内容
``` 

## Development和Production模式的区分打包
可分别使用打包和运行命令

```
"scripts": {
    "dev": "webpack-dev-server --config webpack.dev.js",
    "build": "webpack --config webpack.prod.js"
}
```
配置中有代码重复的部分，可以使用webpack-merge实现整合
``` npm i -D webpack-merge```

在webpack.dev.js中引入整合后到处

```
const merge = require('webpack-merge')
const baseConfig = require('./webpack.base.js')
const devConfig = {}//自身不同配置
module.exports = merge(baseConfig, devConfig)
```
## code splitting
为了避免初始加载过大，加载时间过长
两种打包方式
1.同步，在webpack的配置恩家中配置splitChunks
2.异步，无需配置，会自动进行分割

```
    optimization: {
        splitChunks: {
            chunks: 'all'
        }
    }
```

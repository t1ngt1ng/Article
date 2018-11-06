# 用nodejs爬取googlePlay前台内容  

大致思路：
请求一个页面接收到页面的html代码，解析代码拿到想要的数据。

我们以抓取googlePlay上2016年度最佳游戏Pokémon GO的评论为例，写一个简单的爬虫

- 获取页面html代码

```javascript   
var request=require('request');
var url='https://play.google.com/store/apps/details?id=com.nianticlabs.pokemongo&hl=zh_CN';
request(url,(error, response, body)=>{
		console.log(body);
})

``` 

node 运行后，控制台打印如下内容：
![body打印结果](http://upload-images.jianshu.io/upload_images/4066840-dceecf4871086713.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 筛选需要的数据
这个步骤中可以用到cheerio来解析html，用jquery的方式就可以轻松取到内容了，很方便。
然后我们用chorme的开发者工具看一下单个评论的结构，选择其中红框后面的数据进行抓取


![评论结构分析](http://upload-images.jianshu.io/upload_images/4066840-d7c476066d2e6d05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

给之前的代码添加一个解析顺便打印的函数

``` javascript   
function formatHtml(html){
     var $=cheerio.load(html);
 	var reveiws=$('.single-review');
 	var reviewList=[];
 	reveiws.each(function(item){
 		var reveiws=$(this);
 		var author=reveiws.find('.author-name').text();
 		var star=reveiws.find('.tiny-star').attr('aria-label');
 		var rewTitle=reveiws.find('.review-title').text();
 		var rewContent=reveiws.find('.review-body').text();
 		console.log('rewTitle',rewTitle);
 		console.log('\n author:',author,'   star:',star);
 		console.log('\n rewContent:',rewContent);
 		console.log('-------------------------------------');
 	})
 }
 
```   
  
 
在之前打印body的地方调用formatHtml函数，传入body

```javascript  

request(url,(error, response, body)=>{
		formatHtml(body);
	})
	
```
运行文件，就可以看到打印结果了：

![Pokémon GO评论结果（部分）](http://upload-images.jianshu.io/upload_images/4066840-c5e8d0c422f2510d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最基本的抓取就是这样，抓不同内容也大同小异  
可以把url最后的语言改成ja或者其他语言看看别的区的评论。

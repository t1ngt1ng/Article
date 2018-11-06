# 用nodejs抓取Itunes前台内容  
 
思路可以看之前的文章，我就不多说了。  
因为AppStore的页面版只显示3天评论，所以，这次介绍的是抓取应用版的内容。  
我们以抓取AppStore上一款新上的**Super Mario Run**的评论为例，写一个简单的爬虫 
 
- 获取itunes评论
  
```js

var request=require('request');
var url='http://itunes.apple.com/WebObjects/MZStore.woa/wa/userReviewsRow?cc=us&id=1145275343&displayable-kind=11&startIndex=0&endIndex=100&sort=0&appVersion=all'
 var options = {
        port: 80,
        uri: url,
        method: 'GET',
        headers: {
            'User-Agent': 'iTunes/11.0 (Windows; Microsoft Windows 7 Business Edition Service Pack 1 (Build 7601)) AppleWebKit/536.27.1'
        }
    };
    request(options,(error, response, body)=>{
    	console.log(body);
    })
    
```
node 运行后，控制台打印如下内容：  

![body打印结果](http://upload-images.jianshu.io/upload_images/4066840-ee06e1195b8452e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个链接就是专门获取评论相关数据的，我们可以整理一下，看看每一条评论的结构  

![接收到的内容](http://upload-images.jianshu.io/upload_images/4066840-38e6c58cad439a73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 筛选需要的数据

知道结构了就很简单了，直接写一个format方法就可以打印数据了  

```javascript
    function formatJson(contentsDate){
        content=contentsDate.userReviewList;
        content.forEach(function(item){
        console.log('orginId:'+item.userReviewId);
        console.log('username: ' + item.name + '     time:' + item.date);
        console.log('star: ' + item.rating);
        console.log('title: ' + item.title);
        console.log('content: ' + item.body);
        console.log('------------------------------------');
    })
    }
```
在之前打印body的地方调用formatJson,传入转换成json的body

```javascript

 request(options,(error, response, body)=>{
    	var datas=JSON.parse(body)
    	formatJson(datas);
    })
    
```
运行文件，就可以看到打印结果了：

![Super Mario Run评论结果](http://upload-images.jianshu.io/upload_images/4066840-b3e0abdc3f3ee5b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为这款游戏没有在中国区上，所以要翻墙，不翻墙可以找一款中国区商店有的应用，将链接中的cc换成cn即可。不明白具体可参照之前抓取思路那篇文章。
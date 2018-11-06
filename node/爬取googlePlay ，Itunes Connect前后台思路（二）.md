# 爬取googlePlay ，Itunes Connect前后台思路（二）
### 后台
- google
googlePlay要抓取的后台是GooglePlay Developer Console
我简单说一下自动登录的思路吧。  
就是获取页面上的几个隐藏域的值，然后转到第二页，google是先填写名字查询到信息进入第二页输入密码。这时候有一个机器人认证，你找到页面中的这样开头的一个js函数：  

```
/* Anti-spam. Want to say hello? Contact (base64) 
```

然后在提交的时候连同这个js函数算出来的bgresponse字段带上。骗过机器人认证才行。  
然而，这个方法一个是很容易403，就算成功了也可能被google警告，如果被封号了就得不偿失了～所以最好不要用这种方式。  
想尝试可以去github上搜，有代码。单独搜bgresponse字段也有人介绍。  
通过GooglePlay Developer Console获取评论的正确方式只有官方给的api，因为这个过程要配置的地方有点乱，所以之后再专门写一篇吧。  

- apple
itunes要抓取的后台是Itunes Connect（我就简称IC了） 
IC是可以做自动登录的，所以我详细说一下     
IC需要两步请求，获得两个cookie   
第一步请求的地址：  

```
https://idmsa.apple.com/appleauth/auth/signin 

```  

headers中要携带一个X-Apple-Widget-Key，post携带用户名密码。cookie里有一个myacinfo字段，获取第一个cookies   
第二布，请求地址：
  
```
https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa 

```  

把第一步的cookie 携带上发一个get请求。返回的cookies里有个itCtx字段。获取到连个cookie之后拼接到一起就可以请求后台了。  
<strong>注意:如果myacinfo和itCtx字段在返回中没有了，很可能IC改变了登录方式。目前为止是可以用的</strong>   
给大家一个代码参考地址：  
<a href="https://github.com/stoprocent/node-itunesconnect">https://github.com/stoprocent/node-itunesconnect</a>   
我就是参考了这个项目的登陆写的IC的自动登录，如果你要的正好是他写的这部分功能，可以直接用，不需要另外写登录   




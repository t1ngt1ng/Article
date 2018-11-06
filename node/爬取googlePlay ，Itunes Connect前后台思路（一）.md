# 爬取googlePlay ，Itunes Connect前后台思路（一）
  
最近在用node.js爬取google Play, Itunes Connect 的数据。
总结一下思路，给大家做参考。
我抓取的都是某个app的评论，评星相关的信息。
以下我分前后台和分网站说明。  

## 前台：  
##### 注意  
<em>要注意的就是这两个网站的评论都按语言分区的</em>  
- googlePlay  
举几个lang的例子：  
英文(en) 中文-简体(zh-cn) 中文－繁体(zh_TW)  日语(ja) 法语(FR) 德语(DE_DE) 意大利语(it_IT) 西班牙语(ES) 荷兰语(nl_NL) 土耳其语(tr_TR) 阿拉伯语(ar-BH) 克罗地亚语(hr-BA) 
后几个DE_DE这种'_'前后一致的可以只写一个  

- app Store／Itunes    
举几个lang的例子：    
en, cn, TW, fr, DE, it, nl, tr  

#### 前台抓取

- google Play  
前台抓取其实没什么好说的，常规抓取就好了。  
googlePlay的语言在链接最后，链接如下：  

```  
https://play.google.com/store/apps/details?id={youAppName}&hl={lang}   
```  
有一个问题，是这样的：从页面抓取的评论时间都是不同语言显示的，可能是我技术不到家😂，像什么阿拉伯语之类的不会转换，所以无法按时间排序。如果只抓取中文英文什么的就不是问题了。
    
- appStore /Itunes     
网页版appStore只会显示最好的3条评论（四星以上不够3条的也不显示其他评论），所以没有什么抓取的价值，链接如下：   
 
```   
https://itunes.apple.com/{lang}/app/{appName}/id{youAppId}?mt=8   

```  
appName有的app有，有的没有，看具体app的链接吧～  
<p>用前台方式获取完整评论的方式就是通过Itunes  </p>
具体链接如下：  

```  
itunes.apple.com/WebObjects/MZStore.woa/wa/userReviewsRow?cc={lang}&id={youAppId}&displayable-kind=11&startIndex=0&endIndex=100&sort=0&appVersion=all

```

通过这个链接获取需要在请求的时候，在header中配置一个用户代理  


```
 headers: {'User-Agent': 'iTunes/11.0 (Windows; Microsoft Windows 7 Business Edition Service Pack 1 (Build 7601)) AppleWebKit/536.27.1'}   
 
```  


再通过变换lang就可以获取全部的评论了，google一定要翻墙，itunes中文可以不翻墙，其他语言好像也需要。
google一定要能在终端里ping通google，如果只是网页可以访问，终端ping不通也是没法抓的。

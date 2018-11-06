# 关于css结构和层叠的习题
最近学习了css权威指南这本书，大多基于css2和css2.1的介绍。
总结一些层叠样式相关的知识和习题，供自测。

* 特殊性
所有样式冲突的解决都由层叠来处理  
下面是特殊性如何计算

![特殊性计算.png](https://upload-images.jianshu.io/upload_images/4066840-fb1eeca719fa5829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


另外：结合符和通配符等选择器对特殊性无影响

题目如下：  
例：h1 { color : "red" } ; （0，0，0，1）
{}下面题中大括号内的内容就省略了，和特殊性没有太大影响

1.  body h1 {  ... }
2.  h2.grape {  ... }
3.  html>body table tr[id="totals"] td ul>li { ... }
4.  li #answer { ... }
5.  p em { ... }
6.  *.light { ... }
7.  p.bright em.dark { ... }
8.  \#id216 { ... }
9.  div#sidebar *[href]{ ... }
10.  h1+p { ... }


答案：  
1.  0，0，0，2  
2.  0，0，1，1  
3.  0，0，1，7  
4.  0，1，0，1  
5.  0，0，0，2  
6.  0，0，1，0  
7.  0，0，2，2  
8.  0，1，0，0  
9.  0，1，1，1  
10.  0，0，0，2  

* 层叠的规范
  
1.找出所有相关的规则，这些规则都包含一个给定元素匹配的选择器  
2.按显式权重对应用到该元素的所有声明排序。标志！important 的规则权重高于没有！important标志的规则。  
按来源对应用到给定元素的所有声明排序。共有三种：创作人员、读者和用户代理。  
正常情况下：创作人员的样式要胜过读者样式。  
有！important标志的读者样式要强于所有其他样式，这包括有！important标志的创作人员样式。  
创作人员样式和读者样式都比用户嗲里的默认样式要强  

``` 
正常：创作人员 > 读者 > 用户代理
有！important情况下：读者 > 创作人员 > 用户代理

```  
3.按特殊性对应用到给定元素的所有声明排序。有较高特殊性的元素权重要大于有较低特殊性的元素  
4.按出现顺序对应用到给定元素的所有声明排序。一个声明在样式表或文档中越后出现，它的权重就越大。如果样式表中有倒入的样式表，一般认为出现在导入样式表中的声明在前，主样式表中的所有声明在后。  

题目：以下样式会显示哪个 
 
``` 
1. 	 p{ color : gray !important ; }
    <p style = " color : black " >XX</p> 
   
2.	 p em { color : black ; } / * author's style sheet * /
 	 p em { color : yellow ; } / * reader's style sheet * /

3.  p em { color : black !important ; } / * author's style sheet * /
    p em { color : yellow !important ; } / * reader's style sheet * /
     
4.  p#bright { color : silver ;}
    p { color: black ; }
	<p id = "bright " >XXXXXX</p>

5.  h1  { color : red }
    h1  { color : black }
     
6.  p em{ color : purple } /* from import style sheet*/
    p em{ color :grad } /* rule contained within the document */

```

答案：
1.  1
2.  1
3.  2
4.  1
5.  2
6.  2




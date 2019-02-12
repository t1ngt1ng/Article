# icon图标
网页中经常会使用到icon，之前都是雪碧图，后来出现了查看之后出现奇怪符号的图标，就是使用了字体来定义icon

### 常使用的绘制图标的方式
- css sprite
- unicode
- font class
- Symbol  
 
后面三种都可以通过矢量图标库获取，这里使用比较火的阿里巴巴矢量图标库。  

##  sprite  

sprite就是常说的雪碧图，一张图上缩印了很多小图片，通过设置图片的position来显示不同的图片。

### 主要是用的css属性：

```
{
    background-image: url('');
    background-position: 0 0;
 }
```  
### 特点
1.相对于单个的小图标，节省文本体积和服务器请求次数
2.一般情况下，要保存为PNG-24 wenjiangeshi 
3.可以使用彩色图片做图标

百度百科的优缺点总结如下：

``` 
优点
1.利用CSS Sprites能很好地减少网页的http请求，从而大大的提高页面的性能，这也是CSS Sprites最大的优点，也是其被广泛传播和应用的主要原因；  
2.CSS Sprites能减少图片的字节，曾经比较过多次3张图片合并成1张图片的字节总是小于这3张图片的字节总和。
3.解决了网页设计师在图片命名上的困扰，只需对一张集合的图片上命名就可以了，不需要对每一个小元素进行命名，从而提高了网页的制作效率。
4.更换风格方便，只需要在一张或少张图片上修改图片的颜色或样式，整个网页的风格就可以改变。维护起来更加方便。 

缺点
诚然CSS Sprites是如此的强大，但是也存在一些不可忽视的缺点，如下：
1.在图片合并的时候，你要把多张图片有序的合理的合并成一张图片，还要留好足够的空间，防止板块内出现不必要的背景；这些还好，最痛苦的是在宽屏，高分辨率的屏幕下的自适应页面，你的图片如果不够宽，很容易出现背景断裂；
2.CSS Sprites在开发的时候比较麻烦，你要通过photoshop或其他工具测量计算每一个背景单元的精确位置，这是针线活，没什么难度，但是很繁琐；
3.CSS Sprites在维护的时候比较麻烦，如果页面背景有少许改动，一般就要改这张合并的图片，无需改的地方最好不要动，这样避免改动更多的css，如果在原来的地方放不下，又只能（最好）往下加图片，这样图片的字节就增加了，还要改动css。
4.CSS Sprites非常值得学习和应用，特别是页面有一堆icon（图标）。总之很多时候大家要权衡一下利弊，再决定是不是应用CSS Sprites。
```  

演示代码请查看[code/css/icon/sprite.html](). 
此处代码中因为不想引入新的图，用的sprite图不是图标，是canvas例子中的birds。  
将小鸟当作图标，后面做了个划过时切换下一张小鸟图片，最后一张切换到前一张，平时使用图标时，可以通过这个方法让鼠标滑过时切换图标。

不想依赖jquery所以用了原生js。  
部分代码如下：

```  
 let is = document.getElementsByTagName('i'),
        lis = document.getElementsByTagName('li');
        
   //is.lis获取到的是TMLCollection类型不是array因为也是iterator,可以用for和for in遍历，这里我转成了数组  
  [...lis].forEach((item, idx) => {
        item.childNodes[0].style.backgroundPosition = '-' + width*idx + 'px 0';

        //此处做，滑过切换下一张小鸟，最后一张切到倒数第二张
        // 平时图标可以做滑过变色，这里就不再特别引入新的sprite图了
        item.addEventListener('mouseover',function(){
            item.childNodes[0].style.backgroundPosition='-' + width*(idx>1?idx-1:idx+1) + 'px 0';
        })
        item.addEventListener('mouseout',function(){
            item.childNodes[0].style.backgroundPosition = '-' + width*idx + 'px 0';
        })
    })
```  

## iconfont  

以下的图标使用方法都出自https://www.iconfont.cn/

网站登录后，可以将图标单独下载，也可以放到一起用于项目   

单独下载可以导出三种类型：svg，ai，png，可以做简单的颜色修改。 
如果想用于项目可以点购物车图标，加入到项目。然后下载压缩包。

文件夹中有九个文件

```  
- demo_index.html  图标的使用方法，非常详细，代码可以直接复制  
- demo.css         这个文件是自带demo_index页的css，可以忽略  
- iconfont.css     这个文件是提供给font class形式的css样式  
- iconfont.eot     微软开发的用于潜入页面中的字体，IE专用(ie6+)   
- iconfont.woff/    web字体中最佳格式，开放的trueType/openType的压缩版本 （IE9+，其他浏览器可用）
  iconfont.woff2
- iconfont.ttf     由MS和Apple联合开发的一套字体标准，是Mac OS和WIN操作系统中最常见的一种字体 （IE9+，其他浏览器可用）
- iconfont.svg	   用于svg字体渲染的一张格式，由w3c制定，开放标准的图形格式     
- iconfont.js	   使用Symbol 方式的js文件  
```  
因为具体的使用方法非常详细，所以就不解释了，官网的代码应用方法：
https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.15&helptype=code  


相比较而言，使用unicode方式，需要引入所有不同后缀字体文件，其他两种只要引入对应的css和js即可。官方推荐使用Symbol，也就是svg形式，但class方式也很操作 

用字体方式引入icon可以像操作字体一样操作图片，
下面例子使用css让图标有动画效果。
[code/css/icon/DyIcon.html](https://github.com/t1ngt1ng/code/tree/master/css/icon)

分别为添加边框，缩放效果，动画

# canvas相关公式总结

## 角度弧度转换  

因为canvas中大多数使用的都是弧度，所以还是熟悉转换比较好
基本对应关系就是360度=2PI  
常见对应角   
  
|角度        | 弧度        | 
| ----------- |:----------:| 
| 0 / 360度   | 0 / 2PI  | 
| 57.2958度   | 1        |
| 90度        | PI／2    |
| 180度        | PI      |
| 270度        | 3*PI／2  |    

```javascript   

let radians,//角度
    degress; //弧度

radians = degress * Math.PI / 180;
degress = radians * 180 / Math.PI;


```   

## 创建正弦波（或震荡）
思路就是：
1.不清楚画布上一步操作
2.x轴匀速运动
3.y轴随正弦值波动 
4.不想绘制或者沿着正弦波方向运动，只想上下往返运动，可以保持x不变
  
#### 说明    
- obj为运动物体，有x,y轴属性  
- angle为变化的角度，range为运动幅度，speed为运动速度
	
 
```javascript   
(function drawFrame() {
    window.requestAnimationFrame(drawFrame, canvas);

    obj.y = Math.sin(angle) * range;
    obj.x += speed;
    angle += speed;
})()
```  

## 圆周运动
圆周运动核心就是x.y增加的幅度是一样的，依然是知道夹角和其中一边求另一边的公式.   
下面代码每次不清除画布就是，不添加运动物体也可以画出一个圆的创建过程。   
#### 说明
- obj为运动物体，包括x,y属性
- 圆心坐标为（centerX,centerY）
- speed为速度，radius为半径

```javascript  
(function drawFrame() {
    window.requestAnimationFrame(drawFrame, canvas);
    canvas.clearRect(0, 0, canvas.width, canvas.height);
    obj.x = centerX + Math.cos(angle) * radius;
    obj.y = centerY + Math.cos(angle) * radius;
    angle += speed;
})();

``` 

 
## 椭圆运动
椭圆运动其实和圆是一样的唯一的不同就是椭圆的，xy方向上的半径是不一样的

```javascript  
(function drawFrame() {
    window.requestAnimationFrame(drawFrame, canvas);
    canvas.clearRect(0, 0, canvas.width, canvas.height);
    obj.x = centerX + Math.cos(angle) * radiusX;
    obj.y = centerY + Math.cos(angle) * radiusY;
    angle += speed;
})();

``` 



## 两点之间距离公式

两点之间公式用到勾股定理，直角三角形中已知其中两边可以得到第三边

```javascript   

let p1 = {x: width, y: height},
    p2 = {x: width, y: height};
let dx = p1.x - p2.x;
let dy = p1.y - p2.y;
let dist = Math.sqrt(dx * dx + dy * dy);
```   
#### 参考书籍  
1.html5+javascript动画基础  

本文具体代码可以参照code文件夹下的相关文件，详情看code/REDME.md文件
# canvas动画边界

在动画中如果不限制一个运动物体，它就会运动出边界。

## 处理物体越界的方法
- 将物体移除
- 将其物体置回边界内，就像产生了一个新的物体（重置对象）
- 让同一个物体出现在边界内的另一个位置
- 将物体反弹回到边界内

## 设置边界
因为canvas是以左上角（0,0）开始，到右下角(canva.width,canvas.height)结束。
我们可以列出这个canvas的四个边界：

``` javascript
let left=0;
	 top=0;
	 right=canvas.width;
	 height=canvas.height;
```

于是可以做简单的if／else判断

``` 
if(ball.x > right){ ... }
else if(ball.x < left){ ... }

if(ball.y > bottom){ ... }
else if(ball.y < top){ ... }

``` 

### 将物体移除 
思路：  
当多个物体在移动时，应该将它们的引用保存到一个数组中，再通过遍历整个数组来移动它们。  
应用：  
如果物体会不断产生，那么移除物体的做法很有效。被移除的物体会被新加入的对象所取代，这样canvas就永远不会空，同时边界内也不会有太多物体在移动，从而导致浏览器变慢。  


但是此种方式判断，如果物体是一个圆，坐标点在中心，只有当圆心移动到边界才会判断为越界。为了实现球体完全离开canvas再移除，必须将物体的宽度考虑进来。

```  
  if (ball.x - ball.radius > canvas.width ||
                ball.x + ball.radius < 0 ||
                ball.y - ball.radius > canvas.height ||
                ball.y + ball.radius < 0){
                //移除操作
                balls.splice(pos, 1);//从数组中移除
                }
```   

### 重置物体

思路：
当一个物体移出canvas且不再需要时，重新设定其位置，看上去像重新创建了一个全新的对象。   
应用：可用于创建喷泉或其他各种粒子特效，水滴不断飞溅出来，飞出canvas后重新加入到水流的源头

```    
    if (ball.x - ball.radius > canvas.width ||
                ball.x + ball.radius < 0 ||
                ball.y - ball.radius > canvas.height ||
                ball.y + ball.radius < 0) {
                //重置位置
                ball.x = canvas.width / 2;
                ball.y = canvas.height;
            }
 
```  

### 屏幕环绕   
思路／应用：  
如果一个物体从屏幕左边移出，就出现在屏幕右边再次出现；当物体从屏幕上方移出时，又会出现在屏幕下方。    
屏幕环绕与重置比较：  
相同：都会将越界的物体放回边界中再改变它们的位置。  
不同：
重置时，一般会统一将所有物体移回同一位置。  
屏幕环绕时，遵循同一个物体的原则，从后面出去的物体转而出现在前面。  
所以，在屏幕环绕的方法中一般不会改变物体的速度。  

```  
 if (ship.x - ship.width / 2 > right) {
            ship.x = left - ship.width/2;
          } else if (ship.x + ship.width / 2 < left) {
            ship.x = right + ship.width / 2;
          }

          if (ship.y - ship.height / 2 > bottom) {
             ship.y = top - ship.height / 2
         } else if (ship.y < top - ship.height / 2) {
             ship.y = bottom + ship.height / 2;
         }
         
```   

### 反弹  
 思路：
 反弹方法需要检测物体何时离开屏幕，在刚离开时要保持它的位置不变只改变它的速度向量。  
 如果物体从屏幕的左边或右边越界，就对它的x轴速度向量取反。    
 
 反弹和其他计算不同，不能小球完全消失再反弹，所以要加上自身长度的一半  
 
 ```   
 if (ball.x + ball.radius > right) { ... }
 ```  
  
  在反弹操作的时候需要注意，一定要先将小球放回边界位置，如果不放回边界，下一帧，尽管物体已经开始反向运动，由于它可能还尚未回到边界内，物体的速度反向再次取反，物体范围会掉头钻进墙里。然后就会出现物体陷在墙里，或进或出徘徊震荡的效果。   
  
  解决震荡或陷进墙里思路：  
    
 - 检查物体是否越过人意边界  
 - 如果发生越界，立即将物体置回边界  
 - 反转物体的速度向量方向   
 
 ```  
      if (ball.x + ball.radius > right) {
      			//先放回边界
                ball.x = right - ball.radius;
                //再反向
                vx *= -1;
            } else if (ball.x - ball.radius < left) {
                ball.x = bottom - ball.radius;
                vy *= -1;
            }
            if (ball.y + ball.radius > bottom) {
                ball.y = bottom - ball.radius
                vy *= -1;
            } else if (ball.y - ball.radius < top) {
                ball.y = top + ball.radius;
                vy *= -1
            }
 ```  
 
 这个在慕课网我记得有一个满屏幕弹小球的课程，有兴趣可以看看。
 
 
 使用实例见code目录


 




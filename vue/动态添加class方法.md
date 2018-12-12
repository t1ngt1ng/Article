#vue中动态添加class方法  

定义要添加的两个类名 

``` 
 data() {
            return {
              a1: 'test-1',
              a2: 'test-2'
                }
```  
##数组方式  

``` 
<div :class="[a1,a2]"></div>

or 
 data() {
       return { 
          classArr: classArr: ['test-1','test-2']
            }
 <div :class="classArr"></div>

```  

##对象方式 
``` 
  data() {
       return { 
          a3:true
          }
          
<div :class="{'test-1':a3}">obj class</div>

or  

 data() {
       return { 
          classArr:  obj: {'test-1': true}
            }  
    <div :class="obj">class obj1</div>         
 
```

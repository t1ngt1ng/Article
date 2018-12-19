# ES6种的Map类型  

## Map类型什么样  

map是键值对的集合，但是“键”不仅限于字符串，可以包含各种类型，包括对象。是一种更完善的 Hash 结构实现。  key是不重复的，如果出现重复插入的key，后面会覆盖前面的。  

以前我要用到键值对形式，会使用Object类型，如下：  

```  
  {
        c1: '产品定义',
        c2: '试条问题',
        c3: 'App Bug',
        c4: '适配性',
        c5: '电池'
    }
```

如果打印map得到的结果显示如下：  

``` 
Map {
  'c1' => '产品定义',
  'c2' => '试条问题',
  'c3' => 'App Bug',
  'c4' => '适配性',
  'c5' => '电池' }
```  


## 创建Map类型 
直接使用new Map()就可以创建Map类型的变量。  
使用set方法可以添加数据。  

const map = new Map()
.set(1, 'a').set(2, 'b').set(3, 'c');

可以将多种形式的数据转换成map,比如上面的object、数组。

插入数组：  

 ``` 
 let array = [
    ['c1', '产品定义'],
    ['c2', '试条问题'],
    ['c3', 'App Bug'],
    ['c4', '适配性'],
    ['c5', '电池']
]

let classifyMap = new Map(array)
console.log(classifyMap1);

 ```  
 输出就是上面的map示例。  
 上面相当于执行了如下代码 ：
  
 ```   
 array.forEach(([key, value]) => classifyMap1.set(key, value))
 ```  
 
map接受的参数不仅仅是数组，任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构都可以当作Map构造函数的参数。这就是说，Set和Map都可以用来生成新的 Map。  

## Map类型的方法
### map.size
返回 Map 结构的成员总数.  
### map.set(key, value)
set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。  
###  map.get('c2')
get方法读取key对应的键值，如果找不到key，返回undefined。  
### map.has('c4')
has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。  
### map.delete('c1')
delete方法删除某个键，返回true。如果删除失败，返回false。  
### map.clear() 
clear方法清除所有成员，没有返回值。  

## 遍历Map的方法  
keys()：返回键名的遍历器。  
values()：返回键值的遍历器。  
entries()：返回所有成员的遍历器。  
forEach()：遍历 Map 的所有成员。  

```  
let arr = [['key1', 'value1'], ['key2', 'value2']];
let maps = new Map(arr);

for (let [key, value] of maps) {
    console.log(key, value)
}

key1 value1
key2 value2


for (let [key, value] of maps.entries()) {
    console.log(key, value)
}

key1 value1
key2 value2


for (let item of maps.entries()) {
    console.log(item[0], item[1]);
}

key1 value1
key2 value2


for (let value of maps.values()) {
    console.log(value)
}

value1
value2


for (let key of maps.keys()) {
    console.log(key)
}

key1
key2


maps.forEach((value,key,map)=> {
    console.log(value,key,map)
})

value1 key1 Map { 'key1' => 'value1', 'key2' => 'value2' }
value2 key2 Map { 'key1' => 'value1', 'key2' => 'value2' }
```  

## Map转换为Array
上面例子中Map 结构的默认遍历器接口（Symbol.iterator属性），就是entries方法 

```
map[Symbol.iterator] === map.entries
// true
```  
Map 结构转为数组结构，比较快速的方法是使用扩展运算符（...）。

``` 
[...maps]  

//[ [ 'key1', 'value1' ], [ 'key2', 'value2' ] ]

```  
用此种方法filter和map也可以使用

```
const map = new Map()
.set(1, 'a').set(2, 'b').set(3, 'c');

const map1 = 
new Map([...map].filter(([key, value]) => key < 3));
 
```  
##数据类型转换  
### Object转Map  

``` 
 let maps=new Map()
 let obj={
        c1: '产品定义',
        c2: '试条问题',
        c3: 'App Bug',
        c4: '适配性',
        c5: '电池'
    }
  
    for (let key in obj){
    	maps.set(key,obj[key])
    }
     
```   
  
### Array转Map

 ``` 
 let array = [
    ['c1', '产品定义'],
    ['c2', '试条问题'],
    ['c3', 'App Bug'],
    ['c4', '适配性'],
    ['c5', '电池']
]

let maps = new Map(array);

 ```
   
### Map转Array
```
[...maps]  
```  
### Map转Object 
``` 
for (let [key, value] of maps) {
    obj[key]=value
}
``` 
 






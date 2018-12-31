#javascritp中时间戳  
今天听说一个app端前后台时间戳不同步的问题，前端传的10位，后端做旧帐户迁移的时候，都改成了13位，造成了app无法更新的问题...


##js中几种获取时间戳的方式  

```
new Date().getTime()
//1546229104948 

new Date()).valueOf()
//1546229104954  

Date.parse(new Date())
//1546229104000

```  

Date.parse(new Date())只精确到秒，后三位用000补全，其他都是精确到毫秒

如果想获取10位的时间戳，比较建议用Date.parse处理，不存在除以1000之后的取舍问题。

```
function timestamp10(time){
	return time.toString().substring(0,10)
}

timestamp10(timeStamp)
//1546229104
```

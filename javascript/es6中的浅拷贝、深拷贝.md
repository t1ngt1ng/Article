#es6中的深拷贝、浅拷贝  

之所以写成es6中，因为我要写的方法是es6中的assign，之前版本中如何拷贝Object类型数据并不涉及。


# =号复制Object

```
let company={
	name:'xx',
	place:'china'
}

let movie={
	name:'abc',
	company:company
}


let movie1=movie;

movie1.name='666';
console.log(movie.name)//666

```  
如果我们只使用=号复制对象的话，如果修改movie1的name，此时打印movie.name的值也是被修改的

因此就有了深浅拷贝

#浅拷贝  

es6中浅拷贝的方法是Object.assign(target, source)   
基本用法
Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）  

```
function clone(orgin){
	return Object.assign({},orgin)
}   
let movie2=clone(movie);

movie2.name='ddd'
console.log(movie.name)//xx
```  
使用浅拷贝就可解决上面一块被修改的问题   

注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。  
Object.assign拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）。  
（其他关于assign的具体内容可以看es6相关的具体文档）

之所以叫浅拷贝，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。   

```
movie1.company.name='222';
console.log(movie.company.name)

```  

这就是上面说的，因为movie中的company有Ojbect，movie1如果修改company中的内容，原对象也会被修改。

此时就要使用深拷贝  


##深拷贝 
深拷贝还是和assign相关，只不过用到了另一方法Object.create(). 

Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的``` __proto__  ```。 
  
```
function deepClone(obj){
	let objProto=Object.getPrototypeOf(obj)
	return Object.assign(Object.create(objProto),obj)
}

```  

调用此方法创建的对象，再修改Object中的属性就不会将原有的内容修改了



 





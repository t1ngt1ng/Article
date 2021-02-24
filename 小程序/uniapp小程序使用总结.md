# uniapp小程序总结

## 1.跳转方式
### 1）页面跳转
```
this.$u.route(path);
```
### 2.标签页跳转
```
uni.switchTab({
		url: '/pages/work/index'
	});
```  

## 2.存储storage
```
uni.setStorageSync(key, value);
uni.getStorageSync(key)
```

## 3.textarea穿透
部分机型会出现，解决方法：wath中检测弹窗属性的值，改变textarea的展示状态

```
watch: {
			show_year_select(val) {
				this.show_textarea = !val
			},
			show_other_hospital(val) {
				this.show_textarea = !val
			}
		}
```

## 4.本页内分页
每次分页回到当前页首
```
this.step_index++;
uni.pageScrollTo({
	scrollTop: 0,
	duration: 0
});
```

## 5.调起打电话

```
uni.makePhoneCall({
phoneNumber: '400-898-1618'
});
```

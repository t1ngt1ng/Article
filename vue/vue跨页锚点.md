#vue跨页锚点

点击页面链接，跳转到另一个页面的指定位置
使用toElement.scrollIntoView()方法跳转到该位置

前页传入锚点id

```
接收页
 created(){
//创建时执行跳转锚点位置
            this.$nextTick(() => {this.getlocal()})
        },
        getlocal(){
                //找到锚点id
                let selectId = localStorage.getItem("toId");
                let toElement = document.getElementById(selectId);
                //如果对应id存在，就跳转
                if(selectId){
                    toElement.scrollIntoView()
                }
            }
```
[参考地址]( https://blog.csdn.net/dianwan5205/article/details/102082523)

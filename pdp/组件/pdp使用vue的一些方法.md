# 监听器

```javaScript
 this.$watch("show1",function(nv,ov){
   var ocvm = mainWindow.pdp.$refs.mainIframe.contentWindow.pdp.$refs.ocIframe.contentWindow.pdp;
   if(nv === true){
     ocvm.tableBtnClass = "table-disabled-btn";
   }else{
     ocvm.tableBtnClass = "";
   }
 })

```
show1 是在consts中定义的变量，其他和使用wacth监听器一样，参考  
["watch监听器"](https://cn.vuejs.org/v2/api/#watch)

# refs
获取
```javaScript
<a ref='ctbox1'  ></a>

 var row = this.$refs.ctbox1.;
```

使用它需要在组件加一个ref属性 ，通过$refs来获取

# nextTick 
```javascript
              this.$nextTick(function(){
                this.checkAudioState();
              });
```
使用情况：
1. 在created中操作DOM必须放在 nextTick 中
2. 更改数据后操作新视图 放于 nextTick中

>$nextTick()会返回一个Promise对象 可以用async /await。
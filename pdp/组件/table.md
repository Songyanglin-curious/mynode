# table相关组件

## table 报 source 为 null
**********************
原因在于： table的数据源需要是二维数组，直接从数据源取出来的数据可以直接使用  
table数据源 下划线 _0 _1等获取该列数据作为表格数据  
将二维数组赋给声明的变量需要给变量声明加属性 type="2d" length="6"这样才可以使用

## table拿不到需要的数据
**********
检查声明变量的 type是否为2d  length 是否足够，不够是获取不到那一列数据

## table 某一列根据条件隐藏
********************
使用show进行隐藏

```
 <icol id="icol57" desc="9" show="table_period_label">
```

## table包含select组件拿不到数据
*********
由于select是table的子组件无法直接拿到数据  
可以在select添加on-change事件 获取select所在行号和列号 直接更改数据源
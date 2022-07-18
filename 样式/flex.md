# flex
## flex容器属性
* flex-direction
* flex-wrap
* flex-flow flex-direction || flex-wrap 缩写属性
* justify-content
* align-items 单行
* align-content 多行
## 项目属性
* order 排序越小越在前
* flex-grow  
* flex-shrink 放大，缩小都是0不缩不放 1生效
* flex-basis 给固定像素或者相对父元素百分比或者其他长度 不给默认项目本来大小
* flex 'flex-grow'> <'flex-shrink'>? || <'flex-basis'>   快捷属性 1，0 auto (1 1 auto) 和 none (0 0 auto)。
* align-self 单独排列不同


### 问题flex两行垂直，要居于左侧
>直接更改方向换行，会使内容尽可能多的占据空间，即水平间距大  
> ### 解决方法
>align-content 属性，更改多行对其方式  
flex-start：与交叉轴的起点对齐，添加后解决
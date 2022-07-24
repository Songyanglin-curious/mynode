# 单个按钮
```xml
<button>
	<a type="button" value="新增" class="pdpBtnDefault btbl"></a>
	<e>
		<onclick>
			<list type="script" value="this.refreshData(); "></list>
		</onclick>
	</e>
</button>
```
> 单个按钮标签为button   
> 属性设置为 type="button" 默认样式为 pdpBtnDefault  
> e标签添加事件，一般为点击时间
> 可以在事件标签，添加vue修饰符 如`<onclick.stop>`  
> 生成的是 input标签，类型为button 直接绑定的事件是由 button标签的id和事件名拼起来的 如 button56_onclick ，在这个button56_onclick方法里再通过一系列函数调用我们自身在created 和 mounted 里面编写的函数，即在`<script event="created">`或者`<script event=" mounted">`标签里写的函数

# 按钮组
使用按钮组组件
```xml
<buttongroup desc="按钮组" id="buttongroupSta" from="choosebtnSta" source="btnGroupSta">
  <a class="btngap"></a>
  <e>
    <onclick args="$event">
      <list type="script" value="this.btnSta(this.params.row,$event)"/>
    </onclick>
  </e>
</buttongroup>
```

传递参数 在事件标签上 使用 args属性 同时在list value的处理函数中接收参数
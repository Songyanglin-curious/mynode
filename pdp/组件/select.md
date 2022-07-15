## select对话框打开后多选只显示一个，多余不显示
 ```
 <iselect desc="值班负责人：" id="zbfzr" source="UserAllZB">
                                                                             <sa  filterable="true"></sa>
 </iselect>
 ```

 用 id绑定要赋值的变量，能够实现多选
 用from 绑定要多选的变量不能实现 打开后显示多选

 两者的区别在于  
  用id绑定变量 v-model你指定的变量  
 from 绑定是通过自动添加一个id 并且v-model 自动添加的id 将你from的变量通过Ysh.Vue.m()提取，绑定到自动添加的id 上
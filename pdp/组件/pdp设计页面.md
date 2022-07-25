<!--
 * @Author: songyanglin 1503464667@qq.com
 * @Date: 2022-07-25 12:46:33
 * @LastEditors: songyanglin 1503464667@qq.com
 * @LastEditTime: 2022-07-25 17:30:41
 * @FilePath: \mynode\pdp\组件\pdp设计页面.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# 基础模板

```javaScript
<?xml version="1.0" encoding="utf-8"?>
<root maxid="11" desc="空白页">
    //引入外部js，cs 引入的是公共的js，cs ,多数在coon中
  <includes>
    <![CDATA[
        /conn/pdpjs/ctrl/itabs.js;
      ]]>
  </includes>
    // 从外部获取参数
    <inits>
        <init id="decid" desc="设备Id" type="request" arg="id"></init>
    </inits>
    //声明变量
    <consts>
        <const id="data_voltage_class" desc="电压等级数据" type="2d" length="2"></const>
        <const id="sel_voltage_class" desc="选中的电压等级数据" arg=""></const>
    </consts>
    //数据源
    <datasources>
        <datasource id="ds_val_class" desc="电压等级" db="ConnMain" type="xml" node="sc/stationMonitor:GetVolClass">
            <cols>
                <col id="sel_valu" desc="选择值" name="value"></col>
                <col id="label_value" desc="展示值" name="label"></col>
            </cols>
        </datasource>
    </datasources>
    //dom设计节点，div,各种组件，布局
    <ctrls>
        <div id="div0" desc="根容器">
            <a s_width="100%"></a>
            
            
        </div>
    </ctrls>
    <css>
        <![CDATA[ 
    .station-select-wrap{
        margin-left:40px;
        display:flex;
        align-items: center;
    }
    ]]>
    </css>
    <scripts>
        <script event="created">
            <list type="script">
                <value>
                    <![CDATA[
            
          ]]>
                </value>
            </list>
        </script>
        <script event="mounted">
            //调用获取 数据源的函数，在调用一次这个后，可以使用，select_ds_val_class()函数调用获得数据
            <list type="select" ds="ds_val_class"></list>
            <list type="select" ds="ds_substation"></list>
            <list type="select" ds="dsAlertInfo"></list>
            <list type="script">
                <value>
                    <![CDATA[
                   
          ]]>
                </value>
            </list>
        </script>
    </scripts>
</root>

```
________________________________
## inits从外部获取数据
```javaScript
 <init id="decid" desc="设备Id" type="request" arg="id"></init>
 <init id="userid" desc="新参数" type="session" arg="usernum"></init>
```
参数含义：
* id: 在这个页面的这个参数的变量名
* desc: 描述不会影响代码功能
* type: request 跳转这一页面带的 ?id="" ; session 从缓存中获取数据 
* 传入的变量名

----------------------------
## const 声明变量
```javaScript
<const id="data_voltage_class" desc="电压等级数据" type="2d" length="2"></const>
```
参数含义：
* id：变量名
* desc: 变量含义描述
* type: 变量类型 常用  bool,string,number,2d(二维数组)
* length: 和type:2d搭配使用，声明二维数组的长度，在将这个变量赋给表格做数据源的时候必须声明并且长度不能少于要展示的列，少于会使表格的数据少展示
________________________
## database节点获取数据源
```javaScript
<datasource id="ds_val_class" desc="电压等级" db="ConnMain" type="xml" node="sc/stationMonitor:GetVolClass">
    <cols>
        <col id="sel_valu" desc="选择值" name="value"></col>
        <col id="label_value" desc="展示值" name="label"></col>
    </cols>
</datasource>
```
参数含义：
* id: 执行sql后获取的数据
* desc:描述性的
* db：数据库链接节点，在Config中设置
* type读取的文件类型，目前多为 xml
* node: sql读取节点， 是在 AppData 目录下获取sql语句，sc/ 第一个是在 AppData 下的文件夹,/ 后面的是xml文件名，：后面的是节点名,节点文件如下，{0}代表输入的参数，会进行一定的处理 加''，{{0}} 会原样放入，有利于拼接字符串，{0}或者{{0}} 放在等号左边，在变量为空时会变成null，放在右边就是''空字符串。  

cols节点下的col 是select获取变量的每一项  
配置属性：
*  id：是在table中赋给每一列 from的
* name: 是在table中 每一行点击后对应变量的名字

database在 create 或者mounted中调用  
`<list type="select" ds="ds_val_class">`
每个页面在调用一次这个后可以使用 this.select_ds_val_class()函数来重新调用 刷新数据  
第一次调用最好在mounted中，不然表格请求数据动画不一定出现。
______________________________________
## PDP的函数从数据源获取数据
### PDP.exec
exec可以执行多条sql全部成功返回结果，否则不成功，执行多条语句使用push将生成的每个{}推入数组，统一执行
```javaScript
                PDP.exec([{ type: 'select', db: 'ConnMain', sql: "sc/stationMonitor:GetFilterAlert", args: ['','','','刀闸']}],function(data){
                    if(!data.check('获取告警表格数据',true))return;
                    console.log("告警数据表：",data)
                    this.alert_table =  data.value
                    console.log(this.alert_table)
                });
```
1、 使用，PDP.exec函数获取数据
配置项如下：
* type: select , modify ; select,类型是用来获取数据的；modify 是对数据进行 增删改的
* db：数据库链接节点，在Config中设置
* sql: 是在 AppData 目录下获取sql语句，sc/ 第一个是在 AppData 下的文件夹,/ 后面的是xml文件名，：后面的是节点名,节点文件如下，{0}代表输入的参数，会进行一定的处理 加''，{{0}} 会原样放入，有利于拼接字符串，{0}或者{{0}} 放在等号左边，在变量为空时会变成null，放在右边就是''空字符串。
* args: [] 以数组的形式传递参数 ，第0个对应{0} ，第1个对应 {1}
* 回调函数 回调函数的this指向调用它的函数想要在回调函数调用定义在外部的值需要在外部用变量储存this  常用 vm  _this

sql 调用的文件节点样式如下：
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ROOT>
  <!-- 根据筛选获得告警数据 -->
  <GetFilterAlert>
    <![CDATA[ 
    SELECT a.alert_time ,a.msg_type,  b.col3 AS device ,b. col0 AS substation,a.msg_content FROM YS_DB_HB.ysh_alert_msg  a LEFT JOIN YS_DB_HB.gudtest  b ON a.device_id = b.id 
    WHERE b.col3 LIKE '%{0}%' AND  (a.device_id = {1} OR '' = {1}) AND (a.msg_type = {2} OR ''= {2} ) AND b.col3 LIKE '%{3}%'
    ]]>
  </GetFilterAlert>
* 返回的是一个对象，值value 是一个二维数组[[],[]],每一个子数组都是对应的一条sql的结果，读取一条结果时要注意拿的值是否正确
</ROOT>
 ```
 * 回调函数，回调函数获得sql的执行结果，在其内部直接通过this是拿不到声明的变量，所以需要在外部声明一个变量，储存this 通常用 vm 或者 _this。check来检测结果成功还是失败

### PDP.read
只能读一个
```javaScript
 PDP.read('ConnMain', "TJSmartOwn:GetDeviceInfoById", [vm.init1],function(data){})
```
* 第一个参数 ConnMain 数据库连接节点 
* 第二个参数调用 编写的查询sql语句
* 第三个参数 [] 输入参数
* 第四个参数 回调函数



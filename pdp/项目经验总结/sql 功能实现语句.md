<!--
 * @Author: songyanglin 1503464667@qq.com
 * @Date: 2022-07-25 16:21:36
 * @LastEditors: songyanglin 1503464667@qq.com
 * @LastEditTime: 2022-07-25 16:36:33
 * @FilePath: \mynode\pdp\项目经验总结\sql 功能实现语句.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# 项目功能使用的sql记录

## 实现搜索过程，变量为空就忽略，不为空就调用
>适用于有很多查询条件，有的有有的没有的情况
["编写改语句的参考文章"](https://blog.csdn.net/qq_39651858/article/details/81738199)
```sql
    SELECT a.alert_time ,a.msg_type,  b.col3 AS device ,b. col0 AS substation,a.msg_content 
    FROM YS_DB_HB.ysh_alert_msg  a LEFT JOIN YS_DB_HB.gudtest  b ON a.device_id = b.id 
    WHERE b.col3 LIKE '%{0}%' AND  (b.col0 = {1} OR '' = {1}) AND (a.msg_type = {2} OR ''= {2} ) AND b.col3 LIKE '%{3}%' AND b.col3 LIKE '%{4}%'
```

>思路为  
核心思路为条件为空时忽略这一查询条件  
select * from 表 where (字段=条件 or 条件=' ')  
当条件为空时会执行or后面的 语句 '' = '' 为真但是不起任何作用，当条件为真，执行or前面的将这一查询条件又增加上了  
模糊查询 为空时相当于忽略掉 它

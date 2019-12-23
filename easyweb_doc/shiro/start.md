## 1.1.导入项目      :id=import

1. 导入sql文件到MySql数据库，sql位于项目根目录
2. 使用IDEA打开项目，检查数据库连接密码是否正确
3. 运行src目录下的EasyWebApplication.java运行项目

![运行教程](https://s2.ax1x.com/2019/08/31/mzKJlF.png)


## 1.2.项目结构      :id=structure

```
|-▶src
|   |-▶main
|   |   |-▶java
|   |   |   |-▶common
|   |   |   |   |-▶config                                // SpringBoot配置类
|   |   |   |   |-▶exception                             // 全局异常处理、自定义异常
|   |   |   |   |-▶shiro                                  // shiro配置
|   |   |   |   |-▶utils                                  // 工具类
|   |   |   |   |-JsonResult.java                         // 返回结果封装
|   |   |   |   |-PageResult.java                         // 数据表格分页返回结果封装
|   |   |   |   |-PageParam.java                          // 排序、分页、搜索等参数接收封装
|   |   |   |   |-BaseController.java                     // Controller基类
|   |   |   |-▶system                                     // 系统管理模块
|   |   |   |   |-▶controller
|   |   |   |   |-▶service
|   |   |   |   |-▶dao
|   |   |   |   |-▶model
|   |   |   |-▶api                                        // 接口模块
|   |   |   |   |-▶controller
|   |   |   |-EasyWebApplication.java                     // 启动类
|   |   |-▶resources
|   |   |   |-▶mapper                                     // mapper文件
|   |   |   |-▶static                                     // 静态资源文件
|   |   |   |-▶templates                                  // 页面模板文件
|   |   |   |-application.properties                      // SpringBoot配置文件
|   |   |   |-application-dev.properties                  // 开发环境配置
|   |   |   |-application-prod.properties                 // 生产环境配置
|   |   |   |-application-test.properties                 // 测试环境配置
|   |-▶test
|   |   |-java
|   |   |   |-CodeGenerator.java                          // 代码生成器
```


## 1.3.框架说明      :id=frame

框架 | 说明
:--- | :---
核心框架 | Spring、SpringBoot、SpringMVC
持久层 | MyBatis、[MyBatisPlus](https://mp.baomidou.com/guide)
连接池 | Druid
权限框架 | Shiro
模板引擎 | [Beetl](http://ibeetl.com/guide/)
图形验证码 | [EasyCaptcha](https://gitee.com/whvse/EasyCaptcha)
API权限 | [JwtPermission](https://gitee.com/whvse/JwtPermission)

> JwtPermission 和 EasyCaptcha 是easyweb原创的开源项目，欢迎 [star](https://gitee.com/whvse) 支持。


## 1.4.数据库表介绍      :id=database

用户表(sys_user)：

字段 | 类型 | 说明
:--- | :--- | :---
user_id | int | 用户id
username | varchar | 账号
password | varchar | 密码
nick_name | varchar | 昵称
avatar | varchar | 头像
sex | varchar | 性别
phone | varchar | 手机号
email | varchar | 邮箱
email_verified | int | 邮箱是否验证,0未验证,1已验证
true_name | varchar | 真实姓名
id_card | varchar | 身份证号
birthday | date | 出生日期
department_id | int | 部门id
state | int | 状态，0正常，1冻结
create_time | timestamp | 注册时间
update_time | timestamp | 修改时间

角色表(sys_role)：

字段 | 类型 | 说明
:--- | :--- | :---
role_id | int | 角色id
role_name | varchar | 角色名称
comments | varchar | 备注
create_time | timestamp | 创建时间
update_time | timestamp | 修改时间

用户角色关联表(sys_user_role)：

字段 | 类型 | 说明
:--- | :--- | :---
id | int | 主键
user_id | int | 用户id
role_id | int | 角色id
create_time | timestamp | 创建时间

权限(菜单)表(sys_authorities)：

字段 | 类型 | 说明
:--- | :--- | :---
authority_id | int | 权限id
authority_name | varchar | 权限名称
authority | varchar | 授权标识
menu_url | varchar | 菜单url
parent_id | int | 父id,-1表示无父级
is_menu | int | 权限类型,0菜单,1按钮
order_number | int | 排序号
menu_icon | varchar | 菜单图标
create_time | timestamp | 创建时间
update_time | timestamp | 修改时间

角色权限关联表(sys_role_authorities)：

字段 | 类型 | 说明
:--- | :--- | :---
id | int | 主键
role_id | int | 角色id
authority_id | int | 权限id
create_time | timestamp | 创建时间

登录日志表(sys_login_record)：

字段 | 类型 | 说明
:--- | :--- | :---
id | int | 主键
user_id | int | 用户id
os_name | varchar | 操作系统
device | varchar | 设备名
browser_type | varchar | 浏览器类型
ip_address | varchar | ip地址
create_time | timestamp | 登录时间

数据字典表(sys_dictionary)：

字段 | 类型 | 说明
:--- | :--- | :---
dict_code | int | 字典主键
dict_name | varchar | 字典名称
description | varchar | 描述
create_time | timestamp | 创建时间
update_time | timestamp | 修改时间

数据字典项表(sys_dictionary_data)：

字段 | 类型 | 说明
:--- | :--- | :---
dictdata_code | int | 字典项主键
dict_code | int | 字典主键
dictdata_name | varchar | 字典项值
sort_number | int | 排序
description | varchar | 描述
create_time | timestamp | 创建时间
update_time | timestamp | 修改时间

> 如果需要使用逻辑删除，建议使用MyBatisPlus来统一控制，[查看说明](https://mp.baomidou.com/guide/logic-delete.html)

## 4.1.基类BaseController      :id=ctrl

方法 | 参数 | 说明
:--- | :--- | :---
getLoginUser() | 无 | 获取当前登录的User
getLoginUserId() | 无 | 获取当前登录的userId

使用时Controller继承BaseController即可。


## 4.2.返回结果JsonResult      :id=jr
使用方法：
```java
@ResponseBody
@RequestMapping("/test")
public JsonResult test() {
    return JsonResult.ok();
}
```
这样返回给前端的json为：{code: 200, msg: "操作成功"}

```
JsonResult.ok(); 
JsonResult.ok("添加成功");           // 重写msg
JsonResult.ok(200, "添加成功");      // 重写code和msg

JsonResult.error();                  // {code: 500, msg: "操作失败"}
JsonResult.error("添加失败");
JsonResult.error(500, "添加失败");

// put其他数据，支持链式写法
JsonResult.ok().put("data", object);    // {code: 200, msg: "操作成功", data: {} }
JsonResult.ok().put("data1", object).put("data2", object);
```

> 建议返回json数据的接口都返回JsonResult对象，因为它包含了code、msg字段便于前端处理状态


## 4.3.分页结果PageResult      :id=pr

使用方法：
```java
@ResponseBody
@RequestMapping("/test")
public PageResult<User> test() {
    return new PageResult<>(userList, total);
}
```
这样返回给前端的json为：{code: 0, msg: " ", count: 100, data: [] }，count是总数量，data是当前页数据。

```
new PageResult<>(userList);              // 这样写count为userList的大小
new PageResult<>(userList, total);

PageResult pageResult = new PageResult<>(userList, total);
pageResult.setCode(200);    // 设置code
pageResult.setMsg("查询成功");    // 设置msg
```

数据表格都是需要后端返回code、count、data等几个关键字段的，PageResult就是针对返回给数据表格数据的封装。

> PageResult的code默认为0，主要是为了适配Layui的数据表格，Layui的数据表格认为code为0表示成功


## 4.4.参数接收PageParam      :id=pp
PageParam继承至MyBatisPlus的Page，用于自动接收前端传递的排序、分页、搜索等参数，可以自动完成排序、分页的功能，使用方法：

```java
@ResponseBody
@RequestMapping("/users")
public PageResult<User> users(HttpServletRequest request) {
    PageParam pageParam = new PageParam(request);
    List<User> userList = userService.page(pageParam, pageParam.getWrapper()).getRecords();
    pageParam.getTotal();  // 获取总数据条数
}
```

这样就实现了排序、分页、模糊搜索功能，前端只要传递规定的参数即可：

参数 | 结果
:--- | :--- 
users?page=1&limit=10 | 查询第一页，每页10条
users?page=2&limit=20 | 查询第二页，每页20条
users?sort=username&order=asc | 根据username字段升序
users?sort=username&order=desc | 根据username字段降序
users?username=admin | 查询username为admin的用户
users?username=admin&sex=女 | 查询username为admin并且sex为女的用户

**全部方法：**

方法 | 参数 | 说明
:--- | :--- | :---
Map<String, Object> getPageData() | 无 | 获取除分页、排序外的其他参数
QueryWrapper getWrapper() | 无 | 自动构建查询条件QueryWrapper，全部是like
setDefaultOrder(String[] ascs, String[] descs) | 排序字段 | 设置默认排序方式
addOrderAsc(String... sort) | 排序字段 | 在前端传递的排序参数的基础上增加升序字段
addOrderDesc(String... sort) | 排序字段 | 在前端传递的排序参数的基础上增加降序字段
put(String key, Object value) | key,value | 往PageData里加参数
Object get(String key) | key | 从PageData中取参数
getString(String key) | key | 从PageData中取String参数
getInt(String key) | key | 从PageData中取Int参数
getFloat(String key) | key | 从PageData中取Float参数
getDouble(String key) | key | 从PageData中取Double参数
getBoolean(String key) | key | 从PageData中取Boolean参数

支持链式写法：
```java
@Controller
public class TestController {
    @RequestMapping("/users")
    public PageResult<User> users(HttpServletRequest request) {
        PageParam pageParam = new PageParam(request);
        pageParam.put("username", "admin").put("sex", "男").addOrderAsc("create_time");
    }
}
```

> setDefaultOrder()设置默认排序方式这个方法表示前端没有传递排序参数的时候才有效，前端传递了以前端传递的为主

!> 排序、模糊搜索会把前端传递的字段驼峰转为下划线的形式


## 4.5.mapper中使用PageParam     :id=ppm
```java
public interface UserMapper extends BaseMapper<User> {

    List<User> listFull(@Param("page") PageParam page);  // 这里一定要为page，不然无法分页
}
```
上面这样写就可以自动完成分页和排序，搜索功能在xml中这样写即可：
```xml
<select id="listFull" resultType="com.wf.ew.system.model.User">
    SELECT a.*, b.role_name 
    FROM sys_user a
    LEFT JOIN sys_role b ON a.role_id=b.role_id
    <where>
        <if test="page.pageData.userName!=null">
            and a.user_name like '%${page.pageData.userName}%'
        </if>
        <if test="page.pageData.roleName!=null">
            and b.role_name like '%${page.pageData.roleName}%'
        </if>
    </where>
</select>
```

service写法，可以直接使用pageParam.getTotal()查询总数量：
```java
@Service
public class UserServiceImpl {
    @Override
    public PageResult<User> listUser(PageParam pageParam) {
        List<User> userList = baseMapper.listFull(pageParam);
        return new PageResult<>(userList, pageParam.getTotal());
    }
}
```

!> **注意：** mapper里面一定要加@Param("page")，不然无法分页，是page不是pageParam


## 4.6.PageParam不想分页     :id=ppp

例如做导出的功能时，只想有搜索的功能，不要分页的功能，可以这样做：

mapper里面不要写PageParam，用Map：
```java
public interface UserMapper extends BaseMapper<User> {

    List<User> listAll(@Param("pageData") Map pageData);
}
```

```xml
<select id="listAll" resultType="com.wf.ew.system.model.User">
    SELECT a.*, b.role_name 
    FROM sys_user a
    LEFT JOIN sys_role b ON a.role_id=b.role_id
    <where>
        <if test="pageData.userName!=null">
            and a.user_name like '%${pageData.userName}%'
        </if>
        <if test="pageData.roleName!=null">
            and b.role_name like '%${pageData.roleName}%'
        </if>
    </where>
</select>
```

service里面用pageParam.getPageData()：
```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    @Override
    public PageResult<User> listAll(PageParam pageParam) {
        return new PageResult<>(baseMapper.listAll(pageParam.getPageData()));
    }
}
```

> 可以参考用户管理的导出，可以导出全部，也可以搜索后再导出

## 2.1.单表CRUD    :id=d1
像角色role这样位于最顶层、不需要关联查询的表，不用写sql就可以实现CRUD：

```java
@Api(value = "角色管理", tags = "role")
@RestController
@RequestMapping("/role")
public class RoleController {
    @Autowired
    private RoleService roleService;

    @ApiOperation(value = "分页查询、模糊搜索角色")
    @GetMapping()
    public PageResult<Role> list(HttpServletRequest request) {
        PageParam pageParam = new PageParam(request);
        return new PageResult<>(roleService.page(pageParam, pageParam.getQueryWrapper()).getRecords(), pageParam.getTotal());
    }

    @ApiOperation(value = "添加角色")
    @PostMapping()
    public JsonResult add(Role role) {
        if (roleService.save(role)) {
            return JsonResult.ok("添加成功");
        }
        return JsonResult.error("添加失败");
    }

    @ApiOperation(value = "修改角色")
    @PutMapping()
    public JsonResult update(Role role) {
        if (roleService.updateById(role)) {
            return JsonResult.ok("修改成功");
        }
        return JsonResult.error("修改失败");
    }

    @ApiOperation(value = "删除角色")
    @DeleteMapping("/{id}")
    public JsonResult delete(@PathVariable("id")Integer roleId) {
        if (roleService.removeById(roleId)) {
            return JsonResult.ok("删除成功");
        }
        return JsonResult.error("删除失败");
    }
}
```

> 只用写Controller就可以实现添加、修改、删除、查询(分页、排序、模糊搜索)，Service和Mapper都不用写，之所以这么方便，得益于PageParam和MyBatisPlus的结合。

页面的写法：
```html
<div class="layui-fluid">
    <div class="layui-card">
        <div class="layui-card-body">
            <div class="layui-form toolbar">
                <div class="layui-form-item">
                    <div class="layui-inline">
                        <label class="layui-form-label w-auto">角色名称：</label>
                        <div class="layui-input-inline mr0">
                            <input name="roleName" class="layui-input" type="text" placeholder="输入角色名称"/>
                        </div>
                    </div>
                    <div class="layui-inline">
                        <button class="layui-btn icon-btn" lay-filter="roleSearchSubmit" lay-submit>
                            <i class="layui-icon">&#xe615;</i>搜索
                        </button>
                        <button id="roleBtnAdd" class="layui-btn icon-btn"><i class="layui-icon">&#xe654;</i>添加</button>
                    </div>
                </div>
            </div>

            <table class="layui-table" id="roleTable" lay-filter="roleTable"></table>
        </div>
    </div>
</div>

<!-- 表格操作列 -->
<script type="text/html" id="roleTableBar">
    <a class="layui-btn layui-btn-primary layui-btn-xs" lay-event="edit">修改</a>
    <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
</script>
<!-- 表单弹窗 -->
<script type="text/html" id="modelRole">
    <form id="modelRoleForm" lay-filter="modelRoleForm" class="layui-form model-form">
        <input name="roleId" type="hidden"/>
        <div class="layui-form-item">
            <label class="layui-form-label">角色名</label>
            <div class="layui-input-block">
                <input name="roleName" placeholder="请输入角色名" type="text" class="layui-input" maxlength="20"
                       lay-verType="tips" lay-verify="required" required/>
            </div>
        </div>
        <div class="layui-form-item">
            <label class="layui-form-label">备注</label>
            <div class="layui-input-block">
                <textarea name="comments" placeholder="请输入备注" class="layui-textarea" maxlength="200"></textarea>
            </div>
        </div>
        <div class="layui-form-item text-right">
            <button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
            <button class="layui-btn" lay-filter="modelRoleSubmit" lay-submit>保存</button>
        </div>
    </form>
</script>

<!-- js部分 -->
<script>
    layui.use(['layer', 'form', 'table', 'util', 'admin', 'tableX', 'config'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;
        var table = layui.table;
        var util = layui.util;
        var admin = layui.admin;
        var tableX = layui.tableX;
        var config = layui.config;

        // 渲染表格
        var insTb = tableX.render({
            elem: '#roleTable',
            url: config.base_server + '/role',
            header: config.getAjaxHeaders(),
            page: true,
            cellMinWidth: 100,
            cols: [[
                {type: 'numbers'},
                {field: 'roleName', title: '角色名', sort: true},
                {field: 'comments', title: '备注', sort: true},
                {
                    field: 'createTime', sort: true, templet: function (d) {
                        return util.toDateString(d.createTime);
                    }, title: '创建时间'
                },
                {align: 'center', toolbar: '#tableBar', title: '操作', minWidth: 120}
            ]]
        });

        // 添加
        $('#roleBtnAdd').click(function () {
            showEditModel();
        });

        // 搜索
        form.on('submit(roleSearchSubmit)', function (data) {
            insTb.reload({where: data.field, page: {curr: 1}});
        });

        // 工具条点击事件
        table.on('tool(roleTable)', function (obj) {
            var data = obj.data;
            var layEvent = obj.event;
            if (layEvent === 'edit') { // 修改
                showEditModel(data);
            } else if (layEvent === 'del') { // 删除
                doDel(obj);
            }
        });

        // 显示编辑弹窗
        function showEditModel(mRole) {
            admin.open({
                type: 1,
                title: (mRole ? '修改' : '添加') + '角色',
                content: $('#modelRole').html(),
                success: function (layero, dIndex) {
                    form.val('modelRoleForm', mRole);  // 回显数据
                    // 表单提交事件
                    form.on('submit(modelRoleSubmit)', function (data) {
                        layer.load(2);
                        admin.req('role', data.field, function (res) {
                            layer.closeAll('loading');
                            if (res.code == 200) {
                                layer.close(dIndex);
                                layer.msg(res.msg, {icon: 1});
                                insTb.reload();
                            } else {
                                layer.msg(res.msg, {icon: 2});
                            }
                        }, mRole ? 'PUT' : 'POST');
                        return false;
                    });
                }
            });
        }

        // 删除
        function doDel(obj) {
            layer.confirm('确定要删除此角色吗？', {
                shade: .1,
                skin: 'layui-layer-admin'
            }, function (i) {
                layer.close(i);
                layer.load(2);
                admin.req('role/' + obj.data.roleId, {}, function (res) {
                    layer.closeAll('loading');
                    if (res.code == 200) {
                        layer.msg(res.msg, {icon: 1});
                        obj.del();
                    } else {
                        layer.msg(res.msg, {icon: 2});
                    }
                }, 'DELETE');
            });
        }

    });
</script>
```

> 页面虽然有这么长的代码，但是所有的页面都可以套用这个模板，只用修改url和字段名称即可，前端的方便也得益于tableX和table模块的封装，tableX扩展了数据表格table实现了排序自动传递参数。


## 2.2.多表CRUD    :id=d2
像用户user这样的表往往查询的时候需要关联其他表查询关联的属性，写法也很简单，添加、修改、删除跟单表是一样的，只有查询需要写sql：

```java
@Api(value = "用户管理", tags = "user")
@RestController
@RequestMapping("/user")
public class UserController {
    @Autowired
    private UserService userService;

    @ApiOperation(value = "分页查询、模糊搜索、排序用户")
    @GetMapping()
    public PageResult<User> list(HttpServletRequest request) {
        return userService.listFull(new PageParam(request));
    }
}
```

service的写法：

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    @Override
    public PageResult<User> listFull(PageParam pageParam) {
        return new PageResult<>(baseMapper.listFull(pageParam), pageParam.getTotal());
    }
}
```

mapper的写法：

```java
public interface UserMapper extends BaseMapper<User> {
    List<User> listFull(@Param("page") PageParam page);
}

```

xml的写法：

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

上面的例子就是User里面有roleId关联Role表，查询的时候关联查询出roleName字段，然后还需要把roleName字段添加到User对象中：

```java
@TableName("sys_user")
public class User implements Serializable {
    
    @TableId(value = "user_id", type = IdType.AUTO)
    private Integer userId;   // 用户id

    private String userName;   // 姓名

    @TableField(exist = false)
    private String roleName;   // 角色名称

}
```

!> mapper里面一定要加@Param("page")，注意是page不是pageParam，关联的字段要加@TableField(exist = false)注解

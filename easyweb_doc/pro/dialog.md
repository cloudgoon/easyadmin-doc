# 10.弹窗专题
&emsp;&emsp;Layui没有像Bootstrap那样的直接写在页面中的模态弹窗，Layui的弹窗是通过layer模块用js去弹出窗口，
这就意味着Layui的弹窗功能更丰富、操作更灵活，当然灵活也就意味着对于初学者来说使用更为复杂，下面介绍了实现表单弹窗的几种不同方式。

## 10.1.第一种 页面层弹窗 :id=type1
页面层弹窗就是弹窗页面和列表页面在一个文件中，弹窗类型type=1：
```html
<button id="btnAddUser" class="layui-btn">添加</button>
<table id="tableUser" lay-filter="tableUser"></table>

<!-- 表单弹窗 -->
<script type="text/html" id="modelUser">
    <form id="modelUserForm" lay-filter="modelUserForm" class="layui-form model-form">
        <input name="userId" type="hidden"/>
        <div class="layui-form-item">
            <label class="layui-form-label">用户名</label>
            <div class="layui-input-block">
                <input name="nickName" placeholder="请输入用户名" type="text" class="layui-input" maxlength="20"
                       lay-verType="tips" lay-verify="required" required/>
            </div>
        </div>
        <div class="layui-form-item">
            <label class="layui-form-label">性别</label>
            <div class="layui-input-block">
                <input type="radio" name="sex" value="男" title="男" checked/>
                <input type="radio" name="sex" value="女" title="女"/>
            </div>
        </div>
        <div class="layui-form-item text-right">
            <button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
            <button class="layui-btn" lay-filter="modelSubmitUser" lay-submit>保存</button>
        </div>
    </form>
</script>

<!-- 表格操作列 -->
<script type="text/html" id="tableBarUser">
    <a class="layui-btn layui-btn-primary layui-btn-xs" lay-event="edit">修改</a>
    <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
</script>

<!-- js部分 -->
<script>
    layui.use(['layer', 'form', 'table', 'admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;
        var table = layui.table;
        var admin = layui.admin;

        // 渲染表格
        var insTb = table.render({
            elem: '#tableUser',
            url: '../../json/user.json',
            cols: [[
                {type: 'numbers', title: '#'},
                {field: 'nickName', sort: true, title: '用户名'},
                {field: 'sex', sort: true, title: '性别'},
                {align: 'center', toolbar: '#tableBarUser', title: '操作', minWidth: 200}
            ]]
        });

        // 添加
        $('#btnAddUser').click(function () {
            showEditModel();
        });

        // 工具条点击事件
        table.on('tool(tableUser)', function (obj) {
            var data = obj.data;
            var layEvent = obj.event;
            if (layEvent === 'edit') { // 修改
                showEditModel(data);
            } else if (layEvent === 'del') { // 删除
                layer.msg('点击了删除', {icon: 2});
            }
        });

        // 显示表单弹窗
        function showEditModel(mUser) {
            admin.open({
                type: 1,
                title: (mUser ? '修改' : '添加') + '用户',
                content: $('#modelUser').html(),
                success: function (layero, dIndex) {
                    var url = mUser ? '/updateUser' : '/addUser';
                    // 回显数据
                    form.val('modelUserForm', mUser);
                    // 表单提交事件
                    form.on('submit(modelSubmitUser)', function (data) {
                        layer.load(2);
                        $.post(url, data.field, function (res) {
                            layer.closeAll('loading');
                            if (res.code == 200) {
                                layer.close(dIndex);
                                layer.msg(res.msg, {icon: 1});
                                insTb.reload();
                            } else {
                                layer.msg(res.msg, {icon: 2});
                            }
                        }, 'json');
                        return false;
                    });
                }
            });
        }

    });
</script>
```

&emsp;弹窗的参数`type: 1`表示弹窗类型是页面层类型而不是iframe层类型，`content: $("#modelUser").html()`是指定弹窗的内容，
表单使用`<script type="text/html">`而不是使用div，这样表单页面会默认隐藏，需要注意的是，操作表单的代码，
包括表单的事件监听都要写在弹窗的`success`回调里面，因为layer弹窗是把表单的html**复制一份**动态插入到页面上，而不是把那一段代码变成模态窗，
所以事件绑定必须写在success里面。

> 这种方式表单和表格在一个页面上面，数据传递、页面相互操作比较方便，不涉及到iframe父子页面传值的问题，推荐初学者使用这种方式

!> **切记：** 对于type=1的弹窗，所有操作弹窗内元素的代码都要放在弹窗的success里面。


<br/>


## 10.2.第二种 iframe层弹窗 :id=type2

使用iframe类型的弹窗可以让表单页面和表格页面分开，逻辑更清晰。

弹窗页面，userForm.html：
```html
<html>
<head>
    <link rel="stylesheet" href="../../assets/libs/layui/css/layui.css"/>
    <link rel="stylesheet" href="../../assets/module/admin.css"/>
</head>
<body>
<form id="modelUserForm" lay-filter="modelUserForm" class="layui-form model-form">
    <input name="userId" type="hidden"/>
    <div class="layui-form-item">
        <label class="layui-form-label">用户名</label>
        <div class="layui-input-block">
            <input name="nickName" placeholder="请输入用户名" type="text" class="layui-input" maxlength="20"
                   lay-verType="tips" lay-verify="required" required/>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">性别</label>
        <div class="layui-input-block">
            <input type="radio" name="sex" value="男" title="男" checked/>
            <input type="radio" name="sex" value="女" title="女"/>
        </div>
    </div>
    <div class="layui-form-item text-right">
        <button class="layui-btn layui-btn-primary" type="button" ew-event="closeDialog">取消</button>
        <button class="layui-btn" lay-filter="modelSubmitUser" lay-submit>保存</button>
    </div>
</form>

<!-- js部分 -->
<script type="text/javascript" src="../../assets/libs/layui/layui.js"></script>
<script type="text/javascript" src="../../assets/js/common.js"></script>
<script>
    layui.use(['layer', 'form', 'admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;
        var admin = layui.admin;

        var mUser = admin.getLayerData();  // 获取列表页面传递的数据
        // 回显数据
        form.val('modelUserForm', mUser);
        // 表单提交事件
        form.on('submit(modelSubmitUser)', function (data) {
            layer.load(2);
            var url = mUser ? '/updateUser' : '/addUser';
            $.post(url, data.field, function (res) {
                layer.closeAll('loading');
                if (res.code == 200) {
                    layer.msg(res.msg, {icon: 1});
                    admin.putLayerData('formOk', true);  // 设置操作成功的标识
                    admin.closeThisDialog();  // 关闭当前iframe弹窗
                } else {
                    layer.msg(res.msg, {icon: 2});
                }
            }, 'json');
            return false;
        });
        
    });
</script>
</body>
</html>
```

表格页面，list.html：
```html
<button id="btnAddUser" class="layui-btn">添加</button>
<table id="tableUser" lay-filter="tableUser"></table>

<!-- 表格操作列 -->
<script type="text/html" id="tableBarUser">
    <a class="layui-btn layui-btn-primary layui-btn-xs" lay-event="edit">修改</a>
    <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
</script>

<!-- js部分 -->
<script>
    layui.use(['layer', 'table', 'admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var table = layui.table;
        var admin = layui.admin;

        // 渲染表格
        var insTb = table.render({
            elem: '#tableUser',
            url: '../../json/user.json',
            cols: [[
                {type: 'numbers', title: '#'},
                {field: 'nickName', sort: true, title: '用户名'},
                {field: 'sex', sort: true, title: '性别'},
                {align: 'center', toolbar: '#tableBarUser', title: '操作', minWidth: 200}
            ]]
        });

        // 添加
        $('#btnAddUser').click(function () {
            showEditModel();
        });

        // 工具条点击事件
        table.on('tool(tableUser)', function (obj) {
            var data = obj.data;
            var layEvent = obj.event;
            if (layEvent === 'edit') { // 修改
                showEditModel(data);
            } else if (layEvent === 'del') { // 删除
                layer.msg('点击了删除', {icon: 2});
            }
        });

        // 显示表单弹窗
        function showEditModel(mUser) {
            var layIndex = admin.open({
                type: 2,
                title: (mUser ? '修改' : '添加') + '用户',
                content: 'userForm.html',
                data: mUser,       // 使用data参数传值给弹窗页面
                end: function () {
                    if (admin.getLayerData(layIndex, 'formOk')) {  // 判断表单操作成功标识
                        insTb.reload();  // 成功刷新表格
                    } 
                }
            });
        }

    });
</script>
```

&emsp;弹窗参数`type: 2`就表示是一个iframe类型的弹窗，content参数写表单的页面url，
然后在end回调里面判断操作成功的标识然后刷新表格，注意是end而不是success，这样做的好处就是，
弹窗页面跟表格独立开了，大大减少了每个页面的代码量，将业务逻辑分开，坏处就是，
如果页面有下拉框、日期选择控件等元素，它们的范围不能超出弹窗的范围，会导致弹窗出现滚动条，甚至不显示出来。

&emsp;那么有没有办法既能让表单页面独立，又能让表单里面的下拉框、日期等组件能够超出弹窗的范围呢，请看第三种方式。


<br/>

## 10.3.第三种 url方式弹窗 :id=type3
url方式弹窗就是使用url参数将弹窗页面独立出来：

弹窗页面，userForm.html：
```html
<form id="modelUserForm" lay-filter="modelUserForm" class="layui-form model-form">
    <input name="userId" type="hidden"/>
    <div class="layui-form-item">
        <label class="layui-form-label">用户名</label>
        <div class="layui-input-block">
            <input name="nickName" placeholder="请输入用户名" type="text" class="layui-input" maxlength="20"
                   lay-verType="tips" lay-verify="required" required/>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">性别</label>
        <div class="layui-input-block">
            <input type="radio" name="sex" value="男" title="男" checked/>
            <input type="radio" name="sex" value="女" title="女"/>
        </div>
    </div>
    <div class="layui-form-item text-right">
        <button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
        <button class="layui-btn" lay-filter="modelSubmitUser" lay-submit>保存</button>
    </div>
</form>

<script>
    layui.use(['layer', 'form', admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;
        var admin = layui.admin;

        var mUser = admin.getLayerData('#modelUserForm');  // 列表页面传递的数据，#modelUserForm这个只要写弹窗内任意一个元素的id即可
        // 回显数据
        form.val('modelUserForm', mUser);
        // 表单提交事件
        form.on('submit(modelSubmitUser)', function (data) {
        layer.load(2);
        var url = mUser ? '/updateUser' : '/addUser';
            $.post(url, data.field, function (res) {
                layer.closeAll('loading');
                if (res.code == 200) {
                    layer.msg(res.msg, {icon: 1});
                    admin.putLayerData('formOk', true, '#modelUserForm');  // 设置操作成功的标识，#modelUserForm这个只要写弹窗内任意一个元素的id即可
                    admin.closeDialog('#modelUserForm');  // 关闭页面层弹窗
                } else {
                    layer.msg(res.msg, {icon: 2});
                }
            }, 'json');
            return false;
        });
        
    });
</script>
```
&emsp;注意这里，页面不需要写`<html><body>`这些东西，它是一个html片段，不是完整的html页面。

表格页面，list.html：
```html
<button id="btnAddUser" class="layui-btn">添加</button>
<table id="tableUser" lay-filter="tableUser"></table>

<!-- 表格操作列 -->
<script type="text/html" id="tableBarUser">
    <a class="layui-btn layui-btn-primary layui-btn-xs" lay-event="edit">修改</a>
    <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
</script>

<!-- js部分 -->
<script>
    layui.use(['layer', table', 'admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var table = layui.table;
        var admin = layui.admin;

        // 渲染表格
        var insTb = table.render({
            elem: '#tableUser',
            url: '../../json/user.json',
            cols: [[
                {type: 'numbers', title: '#'},
                {field: 'nickName', sort: true, title: '用户名'},
                {field: 'sex', sort: true, title: '性别'},
                {align: 'center', toolbar: '#tableBarUser', title: '操作', minWidth: 200}
            ]]
        });

        // 添加
        $('#btnAddUser').click(function () {
            showEditModel();
        });

        // 工具条点击事件
        table.on('tool(tableUser)', function (obj) {
            var data = obj.data;
            var layEvent = obj.event;
            if (layEvent === 'edit') { // 修改
                showEditModel(data);
            } else if (layEvent === 'del') { // 删除
                layer.msg('点击了删除', {icon: 2});
            }
        });

        // 显示表单弹窗
        function showEditModel(mUser) {
            var layIndex = admin.open({
                title: (mUser ? '修改' : '添加') + '用户',
                url: 'userForm.html',
                data: mUser,     // 传递数据到表单页面
                end: function () {
                    if (admin.getLayerData(layIndex, 'formOk')) {  // 判断表单操作成功标识
                        insTb.reload();  // 成功刷新表格
                    } 
                },
                success: function (layero, dIndex) {
                    // 弹窗超出范围不出现滚动条
                    $(layero).children('.layui-layer-content').css('overflow', 'visible');
                }
            });
        }

    });
</script>
```

&emsp;这里与iframe弹窗的区别是，使用`url`参数指定表单的页面链接，而不是使用content，url这个参数是admin.open扩展的，layer不具备，
这样做会把表单页面的html内容使用ajax加载到弹窗中，而不是iframe嵌入，这样表单里面的下拉框、日期等组件就可以超出弹窗的范围，不会导致弹窗出现滚动条。


!> 注意：表单页面是html片段，不是完整的html，这点不要忘记了，另外表单页面和表格页面不要出现重复的id，因为最终是在一个页面上


## 10.4.四种方式选择指南 :id=choose

方式 | 推荐 | 理由
:--- | :--- | :---
第一、四种 | 建议初学者使用这种 | 不涉及两个页面传值问题
第二种 | 推荐表单跟表格无交互使用，比如详情 | 下拉框、日期不能超出弹窗
第三种 | 表单弹窗、页面有交互建议使用 | 页面传值方便，不存在问题


<br/>

## 10.5.admin.modelForm方法 :id=model_form
此方法是把layer弹窗自带的确定按钮绑定成表单的提交按钮。

```html
<!-- 表单弹窗 -->
<script type="text/html" id="modelUser">
    <div class="layui-form-item">
        <label class="layui-form-label">用户名</label>
        <div class="layui-input-block">
            <input name="nickName" placeholder="请输入用户名" type="text" class="layui-input" maxlength="20"
                   lay-verType="tips" lay-verify="required" required/>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">性别</label>
        <div class="layui-input-block">
            <input type="radio" name="sex" value="男" title="男" checked/>
            <input type="radio" name="sex" value="女" title="女"/>
        </div>
    </div>
</script>

<!-- js部分 -->
<script>
    layui.use(['layer', 'form', admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;
        var admin = layui.admin;
        
        admin.open({
            type: 1,
            title: '添加用户',
            btn: ['确定', '取消'],
            content: $('#modelUser').html(),
            success: function (layero, dIndex) {
                // 把确定按钮绑定表单提交，参数二是给按钮起一个lay-filter，参数三是给表单起一个lay-filter
                admin.modelForm(layero, 'demoFormSubmit', 'demoForm');
                
                // 给表单赋值
                form.val('demoForm', {nickName: '张三', sex: '男'}); 
                
                // 监听表单提交
                form.on('submit(demoFormSubmit)', function (data) {
                    layer.msg(JSON.stringify(data.field));
                    return false;
                });
                
            },
            yes: function () {
                // 确定按钮方法什么都不要操作
            }
        });

    });
</script>
```

&emsp;admin.modelForm()这个方法会把弹窗外面包一个form，然后把确定按钮加lay-submit，所以你的表单页面不需要写form和确定、关闭按钮，只需要写表单项。

&emsp;这个方法的使用场景为你想要表单的按钮固定，只滚动表单内容部分，可以用这个操作，
当然那种自己写确定和取消按钮的表单弹窗的写法也是支持固定按钮的，
前面css组件样式中有介绍。


## 10.6.参数传递方法详解     :id=model_param
**参数传递：**
```javascript
admin.open({
    type: 2,
    content: 'userForm.html',
    data: {
        name: '李白',
        sex: '男'
    }
});
```
通过data属性进行参数传递，data同样是admin.open扩展的，layer不具备。


**获取参数：**

方法 | 说明 | 参数
:--- | :--- | :---
admin.getLayerData(index) | 获取某弹窗的全部参数 | layer的index
admin.getLayerData(index, key) | 参数某弹窗参数的某个字段 | index，字段
admin.getLayerData() | iframe弹窗子页面获取参数 | 无任何参数
admin.getLayerData('#xxForm') | url方式弹窗子页面获取参数 | 弹窗内任意元素id

&emsp;如果是在iframe弹窗的子页面中可以使用admin.getLayerData()直接获取父页面传递的全部参数，
如果是在url方式打开的弹窗中可以使用admin.getLayerData('#xx')直接获取父页面的全部参数，'#xx'是弹窗内任意元素的id。


**增加参数：**

方法 | 说明 | 参数
:--- | :--- | :---
admin.putLayerData(key, value, index) | 增加参数 | 字段名，值，index
admin.putLayerData(key, value) | iframe弹窗子页面增加参数 | 字段名，值
admin.putLayerData(key, value, '#xx') | url方式弹窗子页面增加参数 | 弹窗内任意元素id

!> **注意：**data参数必须是对象的形式，data:1、data:'aa'这种写法会导致无法put新参数。

<br/>

## 10.7.第四种 捕获层弹窗

&emsp;第一种页面层弹窗由于所有关于弹窗内元素的操作代码都要写在弹窗的success里面，
大部分人不适应这种方式，所以介绍第四种捕获层弹窗：

```html
<!-- 表单弹窗 -->
<form id="modelRoleForm" lay-filter="modelRoleForm" class="layui-form model-form" style="display: none;">
    <input name="roleId" type="hidden"/>
    <div class="layui-form-item">
        <label class="layui-form-label">角色名</label>
        <div class="layui-input-block">
            <input name="roleName" placeholder="请输入角色名" type="text" class="layui-input"
                   lay-verType="tips" lay-verify="required" required/>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">备注</label>
        <div class="layui-input-block">
            <textarea name="comments" placeholder="请输入内容" class="layui-textarea"></textarea>
        </div>
    </div>
    <div class="layui-form-item text-right">
        <button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
        <button class="layui-btn" lay-filter="modelSubmitRole" lay-submit>保存</button>
    </div>
</form>

<script>
    layui.use(['layer', 'form', 'table', 'admin'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;
        var table = layui.table;
        var admin = layui.admin;
        var formUrl;

        // 渲染表格
        var insTb = table.render({...});

        // 添加
        $('#btnAddRole').click(function () {
            showEditModel();
        });

        // 表格工具条点击事件
        table.on('tool(tableRole)', function (obj) {
            var data = obj.data;
            var layEvent = obj.event;
            if (layEvent === 'edit') { // 修改
                showEditModel(data);
            }
        });

        // 显示编辑弹窗
        function showEditModel(mRole) {
            // 如果是单标签版本可以使用下面代码把表单移到body中
            /* if ($('#modelRoleForm').parent('body').length == 0) {
                $('body').append($('#modelRoleForm'));
            } */
            $('#modelRoleForm')[0].reset();  // 重置表单
            form.val('modelRoleForm', mRole);  // 回显数据
            formUrl = mRole ? 'role/update' : 'role/add';
            admin.open({
                type: 1,
                title: (mRole ? '修改' : '添加') + '角色',
                content: $('#modelRoleForm')
            });
        }

        // 表单提交事件
        form.on('submit(modelSubmitRole)', function (data) {
            layer.load(2);
            $.post(formUrl, data.field, function (res) {
                layer.closeAll('loading');
                if (res.code == 200) {
                    admin.closeDialog('#modelRoleForm');
                    layer.msg(res.msg, {icon: 1});
                    insTb.reload();
                } else {
                    layer.msg(res.msg, {icon: 2});
                }
            }, 'json');
            return false;
        });

    });
</script>
```

&emsp;与第一种的区别是form不用`<script>`包裹，加`style="display:none"`隐藏，
admin.open的content是$('#roleForm')而不是$('#roleForm').html()，
表单的提交事件可以直接写在外面，而不用写在弹窗的success里面。

!> 捕获层的弊端就是弹窗的页面代码最好是写在body下面，不然样式会被其他影响，所以spa版本不建议使用这种方式，单标签版、iframe版本可以使用。

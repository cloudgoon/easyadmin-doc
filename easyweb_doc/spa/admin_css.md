## 5.1.公共类  :id=public

类名（class） | 说明
:---|:--- 
pull-left | 左浮动
pull-right | 右浮动
text-left | 内容居左
text-center | 内容居中
text-right | 内容居右
inline-block | 设置display为inline-block
bg-white | 设置背景为白色
layui-link | 设置a标签颜色为主题色 
layui-text | .layui-text下面的a标签为蓝色
 | 
text-muted | 文字颜色为灰色
text-success | 文字颜色为绿色，成功色
text-warning | 文字颜色为黄色警告色
text-danger | 文字颜色为红色危险色
text-info | 文字颜色为蓝色信息色
text-primary | 文字颜色为主题色

以上是easyweb增加的公共类，当然也可以使用[Layui公共类](https://www.layui.com/doc/base/element.html)。

## 5.2.组件样式  :id=module

类名（class） | 说明
:---|:--- 
icon-btn | 带图标的按钮，会缩小边距
date-icon | 在元素的右边加入日期的图标 
icon-search | 在元素的右边加入搜索的图标 
btn-circle | 圆形按钮，参见便签界面
 | 
layui-form-select-top | 控制下拉框上弹出，加载select父元素上
xm-select-nri | 多选下拉框去掉最后那个没用的图标，加在父元素上
 | 
mini-bar | 如果有滚动条，使用细的风格
 | 
arrow2 | 设置侧边栏小三角为箭头图标，加在layui-nav上
arrow3 | 设置侧边栏小三角为加减号图标，加在layui-nav上
 | 
close-footer | 关闭页脚，加在body上
 | 
table-tool-mini | 数据表格工具栏mini样式，加在table父元素上
full-table | 针对full-xxx的table的工具栏mini样式
 | 
hide-body-title | 全局隐藏单标签模式标题栏，加在body上

```html
<!-- 图标按钮 -->
<button class="layui-btn icon-btn"><i class="layui-icon">&#xe615;</i>搜索</button>

<!-- 日期图标 -->
<input class="layui-input date-icon" type="text"/>

<!-- 圆形按钮 -->
<div class="btn-circle">
    <i class="layui-icon layui-icon-add-1"></i>
</div>

<!-- 下拉框上弹出 -->
<div class="layui-form-select-top">
    <select>....</select>
</div>

<!-- 多选下拉框去掉最后那个没用的图标 -->
<div class="xm-select-nri">
    <select xm-select="xx">....</select>
</div>

<!-- 关闭页脚 -->
<body class="layui-layout-body close-footer">

<!-- 表格工具栏mini样式 -->
<div class="table-tool-mini full-table">
    <table id="xxTable" lay-filter="xxTable"></table>
</div>
```

![](https://s2.ax1x.com/2019/08/28/mTfj0K.png)
![](https://s2.ax1x.com/2019/07/06/ZwTtMD.jpg)
![](https://s2.ax1x.com/2019/07/11/Z2UdEV.png)


## 5.3.表单弹窗  :id=form

类名（class） | 说明
:---|:--- 
model-form | 调整弹窗内的表单的间距使之更好看
model-form-body | 表单内容部分，高度自适应，超过屏幕高度显示滚动条
model-form-footer | 表单底部按钮部分，用于固定底部按钮

**表单弹窗示例：**
```html
<script type="text/html" id="modelUser">
    <form id="modelUserForm" lay-filter="modelUserForm" class="layui-form model-form">
        <input name="userId" type="hidden"/>
        <div class="layui-form-item">
            <label class="layui-form-label">账号</label>
            <div class="layui-input-block">
                <input name="username" placeholder="请输入账号" type="text" class="layui-input" maxlength="20"
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
        <div class="layui-form-item">
            <label class="layui-form-label">角色</label>
            <div class="layui-input-block">
                <select name="roleId" lay-verType="tips" lay-verify="required">
                    <option value="">请选择角色</option>
                    <option value="1">管理员</option>
                    <option value="2">普通用户</option>
                </select>
            </div>
        </div>
        <div class="layui-form-item text-right">
            <button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
            <button class="layui-btn" lay-filter="modelSubmitUser" lay-submit>保存</button>
        </div>
    </form>
</script>
<script>
    layui.use(['admin'],function(){
        var admin = layui.admin;
    
        admin.open({
            type: 1,
            title: '添加用户',
            content: $('#modelUser').html(),
            success: function (layero, dIndex) {
                // 表单的操作，事件绑定等都写在success回调里面
            }
        });
    });
</script>
```
![](https://s2.ax1x.com/2019/08/28/mTh3n0.png)


**固定底部操作按钮示例：**
```html
<script type="text/html" id="modelUser">
    <form id="modelUserForm" lay-filter="modelUserForm" class="layui-form model-form no-padding">
        <div class="model-form-body" style="max-height: 320px;"> <!-- 如果要超出屏幕才固定底部，不要写max-height -->
            <div class="layui-form-item">
                <label class="layui-form-label">实习公司</label>
                <div class="layui-input-block">
                    <input name="companyName" class="layui-input"/>
                </div>
            </div>
            <!-- ......省略 -->
        </div>
        <div class="layui-form-item text-right model-form-footer">
            <button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
            <button class="layui-btn" lay-filter="modelSubmitUser" lay-submit>保存</button>
        </div>
    </form>
</script>
```

![](https://s2.ax1x.com/2019/08/28/mThoHf.png)

> 两者的区别就在于固定底部操作按钮需要增加model-form-body和model-form-footer，而普通的表单弹窗只需要model-form


## 5.4.表格工具栏  :id=toolbar

类名（class） | 说明
:---|:--- 
toolbar | 调整表格上面的表单间距使之更好看
w-auto | 设置width:auto，用于重置一些有固定宽度表单元素
mr0 | 设置margin-right:0，用于重置一些表单元素的样式

```html
<!-- 表格顶部工具栏区域 -->
<div class="layui-form toolbar">
    <div class="layui-form-item">
        <div class="layui-inline">
            <label class="layui-form-label w-auto">账&emsp;号：</label>
            <div class="layui-input-inline mr0">
                <input name="username" class="layui-input" type="text" placeholder="输入账号"/>
            </div>
        </div>
        <div class="layui-inline">
            <label class="layui-form-label w-auto">用户名：</label>
            <div class="layui-input-inline mr0">
                <input name="nickName" class="layui-input" type="text" placeholder="输入用户名"/>
            </div>
        </div>
        <div class="layui-inline">
            <button class="layui-btn icon-btn" lay-filter="formSubSearchUser" lay-submit>
                <i class="layui-icon">&#xe615;</i>搜索
            </button>
            <button id="btnAddUser" class="layui-btn icon-btn"><i class="layui-icon">&#xe654;</i>添加</button>
        </div>
    </div>
</div>

<!-- 表格 -->
<table class="layui-table" id="tableUser" lay-filter="tableUser"></table>
```

![](https://s2.ax1x.com/2019/08/28/mTfUyt.png)

**移动端自动适配效果：**

![](https://s2.ax1x.com/2019/08/28/mTfrFg.png)

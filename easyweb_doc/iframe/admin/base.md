## 3.1.默认主题、存储表名  :id=option
修改这两个默认的配置打开admin.js修改如下参数即可：
```javascript
var admin = {
    defaultTheme: 'theme-admin',  // 默认主题
    tableName: 'easyweb'  // 存储表名
    // ...省略
}
```

- defaultTheme &emsp; 默认主题，可修改成`theme-red`等。
- tableName &emsp; 是本地缓存的表名，一个域名下多个项目，建议修改成不同的名字。


## 3.2.全部方法  :id=method

方法 | 参数 | 描述
:---|:--- |:--- 
flexible(expand) | true和false | 折叠/展开侧导航 
activeNav(url) | a标签里面的lay-href值 | 设置侧导航栏选中
refresh(url) | url，可为空 | 刷新指定Tab或当前Tab
closeAllTabs() | 无 | 关闭所有选项卡
closeOtherTabs(url) | url | 关闭除url外所有选项卡
closeThisTabs(url) | url，可为空 | 关闭url或当前选项卡
rollPage(d) | 方向(left、right、auto) | 滚动选项卡tab
 | | 
iframeAuto() | 无 | 让当前的ifram弹层自适应高度
closeThisDialog() | 无 | 关闭当前iframe弹窗
closeDialog(elem) | dom选择器 | 关闭elem元素所在的弹窗(页面层弹窗)
 | | 
putTempData(key, value) | key,value | 缓存临时数据
getTempData(key) | key | 获取缓存的临时数据
parseJSON(string) | 字符串 | 解析json，解析失败返回undefined
getPageHeight() | 无 | 获取浏览器高度
getPageWidth() | 无 | 获取浏览器宽度
 | | 
 btnLoading(elem) | dom选择器 | 设置按钮为加载状态
 openSideAutoExpand() | 无 | 开启鼠标移入侧边栏自动展开
 openCellAutoExpand() | 无 | 开启鼠标移入表格单元格超出内容自动展开
 modelForm(layero, btnFilter, formFilter) |  | 把弹窗自带的按钮跟弹窗内的表单绑定一起

使用示例：
```javascript
layui.use(['admin'], function () {
    var admin = layui.admin;

    var pageHeight = admin.getPageHeight();    // 获取浏览器高度
});
```

**admin.iframeAuto()方法：** <br/>
&emsp;针对type:2的弹窗自适应弹窗高度，写在弹窗的子页面中，此方法是调用一次做一次高度自适应，
如果你用js动态修改了弹窗子页面的高度，需要再调用一次。

**admin.closeThisDialog()：** <br/>
&emsp;关闭当前iframe类型弹窗，针对type:2的弹窗，在弹窗的子页面调用即可关闭当前的iframe弹窗。


**admin.closeDialog(elem)：** <br/>
&emsp;关闭非iframe类型的弹窗，调用需要传递弹窗页面里面任意一个的元素：

```
admin.closeDialog('#xxx');  // 关闭id为xxx元素所在的页面层的弹窗
```

关闭弹窗还可以使用ew-event操作：
```html
<!-- 关闭iframe类型的弹窗 -->
<button ew-event="closeDialog"></button>
<!-- 关闭页面层的弹窗 -->
<button ew-event="closePageDialog"></button>
```

&emsp;admin.openSideAutoExpand()方法需要在index.html中调用，admin.openCellAutoExpand()可在任何页面调用。


## 3.3.popupRight和open  :id=open
快速使用：
```javascript
// 打开弹窗
admin.open({
    type: 2,
    content: 'tpl-theme.html'
});

// 打开右侧面板
admin.popupRight({
    type: 2,
    content: 'tpl-theme.html'
});
```

&emsp;这两个方法只是对layer.open进行了一层封装，用法和layer一样，查看[layer文档](https://www.layui.com/doc/modules/layer.html)。

**新增参数url：**

```javascript
admin.open({ 
    title: 'Hello',
    url: 'tpl-theme.html' 
});


admin.popupRight({ 
    url: 'tpl-theme.html' 
});
```

&emsp;`type:2, content:xxx`这种是iframe类型的弹窗，使用`url`会通过ajax加载页面到弹窗中，而不是iframe嵌入。
&emsp;当使用url方式的时候，弹窗页面应该是代码片段，而不是完整的html，如下所示：
```html
<style>
* { color: #333; }
</style>
<form id="modelRoleForm" lay-filter="modelRoleForm" class="layui-form model-form">
	<input name="roleId" type="hidden"/>
	<div class="layui-form-item">
		<label class="layui-form-label">角色名</label>
		<div class="layui-input-block">
			<input name="roleName" placeholder="请输入角色名" type="text" class="layui-input" maxlength="20"
				   lay-verType="tips" lay-verify="required" required/>
		</div>
	</div>
	<div class="layui-form-item text-right">
		<button class="layui-btn layui-btn-primary" type="button" ew-event="closePageDialog">取消</button>
		<button class="layui-btn" lay-filter="modelSubmitRole" lay-submit>保存</button>
	</div>
</form>
<script>
    layui.use(['layer', 'form'], function () {
        var $ = layui.jquery;
        var layer = layui.layer;
        var form = layui.form;

        // 表单提交事件
        form.on('submit(modelSubmitRole)', function (data) {
            console.log(data.field);
            return false;
        });

    });
</script>
```
&emsp;页面不需要html、body这些东西，并且可以直接用`<script>`标签来写事件，至于参数传递，请到弹窗专题查看。


## 3.4.加载动画loading  :id=loading
快速使用：
```javascript
admin.showLoading('#xxx');  // 在id为xxx的元素中显示加载层
admin.showLoading('#xxx', 1, '.8');  // 显示type为1、透明度为0.8的遮罩层
```

- 参数一`elem`&emsp;非必填&emsp;元素选择器，不填为body；
- 参数二`type`&emsp;非必填&emsp;动画类型(1 小球，2 魔方，3信号)，默认是1；
- 参数三`opacity`&emsp;非必填&emsp;透明度(0 - 1)，默认不透明；

尺寸控制，提供有两种尺寸，用法：
```javascript
admin.showLoading({
    elem: '#xxx',
    type: 1,
    size: 'sm'  // 默认是sm小型尺寸，还可以选md大型尺寸
});
```

移除加载动画：
```javascript
admin.removeLoading('#xxx');
admin.removeLoading('#xxx', true, true);
```

- 参数一&emsp;非必填&emsp;元素选择器，不填为body；
- 参数二&emsp;非必填&emsp;true是淡出效果，false直接隐藏，默认是true；
- 参数三&emsp;非必填&emsp;true是删除，false是隐藏不删除，默认是false；

页面载入的加载动画：
```html
<body class="page-no-scroll"> <!-- page-no-scroll这个不要忘了 -->
    <!-- 小球样式 -->
    <div class="page-loading">
        <div class="ball-loader">
            <span></span><span></span><span></span><span></span>
        </div>
    </div>
    
    <!-- 魔方样式 -->
    <div class="page-loading">
        <div class="rubik-loader"></div>
    </div>
    
    <!-- 信号样式 -->
    <div class="page-loading">
        <div class="signal-loader">
            <span></span><span></span><span></span><span></span>
        </div>
    </div>
    
    <!-- 加sm是小型尺寸 -->
    <div class="page-loading">
        <div class="signal-loader sm">
            <span></span><span></span><span></span><span></span>
        </div>
    </div>
</body>
```

&emsp;写在页面中需要在js中调用`admin.removeLoading()`移除加载动画，common.js中已经写好了。


**按钮loading：**
```javascript
admin.btnLoading('#btn1');   // 设置按钮为loading状态
admin.btnLoading('#btnLoading', false);  // 移除按钮的loading状态

admin.btnLoading('#btn1', '加载中');   // 设置按钮为loading状态，同时修改按钮文字
admin.btnLoading('#btnLoading', '保存', false);  // 移除按钮的loading状态，同时修改按钮文字
```


## 3.5.ajax封装  :id=ajax
req方法：
```javascript
admin.req('url',{
    username: 'admin',
    password: '123456'
}, function(res){
    alert(res.code + '-' + res.msg);
}, 'get');
```

- 参数一&emsp;请求的url
- 参数二&emsp;请求参数
- 参数三&emsp;请求回调（失败也进此回调，404、403等）
- 参数四&emsp;请求方式，get、post、put、delete等

ajax方法，参数同$.ajax：
```javascript
admin.ajax({
    url: 'url',
    data: {},
    headers： {},
    type: 'post',
    dataType: 'json',
    success: function(res){
        alert(res.code + '-' + res.msg);
    }
});
```

&emsp;req和ajax都实现了自动传递header、预处理、系统错误依然回调到success等功能。

**自动传递header：**

&emsp;重写admin的`getAjaxHeaders`方法：
```javascript
admin.getAjaxHeaders = function (requestUrl) {
    var headers = new Array();
    headers.push({name: 'token', value: 'xxxxx'});
    return headers;
}
```

**请求回调预处理：**

&emsp;重写admin的`ajaxSuccessBefore`方法：
```javascript
admin.ajaxSuccessBefore = function (res, requestUrl) {
    if(res.code==401){
        alert('登录超时，请重新登录');
        return false;  // 返回false阻止代码执行
    }
    return true;
}
```

> 重写的操作建议放在common.js里面，这样就不用每个页面都去重写了，只要保证重写是在调用req、ajax之前即可。


## 3.6.ew-event事件绑定  :id=event
使用示例：
```html
<a ew-event="fullScreen">全屏</a>
<a ew-event="flexible">折叠导航</a>
```

事件 | 描述
:---|:--- 
flexible | 折叠侧导航 
refresh | 刷新主体部分 
closeThisTabs | 关闭当前选项卡
closeOtherTabs | 关闭其他选项卡
closeAllTabs | 关闭全部选项卡
leftPage | 左滚动选项卡
rightPage | 右滚动选项卡
  |  
closeDialog | 关闭当前iframe弹窗
closePageDialog | 关闭当前页面层弹窗
  |  
theme | 打开主题设置弹窗 
note | 打开便签弹窗
message | 打开消息弹窗
psw | 打开修改密码弹窗
logout | 退出登录
  |  
fullScreen | 全屏切换
back | 浏览器后退
  |  
open | 打开弹窗
popupRight | 打开右侧弹窗

ew-event属性可用于任何元素，不仅仅是a标签，theme、note等可以通过`data-url`属性配置对应的url，
还可以通过`data-window="top"`属性配置在父页面处理事件。

```html
<a ew-event="theme" data-url="xxx.html">主题</a>
```

## 3.7.open和popupRight事件  :id=event_open

这两个事件是用来支持非js方式打开弹窗：
```html
<button ew-event="open" data-type="2" data-content="http://baidu.com">iframe弹窗</button>

<button ew-event="open" data-type="1" data-url="form.html">页面弹窗</button>

<button ew-event="open" data-type="1" data-content="#userForm">页面弹窗</button>
<form id="userForm">......省略</form>

<!-- 设置area和offset -->
<button ew-event="open" data-type="1" data-content="Hello" data-area="80px,60px" data-offset="10px,10px">页面弹窗</button>

<!-- popupRight一样的用法 -->
<button ew-event="popupRight" data-type="2" data-url="http://baidu.com" data-title="百度一下，你就知道">右侧弹窗</button>

<!-- function类型参数写法 -->
<button ew-event="open" data-type="1" data-content="Hello" data-success="onDialogSuccess">页面弹窗</button>
<script>
    layui.use(['layer'], function(){
        var layer = layui.layer;
        
        // 方法需要加window
        window.onDialogSuccess = function(){
            layer.msg('弹窗被成功打开了');    
        };
    });
</script>
```
layer支持的参数大部分都可以通过data属性来设置，数组类型用逗号分隔，function类型需要把作用域放在window对象下。

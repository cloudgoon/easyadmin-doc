## 1.1.导入项目  :id=import

1. 下载项目后进行解压
2. 使用IDEA、WebStorm、HBuilder等前端开发工具打开
3. 打开index.html点击右上角图标运行到浏览器：

![运行教程](https://s2.ax1x.com/2019/08/29/mHOQTH.png)

!> **注意：** 必须以http://的形式访问，而不是file://的形式访问。


## 1.2.项目结构  :id=structure

```
|-assets
|     |-css               // 样式
|     |-images            // 图片
|     |-js
|         |-main.js       // 入口js
|     |-libs              // 第三方库，echarts(图表)、layui
|     |-module            // layui扩展模块，版本更新只用替换此目录
|         |-theme             // 主题资源
|         |-img               // easyweb框架用到的图片
|         |-admin.css         // easyweb框架核心样式
|         |-admin.js          // admin模块
|         |-index.js          // index模块
|         |-********          // 其他扩展模块，不一一列举
|-components              // html子页面
|-json              // 模拟数据
|-index.html        // 主页面
```

> main.js为入口js，上手使用项目前最好先看看它的说明。


## 1.3.index.html结构说明  :id=declare

```html
<html>
<head>
    <link rel="stylesheet" href="assets/libs/layui/css/layui.css"/>
    <link rel="stylesheet" href="assets/module/admin.css"/>
</head>
<body class="layui-layout-body">
    <div class="layui-layout layui-layout-admin">
        <!-- 头部 -->
        <div class="layui-header">...</div>
        <!-- 侧边栏 -->
        <div class="layui-side">...</div>
        <!-- 主体部分 -->
        <div class="layui-body">...</div>
        <!-- 底部 -->
        <div class="layui-footer">...</div>
    </div>
    
    <script type="text/javascript" src="assets/libs/layui/layui.js"></script>
    <script type="text/javascript" src="assets/js/main.js"></script>
</body>
</html>
```
index.html就是这样固定的结构，一般不需要修改，只用在components下写子页面即可。


## 1.4.添加一个菜单  :id=add_menu

1. 打开json/menus.json，在合适的位置添加一个菜单：
    ```json
   {
       "name": "xx管理",
       "url": "#/xxx.html"
    }
    ```
2. 在components下面新建一个xxx.html页面
3. 运行index.html，查看效果

!> **注意：** url以"#/"开头才会受路由控制，实现局部刷新。


## 1.5.main.js说明  :id=main_js

```javascript
layui.config({
    base: 'assets/module/'  // 配置layui扩展模块目录
}).extend({  // 配置每个模块分别所在的目录
    notice: 'notice/notice',
    step: 'step-lay/step'
}).use(['layer', 'element', 'config', 'index', 'admin', 'laytpl'], function () {
    var $ = layui.jquery;
    var layer = layui.layer;
    var element = layui.element;
    var config = layui.config;
    var index = layui.index;
    var admin = layui.admin;
    var laytpl = layui.laytpl;

    // 加载侧边栏
    admin.req('menus.json', {}, function (res) {
        laytpl(sideNav.innerHTML).render(res, function (html) {
            $('.layui-layout-admin>.layui-side .layui-nav').html(html);
            element.render('nav');
        });
        index.regRouter(res);  // 注册路由
        index.loadHome({  // 加载主页
            url: '#/console/console1',
            name: '<i class="layui-icon layui-icon-home"></i>'
        });
    }, 'get');

    // 移除loading动画
    setTimeout(function () {
        admin.removeLoading();
    }, 300);
});
```

- layui.config是告诉layui扩展模块所在目录
- layui.extend是配置每个模块具体js位置
- admin.removeLoading()是移除页面加载动画
- 侧边栏通过ajax加载，然后注册路由，加载主页

&emsp;像`admin.js`、`index.js`这些没有用子目录存放的模块不需要配置layui.extend，延时移除加载动画是给切换主题预留时间。

!> **注意：** 只有注册了路由，点击"#/xxx"的时候，才能找到对应的地址并实现局部刷新


## 1.6.config模块   :id=config
config模块主要是用于给index模块和admin模块提供配置，可根据自己需要进行更改。

参数配置：

配置名 | 默认 | 说明
:---|:--- | :---
version |  312 | 版本号，加载子页面会带上,为true则随机，意味着不会有缓存
base_server | 'json/' | 接口地址，admin.req方法会自动加
tableName | 'easyweb-spa' | 存储表名
pageTabs | false | 是否开启多标签
openTabCtxMenu | true | 是否开启Tab右键菜单
maxTabNum | 20 | 最多打开多少个tab
viewPath | 'components' | 视图位置
viewSuffix | '.html' | 视图后缀
defaultTheme | 'theme-admin' | 默认主题
reqPutToPost | true | req请求put方法变成post
cacheTab | true | 是否记忆Tab

方法：

配置名 | 默认 
:---|:--- 
getToken() | 获取token
putToken(token) | 缓存token
removeToken() | 清除token
getUser() | 获取当前登录的用户信息
putUser(user) | 缓存当前登录的用户信息
getUserAuths() | 获取用户所有权限
getAjaxHeaders(requestUrl) | ajax请求的header
ajaxSuccessBefore(res, requestUrl) | ajax请求结束后的处理，返回false阻止代码执行
routerNotFound(r) | 路由不存在处理

解决缓存问题配置version版本号在加载子页面时会带上版本号，为true会随机生成，
如果config.js被缓存，请检查main.js里面layui.config有没有写version: true。


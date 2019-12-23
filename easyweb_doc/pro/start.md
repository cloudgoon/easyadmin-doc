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
|         |-common.js     // 公共js，主要配置layui扩展模块位置
|     |-libs              // 第三方库，echarts(图表)、layui
|     |-module            // layui扩展模块，版本更新只用替换此目录
|         |-theme             // 主题资源
|         |-img               // easyweb框架用到的图片
|         |-admin.css         // easyweb框架核心样式
|         |-admin.js          // admin模块
|         |-********          // 其他扩展模块，不一一列举
|-page              // html页面
|-json              // 模拟数据
|-index.html        // 主页面
```

> 与后端整合从index.html页面入手，page、json可以删除，assets整个目录都是静态资源。


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
    <script type="text/javascript" src="assets/js/common.js"></script>
    <script>
        layui.use(['admin'], function () {
            var admin = layui.admin;
            
            admin.activeNav('index.html');  // 选中侧边栏
        });
    </script>
</body>
</html>
```

> index.html就是这样固定的结构，一般不需要修改，在整合到后台时，可以把头部、侧边栏抽离出来，再include进去。


## 1.4.添加一个菜单  :id=add_menu

1. 打开index.html，找到侧导航栏部分，添加一个菜单：
    ```html
   <dd><a href="xxx.html">XX管理</a></dd>
    ```
2. 在index.html同目录下新建一个xxx.html页面
3. 运行index.html，查看效果

!> **注意：** 因为是单标签，所以每个页面都要包含头部、侧边栏、底部，实际项目可通过抽取公共部分引用。

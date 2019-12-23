## 2.1.common.js说明  :id=commonjs

```javascript
layui.config({
    base: getProjectUrl() + 'assets/module/'  // 配置layui扩展模块目录
}).extend({  // 配置每个模块分别所在的目录
    notice: 'notice/notice',
    step: 'step-lay/step'
}).use(['admin'], function () {
    var admin = layui.admin;
    
    setTimeout(function () {
        admin.removeLoading();  // 移除loading动画
    }, window == top ? 600 : 100);
});

// 获取项目根路径
function getProjectUrl() {
    return '...省略';
}
```

- layui.config是告诉layui扩展模块所在目录
- layui.extend是配置每个模块具体js位置
- admin.removeLoading()是移除页面加载动画

&emsp;像`admin.js`、`contextMenu.js`这些没有用子目录存放的模块不需要配置layui.extend，延时移除加载动画是给切换主题预留时间。

!> **注意：** 每个页面都需要引入common.js


## 2.2.侧边栏手风琴折叠  :id=accordion

默认是开启的，如要关闭配置如下：
```html
<!-- 侧边栏 -->
<div class="layui-side">
    <div class="layui-side-scroll">
        <ul class="layui-nav layui-nav-tree" lay-accordion="false">
            ......省略其他部分
        </ul>
    </div>
</div>
```

修改layui-nav-tree上的`lay-accordion`属性为false或者直接去掉即可，所谓手风琴效果就是一个菜单展开的时候，其他菜单自动折叠。

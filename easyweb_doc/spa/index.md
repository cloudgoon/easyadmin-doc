# 2.index模块  :id=index
index模块主要是负责管理选项卡的打开、关闭，使用方法：
```javascript
layui.use(['index'], function () {
    var index = layui.index;
    
    index.xxx();  // 调用index模块的方法
});
```

## 2.1.批量注册路由  :id=reg

```javascript
index.regRouter([{
    name: '用户管理',
    url: '#/system/user'
}]);
```

参数是一个数组，可以批量注册路由，当你访问`#/system/user`的时候，就会打开一个“用户管理”的标签页，“url”是路由关键字，
对应的页面地址是“components/system/user.html”，“components”和“.html”可以在config模块中配置。


## 2.2.加载默认主页  :id=load_home

```javascript
index.loadHome({
    url: '#/console/console1',
    name: '<i class="layui-icon layui-icon-home"></i>',
    loadSetting: true,
    onlyLast: false
});
```

- url：&emsp;必填&emsp;主页路径
- name：&emsp;必填&emsp;Tab标题
- loadSetting：&emsp;非必填&emsp;加载缓存的设置
- onlyLast：&emsp;非必填&emsp;是否仅恢复最后一个Tab

> 主页的地址可以不在侧边栏中，因为“loadHome()”方法会检查并注册路由。


## 2.3.打开一个选项卡  :id=open_tab

```javascript
index.openNewTab({
    name: '用户管理', 
    url: '#/system/user'
});

// 也可以加参数
index.openNewTab({
    name: '用户管理', 
    url: '#/system/user/id=1'
});
```

- name： 选项卡的标题
- url： 路由关键字

也可以使用`<a href="#/system/user">用户管理</a>`，这种写法必须保证“#/system/user”已经注册了路由，而“openNewTab()”方法会自动注册路由。

!> 注意：openNewTab方法只是在调用这个方法的时候注册路由，如果刷新页面，路由就会不存在了，如果想刷新页面也能使用，需要提前在main.js中注册路由。

```javascript
// 在main.js的index.loadHome()方法之前注册即可，注册路由不用加参数
index.regRouter([{
    name: '用户管理',
    url: '#/system/user'
}]);
```

## 2.4.关闭指定选项卡  :id=close_tab

```javascript
index.closeTab('/system/user');
```

!> 注意：关闭选项卡不要带“#”号。


## 2.5.跳到指定选项卡  :id=go_tab

```javascript
index.go('/system/user');
```

跳转页面不要带“#”号，相当于点击了`<a href="#/system/user">`。


## 2.6.修改Tab标题  :id=set_tab

```javascript
index.setTabTitle('Hello');  // 修改当前Tab标题文字，也支持单标签模式

index.setTabTitle('Hello', tabId);  // 修改指定Tab标题文字

index.setTabTitleHtml('<span>Hello</span>');  // 修改整个标题栏的html，此方法只在单标签模式有效
```

关闭多标签时框架会自动生成一个标题栏，可使用此方法修改标题栏内容，参数为undefined时为隐藏标题栏。


## 2.7.获取hash路径  :id=hash_path

```javascript
index.getHashPath('#/system/user');

index.getHashPath('#/system/user/id=1/name=aa');
```

这两种hash最后返回的都是“/system/user”，后面属于参数传递，不属于视图的路径。


## 2.8.侧边栏手风琴折叠  :id=accordion

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


## 2.9.切换Tab自动刷新

```javascript
// 选项卡切换自动刷新
element.on('tab(admin-pagetabs)', function (data) {
    var tabId = $(this).attr('lay-id');
    var $content = $('.layui-layout-admin>.layui-body>.layui-tab>.layui-tab-content>.layui-tab-item>div[lay-id="' + tabId + '"]');
    var layHash = $content.attr('lay-hash');
    if ($content.html() && !layui.layRouter.isRefresh) {
        layui.layRouter.refresh(layHash);
    }
});
```

上面代码写在main.js中即可。

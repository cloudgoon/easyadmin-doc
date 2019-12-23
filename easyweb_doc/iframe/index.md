# 2.index模块  :id=index
index模块主要是负责管理选项卡的打开、关闭，使用方法：
```javascript
layui.use(['index'], function () {
    var index = layui.index;
    
    index.xxx();  // 调用index模块的方法
});
```

## 2.1.选项卡配置  :id=config

配置名 | 默认 | 说明
:---|:--- | :---
pageTabs | true | 是否开启多标签
cacheTab | true | 是否记忆打开的选项卡
openTabCtxMenu | true | 是否开启Tab右键菜单
maxTabNum | 20 | 最多打开多少个tab

如果要修改默认配置打开index.js修改这几个参数即可：
```javascript
var index = {
    pageTabs: true,  // 是否开启多标签
    cacheTab: true,  // 是否记忆打开的选项卡
    openTabCtxMenu: true,  // 是否开启Tab右键菜单
    maxTabNum: 20  // 最多打开多少个tab
    // ... 省略
}
```

> 在设置界面修改的配置会高于index中的默认配置，设置界面的配置信息是放在缓存中，清除缓存就会以index中配置为主。


## 2.2.加载默认主页  :id=load_home

```javascript
index.loadHome({
    menuPath: 'page/console/console.html',
    menuName: '<i class="layui-icon layui-icon-home"></i>',
    loadSetting: true,
    onlyLast: false
});
```

- menuPath：&emsp;必填&emsp;主页路径
- menuName：&emsp;必填&emsp;Tab标题
- loadSetting：&emsp;非必填&emsp;加载缓存的设置
- onlyLast：&emsp;非必填&emsp;是否仅恢复最后一个Tab

> index.loadHome()方法在index.html中调用即可。


## 2.3.打开一个选项卡  :id=open_tab

使用js的方式：
```javascript
index.openTab({
    title: '百度', 
    url: 'https://www.baidu.com',
    end: function() {
        // insTb.reload(); 
    }
});
```

- title： 选项卡的标题
- url： 打开的页面地址
- end： Tab关闭的回调事件(非必填)

在页面中使用：
```html
<a ew-href="xxx.html">XXX</a>
<a ew-href="xxx.html" ew-title="XXX管理">XXX</a>
```

> 注意：单标签模式下end回调无效。


## 2.4.关闭指定选项卡  :id=close_tab

```javascript
index.closeTab('xxx/xxx.html');
```

关闭已经打开的Tab选项卡，参数是Tab的url，单标签模式下此方法无效。

> url是必填项，如果要关闭当前选中的选项卡，请使用admin.closeThisTabs()


## 2.5.清除Tab记忆  :id=clear_tab

```javascript
index.clearTabCache();
```

&emsp;Tab记忆功能是刷新页面的时候可以恢复上次打开的所有Tab，此方法用于清除已经缓存的Tab。

> 注意：在实际项目中，登录成功后应该调用此方法清除Tab记忆


## 2.6.修改Tab标题  :id=set_tab

```javascript
index.setTabTitle('Hello');  // 修改当前Tab标题文字，也支持单标签模式

index.setTabTitle(title, tabId);  // 修改指定Tab标题文字，单标签模式始终是修改当前

index.setTabTitleHtml('<span>Hello</span>');  // 修改整个标题栏的html，只在单标签模式有效

index.setTabTitleHtml();  // 隐藏单标签模式的标题栏
```

关闭多标签时框架会自动生成一个标题栏，可使用此方法修改标题栏内容，参数为undefined时为隐藏标题栏。


## 2.7.切换Tab自动刷新  :id=auto_refresh

默认是关闭的，在设置界面可开启，如果要直接开启在index.html中添加如下代码：
```javascript
$('.layui-body>.layui-tab[lay-filter="admin-pagetabs"]').attr('lay-autoRefresh', 'true');
```
给layui-tab增加`lay-autoRefresh`属性为true即可，因为tab的dom是框架自动生成的，所以需要js添加。

如果要想只有部分Tab切换刷新：
```javascript
// 选项卡切换监听
element.on('tab(admin-pagetabs)', function (data) {
    var layId = $(this).attr('lay-id');
    if (layId == 'system/user') {
        admin.refresh(layId);
    }
});
```
这段代码写在index.html里面的loadHome方法后面即可。


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

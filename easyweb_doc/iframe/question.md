## 11.1.后台生成侧边栏  :id=back_side
这里用java的一个模板引擎beetl的语法示例：
```html
<ul class="layui-nav layui-nav-tree" lay-filter="admin-side-nav">
    
    <% for(menu in menus) { %>
    <li class="layui-nav-item">
        <a lay-href="${menu.menuUrl}"><i class="${menu.menuIcon}"></i>&emsp;<cite>${menu.menuName}</cite></a>
        <% if(menu.subMenus.~size>0) { %>
        <dl class="layui-nav-child">
            <% for(subMenu in menu.subMenus) { %>
            <dd>
                <a lay-href="${subMenu.menuUrl}">${subMenu.menuName}</a>
                <% if(subMenu.subMenus.~size>0) { %>
                <dl class="layui-nav-child">
                    <% for(temp in subMenu.subMenus) { %>
                    <dd><a lay-href="${temp.menuUrl}">${temp.menuName}</a></dd>
                    <% } %>
                </dl>
                <% } %>
            </dd>
            <% } %>
        </dl>
        <% } %>
    </li>
    <% } %>
</ul>
```


## 11.2.ajax加载侧边栏  :id=side_ajax

演示地址：[side-ajax.html](https://demo.easyweb.vip/iframe/page/example/side-ajax.html)

```html
<div class="layui-side">
    <div class="layui-side-scroll">
        <ul class="layui-nav layui-nav-tree" lay-filter="admin-side-nav"></ul>
    </div>
</div>
<script id="sideNav" type="text/html">
    {{#  layui.each(d, function(index, item){ }}
    <li class="layui-nav-item">
        <a lay-href="{{item.url}}"><i class="{{item.icon}}"></i>&emsp;<cite>{{item.name }}</cite></a>
        {{# if(item.subMenus&&item.subMenus.length>0){ }}
        <dl class="layui-nav-child">
            {{# layui.each(item.subMenus, function(index, subItem){ }}
            <dd>
                <a lay-href="{{ subItem.url }}">{{ subItem.name }}</a>
                {{# if(subItem.subMenus&&subItem.subMenus.length>0){ }}
                <dl class="layui-nav-child">
                    {{# layui.each(subItem.subMenus, function(index, thrItem){ }}
                    <dd><a lay-href="{{ thrItem.url }}">{{ thrItem.name }}</a></dd>
                    {{# }); }}
                </dl>
                {{# } }}
            </dd>
            {{# }); }}
        </dl>
        {{# } }}
    </li>
    {{#  }); }}
</script>
<script>
layui.use([ 'element', 'admin','laytpl'], function () {
    var $ = layui.jquery;
    var admin = layui.admin;
    var laytpl = layui.laytpl;
    var element = layui.element;
        
    $.get('json/side.json', function (res) {
        laytpl(sideNav.innerHTML).render(res.data, function (html) {
            $('[lay-filter=admin-side-nav]').html(html);
            element.render('nav');  // 这里非常重要
        });
    }, 'json');
});
</script>
```

json数据格式：

```json
{
  "code": 200,
  "msg": "",
  "data": [{
      "name": "系统管理",
      "icon": "layui-icon layui-icon-set",
      "url": "javascript:;",
      "subMenus": [
        {
          "name": "用户管理",
          "url": "system/user.html"
        }
      ]
    }
  ]
}
```


## 11.3.多系统模式  :id=more_side

&emsp;在header中有几个选项：“xx系统”、“xx系统”，点击不同的系统切换不同的侧边菜单，
在线演示：[side-more.html](https://demo.easyweb.vip/iframe/page/example/side-more.html)

侧边栏代码，使用`nav-id`标识不同的菜单：
```html
<div class="layui-side">
    <div class="layui-side-scroll">
        <!-- 系统一的菜单，每个ul都加nav-id -->
        <ul nav-id="xt1" class="layui-nav layui-nav-tree" lay-filter="admin-side-nav">
            <!-- ...省略代码... -->
        </ul>
        <!-- 系统二的菜单，加layui-hide隐藏 -->
        <ul nav-id="xt2" class="layui-nav layui-nav-tree layui-hide" lay-filter="admin-side-nav">
            <!-- ...省略代码... -->
        </ul>
    </div>
</div>
```

header.html代码，使用`nav-bind`绑定侧边栏菜单：

```html
<div class="layui-header">
    <ul class="layui-nav layui-layout-left">
        <li class="layui-nav-item"><a nav-bind="xt1">系统一</a></li>
        <li class="layui-nav-item"<a nav-bind="xt2">系统二</a></li>
    </ul>
</div>
```


## 11.4.弹窗下拉框出现滚动条  :id=dialog_scroll

非iframe类型的弹窗才能解决，解决办法如下：

```javascript
 admin.open({
        type: 1,
        title: '添加用户',
        content: $('#model').html(),
        success: function (layero, index) {
            // 禁止出现滚动条
            $(layero).children('.layui-layer-content').css('overflow', 'visible'); 
        }
    });
```

> 关键代码就是在success回调中写上面那句话


## 11.5.弹窗宽度不能超出屏幕  :id=dialog_overflow

这个是针对手机屏幕下做了让弹窗宽度自适应，如果你需要让弹窗宽度超出屏幕如下代码即可：


```javascript
admin.open({
        title: '添加用户',
        content: $('#model').html(),
        success: function (layero, index) {
            $(layero).css('max-width', 'unset');   // 去掉max-width属性
        }
});
```

> success回调中去掉max-width属性


## 11.6.表单文字出现换行  :id=form_text

layui的表单左边的标题文字最多显示5个字，超出会换行，通过添加css修改宽度：

```css
#userForm .layui-form-label {
    width: 100px;  // 这里修改标题宽度
}

#userForm .layui-input-block {
    margin-left: 130px;  // 这里要比上面始终大30px
}
```

`#userForm`是表单的id，加id避免影响其他表单样式：
```html
<form id="userForm" class="layui-form">
    <div class="layui-form-item">
        <label class="layui-form-label">活动起止时间</label>
        <div class="layui-input-block">
            <input type="text" class="layui-input"/>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">活动详细介绍</label>
        <div class="layui-input-block">
            <textarea class="layui-textarea" maxlength="200"></textarea>
        </div>
    </div>
</form>
```

## 11.7.侧边栏折叠图标放大  :id=side_icon
侧边栏折叠后图标会进行放大，如果要修改大小，添加如下css：
```css
@media screen and (min-width: 750px) {
    .layui-layout-admin.admin-nav-mini .layui-side .layui-nav .layui-nav-item > a > .layui-icon {
        font-size: 18px;
    }
}
```
&emsp;修改font-size即可，如果不想放大，改成14px即可。


## 11.8.select、radio不显示  :id=select_none
select(下拉框)、radio(单选框)等表单元素在layui中会被美化，对于动态生成的元素需要重新渲染才能美化：
```javascript
$('div').appen('<select><option value="1">xxxx</option></select>');
form.render('select');  // 重新渲染select
form.render('radio');  // 重新渲染radio

// 对于弹窗内select不显示
admin.open({
    type: 1,
    content: '<select><option value="1">xxxx</option></select>',
    success: function(){
        form.render('select');  // 弹窗要在success里重新渲染
    }
});
```


## 11.9.IOS下表格不能自适应  :id=ios_table

给子页面的body加一个样式，固定body的宽高即可解决：
```html
<style>
    .ew-iframe-body {
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        overflow: auto;
    }
</style>
<body class="ew-iframe-body">
</body>
```
每个子页面都需要加，这段样式会在3.1.6版本开始加入到admin.css中，3.1.6版本之后你只用给body加上这个class即可。

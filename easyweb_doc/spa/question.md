## 11.1.跨页面操作  :id=page_oper
单页面是可以直接跨页面操作的：
```javascript
table.reload('userTab', {});  // 刷新其他页面表格
```

layui.use里面定义的变量想在其他页面访问：
```javascript
layui.use(['layer'], function() {
    
    window.xxx = 'xxxx';  // 变量给其他页面访问
    
    // 方法给其他页面访问
    window.xxx = function(){
        alert('aa');
    };
});
```


## 11.2.nginx部署解决跨域  :id=nginx_cors
部署只需要把前端所有代码放在nginx的html中，修改config.js的base_server为接口地址即可，
如果接口没有做跨域支持，可使用nginx反向代理来解决跨域问题：

1. 打开“nginx/conf/nginx.conf”配置文件
2. 设置反向代理：
    ```
    http {
        server {
            # 加入以下配置，之前的配置全部不要动，这个location是新加入的
            location /api/ {
                proxy_pass  http://11.11.111.111:8088/; # 这个是后台接口所在的地址
            }
        }
    }
    ```
3. 修改config.js里面的base_server为/api/。

> 这样配置接口就会访问localhost:80/api/，然后nginx再反向代理到实际接口


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
select、radio在layui中会被美化，对于动态生成的元素需要重新渲染才能美化：
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

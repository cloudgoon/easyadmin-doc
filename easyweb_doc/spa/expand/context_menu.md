## 7.6.1.快速使用  :id=start
```javascript
layui.use(['contextMenu'], function () {
    var contextMenu = layui.contextMenu;
    
    // 重写整个页面右键菜单
    contextMenu.bind('html', [{
        icon: 'layui-icon layui-icon-snowflake',
        name: '菜单一',
        click: function (e, event) {
            // 通过$(event.currentTarget)可获取事件触发的元素
            alert('点击了菜单一');
        }
    }, {
        name: '菜单二',
        click: function (e) {
            alert('点击了菜单二');
        }
    }]);
});
```

- 参数一： 绑定元素；
- 参数二：菜单数组；

菜单数组可支持无限极：
```javascript
[{
    icon: 'xxxxx',
    name: '菜单三',
    subs: [{
        icon: 'xxxxx',
        name: '子菜单一',
        click: function (e, event) {
            alert('点击了子菜单一');
        }
    }]
}]
```

- icon： &emsp; 图标
- name： &emsp; 菜单名
- click： &emsp; 点击事件
- subs： &emsp; 子菜单(支持无限极)

!> click事件里面的e是右键菜单的事件对象，event才是绑定的目标元素的事件对象


## 7.6.2.自定义使用  :id=div
```javascript
// 用于点击事件
$('#btn').click(function (e) {
    var x = $(this).offset().left;
    var y = $(this).offset().top + $(this).outerHeight();
    contextMenu.show([{
        name: '按钮菜单一',
        click: function () {
        }
    }], x, y, e);
    e.preventDefault();
    e.stopPropagation();
});
```

你可以自己绑定事件，通过show方法显示出来，contextMenu.show(item, x, y, e)方法参数说明：

- 参数一： 菜单数组
- 参数二： x坐标；
- 参数三： y坐标；
- 参数四： e


## 7.6.3.动态元素绑定  :id=show
对于动态生成的元素可以使用事件委托的方式来绑定：
```javascript
// 对.btn元素绑定鼠标右键
$ (document). bind('contextmenu', '.btn', function (e) {
    contextMenu.show([{
        icon: 'layui-icon layui-icon-set',
        name: '删除',
        click: function (e, event) {
            layer.msg('点击了刪除');
        }
    }],e. clientX, e. clientY, e);
    return false;
});
```
你需要自己写contextmenu事件绑定，然后使用contextMenu的show方法显示出来。

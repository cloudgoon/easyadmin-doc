## 7.5.1.快速使用  :id=start
&emsp;大多数的后台系统页面全是数据表格，表格看多了，难免喜欢其他布局，例如网格、瀑布流、卡片列表等形式，
如果使用这些布局，分页、自动渲染、搜索、排序等功能都需要自己实现，相比数据表格，大大增加工作量，
dataGrid插件就是对非表格形式的列表页面实现自动分页、自动渲染的功能。

```html
<div id="demoGrid"></div>

<!-- 模板 -->
<script type="text/html" id="demoGridItem">
    <div>
        <h2>{{d.title}}</h2>
        <p>{{d.content}}</p>
        <a lay-event="edit">修改</a>
    </div>
</script>

<script>
layui.use(['dataGrid'], function () {
    var dataGrid = layui.dataGrid;

    // 渲染
    var ins = dataGrid.render({
        elem: '#demoGrid',  // 容器
        templet: '#demoGridItem',  // 模板
        data: '../../../json/data-grid.json',  // 数据接口
        page: {limit: 5},  // 开启分页
        onItemClick: function (obj) { 
            // item点击事件监听
        },  
        onToolBarClick: function (obj) { 
            // item子元素点击事件监听
        }
    });

});
</script>
```

使用方法与数据表格table类似。


## 7.5.2.事件监听  :id=event
```javascript
dataGrid.render({
    // item点击事件监听
    onItemClick: function (obj) { 
        var index = obj.index;
        console.log('点击了第' + (index + 1) + '个');
    },  
    onToolBarClick: function (obj) { 
        // item子元素点击事件监听
        var event = obj.event;
        console.log(event);
    }
});
```
onToolBarClick就是item里面加了lay-event属性的子元素的点击事件。

**obj详细介绍：**
```javascript
obj.data;           // 当前操作的数据对象
obj.index;          // 当前操作的数据索引
obj.elem;           // 当前操作的dom元素
obj.event;          // 当前操作的lay-event值
obj.e;              // 原生点击事件的e

obj.del();          // 删除当前item
obj.update(o);      // 修改当前item
```

**obj.update()用法：**

```javascript
dataGrid.render({
    onToolBarClick: function (obj) { 
        var data = obj.data;
        data.title = '我是新的值';
        obj.update(data);
    }
});
```

## 7.5.3.分页功能  :id=page
```javascript
dataGrid.render({
    elem: '#demoGrid',  // 容器
    templet: '#demoGridItem',  // 模板
    data: '../../../json/data-grid.json',  // 数据接口
    page: {limit: 5}  // 开启分页
});
```
page参数同layui分页组件的参数一致，[前往查看](https://www.layui.com/doc/modules/laypage.html#options)。


## 7.5.4.加载更多功能  :id=load_more
```javascript
dataGrid.render({
    elem: '#demoGrid',  // 容器
    templet: '#demoGridItem',  // 模板
    data: '../../../json/data-grid.json',  // 数据接口
    loadMore: {limit: 5}  // 开启加载更多
});
```
loadMore的可选配置：

参数 | 说明 | 默认值
:--- | :--- | :---
class | 自定义class | ''
limit | 每页多少条 | 10
text | 显示文字 | '加载更多'
loadingText | 加载中文字 | '加载中...'
noMoreText | 无数据文字 | '没有更多数据了~'
errorText | 加载失败文字 | '加载失败，请重试'

> loadMore和page只能二选一


## 7.5.5.重载功能  :id=reload
```javascript
var ins = dataGrid.render({});

ins.reload();  // 重载

ins.reload({page: {curr: 2} });  // 跳到第二页
```


## 7.5.6.渲染完成的回调  :id=done
```html
dataGrid.render({
    done: function(dataList, curr, count) {
        dataList;  // 当前页数据
        curr;      // 当前第几页
        count;     // 总数量
    }
});
```


## 7.5.7.自动渲染  :id=auto_render
```html
<div id="demoGrid" lay-data="{url: 'json/data-grid.json', page: {limit: 5} }" data-grid>
    <script type="text/html" data-grid-tpl>
        <div>
            <h2>{{d.title}}</h2>
            <p>{{d.content}}</p>
            <a lay-event="edit">修改</a>
        </div>
    </script>
</div>
```
通过添加data-grid和data-grid-tpl属性，dataGrid组件便会自动渲染，通过lay-data属性进行参数配置。


## 7.5.8.绑定事件  :id=bind_event
```javascript
// 绑定item事件
dataGrid.onItemClick('demoGrid', function(obj){
    console.log(obj);
});

// 绑定item子元素事件
dataGrid.onToolBarClick('demoGrid', function(obj){
    console.log(obj);
});
```

参数一是容器的id，这种方式绑定事件的优先级比直接在渲染的时候绑定事件低。


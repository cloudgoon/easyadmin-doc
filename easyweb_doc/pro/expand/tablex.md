## 7.3.1.全部方法  :id=method

 方法 | 参数 | 描述
:--- | :--- | :---
merges(tableId, indexs, fields) | 见单独说明 | 合并单元格
bindCtxMenu(tableId, items) | 见单独说明 | 给表格行绑定鼠标右键
render(object) | 同layui表格 | 渲染表格，带后端排序功能
renderFront(object) | 同layui表格 | 渲染表格，前端分页、排序、搜索
exportData(object) | 见单独说明 | 导出任意数据为excel
 | 
loadUrl(object, callback) | 见单独说明 | 请求表格数据
parseTbData(cols, dataList, overwrite) | 见单独说明 | 获取解析templet后数据
filterData(dataList, searchName, searchValue) | 见单独说明 | 前端搜索数据

查看[在线演示](https://demo.easyweb.vip/iframe/page/plugin/advance/tableX.html)

## 7.3.2.合并单元格  :id=merge

```javascript
layui.use(['tableX'], function () {
    var tableX = layui.tableX;
    
    table.render({
        elem: '#xTable2',
        url: '../../json/tablex1.json',
        cols: [[
            {type: 'numbers'},
            {field: 'parentName', title: '模块名称', sort: true},
            {field: 'authorityName', title: '菜单名称', sort: true}
        ]],
        done: function () {
            tableX.merges('xTable2', [1]);  // 在done回调里面调用
        }
    });
});
```

参数说明：
```javascript
tableX.merges('xTable2', [1]);  // 合并第2列相同的单元格
tableX.merges('xTable2', [1], ['parentName']);  // 合并第2列相同的单元格
tableX.merges('xTable2', [1, 2]);   // 合并第2、3列相同的单元格
tableX.merges('xTable2', [1, 2], ['parentName', 'authorityName']);   // 合并第2、3列相同的单元格

tableX.merges('xTable2', [1, 2], false);   // 在后面加一个false可解决排序冲突的问题
```
- 参数一 &emsp; 必填 &emsp;&emsp; 表格的lay-filter
- 参数二 &emsp; 必填 &emsp;&emsp; 要合并列的索引，数组类型
- 参数三 &emsp; 非必填 &emsp; 根据数据字段判断是否相同，为空时根据单元格的html内容判断


## 7.3.3.行绑定鼠标右键  :id=ctx

```javascript
table.render({
    elem: '#xTable3',
    url: '../../json/user.json',
    cols: [[
        {field: 'nickName', title: '用户名', sort: true},
        {field: 'sex', title: '性别', sort: true}
    ]],
    done: function () {
        // 在done回调里面绑定
        tableX.bindCtxMenu('xTable3', [{
            icon: 'layui-icon layui-icon-edit',
            name: '修改此用户',
            click: function (d) {
                layer.msg('点击了修改，userId：' + d.userId);
            }
        }, {
            icon: 'layui-icon layui-icon-close text-danger',
            name: '<span class="text-danger">删除此用户</span>',
            click: function (d) {
                layer.msg('点击了删除，userId：' + d.userId);
            }
        }]);
    }
});
```

- 参数一 &emsp; 表格的lay-filter
- 参数二 &emsp; 右键菜单
    - icon &emsp;&nbsp;&nbsp; 图标
    - name &emsp; 标题
    - click &emsp;&nbsp;&nbsp; 点击事件，d是当前行的数据


## 7.3.4.后端排序  :id=render

```javascript
tableX.render({
    elem: '#xTable3',
    url: '../../json/user.json',
    cols: [[
        {type: 'numbers'},
        {field: 'nickName', title: '用户名', sort: true},
        {field: 'sex', title: '性别', sort: true}
    ]]
});
```
&emsp;仅仅是把`table.render`换成`tableX.render`就会在点击排序时自动传递`sort`和`order`参数，例如：user?page=1&limit=10&sort=sex&order=asc。

- sort &emsp;&nbsp;&nbsp; 排序字段，值是点击排序列的field
- order &emsp; 排序方式，升序是asc，降序是desc

> 注意：如果写了sort: true开启排序，一定要写field: 'xxx'。


## 7.3.5.前端分页排序  :id=render_f

```javascript
tableX.renderFront({
    elem: '#xTable1',
    url: '../../json/userAll.json',
    page: { groups: 6 },
    cols: [[
        {type: 'checkbox'},
        {field: 'nickName', title: '用户名', sort: true},
        {field: 'sex', title: '性别', sort: true}
    ]]
});
```
&emsp;仅仅是把`table.render`换成`tableX.renderFront`就可以有前端分页和排序功能了，参数跟layui表格的参数一摸一样，url方式你的接口可以返回全部数据，前端来分页，也支持data方式。

**前端模糊搜索：**
```html
<input tb-search="xTable1" class="layui-input icon-search" type="text"/>
```
&emsp;页面中加入上面的输入框，通过`tb-search`关联表格，就实现了对表格的模糊搜索功能，你可以对该input加任意样式，放在任意位置。

&emsp;还可以增加name属性来设置搜索时只搜索某些字段，多个字段通过逗号分隔：
```html
<input tb-search="xTable1" name="sex,phone" class="layui-input icon-search" type="text"/>
```

**刷新功能：**
```html
<!-- 通过tb-refresh绑定刷新按钮 -->
<button tb-refresh="xTable1" class="layui-btn">刷新</button>
```
&emsp;也可以通过js刷新：
```javascript
var insTb = tableX.renderFront('.....省略');

insTb.reloadUrl();  // url方式刷新

insTb.reloadData({data: dataList, page: {curr: 1}});  // data方式的刷新
```

> 注意：url方式的前端分页使用reloadUrl()方法，reloadUrl()方法暂时不支持加参数，reloadData方法跟table.reload()方法参数一致。

**前端排序：**

&emsp;前端排序如果有field字段，会根据field字段的值来排序，如果没有field有templet会根据templet转换后的值排序，
templet可能会返回表单元素，比如switch开关等，这些元素根本无法用来排序，那么可以通过`export-show`和`export-hide`这两个东西来控制。

```html
<!-- 开关，state=0开关打开,state=1开关关闭 -->
<script type="text/html" id="tableState">
    <input type="checkbox" lay-skin="switch" lay-text="正常|锁定" lay-filter="ckState" value="{{d.userId}}" {{d.state==0?'checked':''}}/>
    <div class="export-show">{{d.state==0?'正常':'锁定'}}</div>
</script>
<!-- 图标，state=0显示ok的图标，state=1显示叉叉的图标 -->
<script type="text/html" id="tableState">
    <div class="export-hide">{{d.state==0?'<i class="layui-icon layui-icon-ok"></i>':'<i class="layui-icon layui-icon-close"></i>'}}</div>
    <div class="export-show">{{d.state==0?'正常':'锁定'}}</div>
</script>
<!-- 图标和开关没法用于排序，可以写两份，一份用于表格展示，一份用于排序和导出功能 -->
```

- `export-hide`用于表格展示，但是对排序和导出时屏蔽
- `export-show`用于排序和导出功能获取单元格数据，但是对表格展示时屏蔽

> 排序建议最好是通过field来根据数据的字段排序，field不支持d.xxx.xxx这种嵌套的对象，就需要用上面的方法


## 7.3.6.导出数据  :id=export

```javascript
tableX.exportData({
    cols: insTb.config.cols,   // 表头配置
    data: table.cache.xTable3,  // 数据，支持url方式
    fileName: '用户表'          // 文件名称
});
```

参数 | 必填 | 说明 | 默认
:--- | :--- | :--- | :---
cols | 是 | 表头配置 | 
data | 是 | 导出的数据，支持数组和string的url | 
fileName | 否 | 导出的文件名称 | table
expType | 否 | 导出的文件类型 | xls
option | 否 | url方式的配置 | 

&emsp;如果data是string类型会把data当url请求数据，option是请求的配置，跟表格的配置一样，配置method、where、headers等，
接口返回的格式也要跟表格一样包含code、count、data等信息。

&emsp;cols的配置也跟表格一样，是一个多维数组，可以通过`insTb3.config.cols`来获取表格的cols，也可以重写。

**导出的数据会包含templet转换：**
```javascript
tableX.renderFront({
    elem: '#xTable1',
    url: '../../json/userAll.json',
    page: { groups: 6 },
    cols: [[
        {type: 'checkbox'},
        {field: 'nickName', title: '用户名', sort: true},
        {field: 'sex', title: '性别', sort: true},
        {
            title: '状态', templet: function (d) {
                 var stateStr = ['正常', '冻结', '欠费'];
                 return stateStr[d.state];
            }
        }
    ]]
});
```
&emsp;像上面这种state数据存的是0和1，但是显示是文字，导出数据会自动转成文字(layui2.5后自带的带出也包含此功能)。

&emsp;如果列是表单元素，比如switch开关，导出的方法：

```html
<!-- 状态列 -->
<script type="text/html" id="tableState">
    <input type="checkbox" lay-skin="switch" lay-text="正常|锁定" lay-filter="ckState" value="{{d.userId}}" {{d.state==0?'checked':''}}/>
    <div class="export-show">{{d.state==0?'正常':'锁定'}}</div>
</script>
<script>
layui.use(['tableX'], function () {
    var tableX = layui.tableX;
    
    tableX.render({
        elem: '#xTable3',
        url: '../../json/user.json',
        cols: [[
            {field: 'nickName', title: '用户名', sort: true},
            {field: 'phone', title: '手机号', sort: true},
            {field: 'state', templet: '#tableState', title: '状态', sort: true}
        ]]
    });
});
</script>
```

&emsp;通过加一个`export-show`类可以保证里面的内容只作为导出的时候用，不作为表格展示，还可以加`export-hide`来设置导出的时候屏蔽的内容。

```html
<!-- 图标，state=0显示ok的图标，state=1显示叉叉的图标 -->
<script type="text/html" id="tableState">
    <div class="export-hide">{{d.state==0?'<i class="layui-icon">&#xe605;</i>':'<i class="layui-icon">&#x1006;</i>'}}</div>
    <div class="export-show">{{d.state==0?'正常':'锁定'}}</div>
</script>
<!-- 不可能把图标的unicode字符导出吧，那就写两份，一份用于表格展示，一份用于排序和导出功能 -->
```

- `export-hide`用于表格展示，但是对排序和导出时屏蔽
- `export-show`用于排序和导出功能获取单元格数据，但是对表格展示时屏蔽


## 7.3.7.导出全部、搜索  :id=export_adv

**导出当前页数据：**
```javascript
tableX.exportData({
    cols: insTb.config.cols,
    data: table.cache.xTable3,
    fileName: '用户表'
});
```
insTb是表格渲染后的实例，table.cache获取表格当前页的数据。

**导出全部数据：**
```javascript
tableX.exportData({
    cols: insTb.config.cols,
    data: 'listAll.json',
    fileName: '用户表'
});
```
导出全部数据后端再写一个查询全部的接口，导出时只需要把data写接口地址即可。

**导出搜索后的全部数据：**
```javascript
tableX.exportData({
    cols: insTb.config.cols,
    data: 'listAll.json',
    option: {
        where: lastWhere
    },
    fileName: '用户表'
});
```
在options里面加一个where把搜索条件传给后端，搜索条件可以这样获取：
```javascript
var lastWhere;
// 搜索
form.on('submit(formSubSearchUser)', function (data) {
    lastWhere = data.field;
    insTb.reload({where: data.field}, 'data');
});
```
在表格的搜索里面把搜索条件传给lastWhere。

**cols也可以重写：**
```javascript
tableX.exportData({
    cols: [[
        {field: 'username', title: '账号'},
        {field: 'nickName', title: '用户名'}
    ]],
    data: table.cache.xTable3,
    fileName: '用户表'
});
```


## 7.3.8.后端导出  :id=export_back
后端导出一般是只用window.open(url)即可，如果要传递参数，并且要post提交，那就麻烦了，tableX进行了完美的封装：
```javascript
tableX.exportDataBack('user/export', {sex: '男'});

tableX.exportDataBack('user/export', {sex: '男'}, 'post');
```

- 参数一： &emsp; 导出的url
- 参数二： &emsp; 参数
- 参数三： &emsp; 请求方式

如果是get方式，会使用?和&拼接参数，如果是post方式，会创建一个隐藏的表单来提交，如果你的参数有复杂的类型，比如json格式，建议用post的形式

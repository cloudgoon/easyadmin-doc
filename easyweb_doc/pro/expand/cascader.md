## 6.3.1.快速使用  :id=start
```html
<input id="demoCascader1" placeholder="请选择" class="layui-hide"/>

<script>
layui.use(['cascader'], function () {
    var $ = layui.jquery;
    var cascader = layui.cascader;

    cascader.render({
        elem: '#demoCascader1',
        data: [{
              value: 'beijing',
              label: '北京',
              children: [
                  {
                      value: 'gugong',
                      label: '故宫'
                  },
                  {
                      value: 'tiantan',
                      label: '天坛'
                  },
                  {
                      value: 'wangfujing',
                      label: '王府井'
                  }
              ]
          }]
    });
});
</script>
```

![](https://s2.ax1x.com/2019/08/28/mHdhnJ.png)


## 6.3.2.异步加载  :id=async
```html
<input id="demoCascader1" placeholder="请选择" class="layui-hide"/>

<script>
layui.use(['cascader'], function () {
    var $ = layui.jquery;
    var cascader = layui.cascader;

    cascader.render({
        elem: '#demoCascader1',
        reqData: function (values, callback, data) {
            // values是当前所有选中的值，data是当前选中的对象
            $.get('xxxx.json', { id: data.value }, function(res){
                callback(res.data);  // 数据请求完成通过callback回调
            },'json');
        }
    });
});
</script>
```
异步加载的数据格式为：
```
[
    {value: 'beijing', label: '北京', haveChildren: true},
    {value: 'jiangsu', label: '江苏', haveChildren: true}
]
```
通过haveChildren字段来标识是否还有子节点，如果你的后台数据格式不是这样，可以在callback之前格式化：
```js
cascader.render({
    elem: '#demoCascader1',
    reqData: function (values, callback, data) {
        // values是当前所有选中的值，data是当前选中的对象
        $.get('xxxx.json', { id: data.value }, function(res){
	        var newList = [];
            for(var i=0;i<res.data.length;i++){
                  var item = res.data[i];
                  newList.push({
                       value: item.id,
                       label: item.name,
                       haveChildren: item.haveChildren
                  });
            }
            callback(newList);  // 数据请求完成通过callback回调
        },'json');
    }
});
```

## 6.3.3.自定义分隔符  :id=format
```html
<input id="demoCascader1" placeholder="请选择" class="layui-hide"/>

<script>
layui.use(['cascader'], function () {
    var $ = layui.jquery;
    var cascader = layui.cascader;

    cascader.render({
        elem: '#demoCascader1',
        data: [],
        renderFormat: function (labels, values) {
            return labels.join(' / ');  // 默认是用斜杠分割，可以自定义
        }
    });
});
</script>
```

## 6.3.4.搜索功能  :id=search
```html
<input id="demoCascader1" placeholder="请选择" class="layui-hide"/>

<script>
layui.use(['cascader'], function () {
    var $ = layui.jquery;
    var cascader = layui.cascader;

    cascader.render({
        elem: '#demoCascader1',
        data: [],
        filterable: true   // 这个参数是开启搜索功能
    });

    // 自定义搜索
    cascader.render({
        elem: '#demoCascader1',
        data: [],
        filterable: true,
        reqSearch: function (keyword, callback, dataList) {
            // keyword是搜索的关键字，dataList是当前的全部数据集合
            $.get('xxx.json', { keyword: keyword }, function(res){
                callback(res);
            },'json')
        }
    });
});
</script>
```
搜索后端接口返回的数据格式为:
```json
[ 
  {"value": "1,2", "label": "北京 / 王府井"},
  {"value": "1,3", "label": "北京 / 故宫"}
]
```
如果要标记关键字为红色：`<span class=\"search-keyword\">北京</span> / 王府井`

![](https://s2.ax1x.com/2019/08/28/mHd4B9.png)


## 6.3.5.省市区级联选择  :id=city
```html
<input id="demoCascader1" placeholder="请选择" class="layui-hide"/>

<script type="text/javascript" src="/assets/module/cascader/citys-data.js"></script>
<script>
layui.use(['cascader'], function () {
    var $ = layui.jquery;
    var cascader = layui.cascader;

    cascader.render({
        elem: '#demoCascader1',
        data: citysData,
        itemHeight: '250px',
        filterable: true
    });
});
</script>
```
省市区的数据已经封装好了，只需要引入数据的js即可，项目里面的数据value是区号，如果你的数据库直接存中文，可以使用下面的数据：

[citys-data.js](http://whvse.gitee.io/html-test/file/citys-data.js) 未压缩(311kb)、[citys-data.min.js](http://whvse.gitee.io/html-test/file/citys-data.min.js) 压缩(136kb)。

如果想去掉区域，只显示省和市，可以用js处理一下数据：
```javascript
// 去掉第三级的数据
for (var i = 0; i < citysData.length; i++) {
    for (var j = 0; j < citysData[i].children.length; j++) {
        delete citysData[i].children[j].children;
    }
}
cascader.render({
    elem: '#demoCascader11',
    data: citysData
});
```

如果一个页面同时有省市和省市区选择，去掉第三级的时候应该克隆一下数据：
```javascript
var citysData2 = admin.util.deepClone(citysData);
for (var i = 0; i < citysData2.length; i++) {
    for (var j = 0; j < citysData2[i].children.length; j++) {
        delete citysData2[i].children[j].children;
    }
}
cascader.render({
    elem: '#demoCascader11',
    data: citysData2
});
```


## 6.3.6.全部方法  :id=method
```html
<input id="demoCascader1" placeholder="请选择" class="layui-hide"/>

<script>
layui.use(['cascader'], function () {
    var $ = layui.jquery;
    var cascader = layui.cascader;

    var ins1 = cascader.render({
        elem: '#demoCascader1',
        data: []
    });
    
    ins1.data;  // 获取当前的数据
    ins1.open();  // 展开
    ins1.hide();  // 关闭
    ins1.removeLoading();  // 移除加载中的状态
    ins1.setDisabled(true);  // 禁用或取消禁用
    ins1.getValue();  // 获取选中的数据值
    ins1.getLabel();  // 选取选中的数据名称
    ins1.setValue('1,2');   // 设置值
    
});
</script>
```


## 6.3.7.全部参数  :id=options

参数名称 | 介绍 | 默认值
--- | --- | ---
elem | 需要渲染的元素 | 
data | 数据 | 
clearable | 是否开启清除 | true
clearAllActive | 清除时清除所有列选中 | false
trigger | 次级菜单触发方式，可选'hover' | 'click'
disabled | 是否禁用 | false
changeOnSelect | 是否点击每一项都改变值 | false
filterable | 是否开启搜索功能 | false
notFoundText | 搜索为空是提示文字 | '没有匹配数据'
itemHeight | 下拉列表的高度 | '180px'
reqData(values, callback, data) | 异步获取数据的方法 | 
reqSearch(keyword, callback, dataList) | 自定义搜索的方法 | 
renderFormat(labels, values) | 选择后用于展示的函数 | 
onChange(values, data) | 数据选择改变的回调 | 
onVisibleChange(isShow) | 展开和关闭的回调 | 


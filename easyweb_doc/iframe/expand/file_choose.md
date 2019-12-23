## 7.2.1.快速使用  :id=start
```javascript
layui.use(['fileChoose'], function () {
    var fileChoose = layui.fileChoose;
    
    fileChoose.open({
        fileUrl: '',  // 文件查看的url
        listUrl: '../template/file/files.json',  // 文件列表的url
        where: {
          access_token: 'xxxxxx'  
        },
        num: 3,  // 最多选择数量
        dialog: {
            offset: '60px'
        },
        onChoose: function (urls) {
            layer.msg('你选择了：' + JSON.stringify(urls), {icon: 1});
        }
    });
});
```

## 7.2.2.全部参数  :id=options

参数 | 描述 | 默认值
--- | --- | ---
fileUrl | 文件查看的url | 
listUrl | 文件列表的url | 
where | 文件列表请求参数 | {}
num | 文件选择的数量 | 1
onChoose | 选择后回调 | 
upload | 文件上传配置(同layui配置) | {}
dialog | 弹窗配置(同layui配置) | {}
menu | 点击弹出的菜单 | 数组类型
menuClick | 菜单点击事件处理 | 
response | 接口数据格式化 | 

**菜单配置及点击事件：**
```javascript
fileChoose.open({
    menu: [{
        name: '预览',
        event: 'preview'
    }, {
        name: '复制',
        event: 'copy'
    }, {
        name: '<span style="color: red;">删除</span>',
        event: 'del'
    }],
    menuClick: function(event, item) {
        // event  事件名称
        // item   当前数据
    }
});
```
&emsp;name菜单项名称，event点击事件名称

**接口数据格式化：**
```javascript
fileChoose.open({
    response: {
        method: 'get',  // 请求方式
        code: 0,  // 成功码，默认200
        name: 'name',  // 文件名称字段名称
        url: 'url',  // 文件url字段名称
        smUrl: 'smUrl',  // 文件缩略图字段名称
        isDir: 'isDir',  // 是否是文件夹字段名称,boolean类型
        dir: 'dir'  // 当前文件夹参数名称
    }
});
```
接口数据返回的格式需要为：
```json
{
    "code": 200,
    "msg": "请求成功",
    "data": [
        {
            "name": "图片一",
            "url": "2019/07/11/001.png",
            "smUrl": "sm/2019/07/11/001.png",
            "isDir": false
        }
    ]
}
```
&emsp;code、msg、data是必须按这个名字的，name、url、smUrl、isDir这几个字段的名称可以通过response参数配置，也可以加其他字段，
比如id、create_time等，这些字段会在菜单点击事件和选择回调事件中返回。

&emsp;如果你的接口返回的数据不是code、msg，是其他的，比如status、message，可以使用parseData参数格式化：
```javascript
fileChoose.open({
    response: {
        parseData: function(res){
            return {
                code: res.status,
                msg: res.message,
                data: res.list
            }
        }
    }
});
```

&emsp;如果是文件夹，点击文件夹会重新请求接口，并且传递文件夹的名称，传递的字段名称可以通过response.dir修改。

&emsp;不同文件显示不同的图标是前端根据文件url的后缀名称来判断的，在之前版本是服务器根据文件的content-type判断的。

![](https://s2.ax1x.com/2019/08/29/mLIkon.png)

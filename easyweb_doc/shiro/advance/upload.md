## 7.1.1.全部方法     :id=method
框架已经内置了FileController.java用于上传文件、查看文件、下载文件。

接口 | 参数 | 方法 | 说明
:--- | :--- | :--- | :---
file/upload | file | post | 上传文件
file/uploadBase64 | base64 | post | 上传文件
file/{y}/{m}/{d}/{file} | 无 | get | 查看原文件
file/sm/{y}/{m}/{d}/{file} | 无 | get | 查看缩略图
file/download/{y}/{m}/{d}/{file} | 无 | get | 下载原文件
file/list | dir,exts | get | 查看文件列表
file/del  | file | get | 删除文件

> 文件上传位置默认在第一个磁盘的/upload/目录下，缩略图文件位于/upload/sm/目录下，
> 缩略图的生成规则为第一次访问缩略图的url并且是图片类型的文件时才去生成缩略图，如果原图小于100kb不生成缩略图直接读取原图

## 7.1.2.上传文件     :id=upload
file形式：
```html
<form action="file/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file" />
    <button type="submit">上传</button>
</form>
```
> 实际项目建议用ajax提交表单

base64形式：
```javascript
$.post('file/upload', {
    base64: 'data:image/png;base64,xJsfek3hJHSfejhj6sdakjed=='
}, function(res){
    console.log(res);
}, 'json');
```

文件上传接口返回的是json数据，格式为：
{"code": 200, "msg": "上传成功", "url": "2019/08/31/xxx.png"}


## 7.1.3.查看、下载文件     :id=view
```javascript
// 查看原文件
window.open('file/2019/08/31/xxx.png');

// 查看缩略图
window.open('file/sm/2019/08/31/xxx.png');

// 下载原文件
window.open('file/download/2019/08/31/xxx.png');
```

> 查看文件如果是图片、pdf、视频等类型浏览器会预览，下载文件不管是什么类型都直接下载不预览。


## 7.1.4.结合layui上传     :id=layui
```html
<button id="btnUpload">上传</button>
<img id="imgView" src=""/>

<script>
layui.use(['layer', 'upload'], function () {
    var $ = layui.jquery;
    var layer = layui.layer;
    var upload = layui.upload;
    
    upload.render({
        elem: '#btnUpload',
        url: '/file/upload',
        before: function(obj){  // 上传前回调
            layer.load(); 
        },
        done: function (res) {  // 上传完毕回调
            layer.closeAll('loading');
            if (res.code == 200) {
                layer.msg(res.msg, {icon: 1});
                $('#imgView').attr('src', 'file/' + res.url); 
            } else {
                layer.msg(res.msg, {icon: 2});
            }
        },
        error: function () {  // 请求异常回调
            layer.closeAll('loading');
            layer.msg('上传失败', {icon: 2});
        }
    });
});
</script>
```


## 7.1.5.在表单中使用     :id=form

```html
<form class="layui-form model-form" id="demoForm">
    <div class="layui-form-item">
        <label class="layui-form-label">课程名称:</label>
        <div class="layui-input-block">
            <input name="courseName" class="layui-input"/>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">课程封面:</label>
        <div class="layui-input-block">
            <input name="cover" class="layui-input" disabled="disabled" style="padding-right: 70px"/>
            <button style="position: absolute;right: 0;top: 0;border-top-left-radius: 0;border-bottom-left-radius: 0;"
                    class="layui-btn" type="button" id="btnUpload">上传
            </button>
        </div>
    </div>
    <div class="layui-form-item">
        <label class="layui-form-label">课程简介:</label>
        <div class="layui-input-block">
            <input name="desc" class="layui-input"/>
        </div>
    </div>
    <div class="layui-form-item">
        <div class="layui-input-block text-right">
            <button class="layui-btn layui-btn-primary" type="button" ew-event="closeDialog">取消</button>
            <button class="layui-btn" lay-filter="submitDemo" lay-submit>保存</button>
        </div>
    </div>
</form>

<script>
layui.use(['layer', 'upload', 'form'], function () {
    var $ = layui.jquery;
    var layer = layui.layer;
    var upload = layui.upload;
    var form = layui.form;
    
    // 监听提交
    form.on('submit(submitDemo)', function (data) {
        console.log(data.field);
        return false;
    });
    
    // 渲染上传按钮
    upload.render({
        elem: '#btnUpload',
        url: '/file/upload',
        before: function(obj){  // 上传前回调
            layer.load(); 
        },
        done: function (res) {  // 上传完毕回调
            layer.closeAll('loading');
            if (res.code == 200) {
                layer.msg(res.msg, {icon: 1});
                $('#demoForm input[name="cover"]').val(res.url); 
            } else {
                layer.msg(res.msg, {icon: 2});
            }
        },
        error: function () {  // 请求异常回调
            layer.closeAll('loading');
            layer.msg('上传失败', {icon: 2});
        }
    });
});
</script>
```

![](https://s2.ax1x.com/2019/08/31/mvOMRg.png)

> 建议表单内上传文件像上面这样，先上传获取url，提交表单时只是把上传后的url提交，而不是传统的表单提交又有文件、
> 又有其他参数，当然这样做会存在文件上传了，表单不提交导致的垃圾文件的问题，现在硬盘已经很便宜了，这个问题可以忽略不记，
> 或者定时清理没有使用的文件即可

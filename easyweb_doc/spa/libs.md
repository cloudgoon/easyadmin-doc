## 8.1.鼠标滚轮监听  :id=mousewheel
模块名：mousewheel，使用方式：
```javascript
layui.use(['mousewheel'], function () {
    var $ = layui.jquery;

    // 滚动监听
    $('#xxx').on('mousewheel', function (event) {
        console.log(event.deltaX, event.deltaY, event.deltaFactor);
        event.stopPropagation();  // 阻止事件冒泡
        event.preventDefault();  // 阻止默认事件
    });

});
```

event对象中可以获取如下三个属性值：

- deltaX：值为负的（-1），则表示滚轮向左滚动。值为正的（1），则表示滚轮向右滚动。
- deltaY：值为负的（-1），则表示滚轮向下滚动。值为正的（1），则表示滚轮向上滚动。
- deltaFactor：增量因子。通过 deltaFactor * deltaX 或者 deltaFactor * deltaY 可以得到浏览器实际的滚动距离。

> 此插件来源于 [jquery-mousewheel](https://github.com/jquery/jquery-mousewheel) 。

<br/>

## 8.2.二维码模块  :id=qrcode
模块名：QRCode，使用方式：
```html
<div id="xxx"></div>
<script>
    layui.use(['QRCode'], function () {
        var $ = layui.jquery;
        var QRCode = layui.QRCode;
    
        // 二维码
        var demoQrCode = new QRCode(document.getElementById("xxx"), {
            text: "Hello Word!",
            width: 101,  // 宽度
            height: 101,  // 高度
            colorDark: "#000000",  // 颜色
            colorLight: "#ffffff",  // 背景颜色
            correctLevel: QRCode.CorrectLevel.H
        });
        
        // 更换内容
        demoQrCode.makeCode("Easyweb");
    
    });
</script>
```

> 此插件来源于 [qrcodejs](https://github.com/davidshimjs/qrcodejs) 。

<br/>

## 8.3.引导插件  :id=introjs
模块名：introJs，使用方式：
```html
<div data-step="1" data-intro="这是步骤一">步骤一</div>
<div data-step="2" data-intro="这是步骤二">步骤二</div>
<script>
    layui.use(['introJs'], function () {
        var introJs = layui.introJs;
    
        // 初始化
        introJs().start();
    
    });
</script>
```

> 此插件来源于 [intro.js](https://github.com/usablica/intro.js)，对样式进行了微调。

<br/>

## 8.4.剪贴板  :id=clipboardjs
模块名：ClipboardJS，使用方式：
```html
<input id="foo" value="https://github.com/zenorocha/clipboard.js.git">
<button id="btnCopy" data-clipboard-target="#foo">复制</button>
<script>
    layui.use(['ClipboardJS'], function () {
        var ClipboardJS = layui.ClipboardJS;
    
        var clipboard = new ClipboardJS('#btnCopy');
        clipboard.on('success', function(e) {
            e.clearSelection();
        });
        
        clipboard.on('error', function(e) {
            console.error('Action:', e.action);
        });
    
    });
</script>
```

> 此插件来源于 [clipboard.js](https://zenorocha.github.io/clipboard.js) 。

<br>

## 8.5.视频播放器  :id=player
模块名：Player，使用方式：
```html
<div id="demoVideo"></div>

<script>
    layui.use(['Player'], function () {
        var Player = layui.Player;

        // 视频播放器
        var player = new Player({
            id: "demoVideo",
            url: "//s1.pstatp.com/cdn/expire-1-M/byted-player-videos/1.0.0/xgplayer-demo.mp4",  // 视频地址
            poster: "https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png",  // 封面
            fluid: true,  // 宽度100%
            playbackRate: [0.5, 1, 1.5, 2],  // 开启倍速播放
            pip: true,  // 开启画中画
            lang: 'zh-cn'
        });

        // 开启弹幕
        var dmStyle = {
            color: '#ffcd08', fontSize: '20px'
        };
        var player = new Player({
            id: "demoVideo2",
            url: "http://demo.htmleaf.com/1704/201704071459/video/2.mp4",  // 视频地址
            autoplay: false,
            fluid: true,  // 宽度100%
            lang: 'zh-cn',
            danmu: {
                comments: [
                    {id: '1', start: 0, txt: '空降', color: true, style: dmStyle, duration: 15000},
                    {id: '2', start: 1500, txt: '前方高能', color: true, style: dmStyle, duration: 15000},
                    {id: '3', start: 3500, txt: '弹幕护体', color: true, style: dmStyle, duration: 15000},
                ]
            }
        });

    });
</script>
```

> 视频播放器使用的是西瓜播放器，更多介绍请前往查看[xgplayer](http://h5player.bytedance.com/)。

<br/>

## 8.6.富文本编辑器  :id=ckeditor
模块名：CKEDITOR，使用方式：
```html
<textarea id="demoCkEditor"></textarea>

<script>
    layui.use(['CKEDITOR'], function () {
        var $ = layui.jquery;
        var CKEDITOR = layui.CKEDITOR;
        
        // 渲染富文本编辑器
        CKEDITOR.replace('demoCkEditor', {height: 450});
        var insEdt = CKEDITOR.instances.demoCkEditor;  // 获取渲染后的实例
        
        var content = insEdt.getData();  // 获取内容
        insEdt.setData('<h1>Hello Word!</h1>');  // 设置内容
        insEdt.insertHtml('<h1>EasyWeb</h1>');  // 插入内容
        
    });
</script>
```

**图片上传配置：**

&emsp;打开`ckeditor/config.js`修改`config.filebrowserImageUploadUrl`为你的后端地址。

**文件上传配置(包括视频、音频、flash等)：**

&emsp;打开`ckeditor/config.js`修改`config.filebrowserUploadUrl`为你的后端地址。

**上传接口返回的数据格式：**

&emsp;成功：
```json
{
  "uploaded": 1,
  "fileName": "demo img",
  "url": "https://pic.qqtn.com/up/2018-9/15367146917869444.jpg"
}
```
&emsp;失败：
```json
{
  "uploaded": 0,
  "error": {
    "message": "演示环境，不允许上传，请替换为自己的url"
  }
}
```

**配置参数还可以在渲染的时候配置：**
```javascript
CKEDITOR.replace('demoCkEditor', {
   height: 450,
   filebrowserImageUploadUrl: '你的接口地址'
});
```


**修改操作按钮布局：**

&emsp;前往[toolbarconfigurator](http://whvse.gitee.io/html-test/ckeditor/samples/toolbarconfigurator)在线配置，并修改`ckeditor/config.js`。

<br/>

> 富文本编辑器使用的是`CKEditor4`，并做好了配置及样式调整。

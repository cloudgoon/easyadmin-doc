## 7.7.1.打印当前页面  :id=print_this

```javascript
printer.print();

printer.print({
    hide: ['.layui-btn', '#btn01'],  // 打印时隐藏的元素
    horizontal: true,  // 是否横向打印
    blank: true,  // 是否打开新页面打印
    close: true,  // 如果是打开新页面，打印完是否关闭
    iePreview: true  // 是否兼容ie打印预览
});
```

> 参数都是可以选参数，默认值已经符合大多数需求。


## 7.7.2.设置不打印的元素  :id=hide
```html
<div class="hide-print">非打印内容</div>
```

> 加`hide-print`这个class即可，可以跟`hide`参数叠加使用。


## 7.7.3.打印自定义内容  :id=print_html

```javascript
var pWindow = printer.printHtml({
    html: '<span>xxxx</span>',  // 要打印的内容
    horizontal: true,  // 是否横向打印
    blank: true,  // 是否打开新页面打印
    close: true,  // 如果是打开新页面，打印完是否关闭
    iePreview: true,  // 是否兼容ie打印预览
    print: true  // 如果是打开新窗口是否自动打印
});
```

&emsp;打印表格时可以给table加`print-table`这个class来设置表格的边框样式：`<table class="print-table"></table>`。

> 如果print为false关闭自动打印，可以使用pWindow.print()触发打印。


## 7.7.4.分页打印  :id=print_page

```javascript
printer.printPage({
    htmls: [
        '<span>xxxx</span>',   // 要打印的内容
        '<span>xxxx</span>'
    ],
    style: '<style> span { color: red; } </style>',  // 页面样式
    horizontal: true,  // 是否横向打印
    padding: undefined,  // 页面间距，例如写'10px'
    debug: false,  // 调试模式
    blank: true,  // 是否打开新页面打印
    close: true,  // 如果是打开新页面，打印完是否关闭
    iePreview: true,  // 是否兼容ie打印预览
    print: true  // 如果是打开新窗口是否自动打印
});
```

参数都是可选参数：
- htmls&emsp;数组，代表每一页；
- style&emsp;样式，也可以写`<link rel="stylesheet">`的形式；
- padding&emsp;间距，undefined表示使用打印机默认间距，如果自定义间距，打印机间距要选择无；
- debug&emsp;调试模式，每一页会加红色边框，便于调试大小；

> `padding`为undefined表示使用打印机默认间距，插件内部会根据不同浏览器做兼容处理，因为不同浏览器的默认间距不同，
> 目前对谷歌、IE、Edge、火狐做了兼容处理，safari、欧朋后续会考虑。


## 7.7.5.拼接html  :id=make_html

```javascript
var htmlStr = printer.makeHtml({
    title: '网页标题',
    style: '<link rel="stylesheet" href="layui.css"><script type="text/javascript" src="layui.js"><\/script>',
    body: $('#xxx').html()
});
```

## 7.7.6.其他方法  :id=method

```javascript
printer.isIE();
printer.isEdge();
printer.isFirefox();
```

## 7.7.7.打印任意url  :id=print_url

目前没有提供直接的方法，可以使用ajax请求到url的html内容，在使用printHtml方法打印：
```javascript
$.ajax({
    url: 'xxx.html',
    type: 'get',
    dataType: 'html',
    success: function (res) {
        printer.printHtml({
            html: res,  // 要打印的内容
            horizontal: true  // 是否横向打印
        });
    }
});
```

> 另外js直接控制横向打印只支持谷歌内核的浏览器，火狐不支持js调用打印预览，
> 火狐打印预览可以通过设置`blank:true,print:false`然后手动选择打印预览。

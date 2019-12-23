## 9.1.主题功能  :id=theme

1. 打开 [主题生成器](https://demo.easyweb.vip/theme/) 在线定制主题

2. 将生成的css放在`assets/moudel/theme`目录下

3. 打开“page/tpl/tpl-theme.html”添加生成的主题：

```javascript
var themes = [
    {title: '黑白主题', theme: 'admin'},
    {title: '黑色主题', theme: 'black'},
    {title: '你的主题', theme: 'xxx'}
];
```

主题css以`theme-xxx.css`的形式命名，主题预览图跟主题css名字保持一致，预览图可在主题生成器中获取。

> 更强大的主题生成器正在开发中，敬请期待...


## 9.2.自定义扩展模块  :id=expand

集成社区模块：

&emsp;Layui社区有很多优秀的扩展模块，[前去查看](https://fly.layui.com/extend/)，
集成到easyweb只需要把下载好的模块放在`/assets/module/`目录下面，如果模块只是一个单纯的js，不用做任何配置就可以使用了，
例如admin.js、index.js，如果模块是一个文件夹，还需要在`common.js`中配置模块的具体位置，例如`treetable-lay/treetable.js`。

自定义扩展模块：

&emsp;如果你需要自己写一个扩展模块，请前往阅读Layui[定义模块](https://www.layui.com/doc/base/infrastructure.html#define)的开发文档，
或者参考admin.js等模块。

集成jquery插件：

&emsp;jquery作为老牌框架，有着丰富的第三方插件，layui基于jquery.js，同样可以使用jquery插件，
最简单的使用方法就是把插件放入libs下，页面中先引入jquery.js，再引入插件js，即可使用。

&emsp;当然更友好的集成方式是把它改造成layui扩展模块，改造方式请参考[layui 封装第三方组件](https://fly.layui.com/jie/5080/)。


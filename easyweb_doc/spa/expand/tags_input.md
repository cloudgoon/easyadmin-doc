## 6.4.1.快速使用  :id=start
```html
<input id="demoTagsInput" value="辣妹子,大长腿" class="layui-hide"/>

<script>
    layui.use(['jquery', 'tagsInput'], function () {
        var $ = layui.jquery;

        // 输入框样式
        $('#demoTagsInput').tagsInput();

        // 无边框样式
        $('#demoTagsInput').tagsInput({skin: 'tagsinput-default'});

        // BackSpace键可删除标签
        $('#demoTagsInput').tagsInput({removeWithBackspace: true});

        // 输入列表提示
        $('#demoTagsInput').tagsInput({
            skin: 'tagsinput-default',
            autocomplete_url: '../../json/tagsInput.json'
        });

    });
</script>
```
&emsp;只需要写一个input框，调用tagsInput方法渲染即可，回显数据写在value中用逗号分隔。

![](https://s2.ax1x.com/2019/07/11/Z26OyT.png)

## 6.4.2.全部参数  :id=options

参数 | 说明 | 默认值
--- | --- | ---
defaultText | 提示文字 | +请输入
skin | 样式风格 | 
removeWithBackspace | 回退键可删除已添加的标签 | false
focusWithClick | 点击已添加标签输入框获取焦点 | true
autocomplete_url | 自动提示接口url | 
autocomplete | 接口配置 | 

**autocomplete参数：**
```javascript
$('#demoTagsInput').tagsInput({
    autocomplete_url: '../../json/tagsInput.json',
    autocomplete: {
        type: 'post',
        data: {
            access_token: 'xxxxx'
        }
    }
});
```

&emsp;type是请求方式，默认是get请求，data是额外参数，请求autocomplete_url会传递`name`参数(输入框的值)。

更详细的使用文档可以参考[jQuery-Tags-Input](http://www.jq22.com/jquery-info426)。

> 此插件基于 [jQuery-Tags-Input](https://github.com/xoxco/jQuery-Tags-Input) 二次修改。

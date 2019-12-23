## 6.5.1.快速使用  :id=start
```html
<div class="split-group">
    <div class="split-item" id="demoSplit1">
        面板一
    </div>
    <div class="split-item" id="demoSplit2">
        面板二
    </div>
</div>

<script>
    layui.use(['Split'], function () {
        var $ = layui.jquery;
        var Split = layui.Split;

        // 水平分割，需要分割的元素(id)、默认大小(百分比)、最小值(单位px)
        Split(['#demoSplit1', '#demoSplit2'], {sizes: [25, 75], minSize: 100});
    });
</script>
```


## 6.5.2.垂直分割  :id=vertical
```html
<div class="split-group-vertical">
    <div class="split-item" id="demoSplit3">
        面板一
    </div>
    <div class="split-item" id="demoSplit4">
        面板二
    </div>
</div>

<script>
    layui.use(['Split'], function () {
        var $ = layui.jquery;
        var Split = layui.Split;

        // 垂直分割
        Split(['#demoSplit3', '#demoSplit4'], {direction: 'vertical'});
    });
</script>
```


## 6.5.3.嵌套使用  :id=nest
```html
<div class="split-group" style="height: 600px;">
    <div class="split-item" id="demoSplit8">
        <div class="split-group-vertical">
            <div class="split-item" id="demoSplit10">
                面板一
            </div>
            <div class="split-item" id="demoSplit11">
                面板二
            </div>
        </div>
    </div>
    <div class="split-item" id="demoSplit9">
        面板三
    </div>
</div>

<script>
    layui.use([Split'], function () {
        var $ = layui.jquery;
        var Split = layui.Split;

        // 垂直水平分割
        Split(['#demoSplit8', '#demoSplit9'], {sizes: [25, 75], minSize: 100});
        Split(['#demoSplit10', '#demoSplit11'], {direction: 'vertical'});
    });
</script>
```

![](https://s2.ax1x.com/2019/08/28/mHsyN9.png)

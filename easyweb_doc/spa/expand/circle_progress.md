## 6.7.1.快速使用  :id=start
```html
<div id="demoProgress1"></div>

<script>
layui.use(['CircleProgress'], function () {
    var CircleProgress = layui.CircleProgress;

    // 快速使用
    new CircleProgress('#demoProgress1', {
        max: 100,
        value: 20
    });
});
</script>
```

## 6.7.2.全部参数  :id=option

参数名称 | 说明 | 默认值
:--- | :--- | :---
max | 最大值 | 
value | 当前值 | 
clockwise | 是否顺时针方向 | true
startAngle | 起始角度 | 0
textFormat | 文字样式 | 

**文字样式：**

- vertical &emsp; 垂直
- percent &emsp; 百分比
- value &emsp; 只显示值
- valueOnCircle &emsp; 值显示在进度条上
- none &emsp; 不显示文字

**自定义文字样式：**

```javascript
new CircleProgress('#demoProgress9', {
    max: 12,
    value: 9,
    textFormat: function (value, max) {
        return value + ' dots';
    }
});
```

## 6.7.3.自定义样式  :id=style
使用css自定义样式：
```html
<div id="demoProgress1"></div>

<script>
layui.use(['CircleProgress'], function () {
    var CircleProgress = layui.CircleProgress;

    // 快速使用
    new CircleProgress('#demoProgress1', {
        max: 100,
        value: 20,
        textFormat: 'percent'
    });
});
</script>

<style>
/* 进度条选中的样式 */
#demoProgress1 .circle-progress-value {
    stroke-width: 8px;  /* 粗度 */
    stroke: #3FDABA;  /* 颜色 */
}

/* 进度条未选中的样式 */
#demoProgress1 .circle-progress-circle {
    stroke-width: 6px;
    stroke: #E0FAF1;
}
</style>
```

> 此插件是使用svg实现的，想要实现更丰富的样式，可先了解下svg

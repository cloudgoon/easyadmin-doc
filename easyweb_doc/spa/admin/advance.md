## 4.1.文字提示  :id=tips
鼠标滑过弹出tips，使用示例：
```html
<button lay-tips="大家好！">按钮</button>

<button lay-tips="大家好！" lay-direction="2" lay-bg="#009788">按钮</button>

<button lay-tips="大家好！" lay-offset="10px">按钮</button>

<button lay-tips="大家好！" lay-offset="10px,10px">按钮</button>
```

- lay-direction： 设置位置，1上面(默认)、2右边、3下面、4左边
- lay-bg： 设置背景颜色
- lay-offset： 设置偏移距离，(上下，左右)

![](https://s2.ax1x.com/2019/07/11/Z2BGmd.png)


## 4.2.地图选择位置  :id=location
快速使用：
```javascript
admin.chooseLocation({
    needCity: true,
    onSelect: function (res) {
        layer.msg(JSON.stringify(res), {icon: 1});
    }
});
```

参数 | 默认 | 描述
:---|:--- | :---
title | "选择位置" | 弹窗标题 
needCity | false | 是否返回行政区，省市区默认不返回
center | 定位当前城市 | 地图默认的中心点
defaultZoom | 11 | 地图默认缩放级别
pointZoom | 17 | 选中时地图的缩放级别
keywords | 无 | poi检索关键字，例如：建筑、写字楼
pageSize | 30 | poi检索最大数量
onSelect | 无 | 选择回调
mapJsUrl | 内置 | 高德地图js的url

- 地图默认中心点参考值：[116.397428, 39.90923]，经度，纬度
- 地图url参考值：`https://webapi.amap.com/maps?v=1.4.14&key=xxxxxxx`
- 返回结果说明：
    - res.name;  // 地点名称
    - res.address;  // 详细地址
    - res.lat;  // 纬度
    - res.lng;  // 经度
    - res.city; // 城市，是一个对象
    - res.city.province;  // 省
    - res.city.city;  // 市
    - res.city.district;  // 区
    - res.city.citycode;  // 城市代码

地图相关参数的详细介绍请前往[高德地图API](https://lbs.amap.com/api/javascript-api/summary)查看。

![](https://s2.ax1x.com/2019/08/29/mL47En.png)


## 4.3.裁剪图片  :id=crop_img
快速使用：
```javascript
admin.cropImg({
    aspectRatio: 1/1,
    imgSrc: '../../assets/images/15367146917869444.jpg',
    onCrop: function (res) {
        // 返回的res是base64编码的裁剪后的图片
        layer.msg('<img src="' + res + '" width="220px" height="220px"/>');
    }
});
```

参数 | 默认 | 描述
:---|:--- | :---
title | "裁剪图片" | 弹窗标题 
aspectRatio | 1/1 | 裁剪比例，例如：16/9
imgSrc | 无 | 要裁剪的图片，无则先弹出选择图片
imgType | 'image/jpeg' | 裁剪的图片类型，非必填
onCrop | 无 | 裁剪完成回调
limitSize | 不限制 | 限制选择的图片大小
acceptMime | 'image/*' | 限制选择的图片类型
exts | 不限制 | 限制选择的图片后缀

- acceptMime参考值：'image/jpg, image/png'（只显示 jpg 和 png 文件）
- exts参考值：jpg|png|gif|bmp|jpeg

后面三个参数配置可参考[upload模块](https://www.layui.com/doc/modules/upload.html)，
如果想单独在页面中使用图片裁剪功能请参考[Cropper插件文档](http://fengyuanchen.github.io/cropper/)

![](https://s2.ax1x.com/2019/08/29/mL5AgO.png)


## 4.2.动画数字  :id=anim_num
快速使用：
```html
<h2 id="demoAnimNum1">12345</h2>
<h2 id="demoAnimNum2">￥2373467.342353</h2>
<h2 id="demoAnimNum3">上浮99.98%</h2>

<script>
layui.use(['admin'], function () {
    var $ = layui.jquery;
    var admin = layui.admin;
    
    admin.util.animateNum('#demoAnimNum1');
    admin.util.animateNum('#demoAnimNum2');
    admin.util.animateNum('#demoAnimNum3', true, 500, 100);
    
});
</script>
```

- 参数一 &emsp; 需要动画的元素
- 参数二 &emsp; 是否开启千分位，开启后每进千加逗号分隔
- 参数三 &emsp; 动画间隔，默认500
- 参数四 &emsp; 动画粒度，默认100

&emsp;此方法会智能过滤除数字外的字符。


## 4.3.经纬度转换  :id=jwdzh
快速使用：
```javascript
// GCJ02转BD09
var point = admin.util.Convert_GCJ02_To_BD09({
   lat: 30.505674, 
   lng: 114.40043
});
console.log(point.lng + ',' + point.lat);

// BD09转GCJ02
var point = admin.util.Convert_BD09_To_GCJ02({
   lat: 30.512004, 
   lng: 114.405701
});
console.log(point.lng + ',' + point.lat);
```

- 高德地图、腾讯地图以及谷歌中国区地图使用的是GCJ-02坐标系
- 百度地图使用的是BD-09坐标系

&emsp;不同的坐标系之间会有几十到几百米的偏移，所以在开发基于地图的产品时，可以通过此方法修正不同坐标系之间的偏差，
[详细了解国内各坐标系](https://blog.csdn.net/m0_37738114/article/details/80452485)，当然即使用此方法转换也会存在几米的误差，
因为浮点运算精度难免会丢失。


## 4.4.深度克隆对象  :id=deepclone
快速使用：
```javascript
var o1 = {
    name: 'xxx',
    role: ['admin', 'user']
};

var o2 = admin.util.deepClone(o1);
```
&emsp;对象型在参数传递是通常是引用传递，有时需要把对象克隆一份再传递，避免传递的参数被修改导致第二次传递出现问题。


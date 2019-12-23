## 6.2.1.快速使用  :id=start
```javascript
layui.use(['notice'], function(){
    var notice = layui.notice;
    
    // 消息通知
    notice.success({
        title: '消息通知',
        message: '你有新的消息，请注意查收!'
    });
    
    // 提示框，1成功、2失败、3警告、4加载、5信息(蓝色图标)
    notice.msg('Hello', {icon: 1});
});
```

![](https://s2.ax1x.com/2019/08/28/mHaYM4.png)


## 6.2.2.全部方法  :id=method

 方法 | 参数 | 说明
 :--- | :--- | :---
 success(object) | 见下方 | 成功消息（绿色）
 warning(object) | 见下方 |  警告消息（黄色）
 error(object) | 见下方 |  错误消息（红色）
 info(object) |  见下方 | 通知消息（蓝色）
 show(object) | 见下方 |  自定义样式
  | 
 msg(string, object) | 文本,其他参数 |  提示框
  | 
 destroy() |  无 | 关闭全部
 hide(object, toast, closedBy) |  见下方 | 关闭指定的通知
 settings(object) | 见下方 |  统一设置默认值
 
**统一设置默认值：**
 
```javascript
notice.settings({
    timeout: 1500,
    transitionIn: 'flipInX',
    onOpened: function(){
        console.log('notice opend!');
    }
});   
```

**关闭指定的通知：**

```javascript
notice.hide({}, document.querySelector('.toast')); 
```

- 参数一 &emsp; 重写一些参数，比如关闭动画等
- 参数二 &emsp; 根据自定义的className选择关闭的对象

 
## 6.2.3.参数列表  :id=options
 
 参数 | 说明 | 默认值 | 可选值 
:---|:---|:---|:---
 title | 标题 | 无 | string类型
 message | 内容 | 无 |  string类型
 position | 显示位置 | topRight | 见下方
 transitionIn | 进入动画 | fadeInLeft | 见下方
 transitionOut | 退出动画 | fadeOutRight | 见下方
 timeout | 消失时间 | 5000 | 单位毫秒，false永不消失
 progressBar | 进度条 | true | true显示、false不显示
 balloon | 气泡效果 | false | true开启、false关闭
 close | 关闭按钮 | true | true显示，false不显示
 pauseOnHover | 鼠标滑过暂停消失时间 | true | true、false
 resetOnHover | 鼠标滑过重置消失时间 | false | true、false
 animateInside | 文字动画效果 | false | true开启、false关闭
 className | 自定义class | 无 | 多个用空格分隔
 theme | 主题 | light | light、dark
 audio | 音效 | 无 | 1，2，3，4，5，6
 | | | 
 image | 显示图片 | 无 | 图片地址
 imageWidth | 图片宽度 | 60 | 数字 
 buttons | 显示按钮 | [] | [ [ 'btn1', function(){} ], ['btn2', function(){} ] ] 
 overlay | 遮罩层 | false | true显示,false不显示
 drag | 滑动关闭 | true | true开启，false关闭
 layout | 布局类型 | 2 | 1标题和内容并排，2两排显示
 rtl | 布局方向 | false | false内容居左，true居右
 displayMode | 显示模式 | 0 | 0无限制，1同类型存在不显示，2同类型存在先移除
 targetFirst | 插入方式 | 自动 | true从上插入，false下插入
 onOpened | 打开后回调函数 | 无 | function
 onClosed | 关闭后回调函数 | 无 | function
 | | | 
 titleColor | 标题颜色 | 默认| 颜色单位
 titleSize | 标题大小 | 默认| 尺寸单位
 messageColor | 文字颜色 | 默认| 颜色单位
 messageSize | 文字大小 | 默认| 尺寸单位
 backgroundColor | 背景颜色 | 默认 | 颜色单位
 progressBarColor | 进度条颜色 | 默认 | 颜色单位
 maxWidth | 最大宽度 | 空| 尺寸单位

**参数position显示位置可选值：**

 属性 | 说明 
:---|:---
bottomRight | 右下角
bottomLeft | 左下角
topRight | 右上角
topLeft | 左上角
topCenter | 顶部中间
bottomCenter | 底部中间
center | 正中间

**参数transitionIn进入动画可选值：**

 属性 | 说明 
:---|:---
bounceInLeft | 向左反弹
bounceInRight | 向右反弹
bounceInUp | 向上反弹
bounceInDown | 向下反弹
fadeIn | 淡入
fadeInDown | 向下淡入
fadeInUp | 向上淡入
fadeInLeft | 向左淡入
fadeInRight | 向右淡入
flipInX | 翻转进入

**参数transitionOut退出动画可选值：**

 属性 | 说明 
:---|:---
fadeOut | 淡出
fadeOutUp | 向上淡出
fadeOutDown | 向下淡出
fadeOutLeft | 向左淡出
fadeOutRight | 向右淡出
flipOutX | 翻转退出

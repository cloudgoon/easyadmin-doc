## 6.1.1.快速使用  :id=start
```html
<!-- click模式触发 -->
<div class="dropdown-menu">
    <button class="layui-btn icon-btn">
        &nbsp;Click me <i class="layui-icon layui-icon-drop"></i>
    </button>
    <ul class="dropdown-menu-nav">
        <li><a>1st menu item</a></li>
        <li><a>2nd menu item</a></li>
        <li><a>3rd menu item</a></li>
    </ul>
</div>

<!-- hover模式触发，增加dropdown-hover即可 -->
<div class="dropdown-menu dropdown-hover">
    <button class="layui-btn icon-btn">
        &nbsp;Hover me <i class="layui-icon layui-icon-drop"></i></button>
    <ul class="dropdown-menu-nav">
        <li><a>1st menu item</a></li>
        <li><a>2nd menu item</a></li>
        <li><a>3rd menu item</a></li>
    </ul>
</div>

<script>
    layui.use(['dropdown'], function () {
        var dropdown = layui.dropdown;  // 加载模块

    });
</script>
```

![](https://s2.ax1x.com/2019/08/28/mHiA3Q.png)


## 6.1.2.更多样式  :id=style

```html
<!-- 标题及禁用样式 -->
<div class="dropdown-menu dropdown-hover">
    <button class="layui-btn icon-btn">
        &nbsp;更多样式 <i class="layui-icon layui-icon-drop"></i></button>
    <ul class="dropdown-menu-nav">
        <li class="title">HEADER</li>
        <li><a><i class="layui-icon layui-icon-star-fill"></i>1st menu item</a></li>
        <li class="disabled">
            <a><i class="layui-icon layui-icon-template-1"></i>2nd menu item</a></li>
        <hr>
        <li class="title">HEADER</li>
        <li><a><i class="layui-icon layui-icon-set-fill"></i>3rd menu item</a></li>
    </ul>
</div>

<!-- 带小三角样式 -->
<div class="dropdown-menu dropdown-hover">
    <button class="layui-btn icon-btn">
        &nbsp;带小三角 <i class="layui-icon layui-icon-drop"></i>
    </button>
    <ul class="dropdown-menu-nav">
        <div class="dropdown-anchor"></div>
        <li><a>1st menu item</a></li>
        <li><a>2nd menu item</a></li>
        <li><a>3rd menu item</a></li>
    </ul>
</div>

<!-- 暗色主题 -->
<div class="dropdown-menu">
    <button class="layui-btn layui-btn-normal icon-btn">
        &nbsp;暗色主题 <i class="layui-icon layui-icon-drop"></i></button>
    <ul class="dropdown-menu-nav dark">
        <div class="dropdown-anchor"></div>
        <li class="title">HEADER</li>
        <li><a><i class="layui-icon layui-icon-star-fill"></i>1st menu item</a></li>
        <li class="disabled">
            <a><i class="layui-icon layui-icon-template-1"></i>2nd menu item</a></li>
        <hr>
        <li class="title">HEADER</li>
        <li><a><i class="layui-icon layui-icon-set-fill"></i>3rd menu item</a></li>
    </ul>
</div>
```
![](https://s2.ax1x.com/2019/08/28/mHFKsA.png)


## 6.1.3.对任意元素使用  :id=target
```html
<div class="dropdown-menu dropdown-hover">
    <input type="text" placeholder="一个会跳舞的输入框" class="layui-input"/>
    <ul class="dropdown-menu-nav">
        <li class="title">是不是在找</li>
        <li><a>另一个会跳舞的下拉框</a></li>
        <li><a>一杯忧郁的可乐</a></li>
    </ul>
</div>
```
![](https://s2.ax1x.com/2019/08/28/mHVJ7n.png)


## 6.1.4.带遮罩层  :id=shade
遮罩层是分离式的绑定，通过data-dropdown绑定：
```html
<button class="layui-btn layui-btn-normal icon-btn" data-dropdown="#dropdown1">
   带遮罩层 <i class="layui-icon layui-icon-drop"></i>
</button>

<!-- 下拉菜单 -->
<ul class="dropdown-menu-nav dropdown-bottom-right layui-hide" id="dropdown1">
    <div class="dropdown-anchor"></div>
    <li class="title">HEADER</li>
    <li><a><i class="layui-icon layui-icon-star-fill"></i>1st menu item</a></li>
    <li class="disabled">
        <a><i class="layui-icon layui-icon-template-1"></i>2nd menu item</a></li>
    <hr>
    <li class="title">HEADER</li>
    <li><a><i class="layui-icon layui-icon-set-fill"></i>3rd menu item</a></li>
</ul>
```


## 6.1.5.自定义下拉内容  :id=div_drop
```html
<div class="dropdown-menu">
    <button class="layui-btn layui-btn-normal icon-btn">
        &nbsp;克隆/下载 <i class="layui-icon layui-icon-drop"></i></button>
    <div class="dropdown-menu-nav dropdown-bottom-right"
         style="width: 280px;padding: 0 10px 10px 10px;">
        <div class="dropdown-anchor"></div>
        <!-- 下面是自定义内容 -->
        <div class="layui-tab layui-tab-brief">
            <ul class="layui-tab-title">
                <li class="layui-this">HTTPS</li>
                <li>SSH</li>
            </ul>
            <div class="layui-tab-content" style="padding: 10px 0 10px 0;">
                <div class="layui-tab-item layui-show">
                    <input class="layui-input" value="https://gitee.com/whvse/easyweb-jwt.git"/>
                </div>
                <div class="layui-tab-item">
                    <input class="layui-input" value="git@gitee.com:whvse/easyweb-jwt.git"/>
                </div>
            </div>
        </div>
        <button class="layui-btn layui-btn-sm layui-btn-fluid" style="margin-bottom: 10px;">
            Download ZIP
        </button>
        <img src="http://p1.music.126.net/voV3yPduAhNATICMRJza1A==/109951164017919367.jpg"
             width="100%">
        <!-- //end.自定义内容结束 -->
    </div>
</div>
```
![](https://s2.ax1x.com/2019/08/28/mHkQl4.png)


## 6.1.6.控制显示方向  :id=anchor
在dropdown-menu-nav上加dropdown-bottom-center等class控制位置：

 类名 | 位置
:--- | :---
 dropdown-bottom-left | 下左弹出
 dropdown-bottom-center| 下中弹出
 dropdown-bottom-right | 下右弹出
 | 
 dropdown-top-left | 上左弹出
 dropdown-top-center | 上中弹出
 dropdown-top-right | 上右弹出
 | 
 dropdown-left-top | 左上弹出
 dropdown-left-center | 左中弹出
 dropdown-left-bottom | 左下弹出
 | 
 dropdown-right-top | 右上弹出
 dropdown-right-center | 右中弹出
 dropdown-right-bottom | 右下弹出
   
```html
<!-- Bottom Center -->
<div class="dropdown-menu dropdown-hover">
    <button class="layui-btn layui-btn-primary icon-btn">
        Bottom <i class="layui-icon layui-icon-drop"></i> Center
    </button>
    <ul class="dropdown-menu-nav dropdown-bottom-center"><!-- 这里加控制方向的类 -->
        <div class="dropdown-anchor"></div>
        <li><a>1st menu item</a></li>
        <li><a>2nd menu item</a></li>
        <li><a>3rd menu item</a></li>
    </ul>
</div>

<!-- Bottom Right -->
<div class="dropdown-menu dropdown-hover">
    <button class="layui-btn layui-btn-primary icon-btn">
        Bottom Right <i class="layui-icon layui-icon-drop"></i>
    </button>
    <ul class="dropdown-menu-nav dropdown-bottom-right"><!-- 这里加控制方向的类 -->
        <div class="dropdown-anchor"></div>
        <li><a>1st menu item</a></li>
        <li><a>2nd menu item</a></li>
        <li><a>3rd menu item</a></li>
    </ul>
</div>
```
![](https://s2.ax1x.com/2019/08/28/mHEbSU.png)

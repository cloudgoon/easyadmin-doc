## 12.1.快速了解
使用示例：
```html
<div id="m"></div>
<a href="#/user">go user</a>
<a href="#/home">go home</a>

<script>
layui.use(['layRouter'],function(){
    var layRouter = layui.layRouter;
    
    layRouter.reg('/home', function(){
        document.getElementById('m').innerHTML = 'Hello World';
    }).reg('/user', function() {
        document.getElementById('m').innerHTML = 'This is user page';
    });

    layRouter.init({
        index: '/home' /* 首页地址 */
    });
});
</script>
```
&emsp;打开例子后，浏览器会从 http://xxx.com/ 跳转到 http://xx.com/#/home ，并且在id为m的div中显示 Hello World。
然后点击`go user`的链接，会跳转到 http://xx.com/#/user ，并且在id为m的div中显示 his is user page。


## 12.2.注册路由
```javascript
layRouter.reg('/home', function(){
    alert('xxx');
});

// 在框架中要使用这种
index.regRouter([{
    name: '用户管理',
    url: '#/system/user'
}]);
```

&emsp;上面是基础的写法，下面是index模块封装的写法，使用index.regRouter注册后访问`#/system/user`，就会打开一个“用户管理”的标签页，使用上面的写法不会。
     
     
> layRouter是路由的核心实现，提供给index模块作为支撑的，一般不需要去直接用它。


## 12.3.路由参数传递

请参考页面[路由参数传递](https://demo.easyweb.vip/spa/#/template/routerDemo)。

**参数传递的规则：**
```text
#/system/user                  // 无参数

#/system/user/id=1             // 参数id=1

#/system/user/id=1/name=aaa    // 参数id=1，name=aaa
```

&emsp;这三种类型的url注册路由的时候只会注册`#/system/user`这个url，后面两个都是加了参数，而且后面的参数可以无限加，
也就是说如果已经注册了`#/system/user`，`#/system/user`和`#/system/user/id=1/name=aaa`是可以直接访问的。

**如何获取当前传递的参数：**
```javascript
layui.router().search;

layui.router('#/system/user/id=1/name=aa').search;

```

```javascript
var param = layui.router().search
console.log(param.id);
console.log(param.name);
```

> 注意：不要传递太过复杂的参数，如`'/'`和`'='`会影响路由解析的符号不能传递。


## 12.4.路由不存在处理

在config.js中有一个`routerNotFound`方法用于处理404路由。


## 12.5.路由切换监听

注册`pop`方法即可监听路由的切换：
```javascript
layRouter.reg('pop', function(r){
    console.log(r);
});
```

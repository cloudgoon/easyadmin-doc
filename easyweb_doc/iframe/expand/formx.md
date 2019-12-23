## 7.4.1.验证规则  :id=start

规则 | 描述 | 提示信息
:--- | :--- | :---
phoneX | 手机号 | 请输入正确的手机号
emailX | 邮箱 | 邮箱格式不正确
urlX | 网址 | 链接格式不正确
numberX | 数字 | 只能填写数字
dateX | 日期 | 日期格式不正确
identityX | 身份证 | 请输入正确的身份证号
 | |
psw | 密码 | 密码必须5到12位，且不能出现空格
equalTo | 重复 | 两次输入不一致
digits | 整数 | 只能输入整数
digitsP | 正整数 | 只能输入正整数
digitsN | 负整数 | 只能输入负整数
digitsPZ | 非负整数 | 只能输入正整数和0
digitsNZ | 非正整数 | 只能输入负整数和0
 | |
h5 | 兼容h5的规则 | 

使用示例：
```html
<form class="layui-form">
    <input class="layui-input" placeholder="请输入手机号"
           lay-verType="tips" lay-verify="phoneX"/>
    <input class="layui-input" placeholder="请输入手机号"
           lay-verType="tips" lay-verify="required|phoneX"/>
    <input class="layui-input" placeholder="请输入整数"
           lay-verType="tips" lay-verify="digits"/>
    <input class="layui-input" placeholder="请输入正整数"
           lay-verType="tips" lay-verify="digitsP"/>
</form>
```

**equalTo用法：**
```html
<form class="layui-form">
    <input id="demoPsw" class="layui-input" placeholder="请输入密码"
           lay-verType="tips" lay-verify="required|psw"/>
    <input class="layui-input" placeholder="请再次输入密码"
           lay-verType="tips" lay-verify="equalTo" lay-equalTo="#demoPsw"
           lay-equalToText="两次输入密码不一致"/>
</form>
```
equalTo用来验证两次输入是否一致。

属性 | 描述
:--- | :---
lay-equalTo | 关联输入框的dom选择器
lay-equalToText | 自定义提示文本


**h5用法：**

属性 | 描述 
:--- | :---
minlength | 最少输入字符长度
maxlength | 最多输入字符长度
min | 最小输入数值
max | 最大输入数值

```html
<form class="layui-form">
    <input class="layui-input" placeholder="最少输入5个字符" minlength="5"
           lay-verType="tips" lay-verify="required|h5"/>
    <input class="layui-input" placeholder="最多输入10个字符" maxlength="10"
           lay-verType="tips" lay-verify="h5"/>
    <input class="layui-input" type="number" placeholder="值只能在-9到9之间" min="-9" max="9"
           lay-verType="tips" lay-verify="required|numberX|h5"/>
</form>
```

> phoneX、emailX等与layui自带phone、email等的区别是如果没有输入不会验证，输入了才验证格式。


## 7.4.2.扩展方法  :id=method

方法 | 描述
:--- | :---
formVal(filter, object) | 赋值表单，解决top.layui.form.val()方法无效的bug

使用方法：
```javascript
layui.use(['formX'],function(){
    var formX = layui.formX;

    formX.val('userForm', {name: 'user01'});  // 赋值表单，支持top.formX.val()用法
});
```

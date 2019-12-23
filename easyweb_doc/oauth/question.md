## 11.1.IDEA热更新    :id=q1
如果修改了页面代码，需要重启服务器才能更新，对IDEA做如下设置即可：

1. 按住 `Shift`+`Ctrl`+`Alt`+`/`，选择Registry
2. 找到`compiler.automake.allow.when.app.running`并勾选

    ![](https://s2.ax1x.com/2019/08/31/mz33M8.png)
    
3. 打开`file`/`settings`/`Compiler`，勾选`Build project automatically`：

    ![](https://s2.ax1x.com/2019/08/31/mzUhAU.png)

完成上面三步设置，重启服务器即可热更新，如果还不行，再做如下设置：

![](https://s2.ax1x.com/2019/08/31/mzaZ4g.png)


## 11.2.修改包名后无法运行     :id=q2
这个问题一般都是包名没有修改彻底，修改包名建议使用全局替换的方式，对着src目录右键：

![](https://s2.ax1x.com/2019/08/31/mzlhkR.png)

![](https://s2.ax1x.com/2019/08/31/mzlT1K.png)

> 然后还有pom.xml的groupId和artifactId，当热这个改不改都不影响


## 11.3.如何集成验证码     :id=q3
&emsp;前后端分离如果想要使用图形验证码功能，需要配合redis使用，传统的验证码是放在session中的，前后端分离则不能使用session，
如果要集成图形验证码功能请参考[EasyCaptcha](https://gitee.com/whvse/EasyCaptcha)。

后端生成验证码放在redis中设置过期时间并返回base64编码给前端：
```java
@Controller
public class CaptchaController {
    @Autowired
    private RedisUtil redisUtil;
    
    @ResponseBody
    @RequestMapping("/captcha")
    public JsonResult captcha(HttpServletRequest request, HttpServletResponse response) throws Exception {
        SpecCaptcha specCaptcha = new SpecCaptcha(130, 48, 5);
        String verCode = specCaptcha.text().toLowerCase();
        String key = UUID.randomUUID().toString();
        // 存入redis并设置过期时间为30分钟
        redisUtil.setEx(key, verCode, 30, TimeUnit.MINUTES);
        // 将key和base64返回给前端
        return JsonResult.ok().put("key", key).put("image", specCaptcha.toBase64());
    }
    
    @ResponseBody
    @PostMapping("/login")
    public JsonResult login(String username,String password,String verCode,String verKey){
        // 获取redis中的验证码
        String redisCode = redisUtil.get(verKey);
        // 判断验证码
        if (verCode==null || !redisCode.equals(verCode.trim().toLowerCase())) {
            return JsonResult.error("验证码不正确");
        }
    }  
}
```
前端使用ajax获取验证码：
```html
<img id="verImg" width="130px" height="48px"/>

<script>
    var verKey;
    // 获取验证码
    $.get('/captcha', function(res) {
        verKey = res.key;
        $('#verImg').attr('src', res.image);
    },'json');
    
    // 登录
    $.post('/login', {
        verKey: verKey,
        verCode: '8u6h',
        username: 'admin'，
        password: 'admin'
    }, function(res) {
        console.log(res);
    }, 'json');
</script>
```
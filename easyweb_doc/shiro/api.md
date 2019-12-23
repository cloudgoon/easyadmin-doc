## 8.1.JwtPermission框架    :id=jwtp
框架已经集成了JwtPermission作为api接口的权限框架，并且配置Shiro过滤/api/**，所以建议接口的路径都以/api/开头，

JwtPermission的使用说明请到[JwtPermission](https://gitee.com/whvse/JwtPermission)查看。

> 系统还是基于shiro控制权限的，jwt只是给app、小程序等写接口用的，因为接口一般是基于token的验证，所以如果你的项目没有app接口，可以删除api部分，删除jwtpermission框架。

## 8.2.登录接口    :id=login
登录接口已经实现，可以直接调用：
```javascript
$.post('/api/login', {
    username: 'admin',
    password: '123456'
},function(res){
    console.log(res);
});
```
登录成功返回的json格式为：
{code: 200, msg: "登录成功", access_token: "xxxxxxxxxxxxxxxx"}。

除了登录接口，其他接口请求的时候需要传递token，前端在登录获取token后应该存储起来：
```javascript
// 在参数中传递token
$.get('api/xxx', {
    access_token: 'xxxxx'
}, function(res){
}, 'json');

// 在header中传递token
$.ajax({
    url: "/xxx", 
    beforeSend: function(xhr) {
        xhr.setRequestHeader("Authorization", 'Bearer '+ token);
    },
    success: function(data){ }
});
```

## 8.3.API基类    :id=base
基类BaseApiController提供了获取当前登录的用户id的方法，继承该类即可使用。

方法 | 说明
:--- | :---
Token getLoginToken(HttpServletRequest request) | 获取当前登录用户的Token
Integer getLoginUserId(HttpServletRequest request) | 获取当前登录用户的id


## 8.4.支持跨域请求    :id=cors
框架已经做了支持跨域的配置，代码如下：
```java
@Configuration
public class CorsConfigurer implements WebMvcConfigurer {

    // 支持跨域访问
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**").maxAge(3600)
        .allowedHeaders("Content-Type, x-requested-with, X-Custom-Header, Authorization");
    }

    // 支持PUT请求
    @Bean
    public HttpPutFormContentFilter httpPutFormContentFilter() {
        return new HttpPutFormContentFilter();
    }

}
```

关于跨域的相关资料：
- [简单跨域请求和带预检的跨域请求](https://blog.csdn.net/u012017645/article/details/54315923)
- [使用Filter处理跨域请求](https://blog.csdn.net/kendmr/article/details/84793760)



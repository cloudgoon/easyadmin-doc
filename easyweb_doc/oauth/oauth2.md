## 8.1.核心类说明    :id=oauth_desc
com.wf.ew.oauth模块主要是扩展了oauth2框架的客户端类，实现可以在数据库中管理客户端。

类 | 说明
:--- | :---
**client** | 
Client.java | 扩展客户端实体对象，增加客户端名称、客户端密钥两个字段
ClientDao.java | 客户端添删改查的接口
ClientDaoImpl.java | 客户端添删改查的数据库实现
ClientService.java | 客户端添删改查的service
 | 
**configuration** | 
AuthorizationServerConfiguration.java | 认证服务器配置
ResourceServerConfiguration.java | 资源服务器配置
EnableOAuthClient.java | 注解简化，只用在Application上加此注解
 | 
**provider** | 
UrlRegistryProvider.java | 资源服务器将Spring Security的拦截配置抽象出来

OAuth2客户端扩展使用数据库管理参考项目[watchdog-spring-boot-starter](https://github.com/yuequan1997/watchdog-spring-boot-starter)。


## 8.2.UrlRegistryProvider类    :id=urlreg
UrlRegistryProvider.java是将资源服务器的Spring Security拦截配置抽象出来，是一个接口，
在com.wf.ew.common.oauth.AuthorizationProvider.java类中实现了此接口：
```java
@Component
public class AuthorizationProvider implements UrlRegistryProvider {
    @Override
    public boolean configure(ExpressionUrlAuthorizationConfigurer<HttpSecurity>.ExpressionInterceptUrlRegistry config) {
        config
                .antMatchers("/favicon.ico", "/", "/**.html").permitAll()
                .antMatchers("/components/**", "/assets/**").permitAll()
                .antMatchers("/druid", "/druid/**", "/admin/login").permitAll()
                .antMatchers("/swagger-resources", "/v2/**", "/configuration/**", "/webjars/**").permitAll()
                .antMatchers("/file/upload", "/file/uploadBase64", "/file/del").authenticated()
                .antMatchers("/file/**").permitAll()
                .anyRequest().authenticated();
        return true;
    }
}
```

## 8.3.OAuth2.0学习资料
如果要想详细了解每一个类的作用，需要先了解Security-OAuth2框架的一些用法，可参考以下学习资料：

- [Spring Security从入门到进阶系列教程](http://www.spring4all.com/article/428)
- [理解OAuth 2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
- [RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
- [使用Swagger2构建RESTful API](http://www.spring4all.com/article/251)

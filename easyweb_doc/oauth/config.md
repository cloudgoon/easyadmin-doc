## 3.1.application.properties      :id=app

配置文件 |  说明
:--- | :--- 
application.properties | SpringBoot核心配置文件，包括端口、多环境、连接池等配置
application-dev.properties | 开发环境配置，配置开发环境的数据源、日志处理等
application-prod.properties | 生成环境配置，配置生产环境的数据源、日志处理等
application-test.properties | 测试环境配置，配置测试环境的数据源、日志处理等

> 配置application.properties下面的spring.profiles.active=dev来指定使用哪个环境

详细参考资料：
- [Spring Boot 配置文件详解](http://www.spring4all.com/article/248)
- [Spring Boot 入门系列教程](http://blog.didispace.com/spring-boot-learning-1x/)


## 3.2.Security拦截配置      :id=security

Spring Security框架的拦截配置位于com.wf.ew.common.oauth.AuthorizationProvider.java中：

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

> permitAll()代表排除拦截，authenticated()代表需要登录后访问，上面配置了静态资源、登录接口、druid、swagger等排除拦截

SecurityConfiguration.java主要是配置Security的UserDetailsService、密码加密方式等


## 3.3.MybatisPlus配置      :id=mp
MybatisPlus的配置位于com.wf.ew.common.config下面，代码如下：

```java
@Configuration
@EnableTransactionManagement
@MapperScan("com.wf.ew.*.dao")
public class MybatisPlusConfig {
    // 分页插件
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```

> @MapperScan注解是配置扫描mapper的路径，这里配置了就不用在Application上配置的，如果你修改了包名，不要忘记了修改这个位置。
> @EnableTransactionManagement注解是开启事务管理。


## 3.4.日期类型转换器      :id=date

日期类型转换器位于com.wf.ew.common.config.DateConverterConfig.java，如果你的对象有Date类型的字段，前端直接传递如下几种格式的字符串，会自动转换成Date类型：

```
yyyy-MM-dd
yyyy-MM-dd HH:mm
yyyy-MM-dd HH:mm:ss
```

!> **注意：** 一定要是Date类型，代码生成器生成的是LocalTime类型，注意要修改成Date类型


## 3.5.异常处理      :id=exc
&emsp;全局异常处理器位于com.wf.ew.common.exception.GlobalExceptionHandler.java，你在代码的任何位置都可以抛出异常，
异常信息不会直接到达页面，而是返回json数据：
{code: 500, msg: "喔唷，系统出了一个小故障", error: "java.lang.NullPointerException..."}。

**自定义异常：**

异常类 |  说明
:--- | :--- 
BusinessException | 业务异常，默认错误码500，错误信息“系统错误”
ParameterException | 参数异常，默认错误码400，错误信息“参数错误”

有时候异常是需要你抛出，而不是捕获，尤其是Service层，因为抛出异常才可以做事物回滚，例如：

```java
@Service
public class UserServiceImpl {

    @Override
    @Transactional(rollbackFor = Exception.class)
    public boolean addUser(User user, List<Integer> roleIds) {
        boolean result = baseMapper.insert(user) > 0;
        if (result) {
            if (userRoleMapper.insertBatch(user.getUserId(), roleIds) < 0) {
                throw new BusinessException("添加失败");
            }
        }
        return result;
    }
}
```
&emsp;上面是一个添加用户的例子，添加完用户后还要添加用户的角色，如果角色添加失败，应该回滚添加的用户，此时抛出异常才可回滚。

使用场景二：
```java
@Service
public class UserServiceImpl {

    @Override
    public boolean addUser(User user) {
        if (baseMapper.selectByUsername(user.getUsername()) != null) {
            throw new BusinessException("账号已经存在");
        }
        return baseMapper.insert(user) > 0;
    }
}
```
&emsp;上面仍然是一个添加用户的例子，添加用户之前要判断账号是否存在，但我们的service返回的是boolean的类型，直接返回false不合适，
如果在controller里判断，那么每个调用service的controller都要判断，难免不妥，此时可以用自定义的BusinessException抛出异常，
经过异常处理器的处理之后，前端收到的json仍然是：{code: 500, msg: "账号已经存在"}，既简单，又方便。

自定义异常的使用：
```
throw new BusinessException("账号已经存在");
throw new BusinessException(500, "账号已经存在");  // 你可以重写code，默认是500

throw new ParameterException("参数不能为空");
throw new ParameterException(400, "参数不能为空");  // 你可以重写code，默认是400
```

&emsp;BusinessException和ParameterException都继承IException，因为Exception没有code这个字段，只有message字段，
IException增加了code字段，所以要自定义其他类型的异常，最好也继承IException。


## 3.6.resources目录      :id=res

**mapper：**

&emsp;mapper目录用于存放mapper的xml文件，`system`是系统管理模块的xml，建议不同模块分不同目录存放。

**static：**

&emsp;static目录用于存放静态资源，包括前端页面，如果要做分离开发、分离部署，static目录下面的代码可以完全移除，
单独作为一个前端项目，部署直可使用nginx服务器。

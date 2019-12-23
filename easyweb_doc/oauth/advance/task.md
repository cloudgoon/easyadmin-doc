## 7.3.1.定时任务      :id=scheduling
首先在WebApplication上面加`@EnableScheduling`注解开启定时任务功能：
```java
@EnableScheduling
@SpringBootApplication
public class EasyWebApplication {
    public static void main(String[] args) {
        SpringApplication.run(EasyWebApplication.class, args);
    }
}
```

然后在需要定时执行的方法上面加`@Scheduled`注解：
```html
@Service
public class Test {

    @Scheduled(cron="*/5 * * * * *")
    public void reportCurrentTime() {
        System.out.println("Hello");
    }
}
```

常用cron表达式：

表达式 | 含义
:--- | :---
0/2 * * * * ?  |   表示每2秒 执行任务
0 0/2 * * * ?    |  表示每2分钟 执行任务
0 0 2 1 * ?   |  表示在每月的1日的凌晨2点调整任务
0 15 10 ? * MON-FRI   |  表示周一到周五每天上午10:15执行作业
0 15 10 ? 6L 2002-2006   |  表示2002-2006年的每个月的最后一个星期五上午10:15执行作
0 0 10,14,16 * * ?  |   每天上午10点，下午2点，4点 
0 0/30 9-17 * * ?   |  朝九晚五工作时间内每半小时 
0 0 12 ? * WED   |   表示每个星期三中午12点 
0 0 12 * * ?   |  每天中午12点触发 
0 15 10 ? * *   |   每天上午10:15触发 
0 15 10 * * ?    |   每天上午10:15触发 
0 15 10 * * ?   |   每天上午10:15触发 
0 15 10 * * ? 2005   |   2005年的每天上午10:15触发 
0 * 14 * * ?    |   在每天下午2点到下午2:59期间的每1分钟触发 
0 0/5 14 * * ?    |  在每天下午2点到下午2:55期间的每5分钟触发 
0 0/5 14,18 * * ?   |    在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发 
0 0-5 14 * * ?   |   在每天下午2点到下午2:05期间的每1分钟触发 
0 10,44 14 ? 3 WED  |    每年三月的星期三的下午2:10和2:44触发 
0 15 10 ? * MON-FRI   |   周一至周五的上午10:15触发 
0 15 10 15 * ?   |   每月15日上午10:15触发 
0 15 10 L * ?   |   每月最后一日的上午10:15触发 
0 15 10 ? * 6L   |   每月的最后一个星期五上午10:15触发 
0 15 10 ? * 6L 2002-2005  |   2002年至2005年的每月的最后一个星期五上午10:15触发 
0 15 10 ? * 6#3   |  每月的第三个星期五上午10:15触发


详细教程：
- [Spring Boot中使用@Scheduled创建定时任务](http://blog.didispace.com/springbootscheduled/)
- [在线Cron表达式生成器](http://www.bejson.com/othertools/cron/)


## 7.3.2.异步任务      :id=async
首先在WebApplication上面加`@EnableAsync`注解开启异步任务功能：
```java
@EnableAsync
@SpringBootApplication
public class EasyWebApplication {

    public static void main(String[] args) {
        SpringApplication.run(EasyWebApplication.class, args);
    }
}
```

然后在你需要异步执行的方法上面加`@Async`注解：
```html
@Service
public class Test {

    @Async
    public void reportCurrentTime() {
        Thread.sleep(10000);
        System.out.println("任务完成，耗时10s");
    }
}
```

详细教程：
- [Spring Boot中使用@Async实现异步调用](http://blog.didispace.com/springbootasync/)


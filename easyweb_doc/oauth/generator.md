## 9.1.配置代码生成器   :id=config
在项目的test目录下面有一个CodeGenerator.java，此类可以生成model、service、mapper、xml的代码，
使用前首先要修改配置：

```java
public class CodeGenerator {
    // 数据库连接配置
    private static final String DB_URL = "jdbc:mysql://localhost:3306/easyweb-shiro";
    private static final String DB_USERNAME = "root";
    private static final String DB_PASSWORD = "123456";
    // 数据库表前缀
    private static final String[] TB_PREFIX = new String[]{"sys_", "tb_"};
    // 输出位置
    private static final String OUTPUT_DIR = "d:\\codeGen";
    // 包名
    private static final String PACKAGE_NAME = "com.wf.ew.system";
    // 需要生成的表名
    private static final String[] TABLE_NAMES = {"sys_user", "sys_role"};

    public static void main(String[] args) {
        generateByTables(false, PACKAGE_NAME, TABLE_NAMES);
    }
}
```

## 9.2.生成代码   :id=gen
修改好配置后点击绿色的运行图标运行，生成完成后会自动打开文件的输出目录，
把生成好的service、mapper、model、controller复制到项目src中，把xml复制到resources/mapper下面即可。

!> **注意：** 日期和时间类型的字段生成的model里面是LocalTime类型，一定要全局替换成Date类型，不然框架的日期类型转换器和json时间格式设置不会生效

1. 对着src目录右键，先全局替换java.time.LocalDateTime：

![](https://s2.ax1x.com/2019/08/31/mz5fQ1.png)

2. 再全局替换LocalDateTime：

![](https://s2.ax1x.com/2019/08/31/mz5odO.png)


> 更强大的代码生成器正在开发中，敬请期待...

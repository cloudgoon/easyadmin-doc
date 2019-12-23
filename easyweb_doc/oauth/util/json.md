## 6.3.1.全部方法     :id=method

方法 | 说明
:--- | :---
int getCode(String json) | 获取json里面的code
String getMessage(String json) | 获取json里面的msg
getObject(String json, String key, Class<T> clazz) | 得到对象类型的值
getArray(String json, String key, Class<T> clazz) | 得到对象类型的集合
parseObject(String json, Class<T> clazz) | json转换换成对象
parseArray(String json, Class<T> clazz) | json转换换成集合
String getString(String json, String key) | 获取String类型数据
int getIntValue(String json, String key) | 得到int类型的值
short getShortValue(String json, String key) | 得到short类型的值
long getLongValue(String json, String key) | 得到long类型的值
float getFloatValue(String json, String key) | 得到float类型的值
double getDoubleValue(String json, String key) | 得到double类型的值
getBooleanValue(String json, String key) | 得到boolean类型的值
byte getByteValue(String json, String key) | 得到byte类型的值
byte[] getBytes(String json, String key) | 得到byte[]类型的值
Integer getInteger(String json, String key) | 得到Integer类型的值
Short getShort(String json, String key) | 得到Short类型的值
long getLong(String json, String key) | 得到Long类型的值
Float getFloat(String json, String key) | 得到Float类型的值
Double getDouble(String json, String key) | 得到Double类型的值
Boolean getBoolean(String json, String key) | 得到Boolean类型的值
Byte getByte(String json, String key) | 得到Byte类型的值
BigDecimal getBigDecimal(String json, String key) | 得到BigDecimal类型的值
BigInteger getBigInteger(String json, String key) | 得到BigInteger类型的值
Date getDate(String json, String key) | 得到Date类型的值

## 6.3.2.使用示例     :id=demo
```java
String json = "{code: 200, msg: \"ok\", data: {name:\"Joe\", sex: \"man\"} }";
int code = JSONUtil.getCode(json);
User user = JSONUtil.getObject(json, "data", User.class);

String json = "{name:\"Joe\", sex: \"man\"}";
User user = JSONUtil.parseObject(json, User.class);
```


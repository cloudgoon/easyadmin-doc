## 5.1.UUIDUtil      :id=uuid

方法 | 参数 | 说明
:--- | :--- | :---
String randomUUID8() | 无 | 生成8位uuid
String randomUUID32() | 无 | 生成32位uuid，去掉了“-”

```java
String uuid = UUIDUtil.randomUUID8();
```

## 5.2.StringUtil      :id=string

方法 | 参数 | 说明
:--- | :--- | :---
boolean isBlank(String str) | str | 判断字符串是否是空字符串
boolean isBlank(String... strs) | str | 判断字符串是否是空字符串
boolean isNotBlank(String str) | str | 判断字符串是否不是空字符串
String getStr(String str) | str | 如果str是null返回空，用于防止字符串相加出现null
String upperHeadChar | str | 字符串首字母大写


## 5.3.HttpUtil      :id=http

方法 | 说明
:--- | :---
String get(String url, MultiValueMap<String, String> params) | get请求
String get(String url, MultiValueMap<String, String> params, MultiValueMap<String, String> headers) | get请求
String post(String url, MultiValueMap<String, String> params) | post请求
String post(String url, MultiValueMap<String, String> params, MultiValueMap<String, String> headers) | post请求
String request(String url, MultiValueMap<String, String> params, MultiValueMap<String, String> headers, HttpMethod method) | 自定义请求方式
String request(String url, Object params, MultiValueMap<String, String> headers, HttpMethod method, MediaType mediaType) | 自定义请求方式

使用示例：
```java
// get请求
String res = HttpUtil.get("https://baidu.com", null);
System.out.println(res);

// 带参数
MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
params.add("name", "hahaha");
String res = HttpUtil.get("https://baidu.com", params);
System.out.println(res);

// json参数提交
User user = new User();
String res = HttpUtil.request("https://baidu.com", user, null, HttpMethod.POST, MediaType.APPLICATION_JSON);
System.out.println(res);
```

> 此工具类是基于SpringBoot的RestTemplate实现的


## 5.4.ReflectUtil      :id=reflect

方法 | 参数 | 说明
:--- | :--- | :---
Map<String, Object> objectToMap(T t) | object | 把对象转成Map
List<Map<String, Object>> objectToMap(List<T> ts) | list | 把对象集合转成Map集合
Map<String, Object> getResultMap(T t, String[] fileds) | object，fileds | 把对象的指定字段转成Map
List<Map<String, Object>> getResultMap(List<T> ts, String[] fileds) |  | 把对象集合的指定字段转成Map集合


## 5.5.更多      :id=more

MD5加密：

```java
// 使用com.wf.ew.EndecryptUtil.java
String md5 = EndecryptUtil.encrytMd5("123456");
String md5 = EndecryptUtil.encrytMd5("123456", "salt", 3);  // 盐、散列次数

// spring自带的
String md5 = DigestUtils.md5DigestAsHex("123456".getBytes());
```

base64编码、解码：

```java
// 把图片进行base64编码
FileInputStream in = new FileInputStream(new File("C:/a.png"));
byte[] bytes = new byte[in.available()];
in.read(bytes);
String base64 = Base64.getEncoder().encodeToString(bytes);
System.out.println("data:image/png;base64," + base64);

// 对base64进行解码
String base64 = "";
byte[] bytes = Base64.getDecoder().decode(base64.getBytes(base64));
FileOutputStream out = new FileOutputStream(new File("C:/a.png"));
out.write(bytes);
```

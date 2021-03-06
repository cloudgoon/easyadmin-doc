## 6.2.1.使用配置     :id=config
RedisUtil默认不能使用，如果要使用先在RedisUtil中加@Component和@Autowired注解开启：
```java
@Component
public class RedisUtil {
    @Autowired
    private StringRedisTemplate redisTemplate;
}
```
然后在application.properties中配置Redis连接：
```
spring.redis.database=0
spring.redis.host=127.0.0.1
spring.redis.port=6379
spring.redis.password=
```

## 6.2.2.keys相关命令     :id=keys

|  NO  | 方法                                                    |  描述                                    |
|:----:|---------------------------------------------------------|-----------------------------------------|
|   1  | void delete(String key)                                 | key存在时删除key                         |
|   2  | void delete(Collection keys)                            | 批量删除key                              |
|   3  | byte[] dump(String key)                                 | 序列化key，返回被序列化的值               |
|   4  | Boolean hasKey(String key)                              | 检查key是否存在                          |
|   5  | Boolean expire(String key, long timeout, TimeUnit unit) | 设置过期时间                             |
|   6  | Boolean expireAt(String key, Date date)                 | 设置过期时间                             |
|   7  | Set<String> keys(String pattern)                        | 查找所有符合给定模式(pattern)的key        |
|   8  | Boolean move(String key, int dbIndex)                   | 将当前数据库的key移动到给定的数据库db当中  |
|   9  | Boolean persist(String key)                             | 移除key的过期时间，key将持久保持          |
|  10  | Long getExpire(String key, TimeUnit unit)               | 返回key的剩余的过期时间                   |
|  11  | Long getExpire(String key)                              | 返回key的剩余的过期时间                   |
|  12  | String randomKey()                                      | 从当前数据库中随机返回一个key             |
|  13  | void rename(String oldKey, String newKey)               | 修改key的名称                            |
|  14  | Boolean renameIfAbsent(String oldKey, String newKey)    | 仅当newkey不存在时，将oldKey改名为 newkey |
|  15  | DataType type(String key)                               | 返回key所储存的值的类型                   |

TimeUnit是时间单位，可选值有：

- 天: &emsp; TimeUnit.DAYS
- 小时: &emsp; TimeUnit.HOURS
- 分钟: &emsp; TimeUnit.MINUTES
- 秒: &emsp; TimeUnit.SECONDS
- 毫秒: &emsp; TimeUnit.MILLISECONDS


## 6.2.3.String类型操作     :id=string

|  NO  | 方法                                                              | 描述                                 |
|:----:|-------------------------------------------------------------------|-------------------------------------|
|   1  | String get(String key)                                            | 获取指定key的值                      |
|   2  | String getRange(String key, long start, long end)                 | 返回key中字符串值的子字符             |
|   3  | String getAndSet(String key, String value)                        | 将key的值设为value，并返回key旧值     |
|   4  | Boolean getBit(String key, long offset)                           | 对key所储存的值，获取指定位置上的bit   |
|   5  | List multiGet(Collection keys)                                    | 批量获取                             |
|      |     添加相关                                                      |                                      |
|   6  | void set(String key, String value)                                | 设置指定key的值                       |
|   7  | void setEx(String key,String value,long timeout,TimeUnit unit)    | 将值value关联到key，并设置key过期时间  |
|   8  | boolean setBit(String key, long offset, boolean value)            | 设置指定位置上的ASCII码                |
|   9  | boolean setIfAbsent(String key, String value)                     | 只有在 key 不存在时设置 key 的值       |
|  10  | void setRange(String key, String value, long offset)              | 用value覆写key的值，从偏移量offset开始 |
|  11  | void multiSet(Map<String,String> maps)                            | 批量添加                              |
|  12  | boolean multiSetIfAbsent(Map<String,String> maps)                 | 批量添加，仅当所有key都不存在          |
|      |      其他方法                                                      |                                      |
|  13  | Integer append(String key, String value)                          | 追加到末尾                            |
|  14  | Long incrBy(String key, long increment)                           | 增加(自增长), 负数则为自减             |
|  15  | Double incrByFloat(String key, double increment)                  | 增加(自增长), 负数则为自减             |
|  16  | Long size(String key)                                             | 获取字符串的长度                      |

> 关于上面xxBit方法的使用：例如字符'a'的ASCII码是97，转为二进制是'01100001'，
> setBit方法就是把第offset位置上变成0或者1，true是1，false是0。


## 6.2.4.Hash类型操作     :id=hash

|  NO  | 方法                                                           | 描述                                   |
|:----:|----------------------------------------------------------------|---------------------------------------|
|   1  | Object hGet(String key, String field)                          | 获取存储在哈希表中指定字段的值           |
|   2  | Map hGetAll(String key)                                        | 获取所有给定字段的值                    |
|   3  | List hMultiGet(String key, Collection fields)                  | 获取所有给定字段的值                    |
|      |    添加相关                                               |                                       |
|   4  | void hPut(String key, String hashKey, String value)            | 添加字段                               |
|   5  | void hPutAll(String key, Map maps)                             | 添加多个字段                            |
|   6  | Boolean hPutIfAbsent(String key,String hashKey,String value)   | 仅当hashKey不存在时才设置                |
|      |     其他方法                                              |                                        |
|   7  | Long hDelete(String key, Object... fields)                     | 删除一个或多个哈希表字段                 |
|   8  | boolean hExists(String key, String field)                      | 查看哈希表key中指定的字段是否存在         |
|   9  | Long hIncrBy(String key, Object field, long increment)         | 为哈希表key中指定字段的值增加increment   |
|  10  | Double hIncrByFloat(String key, Object field, double delta)    | 为哈希表key中指定字段的值增加increment   |
|  11  | Set hKeys(String key)                                          | 获取所有哈希表中的字段                   |
|  12  | Long hSize(String key)                                         | 获取哈希表中字段的数量                   |
|  13  | List hValues(String key)                                       | 获取哈希表中所有值                       |
|  14  | Cursor hScan(String key, ScanOptions options)                  | 迭代哈希表中的键值对                     |


## 6.2.5.List类型操作     :id=list

|  NO  | 方法                                                     | 描述                                        |
|:----:|----------------------------------------------------------|---------------------------------------------|
|   1  | String lIndex(String key, long index)                    | 通过索引获取列表中的元素                      |
|   2  | List lRange(String key, long start, long end)            | 获取列表指定范围内的元素                      |
|      |     添加相关                                        |                                             |
|   3  | Long lLeftPush(String key, String value)                 | 存储在list头部                               |
|   4  | Long lLeftPushAll(String key, String... value)           | 存储在list头部                               |
|   5  | Long lLeftPushAll(String key, Collection value)          | 存储在list头部                               |
|   6  | Long lLeftPushIfPresent(String key, String value)        | 当list存在的时候才加入                        |
|   7  | lLeftPush(String key, String pivot, String value)        | 如果pivot存在,再pivot前面添加                 |
|      |                                                          |                                             |
|   8  | Long lRightPush(String key, String value)                | 存储在list尾部                               |
|   9  | Long lRightPushAll(String key, String... value)          | 存储在list尾部                               |
|  10  | Long lRightPushAll(String key, Collection value)         | 存储在list尾部                               |
|  11  | Long lRightPushIfPresent(String key, String value)       | 当list存在的时候才加入                        |
|  12  | lRightPush(String key, String pivot, String value)       | 在pivot元素的右边添加值                       |
|      |                                                          |                                              |
|  13  | void lSet(String key, long index, String value)          | 通过索引设置列表元素的值                       |
|      |    删除相关                                          |                                             |
|  14  | String lLeftPop(String key)                              | 移出并获取列表的第一个元素                     |
|  15  | String lBLeftPop(String key,long timeout,TimeUnit unit)  | 移出并获取第一个元素,没有则阻塞直到超时或有为止  |
|      |                                                          |                                              |
|  16  | String lRightPop(String key)                             | 移除并获取列表最后一个元素                     |
|  17  | String lBRightPop(String key,long timeout,TimeUnit unit) | 移出并获取最后个元素,没有则阻塞直到超时或有为止  |
|  18  | String lRightPopAndLeftPush(String sKey,String dKey)     | 移除最后一个元素并加到另一个列表并返回          |
|  19  | String lBRightPopAndLeftPush(sKey,dKey,timeout,unit)     | 移除最后个元素并加到另个列表并返回,阻塞超时或有  |
|      |                                                          |                                              |
|  20  | Long lRemove(String key, long index, String value)       | 删除集合中值等于value得元素                    |
|  21  | void lTrim(String key, long start, long end)             | 裁剪list                                     |
|      |     其他方法                                         |                                              |
|  22  | Long lLen(String key)                                    | 获取列表长度                                  |


## 6.2.6.Set类型操作     :id=set

|  NO  | 方法                                                                     | 描述                          |
|:----:|--------------------------------------------------------------------------|-------------------------------|
|   1  | Set<String> sMembers(String key)                                         | 获取集合所有元素               |
|   2  | Long sSize(String key)                                                   | 获取集合大小                   |
|   3  | Boolean sIsMember(String key, Object value)                              | 判断集合是否包含value          |
|   4  | String sRandomMember(String key)                                         | 随机获取集合中的一个元素       |
|   5  | List<String> sRandomMembers(String key, long count)                      | 随机获取集合count个元素        |
|   6  | Set<String> sDistinctRandomMembers(String key, long count)               | 随机获取count个元素并去除重复的 |
|   7  | Cursor<String> sScan(String key, ScanOptions options)                    | 使用迭代器获取元素             |
|      |                                                                          |                               |
|   8  | Set<String> sIntersect(String key, String otherKey)                      | 获取两个集合的交集             |
|   9  | Set<String> sIntersect(String key, Collection<String> otherKeys)         | 获取key集合与多个集合的交集     |
|  10  | Long sIntersectAndStore(String key, String oKey, String dKey)            | key集合与oKey的交集存储到dKey中 |
|  11  | Long sIntersectAndStore(String key,Collection<String> oKeys,String dKey) | key与多个集合的交集存储到dKey中 |
|      |                                                                          |                               |
|  12  | Set<String> sUnion(String key, String otherKeys)                         | 获取两个集合的并集             |
|  13  | Set<String> sUnion(String key, Collection<String> otherKeys)             | 获取key集合与多个集合的并集     |
|  14  | Long sUnionAndStore(String key, String otherKey, String destKey)         | key集合与oKey的并集存储到dKey中 |
|  15  | Long sUnionAndStore(String key,Collection<String> oKeys,String dKey)     | key与多个集合的并集存储到dKey中 |
|      |                                                                          |                               |
|  16  | Set<String> sDifference(String key, String otherKey)                     | 获取两个集合的差集             |
|  17  | Set<String> sDifference(String key, Collection<String> otherKeys)        | 获取key集合与多个集合的差集     |
|  18  | Long sDifference(String key, String otherKey, String destKey)            | key与oKey集合的差集存储到dKey中 |
|  19  | Long sDifference(String key,Collection<String> otherKeys,String dKey)    | key与多个集合的差集存储到dKey中 |
|      |    添加相关                                                         |                                |
|  20  | Long sAdd(String key, String... values)                                  | 添加                           |
|      |    删除相关                                                          |                               |
|  21  | Long sRemove(String key, Object... values)                               | 移除                           |
|  22  | String sPop(String key)                                                  | 随机移除一个元素                |
|  23  | Boolean sMove(String key, String value, String destKey)                  | 将key集合中value移到destKey中   |


## 6.2.7.zSet类型操作     :id=zset

|  NO  | 方法                                                                       | 描述                             |
|:----:|----------------------------------------------------------------------------|----------------------------------|
|   1  | Set<String> zRange(String key, long start, long end)                       | 获取元素,小到大排序,s开始e结束位置 |
|   2  | Set<TypedTuple<String>> zRangeWithScores(String key, long start, long end) | 获取集合元素, 并且把score值也获取  |
|   3  | Set<String> zRangeByScore(String key, double min, double max)              | 根据score范围查询元素,从小到大排序 |
|   4  | Set<TypedTuple<String>> zRangeByScoreWithScores(key,double min,double max) | 根据score范围查询元素,并返回score |
|   5  | Set<TypedTuple> zRangeByScoreWithScores(key,double min,max,long start,end) | 根据score查询元素,s开始e结束位置   |
|      |                                                                            |                                  |
|   6  | Set<String> zReverseRange(String key, long start, long end)                | 获取集合元素, 从大到小排序         |
|   7  | Set<TypedTuple<String>> zReverseRangeWithScores(key, long start, long end) | 获取元素,从大到小排序,并返回score  |
|   8  | Set<String> zReverseRangeByScore(String key, double min, double max)       | 根据score范围查询元素,从大到小排序 |
|   9  | Set<TypedTuple> zReverseRangeByScoreWithScores(key,double min,double max)  | 根据score查询,大到小排序返回score |
|  10  | Set<String> zReverseRangeByScore(key, double min, max, long start, end)    | 根据score查询,大到小,s开始e结束   |
|      |                                                                            |                                  |
|  11  | Long zRank(String key, Object value)                                       | 返回元素在集合的排名,score由小到大 |
|  12  | Long zReverseRank(String key, Object value)                                | 返回元素在集合的排名,score由大到小 |
|  13  | Long zCount(String key, double min, double max)                            | 根据score值范围获取集合元素的数量  |
|  14  | Long zSize(String key)                                                     | 获取集合大小                      |
|  15  | Long zZCard(String key)                                                    | 获取集合大小                      |
|  16  | Double zScore(String key, Object value)                                    | 获取集合中value元素的score值      |
|      |                                                                            |                                  |
|  17  | Long zUnionAndStore(String key, String otherKey, String destKey)           | 获取key和oKey的并集并存储在dKey中 |
|  18  | Long zUnionAndStore(String key,Collection<String> otherKeys,String dKey)   | 获取key和多个集合并集并存在dKey中  |
|      |                                                                            |                                  |
|  19  | Long zIntersectAndStore(String key, String otherKey, String destKey)       | 获取key和oKey交集并存在destKey中  |
|  20  | Long zIntersectAndStore(String key,Collection<String> oKeys,String dKey)   | 获取key和多个集合交集并存在dKey中  |
|      |                                                                            |                                  |
|  21  | Cursor<TypedTuple<String>> zScan(String key, ScanOptions options)          | 使用迭代器获取                    |
|      |    添加相关                                                            |                                 |
|  22  | Boolean zAdd(String key, String value, double score)                       | 添加元素,zSet按score由小到大排列  |
|  23  | Long zAdd(String key, Set<TypedTuple<String>> values)                      | 批量添加,TypedTuple使用见下面介绍 |
|      |    删除相关                                                           |                                  |
|  24  | Long zRemove(String key, Object... values)                                 | 移除                             |
|  25  | Double zIncrementScore(String key, String value, double delta)             | 增加元素的score值,并返回增加后的值 |
|  26  | Long zRemoveRange(String key, long start, long end)                        | 移除指定索引位置的成员            |
|  27  | Long zRemoveRangeByScore(String key, double min, double max)               | 根据指定的score值的范围来移除成员  |

> 批量添加时`TypedTuple`的使用：
> TypedTuple<String> typedTuple = new DefaultTypedTuple<String>(value,score)


## 6.2.8.额外补充     :id=more
&emsp;RedisUtil是使用StringRedisTemplate实现的，Spring中可使用StringRedisTemplate和RedisTemplate两种方式操作redis，
二者主要区别是使用的序列化类不一样，RedisTemplate使用的是JdkSerializationRedisSerializer， 
StringRedisTemplate使用的是StringRedisSerializer，两者的数据是不共通的。

RedisTemplate：

&emsp;RedisTemplate使用的是JDK的序列化策略，向Redis存入数据会将数据先序列化成字节数组然后在存入Redis数据库，
打开Redis查看的时候，看到的数据是以字节数组显示，类似下面：`\xAC\xED\x00\x05t\x05sr\x00`。 

StringRedisTemplate:

&emsp;StringRedisTemplate默认采用的是String的序列化策略，保存的key和value都是采用此策略序列化保存的，
StringRedisTemplate是继承RedisTemplate的，这种对redis的操方式更优雅，任何Redis连接工具，都可以读出直观的数据，便于数据的维护。


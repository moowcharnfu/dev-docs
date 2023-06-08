Guava Cache是本地缓存的不二之选(caffeine没现世之前)，注：guava cache并没使用额外的线程去做定时清理和加载的功能，而是依赖于查询请求。


三种基于时间的清理或刷新缓存数据的方式：


expireAfterAccess: 当缓存项在指定的时间段内没有被读或写就会被回收。


expireAfterWrite：当缓存项在指定的时间段内没有更新就会被回收。


refreshAfterWrite：当缓存项上一次更新操作之后的多久会被刷新。


考虑到时效性，我们可以使用expireAfterWrite，使每次更新之后的指定时间让缓存失效，然后重新加载缓，如果过期时长过短请慎重；

refreshAfterWrite的特点是，在refresh的过程中，严格限制只有1个重新加载操作，而其他查询先返回旧值，这样有效地可以减少等待和锁争用，所以refreshAfterWrite会比expireAfterWrite性能好。但是它也有一个缺点，因为到达指定时间后，它不能严格保证所有的查询都获取到新值。

综上，由于refreshAfterWrite长时间没触发更新会返回旧数据（场景：多台机器，某台得到最新，其他得到旧值），因此采用expireAfterWrite方式，如5分钟没有更新就重新加载，保证最新数据。

```
com.github.benmanes.caffeine.cache.Cache方式(更高效)
声明
Cache<String, List<TreeDTO>> labelTreeCache = Caffeine.newBuilder().expireAfterWrite(Duration.ofMinutes(30L)).initialCapacity(100).maximumSize(1000L).build();
添加
List<TreeDTO> treeDTOList = labelTreeCache.getIfPresent(key);
if (CollectionUtils.isEmpty(treeDTOList)) {
    treeDTOList = labelService.listLabelTreeByLabelId(userExt.getGroupCode(), labelId, type);
    if (CollectionUtils.isNotEmpty(treeDTOList)) {
        // 加入缓存
        labelTreeCache.put(key, treeDTOList);
    }
}
删除
labelTreeCache.invalidateAll();
# 单个删除性能更好
labelTreeCache.invalidate(key);
```

```
com.google.common.cache.Cache方式
声明
Cache<String, Integer> _5_EMAIL_NUMBER_CODE_CACHE = CacheBuilder.newBuilder().expireAfterWrite(24L, TimeUnit.HOURS).maximumSize(5000L).build();
添加
Integer value = _5_EMAIL_NUMBER_CODE_CACHE.getIfPresent(email);
if (null == value) {
    _5_EMAIL_NUMBER_CODE_CACHE.put(email, code.intValue());
}
删除
_5_EMAIL_NUMBER_CODE_CACHE.invalidateAll();
# 单个删除性能更好
_5_EMAIL_NUMBER_CODE_CACHE.invalidate(key);
```

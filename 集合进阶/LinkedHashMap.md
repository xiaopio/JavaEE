# LinkedHashMap

**由键决定：有序**、不重复、无索引

这里的有序指的是保证存储和取出来的元素顺序一致

**原理**：底层数据结构依旧是Hash表，只是每个键值对元素又额外多了一个双链表的机制，来记录存储的顺序

```java
// 创建集合
LinkedHashMap<String, Integer> linkedHashMap = new LinkedHashMap<>();
// 添加元素
// 键唯一，且集合有序
linkedHashMap.put("a", 123);
linkedHashMap.put("a", 111);
linkedHashMap.put("c", 789);
linkedHashMap.put("b", 456);

// 打印集合
// 结果：{a=111, c=789, b=456}
System.out.println(linkedHashMap);
```
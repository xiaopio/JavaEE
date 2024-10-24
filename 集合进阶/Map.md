# Map

## Map常见的API

Map是

#### put（添加）

```java
// 1.创建Map集合的对象
Map<String, String> m = new HashMap<>();

// 2.添加对象
// put方法的细节：
// 添加/覆盖
// 在添加数据时，如果键不存在，直接把键值对对象添加到map集合中
// 在添加数据的时候，如果键是存在的，会把原有的键值对对象进行覆盖，会把被覆盖的值进行返回。
String value1 = m.put("郭靖", "黄蓉");
m.put("韦小宝", "沐剑屏");
m.put("尹志平", "小龙女");

// 这里 value1 的值为 null
System.out.println(value1);

// 这里键已经存在，进行覆盖操作，并把原有的值进行返回
String value2 = m.put("郭靖", "双儿");
// 这里 value2 的值为 黄蓉
System.out.println(value2);

// 3.打印集合
// 第一次打印结果：{韦小宝=沐剑屏, 尹志平=小龙女, 郭靖=黄蓉}
// 第二次打印结果：{韦小宝=沐剑屏, 尹志平=小龙女, 郭靖=双儿}
System.out.println(m);
```

#### remove（删除）

```java
// remove删除操作
String value = m.remove("郭靖");
// 把键值 黄蓉 进行返回
System.out.println(value);
// 输出结果：{韦小宝=沐剑屏, 尹志平=小龙女}
System.out.println(m);
```

#### clear（清空）

```java
// 清空
m.clear();

// 打印集合
// 输出结果：{}
System.out.println(m);
```

#### containKey（键是否存在）

```java
boolean keyResult = m.containsKey("郭靖");
// 输出结果：true
System.out.println(keyResult);
```

#### containValue（值是否存在）

```java
boolean valueResult = m.containsValue("小龙女");
// 输出结果：true
System.out.println(valueResult);
```

#### isEmpty（是否为空）

```java
boolean result = m.isEmpty();
// 输出结果：false
System.out.println(result);
```

## Map集合的遍历

#### 第一种方式：通过键找值

##### 增强for循环

```java
// Map集合的第一种遍历方式

// 1.创建Map集合的对象
Map<String, String> map = new HashMap<>();

// 2.添加元素
map.put("尹志平", "小龙女");
map.put("郭靖", "穆念慈");
map.put("欧阳克", "黄蓉");

// 3.通过键找值
// 获取所有的键，把这些键放到一个单列集合当中
Set<String> keys = map.keySet();
// 遍历单列集合，得到每一个键
// 增强for循环方式
for (String key : keys) {
    // 打印结果：尹志平 郭靖 欧阳克
    System.out.print(key + " ");
    // 利用map集合中的键获取对应的值
    String value = map.get(key);
    // 输出结果：
    // 尹志平 尹志平 = 小龙女
    // 郭靖 郭靖 = 穆念慈
    // 欧阳克 欧阳克 = 黄蓉
    System.out.println(key + " = " + value);
}
```

##### 迭代器

```java
// 3.通过键找值
// 获取所有的键，把这些键放到一个单列集合当中
Set<String> keys = map.keySet();
Iterator<String> iterator = keys.iterator();
while (iterator.hasNext()) {
    // 得到键
    String key = iterator.next();
    // 得到值
    String value = map.get(key);
    System.out.println(key + " = " + value);
}
```

##### Lambda表达式



#### 第二种方式：通过键值对

```java
Map<String, String> map = new HashMap<>();

map.put("尹志平", "小龙女");
map.put("郭靖", "穆念慈");
map.put("欧阳克", "黄蓉");

// Map集合的第二种遍历方式
// 通过键值对对象进行遍历
// 通过一个方法获取所有的键值对对象，返回一个Set集合
Set<Map.Entry<String, String>> entries = map.entrySet();
// 遍历 集合，得到每一个对象
for (Map.Entry<String, String> entry : entries) {
    String key = entry.getKey();
    String value = entry.getValue();
    System.out.println(key + " = " + value);
}
```

#### 第三种方式：Lambda表达式

forEach其实就是用第二种方法进行遍历，得到每一个键和值

再调用accept方法

```java
Map<String, String> map = new HashMap<>();

map.put("尹志平", "小龙女");
map.put("郭靖", "穆念慈");
map.put("欧阳克", "黄蓉");

// 使用 Lambda 表达式遍历 Map
map.forEach((key, value) -> {
    System.out.println(key + " = " + value);
});

// 简化
map.forEach((key, value) -> System.out.println(key + " = " + value));
```
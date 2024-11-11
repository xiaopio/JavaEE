# properties

```java
public static void main(String[] args) {
    /*
    properties作为Map集合的操作
     */
    // 1.创建集合对象
    Properties prop = new Properties();

    // 2.添加数据
    // 细节：虽然我们可以往properties当中添加任意类型的数据，但是一般只会往里面添加字符串类型的数据
    // 键是唯一的
    prop.put("aaa", "111");
    prop.put("bbb", "222");
    prop.put("ccc", "333");
    prop.put("ddd", "444");

    // 3.打印数据
    System.out.println(prop);
    // 遍历集合
    Set<Object> keys = prop.keySet();
    for (Object key : keys) {
        Object value = prop.get(key);
        System.out.println(key + " = " + value);
    }

    Set<Map.Entry<Object, Object>> entries = prop.entrySet();
    for (Map.Entry<Object, Object> entry : entries) {
        Object key = entry.getKey();
        Object value = entry.getValue();
        System.out.println(key + " = " + value);
    }
    prop.forEach((key, value) -> System.out.println(key + " = " + value));
}
```



## 写入文件

```java
public static void main(String[] args) throws IOException {
    /*
    properties作为Map集合的操作
     */
    // 1.创建集合对象
    Properties prop = new Properties();

    // 2.添加数据
    // 细节：虽然我们可以往properties当中添加任意类型的数据，但是一般只会往里面添加字符串类型的数据
    // 键是唯一的
    prop.put("aaa", "111");
    prop.put("bbb", "222");
    prop.put("ccc", "333");
    prop.put("ddd", "444");

    // 3.把集合中的数据以键值对的形式写到本地文件中
    FileOutputStream fos = new FileOutputStream("a.properties");
    prop.store(fos, "test");
    fos.close();
}
```

### a.properties

```properties
#test
#Sun Nov 10 16:21:45 CST 2024
aaa=111
bbb=222
ccc=333
ddd=444
```

## 读取本地properties文件

```java
public static void main(String[] args) throws IOException {
    // 1.创建集合
    Properties prop = new Properties();
    // 2.读取本地文件properties中的数据
    FileInputStream fis = new FileInputStream("a.properties");
    prop.load(fis);
    fis.close();
    // 3.打印集合
    System.out.println(prop);
}
```


































































































































































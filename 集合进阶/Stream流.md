# Stream流

### 总结：

1. Stream流的作用

​		结合了Lambda表达式，简化集合、数组的操作

2. Stream流的使用步骤

​		获取Stream流对象

​		使用中间方法处理数据

​		使用终结方法处理数据

3. 如何获取Stream流对象

​		单列集合：Collection中的默认方法stream

​		双列集合：不能直接获取

​		数组：Arrays工具类型中的静态方法

​		一堆零散数据：Stream接口中的静态方法of

4. 常见方法

​		中间方法：filter，limit，skip，distinct，concat，map

​		终结方法：forEach，count，collect

        需求：
        创建集合添加元素，要求：
        1.把所有以“张”开头的元素存储到新集合中
        2.把“张”开头的，长度为3的元素再存储到新集合中
        3.遍历打印上面结果

```java
public class StreamDemo1 {
    public static void main(String[] args) {
        /*
        需求：
        创建集合添加元素，要求：
        1.把所有以“张”开头的元素存储到新集合中
        2.把“张”开头的，长度为3的元素再存储到新集合中
        3.遍历打印上面结果
         */
        ArrayList<String> list1 = new ArrayList<>();
        Collections.addAll(list1, "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");

        // 1.把所有以“张”开头的元素存储到新集合中
        ArrayList<String> list2 = new ArrayList<>();
        for (String name : list1) {
            if (name.startsWith("张")) {
                list2.add(name);
            }
        }
        for (String name : list2) {
            System.out.print(name + " ");
        }
        System.out.println();

        // 2.把“张”开头的，长度为3的元素再存储到新集合中
        ArrayList<String> list3 = new ArrayList<>();
        for (String name : list2) {
            if (name.length() == 3) {
                list3.add(name);
            }
        }

        // 3.遍历打印上面结果
        for (String name : list3) {
            System.out.print(name + " ");
        }
    }
}
```

代码好多，好麻烦！！！

解决方法：Stream流！！！

```java
ArrayList<String> list1 = new ArrayList<>();
Collections.addAll(list1, "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");

list1.stream().filter(name -> name.startsWith("张")).filter(name -> name.length() == 3).forEach(name -> System.out.println(name));
```

### Stream流的思想

流水线 数据——》过滤——》输出

### Stream流的作用

​	结合了Lambda表达式，简化集合、数组操作

### Stream流的使用步骤

1. 得到一条Stream流（流水线），并把数据放上去

2. 利用各种Stream流中的API进行各种操作

​		过滤	转换	中间方法	方法调用完毕之后，还可以调用其他方法，例如：过滤操作

​		统计	打印	终结方法	方法调用完毕后，不能再调用其他方法，例如：打印操作

①先得到一条Stream流，把数据放上去

##### 单列集合获取Stream流

```java
// 1. 单列集合获取Stream流
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "a", "b", "c", "d", "e");
// 获取到一条流水线，并把集合中的数据放到流水线上
Stream<String> stream1 = list.stream();
// s：依次表示流水线上的每一个数据
stream1.forEach(s -> System.out.println(s));
// 链式编程
list.stream().forEach(s -> System.out.println(s));
```

##### 双列集合获取Stream流

```java
// 2.双列集合获取Stream流
// 创建双列集合
HashMap<String, Integer> hm = new HashMap<>();
// 添加数据
hm.put("aaa", 111);
hm.put("bbb", 222);
hm.put("ccc", 333);
hm.put("ddd", 444);
// 第一种获取Stream流方法
// 通过键
hm.keySet().stream().forEach(s -> System.out.println(s));
// 第二种获取Stream流方法
// 通过键值对
hm.entrySet().stream().forEach(s -> System.out.println(s));
```

##### 数组获取Stream流

基本数据类型

```java
// 数组获取Stream流
// 创建数组
int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
// 获取Stream流
Arrays.stream(arr).forEach(s -> System.out.println(s));
```

引用数据类型

```java
String[] arr = {"a", "b", "c", "d"};
Arrays.stream(arr).forEach(s -> System.out.println(s));
```

##### 零散数据获取Stream流

Stream接口中静态方法of的细节：
	方法的形参是一个可变参数，可以传递一堆零散的数据，也可以传递数组

​	但是**数组必须是引用数据类型**的，如果传递基本数据类型，是会把整个数组当成一个元素，放到Stream流中

基本数据类型

```java
// 零散数据获取Stream流
Stream.of(1, 2, 3, 4, 5).forEach(s -> System.out.println(s));
```

引用数据类型

```java
// 零散数据获取Stream流
Stream.of("a", "b", "c", "d").forEach(s -> System.out.println(s));
```

### Stream流的中间方法

注意1：中间方法，返回新的Stream流，**原来的Stream流只能使用一次**，建议使用链式编程

注意2：修改Stream流中的数据，**不会影响原来**集合或者数组中**的数据**

##### filter 过滤

```java
Stream<String> stream = list.stream()
							.filter(s -> s.startsWith("张"))
							.filter(s -> s.length() == 2);
stream.forEach(s -> System.out.println(s));


	// 为了提高代码的阅读性：
    list.stream()
		.filter(s -> s.startsWith("张"))
		.filter(s -> s.length() == 2);
```

```java
    ArrayList<String> list = new ArrayList<>();
    Collections.addAll(list, "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");
    Stream<String> stream1 = list.stream().filter(s -> s.startsWith("张"));
    Stream<String> stream2 = stream1.filter(s -> s.length() == 2);
    stream2.forEach(s -> System.out.println(s));

    // 代码报错：stream has already been operated upon or closed
    // 原因：Stream1已经使用过了，参考注意1
    Stream<String> stream3 = stream1.filter(s -> s.length() == 3);
    stream3.forEach(s -> System.out.println(s));

```

##### limit 获取前几个元素

```java
    ArrayList<String> list = new ArrayList<>();
    Collections.addAll(list, "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");
    // limit 获取前几个元素
    Stream<String> limit = list.stream().limit(3);
    limit.forEach(s -> System.out.println(s));
```

##### skip 跳过前几个元素

```java
	ArrayList<String> list = new ArrayList<>();
	Collections.addAll(list, "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");
	// skip 跳过前几个元素
	// 
	list.stream().skip(2).forEach(s -> System.out.println(s));
```

练习：要获取杨幂到刘华强

跳过前2个，获取3个

获取5个，跳过3个效果一样

```java
	list.stream().skip(2).limit(3).forEach(s -> System.out.println(s));
	list.stream().limit(5).skip(2).forEach(s -> System.out.println(s));
```

##### distinct 元素去重，依赖（hashCode和equals方法）

```java
ArrayList<String> list1 = new ArrayList<>();
Collections.addAll(list1, "张无忌", "张无忌", "张无忌", "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");
// distinct 元素去重，依赖（hashCode和equals方法）
// 张无忌 赵敏 杨幂 孙中山 刘华强 刘培茄 张强 张三丰
list1.stream().distinct().forEach(s -> System.out.print(s));
```

##### concat 合并a和b两个流为一个流

```java
ArrayList<String> list1 = new ArrayList<>();
Collections.addAll(list1, "张无忌", "赵敏", "杨幂", "孙中山", "刘华强", "刘培茄", "张强", "张三丰");

ArrayList<String> list2 = new ArrayList<>();
Collections.addAll(list2, "庄周", "姜子牙", "老子", "孟子");

// concat 合并a和b两个流为一个流
// 张无忌 赵敏 杨幂 孙中山 刘华强 刘培茄 张强 张三丰 庄周 姜子牙 老子 孟子 
Stream.concat(list1.stream(), list2.stream()).forEach(s -> System.out.print(s + " "));
```

##### map 转换流中的数据类型

```java
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌-15", "赵敏-16", "杨幂-18", "孙中山-20", "刘华强-22", "刘培茄-24", "张强-26", "张三丰-28");

// 需求：只获取里面的年龄并进行打印
// String -> int
// apply的形参s：依次表示流里面的每一个数据
// 返回值：表示转换之后的数据
// 当map方法执行完毕后，流上的数据就变成了整数
// 所以在下面forEach当中，s依次表示流里面的每一个数据，这个数据现在就是整数了
list.stream().map(new Function<String, Object>() {
    @Override
    public Object apply(String s) {
        String[] arr = s.split("-");
        String ageString = arr[1];
        int age = Integer.parseInt(ageString);
        return age;
    }
}).forEach(s -> System.out.println(s));
```

链式编程

map方法括号里面s.split("-")进行分割，获取1索引（数字），Integer.parseInt转换成整数类型，forEach再进行打印输出

```java
// "张无忌-15"
list.stream()
    .map(s -> Integer.parseInt(s.split("-")[1]))
    .forEach(s -> System.out.println(s + " "));
```

### Stream流的终结方法

##### void forEach(Consumer action) 遍历

```java
 ArrayList<String> list = new ArrayList<>();
 Collections.addAll(list, "张无忌-15", "赵敏-16", "杨幂-18", "孙中山-20", "刘华强-22", "刘培茄-24", "张强-26", "张三丰-28");

 // forEach 遍历
 // Consumer的泛型：表示流里面的数据
 // accept方法的形参s：依次表示流里面的每一个数据
 // 方法体：对每一个数据的处理操作
/* list.stream().forEach(new Consumer<String>() {
     @Override
     public void accept(String s) {

     }
 });*/

 // 张无忌-15 赵敏-16 杨幂-18 孙中山-20 刘华强-22 刘培茄-24 张强-26 张三丰-28
 list.stream().forEach(s -> System.out.print(s + " "));
```

##### long count()  统计

```java
// count  统计
long count = list.stream().count();
System.out.println(count);
```

##### toArray() 收集流中的数据，放到数组中

```java
Object[] array = list.stream().toArray();

// [张无忌-15, 赵敏-16, 杨幂-18, 孙中山-20, 刘华强-22, 刘培茄-24, 张强-26, 张三丰-28]
System.out.println(Arrays.toString(array));

```

```java
// IntFunction的泛型：具体类型的数组
// apply的形参：流中数据的个数，要跟数组的长度保持一致
// apply的返回值：具体类型的数组
// 方法体：就是创建数组

// toArray方法的参数的作用：负责创建一个指定类型的数组
// toArray方法的返回值：是一个装着流里面所有数据的数组
String[] arr = list.stream().toArray(new IntFunction<String[]>() {
    @Override
    public String[] apply(int value) {
        return new String[value];
    }
});

System.out.println(Arrays.toString(arr));
```

```java
String[] array = list.stream().toArray(value -> new String[value]);

System.out.println(Arrays.toString(array));
```

### Stream流的收集方法

##### collect

```java
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌-男-15", "赵敏-女-16", "杨幂-女-18", "孙中山-男-20", "刘华强-男-22", "刘培茄-男-24", "张强-男-26", "张三丰-男-28");

// 收集List集合当中
// 需求：
// 把所有的男性收集起来
List<String> newList = list.stream()
        .filter(s -> "男".equals(s.split("-")[1]))
        .collect(Collectors.toList());

// [张无忌-男-15, 孙中山-男-20, 刘华强-男-22, 刘培茄-男-24, 张强-男-26, 张三丰-男-28]
System.out.println(newList);

// 收集Set集合当中
// 需求：
// 把所有的女性收集起来
Set<String> newSet = list.stream()
    .filter(s -> "女".equals(s.split("-")[1]))
    .collect(Collectors.toSet());

// [赵敏-女-16, 杨幂-女-18]
System.out.println(newSet);


// 收集到Map集合中
// 键不能重复，否则代码会报错
// 谁作为键，谁作为值
// 我要把所有的男性收集起来
// 键：姓名 值：年龄
Map<String, Integer> map = list.stream()
    .filter(s -> "男".equals(s.split("-")[1]))
    /*
                 * toMap：参数一表示键的生成规则
                 *        参数二表示值的生成规则
                 *
                 * 参数一：
                 *       Function泛型一：表示流中每一个数据的类型
                 *               泛型二：表示Map集合中键的数据类型
                 *       方法apply形参：依次表示流里面的每一个数据
                 *               方法体：生成键的代码
                 *               返回值：已经生成的键
                 *
                 * 参数二：
                 *       Function泛型一：表示流中每一个数据的类型
                 *               泛型二：表示Map集合中值的数据类型
                 *       方法apply形参：依次表示流里面的每一个数据
                 *               方法体：生成键的代码
                 *               返回值：已经生成的键
                 *
                 * */
    .collect(Collectors.toMap(new Function<String, String>() {
        @Override
        public String apply(String s) {
            return s.split("-")[0];

        }
    },
                              new Function<String, Integer>() {

                                  @Override
                                  public Integer apply(String s) {
                                      return Integer.parseInt(s.split("-")[2]);
                                  }

                              }));

// {张强=26, 张三丰=28, 刘华强=22, 刘培茄=24, 张无忌=15, 孙中山=20}
System.out.println(map);


Map<String, String> map2 = list.stream()
    .filter(s -> "男".equals(s.split("-")[1]))
    .collect(Collectors.toMap(
        s -> s.split("-")[0],
        s -> s.split("-")[2]
    ));

System.out.println(map2);
```

### 数据操作练习

##### 练习1

```
定义一个集合，并添加一些整数 1,2,3,4,5,6,7,8,9,10
过滤奇数，只留下来偶数
并将结果保存下来
```

```java
/*
定义一个集合，并添加一些整数 1,2,3,4,5,6,7,8,9,10
过滤奇数，只留下来偶数
并将结果保存下来
 */
ArrayList<Integer> list1 = new ArrayList<>();

Collections.addAll(list1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

List<Integer> list2 = list1.stream()
    .filter(s -> (s % 2 == 0))
    .collect(Collectors.toList());

System.out.println(list2);
```

##### 练习2

```
创建一个ArrayList集合，并添加以下字符串，字符串前面是姓名，后面是年龄
   “zhangsan，23“
   “lisi，24”
   “wangwu，25”
   保留年龄大于等于24岁的人，并将结果搜集到Map集合中，姓名为键，年龄为值
```

```java
/*
创建一个ArrayList集合，并添加以下字符串，字符串前面是姓名，后面是年龄
   “zhangsan，23“
   “lisi，24”
   “wangwu，25”
   保留年龄大于等于24岁的人，并将结果搜集到Map集合中，姓名为键，年龄为值
 */
ArrayList<String> list = new ArrayList<>();

Collections.addAll(list, "zhangsan,23", "lisi,24", "wangwu,25");

Map<String, Integer> map = list.stream()
                .filter(s -> Integer.parseInt(s.split(",")[1]) >= 24)
                .collect(Collectors.toMap(
                        s -> s.split(",")[0],
                        s -> Integer.parseInt(s.split(",")[1])
                ));

// {lisi=24, wangwu=25}
System.out.println(map);
```

##### 练习3

```
现在有两个ArrayList集合，
第一个集合中：存储6名男演员的姓名和年龄。第二个集合中存储6名女演员的姓名和年龄。
姓名和年龄中间用逗号分隔开。例如：张三,23
要求完成如下操作：
1.男演员只要名字为3个字的前两人
2.女演员只要姓杨的，并且不要第一个
3.把过滤后的男女演员姓名合并在一起
4.将上一步的演员信息封装成Actor对象
5.将所有演员对象都保存在List集合中。
备注：演员类Actor，属性：name，age
```

```java
/*
现在有两个ArrayList集合，
第一个集合中：存储6名男演员的姓名和年龄。第二个集合中存储6名女演员的姓名和年龄。
姓名和年龄中间用逗号分隔开。例如：张三,23
要求完成如下操作：
1.男演员只要名字为3个字的前两人
2.女演员只要姓杨的，并且不要第一个
3.把过滤后的男女演员姓名合并在一起
4.将上一步的演员信息封装成Actor对象
5.将所有演员对象都保存在List集合中。
备注：演员类Actor，属性：name，age
 */
// 男演员集合
ArrayList<String> actorList = new ArrayList<>();
Collections.addAll(actorList, "陈学冬,26", "胡歌,27", "霍建华,30", "古巨基,29", "六小龄童,27", "陈晓旭,22", "邓伦,26");
// 女演员集合
ArrayList<String> actressList = new ArrayList<>();
Collections.addAll(actressList, "唐嫣,26", "赵丽颖,26", "杨颖,30", "杨幂,23", "六小龄童,27", "孙怡,24", "杨紫,26");

// 筛选
// 男演员
// 只要名字为3个字的前两人
List<String> newActorList = actorList.stream()
        .filter(s -> s.split(",")[0].length() == 3)
    	.limit(2)
        .collect(Collectors.toList());

// 女演员
// 只要姓杨的，并且不要第一个
List<String> newActressList = actressList.stream()
    	// 这里的可以改为.filter(s -> (s.split(",")[0].startsWith("杨"))
        .filter(s -> "杨".equals(s.split(",")[0].substring(0, 1)))
        .skip(1)
        .collect(Collectors.toList());

// 定义一个集合用来合并符合条件男女演员姓名
ArrayList<String> eligibleActorsName = new ArrayList<>();
// 定义一个集合用来合并符合条件男女演员姓名对应的年龄
ArrayList<Integer> eligibleActorsAge = new ArrayList<>();

// 遍历newActorList集合
for (String actor : newActorList) {
    // 拿到男演员姓名
    String name = actor.split(",")[0];
    // 将符合条件男演员添加进集合
    eligibleActorsName.add(name);
    // 拿到男演员年龄
    int age = Integer.parseInt(actor.split(",")[1]);
    eligibleActorsAge.add(age);
}

// 遍历newActressList集合
for (String actress : newActressList) {
    // 拿到女演员姓名
    String name = actress.split(",")[0];
    // 将符合条件女演员添加进eligibleActorsName集合
    eligibleActorsName.add(name);
    // 拿到女演员年龄
    int age = Integer.parseInt(actress.split(",")[1]);
    // 将符合条件女演员年龄添加进eligibleActorsAge集合
    eligibleActorsAge.add(age);
}

// 创建一个最终名单集合用来存储最终的演员对象
ArrayList<Actor> theFinalCastList = new ArrayList<>();
// 得到最终演员人数，决定创建几个演员对象
int count = eligibleActorsName.size();
for (int i = 0; i < count; i++) {
    Actor[] actor = new Actor[count];
    actor[i] = new Actor();
    actor[i].setName(eligibleActorsName.get(i));
    actor[i].setAge(eligibleActorsAge.get(i));
    // 将演员对象存入theFinalCastList集和
    theFinalCastList.add(actor[i]);
}
// 打印最终名单
System.out.println(theFinalCastList);
```

```java
class Actor {
    private String name;
    private int age;

    public Actor() {
    }

    public Actor(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }


    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "姓名：" + name + " 年龄：" + age;
    }
}
```
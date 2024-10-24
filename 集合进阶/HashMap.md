# HashMap

   1.HashMap是Map的一个实现类

2. 没有额外的特有方法

3. 特点都是由键决定的：无序，不重复，无索引

4. HashMap跟HashSet底层原理一模一样，都是哈希表结构

利用键计算出哈希值，和值无关

## 总结

HashMap的底层是Hash表

依赖hashCode方法和equals方法来保证**键的唯一**

**如果键存储的是自定义对象，需要重写hashCode和equals方法**

如果值存储的是自定义对象，不需要重写hashCode和equals方法

#### 小练习

###### 练习1

创建一个HashMap集合，键是Student学生对象，值是籍贯（Stirng）

存储三个键值对元素，并遍历

要求：同姓名，同年龄认为是同一个学生

```java
    // 创建HashMap对象
    HashMap<Student, String> hashMap = new HashMap<>();
    // 创建三个学生对象
    Student stu1 = new Student("张三", 23);
    Student stu2 = new Student("李四", 24);
    // 键唯一，再创建一个属性相同的学生对象，在HashMap中添加元素时会进行覆盖
    Student stu3 = new Student("王五", 25);
    Student stu4 = new Student("王五", 25);
    // 添加元素
    hashMap.put(stu1, "河南");
    hashMap.put(stu2, "山东");
    hashMap.put(stu3, "北京");
    hashMap.put(stu4, "上海");
    // 遍历集和
    Set<Student> students = hashMap.keySet();
    for (Student student : students) {
        String value = hashMap.get(student);
        // Student{name='张三', age=23} = 河南
        // Student{name='李四', age=24} = 山东
        // Student{name='王五', age=25} = 北京
        System.out.println(student + " = " + value);
        // 添加stu5后打印结果
        // Student{name='张三', age=23} = 河南
        // Student{name='李四', age=24} = 山东
        // Student{name='王五', age=25} = 上海
    }
```

###### 多种遍历方式

```java
public static void main(String[] args) {
    // 创建HashMap对象
    HashMap<Student, String> hashMap = new HashMap<>();
    // 创建三个学生对象
    Student stu1 = new Student("张三", 23);
    Student stu2 = new Student("李四", 24);
    // 键唯一，再创建一个属性相同的学生对象，在HashMap中添加元素时会进行覆盖
    Student stu3 = new Student("王五", 25);
    Student stu4 = new Student("王五", 25);
    // 添加元素
    hashMap.put(stu1, "河南");
    hashMap.put(stu2, "山东");
    hashMap.put(stu3, "北京");
    hashMap.put(stu4, "上海");
    // 遍历集和
    Set<Student> students = hashMap.keySet();
    // 第一种方式：根据键找值
    for (Student student : students) {
        String value = hashMap.get(student);
        // Student{name='张三', age=23} = 河南
        // Student{name='李四', age=24} = 山东
        // Student{name='王五', age=25} = 北京
        System.out.println(student + " = " + value);
        // 添加stu5后打印结果
        // Student{name='张三', age=23} = 河南
        // Student{name='李四', age=24} = 山东
        // Student{name='王五', age=25} = 上海
    }
    System.out.println("------------------------");
    // 第二种方式：根据键值对
    Set<Map.Entry<Student, String>> entries = hashMap.entrySet();
    for (Map.Entry<Student, String> entry : entries) {
        Student key = entry.getKey();
        String value = entry.getValue();
        System.out.println(key + " = " + value);
    }
    System.out.println("------------------------");
    // 第三种方式：Lambda表达式
    hashMap.forEach(new BiConsumer<Student, String>() {
        @Override
        public void accept(Student student, String s) {
            System.out.println(student + " = " + s);
        }
    });
    // 简化
    hashMap.forEach((student, s) -> System.out.println(student + " = " + s));
}
```

###### 练习2

需求：
    某班80名学生，组织秋游活动，景点依次为A、B、C、D，每个学生只能选择一个景点，请统计出最终哪个景点想去的人最多

```java
public class Test {
    /*
    需求：
    某班80名学生，组织秋游活动，景点依次为A、B、C、D，每个学生只能选择一个景点，请统计出最终哪个景点想去的人最多
     */
    public static void main(String[] args) {
        // 1.先让同学投票
        String[] arr = {"A", "B", "C", "D"};
        // 2.利用随机数模拟同学投票,并把投票结果存储起来
        ArrayList<String> list = new ArrayList<>();
        Random r = new Random();
        for (int i = 0; i < 80; i++) {
            int index = r.nextInt(arr.length);
            list.add(arr[index]);
        }
        // 要统计的数据比较多，不方便使用计数器思想
        // 可以定义map集合，利用集合来统计
        HashMap<String, Integer> hashMap = new HashMap<>();

        for (String name : list) {
            // 判断当前景点在集合中是否存在
            if (hashMap.containsKey(name)) {
                // 存在
                // 拿到次数
                int count = hashMap.get(name);
                // 次数加1
                count++;
                // 再次添加完成覆盖
                hashMap.put(name, count);
            } else {
                hashMap.put(name, 1);
            }
        }
        // 打印 HashMap 表，完成统计
        System.out.println(hashMap);

        // 求最大值
        int max = 0;
        Set<Map.Entry<String, Integer>> entries = hashMap.entrySet();
        for (Map.Entry<String, Integer> entry : entries) {
            int count = entry.getValue();
            if (count > max) {
                max = count;
            }
        }
        // 得到最大值
//        System.out.println(max);

        // 看看哪一个或者哪几个景点符合要求（会出现票数相同的情况）
        //{A=15, B=27, C=19, D=19}
        //投票人数最多的景点：B景点共：27票
        //{A=22, B=22, C=21, D=15}
        //投票人数最多的景点：A景点共：22票
        //投票人数最多的景点：B景点共：22票
        for (Map.Entry<String, Integer> entry : entries) {
            if (entry.getValue() == max) {
                System.out.println("投票人数最多的景点：" + entry.getKey() + "景点共：" + entry.getValue() + "票");
            }
        }

    }


}
```
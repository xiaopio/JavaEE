# Collections

java.util.Collections 是一个集合工具类

作用：Collections不是集合，而是集合的工具类

#### 常用API

##### addAll	批量添加元素

```java
// addAll 批量添加元素

// 1.创建集合对象
ArrayList<String> list = new ArrayList<>();

// 2.批量添加
Collections.addAll(list, "abc", "bcd", "cde", "def", "efg", "fgh", "ghi", "hij", "ijk", "jkl");

// 3.打印集合
// [abc, bcd, cde, def, efg, fgh, ghi, hij, ijk, jkl]
System.out.println(list);
```

##### shuffle 打乱集合中元素顺序

```java
// shuffle 打乱集合中元素顺序
Collections.shuffle(list);
// 每次打乱的结果不一样
// [hij, bcd, ghi, fgh, jkl, ijk, efg, abc, cde, def]
System.out.println(list);
```

#####  sort 排序

```java
// 1.创建集合对象
ArrayList<String> list = new ArrayList<>();
// 2.批量添加
Collections.addAll(list, "b", "a", "c", "d", "2", "1", "3");
// sort 排序
// 默认排序
Collections.sort(list);
// [1, 2, 3, a, b, c, d]
System.out.println(list);
```

指定排序顺序，可以重写Comparator方法

```java
Collections.sort(list, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o2 - o1;
    }
});
System.out.println(list);
Collections.sort(list, (o1, o2) -> o2 - o1);
System.out.println(list);
```

##### binarySearch 以二分查找法查找元素

```java
// 1.创建集合对象
ArrayList<String> list = new ArrayList<>();

// 2.批量添加
Collections.addAll(list, "b", "a", "c", "d", "2", "1", "3");

// 3.打印集合
// [b, a, c, d, 2, 1, 3]
System.out.println(list);
// binarySearch 以二分查找法查找元素
// 返回元素的下标
int i = Collections.binarySearch(list, "a");
System.out.println(i);
```

##### copy 拷贝集合中的元素

```java
// 1.创建集合对象
ArrayList<String> list = new ArrayList<>();
ArrayList<String> list2 = new ArrayList<>();

// 2.批量添加
Collections.addAll(list, "b", "a", "c", "d", "2", "1", "3");
Collections.addAll(list2, "", "", "", "", "", "", "");

// 3.打印集合
// [b, a, c, d, 2, 1, 3]
System.out.println(list);

// copy 拷贝集合
// 第一个参数是目标 第二个参数是源
// 如果源的长度大于目标的长度，方法会报错
Collections.copy(list2, list);
// [b, a, c, d, 2, 1, 3]
System.out.println(list2);
```

##### max 最大值 min 最小值

根据默认的自然排序获取最大/最小值

```java
// 1.创建集合对象
ArrayList<Integer> list = new ArrayList<>();

// 2.批量添加
Collections.addAll(list, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// max 最大值 min 最小值
int max = Collections.max(list);
int min = Collections.min(list);
System.out.println(max);
System.out.println(min);
```

max min 指定排序规则

```java
// 1.创建集合对象
ArrayList<String> list = new ArrayList<>();

// 2.批量添加
Collections.addAll(list, "a", "aa", "aaa", "aaaa", "aaaaa");

// max min 指定规则
String max = Collections.max(list, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return o1.length() - o2.length();
    }
});
System.out.println(max);
```

##### swap 交换集合中指定位置的元素

```java
// 1.创建集合对象
ArrayList<Integer> list = new ArrayList<>();

// 2.批量添加
Collections.addAll(list, 1, 2, 3, 4, 5);

// swap 交换集合中的元素
Collections.swap(list, 0, 1);
// [2, 1, 3, 4, 5]
System.out.println(list);
```

##### 练习1

班级有N个同学，实现随机点名器

```java
public static void main(String[] args) {
/*
班级有N个同学，实现随机点名器
 */
    // 1.创建学生集合
    ArrayList<String> list = new ArrayList<>();
    // 2.添加元素
    Collections.addAll(list, "张三", "李四", "王五", "赵六");
    // 3.随机打乱集合
    Collections.shuffle(list);
    // 得到下标为0的元素，认为是点名到了他
    String s = list.get(0);
    // 打印输出
    System.out.println(s);

}
```

##### 练习2

班级有N个同学

要求：

70%的概率随机到男生

30%概率随机到女生

自己写的

学生类，属性

```java
private String name;
private char gender;
```

```java
public static void main(String[] args) {
/*
班级有N个同学，实现随机点名器
 */
    Student s1 = new Student("史彭湃", '男');
    Student s2 = new Student("袁阳秋", '男');
    Student s3 = new Student("孙学义", '男');
    Student s4 = new Student("万升荣", '男');
    Student s5 = new Student("刘鸿飞", '男');
    Student s6 = new Student("沈冬儿", '女');
    Student s7 = new Student("高梦晨", '女');
    Student s8 = new Student("郭英莉", '女');
    // 1.创建学生集合
    ArrayList<Student> list = new ArrayList<>();
    // 2.添加元素
    Collections.addAll(list, s1, s2, s3, s4, s5, s6, s7, s8);
    Collections.shuffle(list);
    Random r = new Random();
    int i = r.nextInt(10);
    if (i < 7) {
        for (int j = 0; j < list.size(); j++) {
            if (list.get(j).getGender() == '男') {
                System.out.println(list.get(i).getName());
                return;
            }
        }
    } else {
        for (int j = 0; j < list.size(); j++) {
            if (list.get(j).getGender() == '女') {
                System.out.println(list.get(j).getName());
                return;
            }
        }
    }
}
```

黑马教的



##### 练习3

班级有N个同学

要求：

被点到的学生不会再被点到

但是如果班级中所有的学生都被点完了，重现开启第二轮点名。

我写的

```java
public static void main(String[] args) {
/*
班级有N个同学，实现随机点名器
 */
    Student s1 = new Student("史彭湃", '男');
    Student s2 = new Student("袁阳秋", '男');
    Student s3 = new Student("孙学义", '男');
    Student s4 = new Student("万升荣", '男');
    Student s5 = new Student("刘鸿飞", '男');
    Student s6 = new Student("沈冬儿", '女');
    Student s7 = new Student("高梦晨", '女');
    Student s8 = new Student("郭英莉", '女');
    // 1.创建学生集合
    ArrayList<Student> list1 = new ArrayList<>();
    ArrayList<Student> list2 = new ArrayList<>();
    // 2.添加元素
    Collections.addAll(list1, s1, s2, s3, s4, s5, s6, s7, s8);

    while (true) {
        // 如果集合为空
        if (list1.isEmpty()) {
            // 重新添加对象
            // Collections.addAll(list1, s1, s2, s3, s4, s5, s6, s7, s8);
            list1.addAll(list2);
            list2.clear();
            System.out.println("--------------------");

        } else {
            // 3.随机一下
            Collections.shuffle(list1);
            // 假设0号被点到
            System.out.println(list1.getFirst().getName());
            // 临时集合存储移除的对象
            list2.add(list1.getFirst());
            // 移除0号
            list1.remove(list1.getFirst());
        }
    }


}
```

豆包生成的

```java
public class CollectionsDemo1 {
    public static void main(String[] args) {
        Student s1 = new Student("史彭湃", '男');
        Student s2 = new Student("袁阳秋", '男');
        Student s3 = new Student("孙学义", '男');
        Student s4 = new Student("万升荣", '男');
        Student s5 = new Student("刘鸿飞", '男');
        Student s6 = new Student("沈冬儿", '女');
        Student s7 = new Student("高梦晨", '女');
        Student s8 = new Student("郭英莉", '女');

        List<Student> allStudents = new ArrayList<>();
        Collections.addAll(allStudents, s1, s2, s3, s4, s5, s6, s7, s8);

        List<Student> calledStudents = new ArrayList<>();
        Random random = new Random();

        while (true) {
            if (allStudents.isEmpty()) {
                allStudents.addAll(calledStudents);
                calledStudents.clear();
                System.out.println("All students called once. Starting over.");
            } else {
                int index = random.nextInt(allStudents.size());
                Student calledStudent = allStudents.remove(index);
                System.out.println(calledStudent.getName());
                calledStudents.add(calledStudent);
            }
        }
    }
}

class Student {
    private String name;
    private char gender;

    public Student(String name, char gender) {
        this.name = name;
        this.gender = gender;
    }

    public String getName() {
        return name;
    }

    public char getGender() {
        return gender;
    }
}
```

##### 练习4

TXT文件中事先准备好80个同学，每个同学的名字独占一行

要求1：

每次被点到的同学，再次被点到的概率为原先的一半

举例：80个同学，点名5次，每次都点到A同学，概率变化情况为

第一次：每个人被点到的概率：1.25%

第二次点到A的概率为：0.625%。	其他同学概率：1.2579%。

第三次点到A的概率为：0.3125%。	其他同学概率：1.261867%。

第四次点到A的概率为：0.15625%。	其他同学概率：1.2638449%。

第五次点到A的概率为：0.078125%。	其他同学概率：1.26483386%。

要求2：
作弊要求，第三次点名必定是张三

##### 练习5

添加图形化界面
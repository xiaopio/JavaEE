# TreeMap

1. TreeMap跟TreeSet底层原理一样，都是红黑树结构

2.  由键决定特性：**不重复、无索引、可排序**

3.  可排序：**对键进行排序**

4.  **注意：默认按照键的从大到小进行排序，也可以自己规定键的排序规律**



**代码书写的两种排序规则**

​	实现Comparable接口，指定比较规则

​	创建集合时传递Comparator比较器对象，指定比较规则

#### 练习1

    需求1：
    键：整数表示id
    值：字符串表示商品名称
    要求：按照id的升序排列、按照id的降序排列

```java
public static void main(String[] args) {
    /*
    需求1：
    键：整数表示id
    值：字符串表示商品名称
    要求：按照id的升序排列、按照id的降序排列
     */
    // Integer Double 默认情况下都是按照升序排序
    // String 按照字母在 ASCII 码表中对应的数字升序进行排序
    TreeMap<Integer, String> aCollectionOfGoods = new TreeMap<>(new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            // o1：当前要添加的元素
            // o2：表示已经在红黑树中存在的元素
            return o2 - o1;
        }
    });
    aCollectionOfGoods.put(2, "雷碧");
    aCollectionOfGoods.put(1, "奥利给");
    aCollectionOfGoods.put(3, "康帅傅");
    aCollectionOfGoods.put(5, "可日可乐");
    aCollectionOfGoods.put(4, "王仔");
    // 遍历集合
    System.out.println(aCollectionOfGoods);
}
```

#### 练习2

    需求2：
    键：学生对象
    值：籍贯
    要求：按照学生年龄的升序排列，年龄一样的按照姓名的字母排序，同姓名年龄视为同一人。

```java
public static void main(String[] args) {
    /*
    需求2：
    键：学生对象
    值：籍贯
    要求：按照学生年龄的升序排列，年龄一样的按照姓名的字母排序，同姓名年龄视为同一人。
     */
    // 创建集合
    TreeMap<Student, String> treeMap = new TreeMap<>();
    // 创建三个学生对象
    Student student1 = new Student("张三", 23);
    Student student2 = new Student("李四", 24);
    Student student3 = new Student("王五", 25);
    // 添加元素
    treeMap.put(student1, "北京");
    treeMap.put(student2, "上海");
    treeMap.put(student3, "广州");
    // 打印集合
    System.out.println(treeMap);

}
```

因为这里键传入的是自定义的类，所以在Student类中要，先继承Comparable接口，然后对compareTo方法进行重写

Student类

```java
public class Student implements Comparable<Student> {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /**
     * 获取
     *
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     *
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     *
     * @return age
     */
    public int getAge() {
        return age;
    }

    /**
     * 设置
     *
     * @param age
     */
    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Student{name = " + name + ", age = " + age + "}";
    }

    @Override
    public int compareTo(Student o) {
        // 按照学生年龄的升序排列，年龄一样的按照姓名的字母排序，同姓名年龄视为同一人。
        // this：表示当前要添加的元素
        // o：表示已经在红黑树中存在的元素

        // 返回值：
        // 负数：表示当前要添加的元素是小的，存左边
        // 正数：表示当前要添加的元素是大的，存右边
        int result = this.getAge() - o.getAge();
        result = result == 0 ? this.getName().compareTo(o.getName()) : result;
        return result;
    }
}
```

#### 练习3

        需求：
        统计字符串中每个字符出现的次数，并按照以下格式输出
        输出结果：
        a(5) b(4) c(3) d(2) e(1)

```java
public class Test {
    public static void main(String[] args) {
        /*
        需求：
        统计字符串中每个字符出现的次数，并按照以下格式输出
        输出结果：
        a(5) b(4) c(3) d(2) e(1)
         */
        // 统计的思想：
        // 如果题目中没有要求对结果进行排序，默认使用HashMap
        // 如果题目中要求对结果进行排序，使用TreeMap
        // 键：表示要统计的内容
        // 值：表示统计次数

        // 1.定义字符串
        String s = "aabcdacbdadbcdabcdabdc";

        // 2.创建集合
        TreeMap<Character, Integer> treeMap = new TreeMap<>();
        // 3.遍历字符串得到每一个字符
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            // 拿着c在集合中判断是否存在
            // 存在，当前字符次数加1
            // 不存在添加字符到集合中去
            if (treeMap.containsKey(c)) {
                Integer count = treeMap.get(c);
                count++;
                treeMap.put(c, count);
            } else {
                treeMap.put(c, 1);
            }
        }
        // 三种打印方式：Lambda表达式，StringBuilder，StringJoiner
        treeMap.forEach((key, value) -> System.out.print(key + "（" + value + "）"));
        System.out.println();
        System.out.println("----------------------");
        StringBuilder sb = new StringBuilder();
        treeMap.forEach((key, value) -> sb.append(key).append("（").append(value).append("）"));
        System.out.println(sb);
        System.out.println("----------------------");
        StringJoiner sj = new StringJoiner("", "", "");
        treeMap.forEach((Character key, Integer value) -> sj.add(key + "").add("（").add(value + "").add("）"));
        System.out.println(sj);
    }
```


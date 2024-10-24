# Object

Object是Java中的顶级父类.所有的类都直接或间接继承于Object类.

Object类中所有的方法可以被所有的子类访问,所以我们要学习Object类和其中的方法

| 方法名          | 说明     |
| --------------- | -------- |
| public Object() | 空参构造 |
|                 |          |

Object的成员方法

| 方法名                            | 说明                     |
| --------------------------------- | ------------------------ |
| public String toString()          | 返回对象的字符串表示形式 |
| public boolean equals(Object obj) | 比较两个对象是否相等     |
| protected Object clone(int a)     | 对象克隆                 |

```java
// 运行结果:java.lang.Object@4eec7777包名加类名@地址值
Object obj = new Object();
String str = obj.toString();
System.out.println(str);
```

toString方法的结论:

如果我们打印一个对象,想要看到属性值的话,那么就重写toString方法就可以了

在重写的方法中,对象的属性值进行拼接。

**public boolean equals(Object obj)**

```java
public class test {
    public static void main(String[] args) {
        Student s1 = new Student();
        Student s2 = new Student();
        boolean result1 = s1.equals(s2);
        // 结果：false，比较的是两个对象的地址值
        System.out.println(result1);
    }
}

class Student {
    String name;
    int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```java
public class test {
    public static void main(String[] args) {
        Student s1 = new Student();
        Student s2 = new Student();
        boolean result1 = s1.equals(s2);
        // 运行结果：true
        System.out.println(result1);
    }
}

class Student {
    String name;
    int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    // 重写equals方法，比较的就是对象的属性值

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        Student student = (Student) o;
        return age == student.age && Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

**protected Object clone(int a**)

对象克隆，把A对象的属性值完全拷贝给B对象，也叫对象拷贝，对象复制

```java
public class User implements Cloneable {
    // 游戏id
    private int id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 游戏图片
    private String path;
    // 游戏进度
    private int[] data;

    public User() {
    }

    public User(int id, String username, String password, String path, int[] data) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.path = path;
        this.data = data;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPath() {
        return path;
    }

    public void setPath(String path) {
        this.path = path;
    }

    public int[] getData() {
        return data;
    }

    public void setData(int[] data) {
        this.data = data;
    }

    @Override
    public String toString() {
        return "用户编号：" + id + "，用户名：" + username + "，密码：" + password + "，游戏图片：" + path + "，进度" + arrToString();
    }

    public String arrToString() {
        StringJoiner sj = new StringJoiner(",", "[", "]");
        for (int i = 0; i < data.length; i++) {
            sj.add(data[i] + "");
        }
        return sj.toString();
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        // 调用父类中的clone方法，相当于让Java帮我们克隆一个对象，并把克隆之后的对象返回出去
        return super.clone();
    }
}
```

```java
public class Test {
    public static void main(String[] args) throws CloneNotSupportedException {
        int[] data = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 0};
        // 创建对象
        User user1 = new User(1, "张三", "123456", "girl11", data);
        // 克隆对象
        // 细节：
        // 方法在底层会帮我们创建一个对象，并把原对象中的数据拷贝过去
        // 书写细节：
        // 1.重写Object中的clone方法
        // 2.让javabean类实现Cloneable接口
        // 3.创建原对象并调用clone方法就可以了
        Object user2 = user1.clone();
        System.out.println(user1);
        System.out.println(user2);
    }

}
```

# Objects

| 方法名                                            | 说明                                     |
| ------------------------------------------------- | ---------------------------------------- |
| public static boolean equals (Object a, Object b) | 先做非空判断，比较两个对象               |
| public static boolean isNull (Object obj)         | 判断对象是否为null，为null返回true，反之 |
| public static boolean nonNull (Object obj)        | 判断对象是否为null，跟isNull的结果相反   |


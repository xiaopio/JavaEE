## ①Class.forName("全类名");

## ②类名.class

## ③对象.getClass();

```java
public class MyReflectDemo1 {
    public static void main(String[] args) throws ClassNotFoundException {
        // 1.第一种方式
        // 全类名：包名 + 类名 com.haust.myreflect1.Student
        // 最为常用
        Class<?> clazz1 = Class.forName("com.haust.myreflect1.Student");
        // class com.haust.myreflect1.Student
        System.out.println(clazz1);

        // 2.第二种方式
        // 一般更多是当作参数进行传递
        Class<Student> clazz2 = Student.class;
        // class com.haust.myreflect1.Student
        System.out.println(clazz2);

        // 第三种方式
        // 当我们已经有了这个类的对象时，才可以使用
        Student student = new Student();
        Class clazz3 = student.getClass();
        Class<? extends Student> clazz4 = student.getClass();
        // class com.haust.myreflect1.Student
        System.out.println(clazz3);
        System.out.println(clazz4);
    }
}
```
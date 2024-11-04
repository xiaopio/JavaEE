# IO流

## 概述

什么是IO流？

IO流：存储和读取数据的解决方案

IO流的作用？

用于读写文件中的数据（可以读写数据，或网络中的数据......）

## IO流的分类

**流的方向**：输入流（读取）、输出流（写出）

**操作文件的类型**：字节流（所有类型的文件）、字符流（纯文本文件）

什么是纯文本文件，用Windows自带的记事本打开能读懂的就是纯文本文件

字节流：InputStream、OutputStream

### FileOutputStream

#### 作用

操作本地文件的字节输出流，可以把程序中的数据写到本地文件中

#### 书写步骤

①创建字节输出流对象，②写数据，③释放资源

```java
public static void main(String[] args) throws IOException {
    // 1.创建对象
    // 写出 输出流 OutputStream
    // 本地文件 File
    FileOutputStream fos = new FileOutputStream("a.txt");
    // 2.写出数据
    // a
    fos.write(97);
    fos.close();
}
```

```
字节输出流的细节：
    1.创建字节输出流对象
        细节1：参数是字符串表示的路径或者是File对象都是可以的
        细节2：如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
        细节3：如果文件已经存在，则会清空文件
    2.写数据
        细节：write方法的参数是整数，但实际上写到本地文件中的是整数在ASCII码上对应的字符
    3.释放资源
        细节：每次使用完流之后都要释放资源
```

#### 写数据的3种方式

##### void write(int b)	一次写一个字节数据

##### void write(byte[] b)	一次写一个字节数组数据

##### void write(byte[] b, int off, int len)	一次写一个字节数组的部分数据

```java
public static void main(String[] args) throws IOException {
        /*
        void write(int b)   一次写一个字节数据
        void write(byte[] b)    一次写一个字节数组数据
        void write(byte[] b, int off, int len)  一次写一个字节数组的部分数据
        参数一：
            数组
        参数二：
            起始索引
        参数三：
            个数
         */
        // 1.创建对象
        FileOutputStream fos = new FileOutputStream("a.txt");
        // 2.写出数据

//        fos.write(97);
//        fos.write(98);

//        byte[] bytes = {97, 98, 99, 100, 101};
//        fos.write(bytes);

        byte[] bytes = {97, 98, 99, 100, 101};
        // 从数组下标1开始写，写两个
        fos.write(bytes, 1, 2);
        // 3.释放资源
        fos.close();
    }
```

##### 写数据的两个小问题

###### ①换行写

```
再写出一个换行符就可以了
Windows：    \r\n
Linux:  \n
Mac:    \r
 细节：
            在Windows操作系统中，java对回车换行进行了优化。
            虽然完整的是\r\n，但是我们写其中一个\r或者\n
            java也可以实现换行，因为java底层会补全
         建议：不要省略，还是要写全
```

###### ②续写

```
续写：
   如果想要续写，打开续写开关就可以了
   开关位置：创建对象的第二个参数
   默认是false：表示关闭续写，此时创建对象会清空文件
   手动传递true：表示打开续写，此时创建对象不会清空文件
```

```java
public static void main(String[] args) throws IOException {
    // 1.创建对象
    FileOutputStream fos = new FileOutputStream("a.txt", true);
    // 2.写数据
    String str1 = "books";
    byte[] bytes1 = str1.getBytes();
    fos.write(bytes1);

    // 再次写出一个换行符就可以了
    String wrap = "\r\n";
    byte[] bytes2 = wrap.getBytes();
    fos.write(bytes2);

    String str2 = "666";
    byte[] bytes3 = str2.getBytes();
    fos.write(bytes3);
    // 3.释放资源
    fos.close();
}
```

### FileInputStream

#### 书写步骤

```java
/*
演示：字节输入流FileInputStream
实现需求：读取文件中的数据（暂时不要写中文）
 */
// 1.创建对象
FileInputStream fis = new FileInputStream("a.txt");
// 2.读取文件
int read = fis.read();
System.out.println(read);
// 3.释放资源
fis.close();
```

```
字节输入流的细节：
    1.创建字节输入流对象
        细节1：如果文件不存在，就直接报错
    2.写数据
        细节1：一次读一个字节，读出来的是数据在ASCII码上对应的数字
        细节2：读到文件末尾，read方法返回-1
    3.释放资源
        细节：每次使用完流之后都要释放资源
```

#### 循环读取

```java
public static void main(String[] args) throws IOException {
    // 1.创建对象
    FileInputStream fis = new FileInputStream("a.txt");
    // 2.循环读取
    // 这里为什么要定义一个b，直接在循环里面用fis.read()不行吗？
    // read：表示读取数据，而且是读取一个数据就移动一次指针
    // 相当于while()循环时读取了一次，移动了一次指针，下面打印时又读取了一次（和上面while()循环里面的内容已经不一样了）
    int b;
    while ((b = fis.read()) != - 1) {
        System.out.print((char) b);
    }
    // 3.释放资源
    fis.close();
}
```

#### 练习

##### 文件拷贝

```java
public static void main(String[] args) throws IOException {
    // 1.创建对象
    FileInputStream fis = new FileInputStream("D:\\aaa\\111.avi");
    FileOutputStream fos = new FileOutputStream("copy.avi");
    // 2.拷贝
    // 思想：边读边写
    int b;
    while ((b = fis.read()) != - 1) {
        fos.write(b);
    }
    // 3.释放资源
    // 规则：先开的最后关闭
    fos.close();
    fis.close();
}
```

统计一下上面的拷贝时间

```java
public static void main(String[] args) throws IOException {
        // 1.创建对象
        FileInputStream fis = new FileInputStream("D:\\aaa\\111.avi");
        FileOutputStream fos = new FileOutputStream("copy.avi");

        // 记录开始时间
        long startTime = System.currentTimeMillis();

        // 2.拷贝
        // 思想：边读边写
        int b;
        while ((b = fis.read())!= -1) {
            fos.write(b);
        }

        // 记录结束时间
        long endTime = System.currentTimeMillis();

        // 3.释放资源
        // 规则：先开的最后关闭
        fos.close();
        fis.close();

        // 计算并打印拷贝时间
        long duration = endTime - startTime;
        System.out.println("拷贝用时：" + duration + "毫秒");
    }
```

#### 读数据的两种方式

##### public int read()	一次读一个字节数据

##### public int read(byte[] buffer)	一次读一个字节数组数据

```java
public static void main(String[] args) throws IOException {
    // 1.创建对象
    FileInputStream fis = new FileInputStream("a.txt");
    // 2.读取数据
    byte[] bytes = new byte[2];
    // 一次读取多个字节数据，具体多少，跟数组的长度有关
    // 返回值：本次读取到了多少个字节数据
    int len1 = fis.read(bytes);
    String str1 = new String(bytes, 0, len1);
    System.out.print(str1);

    int len2 = fis.read(bytes);
    String str2 = new String(bytes, 0, len2);
    System.out.print(str2);

    int len3 = fis.read(bytes);
    String str3 = new String(bytes, 0, len3);
    System.out.print(str3);
}
```

##### 大文件拷贝

```java
public static void main(String[] args) throws IOException {
    // 1.创建对象
    FileInputStream fis = new FileInputStream("D:\\aaa\\111.avi");
    FileOutputStream fos = new FileOutputStream("copy.avi");
    // 2.拷贝
    int len;
    byte[] bytes = new byte[1024 * 1024 * 5];
    while ((len = fis.read(bytes)) != - 1) {
        fos.write(bytes, 0, len);
    }
    // 3.释放资源
    fos.close();
    fis.close();
}
```

## IO流中不同JDK版本捕获异常的方式

#### try...catch...finally(了解)

```java
public static void main(String[] args) throws IOException {
    FileInputStream fis = null;
    FileOutputStream fos = null;
    try {
        // 1.创建对象
        fis = new FileInputStream("D:\\aaa\\111.avi");
        fos = new FileOutputStream("copy.avi");
        // 2.拷贝
        int len;
        byte[] bytes = new byte[1024 * 1024 * 5];
        while ((len = fis.read(bytes)) != - 1) {
            fos.write(bytes, 0, len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 3.释放资源
        if (fos != null) {
            try {
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (fis != null) {
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

JDK7

```java
public static void main(String[] args) throws FileNotFoundException {
    /*
    JDK7：IO流中捕获异常的写法
    try后面小括号中写创建对象的代码
            注意：只有实现了AutoCloseable接口的类，才能在小括号中创建对象
    try(){

    }catch(){

    }
     */
    try (FileInputStream fis = new FileInputStream("D:\\aaa\\111.avi");
         FileOutputStream fos = new FileOutputStream("copy.avi")) {
        // 2.拷贝
        int len;
        byte[] bytes = new byte[1024 * 1024 * 5];
        while ((len = fis.read(bytes)) != - 1) {
            fos.write(bytes, 0, len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

}
```

JDK9

```java
public static void main(String[] args) throws FileNotFoundException {
    /*
    JDK9：IO流中捕获异常的写法
     */
    FileInputStream fis = new FileInputStream("D:\\aaa\\111.avi");
    FileOutputStream fos = new FileOutputStream("copy.avi");
    try (fis;fos){
        // 2.拷贝
        int len;
        byte[] bytes = new byte[1024 * 1024 * 5];
        while ((len = fis.read(bytes)) != - 1) {
            fos.write(bytes, 0, len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

}
```
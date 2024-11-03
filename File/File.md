# File

File对象就表示一个路径，可以是文件路径，也可以是文件夹的路径

这个路径可以是存在的，也可以是不存在的

## 三种构造方法

```java
// 1.根据字符串表示的路径，变成File对象
String str = "C:\\Users\\admin\\OneDrive\\桌面\\b.txt";
File f1 = new File(str);
// C:\Users\admin\OneDrive\桌面\b.txt
System.out.println(f1);

// 2.父级路径：C:\Users\admin\OneDrive\桌面
//   子级路径：b.txt
String parent = "C:\\Users\\admin\\OneDrive\\桌面";
String child = "b.txt";
File f2 = new File(parent, child);
// C:\Users\admin\OneDrive\桌面\b.txt
System.out.println(f2);

// 3.把一个File表示的路径和String表示的路径进行拼接
File parent2 = new File("C:\\Users\\admin\\OneDrive\\桌面");
String child2 = "b.txt";
File f3 = new File(parent2, child2);
// C:\Users\admin\OneDrive\桌面\b.txt
System.out.println(f3);
```

## 常见成员方法

### 判断、获取

#### isDirectory()

#### isFile()

#### exists()

```java
// 1.对一个文件的路径进行判断
File f1 = new File("C:\\Users\\admin\\OneDrive\\桌面\\name.txt");
// false    是不是文件夹
System.out.println(f1.isDirectory());
// true 是不是文件
System.out.println(f1.isFile());
// true 是否存在
System.out.println(f1.exists());

// 2.对一个文件夹的路径进行判断
File f2 = new File("C:\\Users\\admin\\OneDrive\\桌面\\abc");
// true    是不是文件夹
System.out.println(f2.isDirectory());
// false 是不是文件
System.out.println(f2.isFile());
// true 是否存在
System.out.println(f2.exists());

// 3.对一个不存在的路径进行判断
File f3 = new File("C:\\Users\\admin\\OneDrive\\桌面\\aaa");
// false
System.out.println(f3.isDirectory());
// false
System.out.println(f3.isFile());
// false
System.out.println(f3.exists());
```

#### length()

#### getAbsolutePath()

#### getPath()

#### getName()

#### lastModified()

```java
// 1.length 返回文件的大小（字节数量）
// 细节1：这个方法只能获取文件的大小，单位是字节
// 如果我们想要M、G，可以不断除以1024
// 细节2：这个方法无法获取文件夹的大小
// 如果我们要获取一个文件夹的大小，需要把这个文件夹里面所有文件的大小都累加在一起

File f1 = new File("C:\\Users\\admin\\OneDrive\\桌面\\name.txt");
long len1 = f1.length();
// 896（单位为：字节）
System.out.println(len1);

File f2 = new File("C:\\Users\\admin\\OneDrive\\桌面");
long len2 = f2.length();
System.out.println(len2);

// 2.getAbsolutePath 返回文件的绝对路径
File f3 = new File("C:\\Users\\admin\\OneDrive\\桌面\\name.txt");
String path1 = f3.getAbsolutePath();
// C:\Users\admin\OneDrive\桌面\name.txt
System.out.println(path1);

File f4 = new File("a.txt");
String path2 = f4.getAbsolutePath();
// D:\Code\IDEA\Java\myfile\a.txt
System.out.println(path2);

// 3.getPath 返回定义文件时使用的路径
File f5 = new File("C:\\Users\\admin\\OneDrive\\桌面\\name.txt");
// C:\Users\admin\OneDrive\桌面\name.txt
String path3 = f5.getPath();
System.out.println(path3);

File f6 = new File("a.txt");
String path4 = f6.getPath();
// a.txt
System.out.println(path4);

// 4.getName 获取名字
// 细节：
// 如果调用者是
// 文件：返回文件名＋后缀名
// 文件夹：文件夹名
File f7 = new File("C:\\Users\\admin\\OneDrive\\桌面\\name.txt");
String name1 = f7.getName();
// name.txt
System.out.println(name1);

File f8 = new File("C:\\Users\\admin\\OneDrive\\桌面");
String name2 = f8.getName();
// 桌面
System.out.println(name2);

// 5.lastModified 返回文件的最后修改时间（时间毫秒值）
File f9 = new File("C:\\Users\\admin\\OneDrive\\桌面\\name.txt");
long time = f9.lastModified();
// 1729677393425
System.out.println(time);

// 获取到的是一个毫秒值形式，如何转换成yyyy年MM月dd日 HH: mm: ss
Instant instant = Instant.ofEpochMilli(time);
LocalDateTime localDateTime = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
// 2024-10-23T17:56:33.425
System.out.println(localDateTime);
```

### 创建、删除

#### createNewFile()

#### mkdir()

#### mkdirs()

```java
// 1.createNewFile 创建一个新的空的文件
// 细节1：如果当前路径表示的文件已经不存在，则创建成功，方法返回true
//      如果当前路径表示的文件已经存在，则创建失败，方法返回false
// 细节2：如果父级路径不存在，方法会有异常：IOException
// 细节3：createNewFile方法创建的一定是文件，如果路径中不包含后缀名，则创建一个没有后缀的文件
File f1 = new File("D:\\aaa\\a.txt");
boolean b1 = f1.createNewFile();
// true
System.out.println(b1);

// 2.mkdir 创建文件夹
// 细节1：windows当中路径是唯一的，如当前路径已经存在，则创建失败，返回false
// 细节2：mkdir方法只能创建单级目录，不能创建多级目录
File f2 = new File("D:\\aaa\\ddd");
boolean b2 = f2.mkdir();
System.out.println(b2);

// 3.mkdirs 创建多级文件夹
// 细节：既可以创建单级的，又可以创建多级的文件夹
File f3 = new File("D:\\aaa\\bbb\\ccc\\ddd");
boolean b3 = f3.mkdirs();
System.out.println(b3);
```

#### delete()

```java
// delete 删除
// 细节：如果删除的是文件，则直接删除，不走回收站
// 如果删除的是空文件夹，则直接删除，不走回收站
// 如果删除的是有内容的文件夹，则删除失败
File f1 = new File("D:\\aaa\\a.txt");
boolean b1 = f1.delete();
System.out.println(b1);

File f2 = new File("D:\\aaa\\bbb\\ccc\\ddd");
boolean b2 = f2.delete();
System.out.println(b2);
```

### 获取并遍历

#### listFiles()

```java
// listFiles()方法    获取当前路径下的所有内容
// 细节：当调用者File表示的路径不存在时，返回null
//      当调用者File表示的路径是文件时，返回null
//      当调用者File表示的路径是一个空文件夹时，返回一个长度为0的数组
//      当调用者File表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放到File数组中返回
//      当调用者File表示的路径是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放到File数组中返回，包含隐藏文件
//      当调用者File表示的路径是需要权限才能访问的文件夹时，返回null
File f = new File("D:\\aaa");
File[] files = f.listFiles();
for (File file : files) {
    // D:\aaa\a.txt
    // D:\aaa\bbb
    // D:\aaa\ddd
    System.out.println(file);
```

#### listRoots()

#### list()

#### list(FilenameFilter filter)

```java
 	// 1.listRoots()    获取系统中所有的盘符
    File[] arr = File.listRoots();
    System.out.println(Arrays.toString(arr));

    // 2.list() 获取当前该路径下的所有内容（仅仅能获取名称）
    File f1 = new File("D:\\aaa");
    String[] arr2 = f1.list();
    for (String s : arr2) {
        // a.txt bbb ddd
        System.out.println(s);
    }

    // 3.list(FilenameFilter filter)    利用文件名过滤器获取当前该路径下的所有内容
    // 需求：我现在想要获取aaa文件夹里面每一个文件或者文件夹下的所有txt文件
    File f2 = new File("D:\\aaa");
    // accept方法的形参，依次表示aaa文件夹里面的每一个文件或者文件夹的路径
    // 参数一：父级路径
    // 参数二：子级路径
    // 返回值：如果返回值为true，就表示当前路径
    //          如果返回值为false，就表示当前路径舍弃不要
    String[] arr3 = f2.list(new FilenameFilter() {
        @Override
        public boolean accept(File dir, String name) {
            File src = new File(dir, name);
            return src.isFile() && name.endsWith(".txt");
        }
    });

    System.out.println(Arrays.toString(arr3));
}
```

#### listFiles()（重点）

```java
// 1.创建File对象
File f = new File("D:\\aaa");
// 2.需求：打印里面所有的txt文件
File[] arr = f.listFiles();
for (File file : arr) {
    // file依次表示aaa文件夹里面每一个文件或者文件夹的路径
    if (file.isFile() && file.getName().endsWith(".txt")) {
        System.out.println(file);
    }
}
```

了解

```java
    // 1.创建File对象
    File f = new File("D:\\aaa");
    // 2.需求：打印里面所有的txt文件
    // 调用listFile(FileFilter filter)
    File[] arr1 = f.listFiles(new FileFilter() {
        @Override
        public boolean accept(File pathname) {
            return pathname.isFile() && pathname.getName().endsWith(".txt");
        }
    });

    // 调用listFile(FilenameFilter filter)
    File[] arr2 = f.listFiles(new FilenameFilter() {
        @Override
        public boolean accept(File dir, String name) {
            File src = new File(dir, name);
            return src.isFile() && name.endsWith(".txt");
        }
    });
    System.out.println(Arrays.toString(arr2));
}
```

## 综合练习

#### 练习1

需求：在当前模块下的aaa文件夹中创建一个a.txt文件

```java
    public static void main(String[] args) throws IOException {
        // 1.创建a.txt的父级路径
        File file = new File("aaa");
        // 2.创建父级路径
        // 如果aaa存在，那么此时创建失败
        // 如果aaa不存在，则创建成功
        file.mkdirs();
        // 拼接父级路劲和子级路径
        File src = new File(file, "a.txt");
        boolean b = src.createNewFile();
        if (b == true) {
            System.out.println("创建成功");
        } else {
            System.out.println("创建失败");
        }
    }
```

#### 练习2

需求：定义一个方法找某一个文件夹中，是否有以.avi结尾的电影（暂时不需要考虑子文件夹）

```java
public static void main(String[] args) {
        File file = new File("D:\\aaa");
        boolean b = haveAVI(file);
        System.out.println(b);

    }

    public static boolean haveAVI(File file) {
        File[] files = file.listFiles();
        for (File f : files) {
            if (f.isFile() && f.getName().endsWith(".avi")) {
                return true;
            }
        }
        return false;
    }
```

#### 练习3

需求：找到电脑中所有以avi结尾的电影（需要考虑子文件夹）

```java
public static void main(String[] args) {
    /**
     * 需求：找到电脑中所有以avi结尾的电影（需要考虑子文件夹）
     * 套路：
     *  1.进入文件夹
     *  2.遍历数组
     *  3.判断
     *  4.判断
     */
    File src = new File("D:\\");
    findAvi(src);
    
    findAvi();
}

public static void findAvi() {
    // 获取本地所有的盘符
    File[] files = File.listRoots();
    for (File file : files) {
        findAvi(file);
    }
}

public static void findAvi(File src) {
    // 1.进入文件夹src
    File[] files = src.listFiles();
    // 2.遍历数组，依次得到src里面每一个文件或者文件夹
    if (files != null) {
        for (File file : files) {
            if (file.isFile()) {
                // 3.判断 如果是文件---执行题目的业务逻辑
                String name = file.getName();
                if (name.endsWith(".avi")) {
                    System.out.println(file);
                }
            } else {
                // 4.判断 如果是文件夹---递归
                // 细节：再次调用本方法时，参数一定要是src的次一级路径
                findAvi(file);
            }
        }
    }
}
```

#### 练习4

需求：删除一个多级文件夹

```java
public static void main(String[] args) {
    /*
    删除一个多级文件夹
    如果我们要删除一个有内容的文件夹
    1.先删除文件夹里面所有的内容
    2.再删除自己
     */

    File file = new File("D:\\aaa\\ddd");
    delete(file);
}

/**
 * @param src 文件夹路径
 * @Description: 删除src文件夹
 * @Author: LiXiaoYang
 * @Date: 2024/11/3 17:51
 */
public static void delete(File src) {
    File[] files = src.listFiles();
    // 1.先删除文件夹里面的所有内容
    for (File file : files) {
        if (file.isFile()) {
            file.delete();
        } else {
            // 如果是文件夹，递归
            delete(file);
        }
    }
    // 2.再删除自己
    src.delete();
}
```

#### 练习5

小练

需求：统计一个文件夹的大小

```java
public static void main(String[] args) {
    /*
    需求：统计一个文件夹的总大小
     */
    File file = new File("D:\\aaa");
    long len = getLen(file);
    System.out.println(len);
}

/**
 * @param src 要统计文件夹的路径
 * @return long
 * @Description: 统计一个文件夹的总大小
 * @Author: LiXiaoYang
 * @Date: 2024/11/3 21:27
 */
public static long getLen(File src) {
    // 1.定义变量，进行累加
    long len = 0;
    // 2.进入src文件夹
    File[] files = src.listFiles();
    for (File file : files) {
        if (file.isFile()) {
            // 如果是文件，大小累加到len中
            len += file.length();
        } else {
            // 如果是文件夹，执行递归操作
            // 这里需要注意getLen()方法会重新定义一个long len = 0;不会和之前定义的len相加，所以需要我们拿之前的len加上子文件夹file的len才是正确结果
            len += getLen(file);
        }
    }
    return len;
}
```

需求：统计一个文件夹中每种文件的个数并打印（考虑子文件夹）

打印格式如下：
txt:3个

doc:4个

jpg:5个

```java
public static void main(String[] args) {

    /*
    需求：统计一个文件夹中每种文件的个数并打印（考虑子文件夹）
    打印格式如下：
        txt:3个
        doc:4个
        jpg:5个

    需要用到的知识：
    File 递归 Map集合（键：文件类型，值：数量）
     */

    File file = new File("D:\\aaa");
    HashMap<String, Integer> count = getCount(file);
    count.forEach((String endName, Integer i) -> System.out.println(endName + ":" + i + "个"));
}

/**
 * @param src 要统计的文件夹路径
 * @return java.util.HashMap<java.lang.String,java.lang.Integer>
 * @Description: 统计一个文件夹中每种文件的个数
 * @Author: LiXiaoYang
 * @Date: 2024/11/3 22:15
 */
public static HashMap<String, Integer> getCount(File src) {
    // 1.定义集合用来统计
    HashMap<String, Integer> hashMap = new HashMap<>();
    // 2.进入src文件夹
    File[] files = src.listFiles();
    // 3.遍历数组
    for (File file : files) {
        // 4.判断，如果是文件，统计
        if (file.isFile()) {
            String name = file.getName();
            String[] arr = name.split("\\.");
            if (arr.length >= 2) {
                String endName = arr[arr.length - 1];
                if (hashMap.containsKey(endName)) {
                    // 存在
                    int count = hashMap.get(endName);
                    count++;
                    hashMap.put(endName, count);
                } else {
                    // 不存在
                    hashMap.put(endName, 1);
                }
            }
        } else {
            // 5.如果是文件夹，递归
            // sonMap里面是子文件夹中每一种文件的个数
            HashMap<String, Integer> sonMap = getCount(file);
            // 遍历sonMap，把里面的值累加到hashMap中
            Set<Map.Entry<String, Integer>> entries = sonMap.entrySet();
            for (Map.Entry<String, Integer> entry : entries) {
                String key = entry.getKey();
                int value = entry.getValue();
                if (hashMap.containsKey(key)) {
                    // 存在
                    int count = hashMap.get(key);
                    count += value;
                    hashMap.put(key, count);
                } else {
                    // 不存在
                    hashMap.put(key, value);
                }
            }
        }
    }
    return hashMap;
}
```
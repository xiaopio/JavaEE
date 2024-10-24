# Runtime

Runtime: 表示当前虚拟机的运行环境

| 方法名                              | 说明                                      |
| ----------------------------------- | ----------------------------------------- |
| public static Runtime getRuntime()  | 当前系统的运行环境对象                    |
| public void exit(int status)        | 停止虚拟机                                |
| public int availableProcessors()    | 获取CPU的线程数                           |
| public long maxMemory()             | JVM能从系统中获取的总内存大小(单位byte)   |
| public long totalMemory()           | JVM已经从系统中获取的总内存大小(单位byte) |
| public long freeMemory()            | JVM剩余的内存大小(单位byte)               |
| public Process exec(String command) | 运行cmd命令                               |

1. 获取Runtime对象

```java
Runtime r = Runtime.getRuntime();
```

2. exit 停止虚拟机

```java
Runtime r = Runtime.getRuntime();
r.exit(0);
// 或者
// 他其实就是System.exit()的底层源码
Runtime.getRuntime().exit(0);
```

3. 获取CPU的线程数

```java
System.out.println(Runtime.getRuntime().availableProcessors());
```

4. 总内存大小,单位byte字节

```java
// 运行结果4257218560
System.out.println(Runtime.getRuntime().maxMemory());
// KB
System.out.println(Runtime.getRuntime().maxMemory() / 1024);
// MB
System.out.println(Runtime.getRuntime().maxMemory() / 1024 / 1024);

```

5. 已经获取的总内存大小

```java
// 已经获取了大概254兆
System.out.println(Runtime.getRuntime().totalMemory() / 1024 / 1024);
```

6. 剩余内存的大小

```java
// 剩余空间大小251兆
System.out.println(Runtime.getRuntime().freeMemory() / 1024 / 1024);
```

7. 运行cmd命令

```java
// 打开notepad
public static void main(String[] args) throws IOException {
    System.out.println(Runtime.getRuntime().exec("notepad"));
}
```

一些有意思的cmd命令

shutdowm: 关机

加上参数才能执行

-s : 默认在一分钟后关机

-s -t 指定时间 : 指定关机时间,单位秒

-a 取消关机操作

-r 关机并重启
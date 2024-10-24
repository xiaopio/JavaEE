# Math

**Math**: 帮助我们进行数学运算的工具类

里面的方法都是**静态的**

常见方法如下:

| **public static int**    | **abs(int a)**             | 获取参数绝对值                           |
| ------------------------ | -------------------------- | ---------------------------------------- |
| **public static double** | **ceil(double a)**         | **向上取整**                             |
| **public static double** | **floor(double a)**        | **向下取整**                             |
| **public static int**    | **round(float a)**         | **四舍五入**                             |
| **public static int**    | **max(int a,int b)**       | **获取两个int值中的较大值**              |
| **public static double** | **pow(double a,double b)** | **返回a的b次方幂的值**                   |
| **public static double** | **sqrt(double a)**         | **返回a的平方根**                        |
| **public static double** | **cbrt(double a)**         | **返回a的立方根**                        |
| **public static double** | **random()**               | **返回值为double的随机值,范围[0.0 1.0)** |

Math.random() [0.0 1.0)

Math.random() * 100 [0.0 100.0)

Math.random() * 100 + 1 [1.0 100.0)

**判断一个数是否为质数**

首先思考什么是质数?

质数(素数)是指在大于1的自然数中,除了1和他本身以外不在有其他因数的自然数,即不能被其他数整除.

思考 

一个数如果能被其他数(大于1)整除,那么他的因子一定是一个大于他的平方根,一个小于他的平方根

那么就可以利用这一点来减少代码运行的次数

```java
public static boolean isPrime(int number){
    for (int i = 2; i < Math.sqrt(number); i++) {
        if (number%i == 0){
            return false;
        }
    }
    return true;
}
```

自幂数:一个n位自然数等于自身各个位数上数字的n次幂之和

举例: 三位数 1^3 + 5^3 + 3^3 = 153

```java
// 要求统计一共有多少水仙花数(3位数)
        int count = 0;
        for (int i = 100; i < 999; i++) {

            int ge = i % 10;
            int shi = i / 10 % 10;
            int bai = i / 10 / 10 % 10;

            double sum = Math.pow(ge, 3) + Math.pow(shi, 3) + Math.pow(bai, 3);
            if (sum == (double) i) {
                count++;
                System.out.print(sum + " ");
            }
        }
        System.out.println();
        System.out.println("一共有" + count + "个水仙花数");
```


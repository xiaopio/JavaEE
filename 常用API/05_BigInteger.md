# BigInteger(07未完)

#### BigInteger构造方法

| 方法名                                   | 说明                                   |
| ---------------------------------------- | -------------------------------------- |
| public BigInteger(int num,Random rnd)    | 获取随机大整数，范围：[1~2的num次方-1] |
| public BigInteger(String val)            | 获取指定范围的大整数                   |
| public BigInteger(String val, int radix) | 获取指定进制的大整数                   |

| public static BigInteger valueOf(long val) | 静态方法获取BigInteger的对象，内部有优化 |
| ------------------------------------------ | ---------------------------------------- |

对象一旦创建，内部记录的值不能做改变

```java
public class Test {
    public static void main(String[] args) throws CloneNotSupportedException {
        // 1.获取一个随机的大整数
        BigInteger bigInteger1 = new BigInteger(4, new Random());
        // 范围[0, 15]
        System.out.println(bigInteger1);
        // 2.获取一个指定的大整数
        // 细节：字符串中必须是整数，否则会报错
        BigInteger bigInteger2 = new BigInteger("9999999999999999");
        System.out.println(bigInteger2);
        // 3.获取指定进制的大整数
        // 细节：1.字符串中的数字必须是整数
        // 2.字符串中的数字必须跟进制吻合，比如：2进制中只能写0和1，否则就会报错
        // radix：10表示为10进制的100，为2则表示为2进制的100
        BigInteger bigInteger3 = new BigInteger("100", 10);
        // 输出结果直接转换为2进制的4
        BigInteger bigInteger4 = new BigInteger("100", 2);
        System.out.println(bigInteger3);
        System.out.println(bigInteger4);
        // 4.静态方法获取BigInteger的对象，内部有优化
        // 细节：
        // 1.表示的范围比较小，只能在long的取值范围之内，超出范围会报错
        // 2.在内部对常用的数字，如-16 ~ +16进行了优化。
        //   提前把-16 ~ +16先创建好BigInteger对象，如多次获取不会重新创建新的。
        BigInteger bigInteger5 = BigInteger.valueOf(16);
        BigInteger bigInteger6 = BigInteger.valueOf(16);
        // true
        System.out.println(bigInteger5 == bigInteger6);
        BigInteger bigInteger7 = BigInteger.valueOf(17);
        BigInteger bigInteger8 = BigInteger.valueOf(17);
        // false
        System.out.println(bigInteger7 == bigInteger8);
    }

}
```
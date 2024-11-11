```java
public static void main(String[] args) throws UnknownHostException {
    // 1.获取InetAddress的对象
    // 确定主机名称的IP地址，主机名称可以是机器名称，也可以是IP地址
    InetAddress address = InetAddress.getByName("DESKTOP-RNPPNPH");
    System.out.println(address);

    // 获取此IP地址的主机名
    String hostName = address.getHostName();
    // DESKTOP-RNPPNPH
    System.out.println(hostName);

    // 返回文本显示中的IP地址字符串
    String hostAddress = address.getHostAddress();
    // 192.168.244.1
    System.out.println(hostAddress);
}
```


# UDP协议

## 发送数据

```java
public class SendMessageDemo {
    public static void main(String[] args) throws IOException {
        // 发送数据
        // 1.创建DatagramSocket对象（快递公司）
        // 细节：
        //      绑定端口，以后我们使用这个端口往外发送
        //      空参：随机端口
        //      有参：指定端口进行绑定
        DatagramSocket ds = new DatagramSocket();

        // 2.打包数据
        String str = "你好！！！";
        byte[] bytes = str.getBytes();
        InetAddress address = InetAddress.getByName("127.0.0.1");
        int port = 10086;
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port);

        // 3.发送数据
        ds.send(dp);

        // 4.释放资源
        ds.close();

    }
}
```

## 接收数据

```java
public class ReceiveMessageDemo {
    public static void main(String[] args) throws IOException {
        // 接收数据
        // 1.创建DatagramSocket对象（快递公司）
        // 细节：
        //      在接收时一定要绑定端口
        //      绑定的端口要与发送数据的端口保持一致
        DatagramSocket ds = new DatagramSocket(10086);

        // 2.打包数据
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length);
        // 该方法是阻塞的
        // 程序执行到这一步时会在这里死等
        // 等待发送端接收数据
        ds.receive(dp);

        // 3.解析数据包
        byte[] data = dp.getData();
        int length = dp.getLength();
        InetAddress address = dp.getAddress();
        int port = dp.getPort();

        System.out.println("接收到数据：" + new String(data, 0, length));
        System.out.println("该数据是从" + address + "这台电脑中的" + port + "端口发出的");

        // 4.释放资源
        ds.close();
    }
}
```

## 聊天室

```
按照下面的要求实现程序

​	UDP发送数据:数据来自键盘录入,直到输入数据是886,发送数据结束

​	UDP接收数据:因为接收端不知道发送端什么时候停止发送,故采用死循环接收

```

### 发送端

```java
public class SendMessageDemo {
    public static void main(String[] args) throws IOException {

        DatagramSocket ds = new DatagramSocket();

        String str;
        do {
            Scanner sc = new Scanner(System.in);
            System.out.println("发送者：");
            str = sc.nextLine();
            byte[] bytes = str.getBytes();

            InetAddress address = InetAddress.getByName("127.0.0.1");
            int port = 10086;

            DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port);

            ds.send(dp);
        } while (! "886".equals(str));

        ds.close();

    }
}
```

### 接收端

```java
public class ReceiveMessageDemo {
    public static void main(String[] args) throws IOException {
        /**
         * 按照下面的要求实现程序
         *      UDP发送数据:数据来自键盘录入,直到输入数据是886,发送数据结束
         *      UDP接收数据:因为接收端不知道发送端什么时候停止发送,故采用死循环接收
         */
        DatagramSocket ds = new DatagramSocket(10086);

        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length);
        while (true) {
            ds.receive(dp);

            byte[] data = dp.getData();
            int length = dp.getLength();
            String ip = dp.getAddress().getHostAddress();
            String hostName = dp.getAddress().getHostName();

            System.out.println("接收到来自IP：" + ip + "，主机名为：" + hostName + "发送来的数据：" + new String(data, 0, length));
//            ds.close();
        }
    }
}
```

UDP（User Datagram Protocol）是一种面向无连接的、不可靠的传输协议，在网络编程中常用的 UDP 通信方式主要有以下三种：

## UDP的三种通信方式

### 1. 单播（Unicast）通信方式

#### 特点

- **一对一通信**：单播是指在网络中，一个发送方（源主机）将 UDP 数据报发送给一个特定的接收方（目的主机）。这是最常见的通信模式，类似于日常生活中的一对一通话场景。
- **指定目的地址**：发送方在发送 UDP 数据报时，需要明确指定接收方的 IP 地址和端口号，以便网络将数据报准确地路由到目标主机上的相应应用程序。

#### 示例场景

- 例如，在一个简单的文件传输应用中，客户端想要从特定的服务器上下载一个文件，客户端作为发送方，就会将包含文件请求信息的 UDP 数据报发送给服务器（接收方）的特定 IP 地址和端口号，服务器接收到请求后进行相应处理并返回文件数据给客户端，这里就是典型的单播通信方式。

### 2. 多播（Multicast）通信方式

#### 特点

- **一对多通信**：多播允许一个发送方将 UDP 数据报同时发送给多个接收方，这些接收方组成一个多播组。发送方只需将数据报发送到多播组地址，网络会自动将数据报复制并分发给组内的各个成员，而不需要逐个向每个接收方发送。
- **多播组地址**：多播使用特殊的多播组 IP 地址，其范围是从 224.0.0.0 到 255.255.255.255（其中一些地址有特定用途，并非所有都可随意使用）。多播组内的成员通过加入或离开多播组来参与或退出多播通信。

#### 示例场景

- 比如，在一个在线直播系统中，主播端作为发送方，将直播视频流数据以 UDP 多播的方式发送到一个特定的多播组地址。观众端的各个设备（接收方）只要加入了该多播组，就可以同时接收到主播端发送的直播视频流，实现一对多的实时视频播放，无需主播端逐个向每个观众发送视频流，大大提高了网络传输效率。

### 3. 广播（Broadcast）通信方式

#### 特点

- **一对所有通信**：广播是指在一个网络范围内，一个发送方将 UDP 数据报发送给该网络内的所有主机，即广播范围内的所有设备都能接收到该数据报，无论这些设备是否需要该数据报所携带的信息。
- **本地网络限制**：广播通信通常是在本地局域网（LAN）范围内进行，因为如果在整个互联网上进行广播，会产生大量不必要的网络流量，导致网络拥塞，所以其应用场景相对局限于局域网环境。

#### 示例场景

- 例如，在一个小型办公室局域网内，有一台服务器用于发布通知消息。当服务器要向局域网内的所有计算机发送一条新的通知消息时，就可以采用 UDP 广播的方式，将包含通知内容的 UDP 数据报发送出去，局域网内的所有计算机都能接收到该消息，然后根据自身需求判断是否对该消息进行处理。

### 代码实现

1. 单播	以前的代码就是单播
2. 组播        组播地址:224.0.0.0 ~ 239.255.255.255

​					其中224.0.0.0 ~ 224.0.0.225为预留的组播地址

**发送端**	**接收端**创建 **MulticastSocket**对象

**接收端**多了一步,将本机添加到**224.0.0.1**的这一组中

```java
MulticastSocket ms = new MulticastSocket(10000);

InetAddress address = InetAddress.getByName("224.0.0.1");

ms.joinGroup(address);
```

3. 广播	广播地址:255.255.255.255

将单播send端的

```java
InetAddress address = InetAddress.getByName("127.0.0.1");
```

改为

```java
InetAddress address = InetAddress.getByName("255.255.255.255");
```


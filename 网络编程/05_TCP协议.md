## TCP通信程序

TCP通信协议是一种可靠的网络协议,它在通信的两端各建立一个Socket对象

通信之前要保证连接已经建立

通过Socket产生IO流来进行网络通信

![TCP通信程序](D:\admin\Documents\Java\总结\网络编程\assets\TCP通信程序-1731335527367-2.png)

### 服务器

```java
public class Server {
    public static void main(String[] args) throws IOException {
        // TCP协议，接收数据

        // 1.创建对象ServerSocket
        ServerSocket ss = new ServerSocket(10086);

        // 2.监听客户端连接
        Socket socket = ss.accept();

        // 3.从连接通道中获取输入流读取数据
        InputStream is = socket.getInputStream();
        int b;
        while ((b = is.read()) != - 1) {
            System.out.print((char) b);
        }

        // 4.释放资源
        socket.close();
        ss.close();
    }
}
```

### 客户端

```java
public class Cilent {
    public static void main(String[] args) throws IOException {
        // TCP协议，发送数据

        //1.创建Socket对象
        // 细节：创建对象的同时会连接服务器
        //      如果连不上，代码会报错
        Socket socket = new Socket("127.0.0.1", 10086);

        // 2.可以从连接通道中获取输出流
        OutputStream os = socket.getOutputStream();
        // 写数据
        os.write("aaa".getBytes());

        // 3.释放资源
        os.close();
        socket.close();
    } 
}
```

### 解决中文乱码问题

服务器中字节输入流改为字符输入流即可

```java
// 3.从连接通道中获取输入流读取数据
InputStream is = socket.getInputStream();
// 改成字符流解决中文乱码问题
InputStreamReader isr = new InputStreamReader(is);
// 缓冲流提高效率
BufferedReader br = new BufferedReader(isr);
```

链式编程

```java
BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
```

完整代码展示

```java
public class Server {
    public static void main(String[] args) throws IOException {
        // TCP协议，接收数据
        // 1.创建对象ServerSocket
        ServerSocket ss = new ServerSocket(10086);
        // 2.监听客户端连接
        Socket socket = ss.accept();
        // 3.从连接通道中获取输入流读取数据
        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        int b;
        while ((b = br.read()) != - 1) {
            System.out.print((char) b);
        }
        // 4.释放资源
        socket.close();
        ss.close();
    }
}
```

## 三次握手

TCP 三次握手是 TCP 协议建立连接的过程，通过三次报文交换来确保通信双方能够建立可靠的连接。

1. 第一次握手：
   - **发起方行为**：客户端（主动发起连接的一方）创建传输控制块，然后向服务器发送一个 TCP 连接请求报文段。该报文段首部中的同步位 `syn` 被设置为 1，表示这是一个 TCP 连接请求报文段；序号字段 `seq` 被设置为一个随机值 `x`（代表客户端选择的初始序号）。请注意，这个报文段不能携带数据，但要消耗掉一个序号。
   - **客户端状态**：发送完这个报文段后，客户端进入 `SYN_SENT` 状态，等待服务器的确认。
2. 第二次握手：
   - **接收方行为**：服务器收到客户端发来的 `SYN` 包后，如果同意建立连接，则向客户端发送 TCP 连接请求确认报文段。该报文段首部中的同步位 `syn` 和确认位 `ack` 都设置为 1，表明这是一个对连接请求的确认；序号字段 `seq` 被设置为一个随机值 `y`（代表服务器选择的初始序号）；确认号字段 `ack` 的值被设置为 `x + 1`，表示对客户端选择的初始序号 `seq` 的确认。这个报文段也不能携带数据，同样要消耗掉一个序号。
   - **服务器状态**：发送完这个报文段后，服务器进入 `SYN_RECV` 状态。
3. 第三次握手：
   - **发起方行为**：客户端收到服务器的 `SYN + ACK` 包后，向服务器发送一个普通的 TCP 确认报文段。该报文段首部中的确认位 `ack` 被设置为 1，表示这是一个普通的 TCP 确认报文段；序号字段 `seq` 被设置为 `x + 1`（因为客户端发送的第一个报文段的序号为 `x`，且不携带数据，所以第二个报文段的序号为 `x + 1`）；确认号字段 `ack` 被设置为 `y + 1`，表示对服务器选择的初始序号的确认。如果该确认报文段不携带数据，则不消耗序号，下一个数据报文段的序号仍是 `x + 1`。
   - **客户端和服务器状态**：客户端发送完这个报文段后进入 `ESTABLISHED` 状态，服务器收到该确认报文段后也进入 `ESTABLISHED` 状态，此时 TCP 双方都进入了连接已建立状态，可以基于已建立好的 TCP 连接进行可靠的数据传输。

TCP 三次握手的主要目的是为了防止已失效的连接请求报文段突然又传送到了服务器，从而产生错误。通过三次握手，通信双方能够相互确认对方的存在、协商一些连接参数，并对运输实体资源进行分配。

## 四次挥手

TCP 四次挥手是 TCP 连接终止的过程，以下是其具体步骤：

1. 第一次挥手：
   - **主动关闭方行为**：主动关闭方（可以是客户端或服务器端）的应用进程调用 `close` 等函数主动发起连接关闭请求，然后向对方发送一个 FIN（Finish）报文段。FIN 报文段的 FIN 标志位被设置为 1，表示请求关闭连接。同时，FIN 报文段会携带一个序列号，该序列号是主动关闭方之前传输数据的最后一个字节的序号加 1（即使这个 FIN 报文段不携带数据，也会消耗一个序号）。之后，主动关闭方进入 `FIN_WAIT_1` 状态，等待对方的确认。
2. 第二次挥手：
   - **被动关闭方行为**：被动关闭方收到主动关闭方发来的 FIN 报文段后，知道对方想要关闭连接。被动关闭方会立即向主动关闭方发送一个 ACK（Acknowledgment）报文段作为应答。ACK 报文段的 ACK 标志位设置为 1，确认号为主动关闭方发送的 FIN 报文段的序号加 1，表示已经收到了对方的关闭请求。发送完 ACK 报文段后，被动关闭方进入 `CLOSE_WAIT` 状态。此时，从主动关闭方到被动关闭方的连接已经关闭，但从被动关闭方到主动关闭方的连接仍然可以传输数据，处于半关闭状态。
3. 第三次挥手：
   - **被动关闭方行为**：当被动关闭方确定自己已经没有数据要发送给主动关闭方时，被动关闭方会向主动关闭方发送一个 FIN 报文段，请求关闭从被动关闭方到主动关闭方的连接。FIN 报文段的 FIN 标志位为 1，序列号为被动关闭方之前传输数据的最后一个字节的序号加 1。发送完 FIN 报文段后，被动关闭方进入 `LAST_ACK` 状态，等待主动关闭方的最后确认。
4. 第四次挥手：
   - **主动关闭方行为**：主动关闭方收到被动关闭方发来的 FIN 报文段后，会向被动关闭方发送一个 ACK 报文段作为应答。ACK 报文段的 ACK 标志位为 1，确认号为被动关闭方发送的 FIN 报文段的序号加 1。发送完 ACK 报文段后，主动关闭方进入 `TIME_WAIT` 状态。经过一段时间的等待（通常为 2 倍的 MSL，Maximum Segment Lifetime，即报文最大生存时间），如果在这段时间内没有收到被动关闭方重发的 FIN 报文段，主动关闭方就会认为被动关闭方已经成功接收到了自己的 ACK 报文段，此时主动关闭方才会真正关闭连接，进入 `CLOSED` 状态。而被动关闭方在收到主动关闭方的 ACK 报文段后，也会立即关闭连接，进入 `CLOSED` 状态。

TCP 四次挥手的过程是为了确保连接双方都能正确地关闭连接，并且在关闭连接的过程中保证数据的可靠传输。
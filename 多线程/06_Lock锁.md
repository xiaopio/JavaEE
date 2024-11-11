# Lock锁

**一、Lock 的主要特点**

1. 灵活性更高
   - 可以尝试获取锁（`tryLock`），如果锁不可用，可以立即返回而不是一直等待，这在某些情况下可以避免死锁和提高性能。
   - 支持中断等待锁的线程（`lockInterruptibly`），如果一个线程在等待锁的过程中被中断，它可以响应中断并抛出`InterruptedException`。
2. 可实现更复杂的同步结构
   - 可以实现公平锁，即按照请求锁的顺序来分配锁，保证等待时间最长的线程先获得锁。而`synchronized`关键字无法实现公平锁。

**二、Lock 的基本使用方法**

1. 创建`Lock`对象
   - 通常使用`ReentrantLock`类来创建一个`Lock`对象，例如：`Lock lock = new ReentrantLock();`。
2. 获取锁和释放锁
   - 使用`lock.lock()`方法获取锁，在代码执行完需要同步的部分后，使用`lock.unlock()`方法释放锁。确保在 finally 块中释放锁，以保证即使在同步代码块中发生异常，锁也能被正确释放。

```java
Lock lock = new ReentrantLock();
try {
    lock.lock();
    // 同步代码块
} finally {
    lock.unlock();
}
```

**三、与`synchronized`的比较**

1. `synchronized`是 Java 语言内置的关键字，使用起来比较简单直观，但灵活性相对较低。
2. `Lock`是一个接口，通过实现类（如`ReentrantLock`）来使用，需要显式地获取和释放锁，但提供了更多的功能和灵活性。

如果需要更灵活的同步控制，`Lock`是一个更好的选择。

```java
public class MyThread extends Thread {
    // 表示这个类所有的对象都共享ticket对象
    static int ticket = 0;

    static Lock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            
            lock.lock();
            
            try {
                if (ticket == 100) {
                    break;
                } else {
                    ticket++;
                    System.out.println(getName() + "...正在卖第" + ticket + "张票！");
                }
            } catch (Exception e) {
                throw new RuntimeException(e);
            } finally {
                lock.unlock();
            }
        }
    }
}
```
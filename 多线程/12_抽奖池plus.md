# 升级

在上一题的基础上继续完成如下需求：

​	每次抽的过程中不打印结果，抽完时一次性打印（随机）

​	在此次抽奖过程中，抽奖箱1一共产生了6个奖项。

​		分别为：10，20，100，500，2，300最高奖项为300元，总计金额932元

​	在此次抽奖过程中，抽奖箱2一共产生了6个奖项。

​		分别为：5，50，200，800，80，700最高奖项为800元，总计金额1835元

```java
public class MyThread extends Thread {
    // {10,5,20,50,100,200,500,800,2,80,300,700}
    // 集合
    ArrayList<Integer> list;

    public MyThread(ArrayList<Integer> list) {
        this.list = list;
    }

    static ArrayList<Integer> list1 = new ArrayList<>();
    static ArrayList<Integer> list2 = new ArrayList<>();

    @Override
    public void run() {
        // 1.循环
        // 2.同步代码块
        // 3.1判断
        // 3.2判断
        while (true) {
            synchronized (com.haust.test05.MyThread.class) {
                if (list.size() == 0) {
                    if ("抽奖箱1".equals(getName())) {
                        System.out.print("抽奖箱1产生了" + list1.size() + "个奖项，分别为：" + list1);
                        int max = 0;
                        int total = 0;
                        for (int i = 0; i < list1.size(); i++) {
                            total += list1.get(i);
                            if (list1.get(i) > max) {
                                max = list1.get(i);
                            }
                        }
                        System.out.print("最高奖项为" + max + "元,");
                        System.out.println("总计额为" + total + "元");
                    } else if ("抽奖箱2".equals(getName())) {
                        System.out.print("抽奖箱2产生了" + list2.size() + "个奖项，分别为：" + list2);
                        int max = 0;
                        int total = 0;
                        for (int i = 0; i < list2.size(); i++) {
                            total += list2.get(i);
                            if (list2.get(i) > max) {
                                max = list2.get(i);
                            }
                        }
                        System.out.print("最高奖项为" + max + "元,");
                        System.out.println("总计额为" + total + "元");
                    }
                    break;
                } else {
                    Collections.shuffle(list);
                    Integer prize = list.remove(0);
                    if ("抽奖箱1".equals(getName())) {
                        list1.add(prize);
                    } else if ("抽奖箱2".equals(getName())) {
                        list2.add(prize);
                    }
                }
            }
            try {
                Thread.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list, 10, 5, 20, 50, 100, 200, 500, 800, 2, 80, 300, 700);

        MyThread t1 = new MyThread(list);
        MyThread t2 = new MyThread(list);
        t1.setName("抽奖箱1");
        t2.setName("抽奖箱2");
        t1.start();
        t2.start();
    }
}
```

# 改进

只需创建一个集合就行了

```java
public class MyThread extends Thread {
    // {10,5,20,50,100,200,500,800,2,80,300,700}
    // 集合
    ArrayList<Integer> list;

    public MyThread(ArrayList<Integer> list) {
        this.list = list;
    }

    @Override
    public void run() {
        ArrayList<Integer> boxList = new ArrayList<>();
        while (true) {
            synchronized (com.haust.test05.MyThread.class) {
                if (list.size() == 0) {
                    System.out.print(getName() + "产生了" + boxList.size() + "个奖项，分别为：" + boxList);
                    int max = 0;
                    int total = 0;
                    for (int i = 0; i < boxList.size(); i++) {
                        total += boxList.get(i);
                        if (boxList.get(i) > max) {
                            max = boxList.get(i);
                        }
                    }
                    System.out.print("最高奖项为" + max + "元,");
                    System.out.println("总计额为" + total + "元");
                    break;
                } else {
                    Collections.shuffle(list);
                    Integer prize = list.remove(0);
                    boxList.add(prize);
                }
            }
            try {
                Thread.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list, 10, 5, 20, 50, 100, 200, 500, 800, 2, 80, 300, 700);

        MyThread t1 = new MyThread(list);
        MyThread t2 = new MyThread(list);
        MyThread t3 = new MyThread(list);
        MyThread t4 = new MyThread(list);
        t1.setName("抽奖箱1");
        t2.setName("抽奖箱2");
        t3.setName("抽奖箱3");
        t4.setName("抽奖箱4");
        t1.start();
        t2.start();
        t3.start();
        t4.start();
    }
}
```

# 升级

统计两个奖池中的最大奖项并进行打印，例如：

​	在此次抽奖过程中抽奖箱1中产生了最大奖项，该奖项的奖金为800元

```java
public class MyCallable implements Callable<Integer> {
    ArrayList<Integer> list;

    public MyCallable(ArrayList<Integer> list) {
        this.list = list;
    }

    @Override
    public Integer call() throws Exception {
        ArrayList<Integer> boxList = new ArrayList<>();
        while (true) {
            synchronized (com.haust.test05.MyThread.class) {
                if (list.size() == 0) {
                    System.out.print(Thread.currentThread().getName() + "产生了" + boxList.size() + "个奖项，分别为：" + boxList);
                    int max = Collections.max(boxList);
                    int total = 0;
                    for (int i = 0; i < boxList.size(); i++) {
                        total += boxList.get(i);
                    }
                    System.out.print("最高奖项为" + max + "元,");
                    System.out.println("总计额为" + total + "元");
                    break;
                } else {
                    Collections.shuffle(list);
                    Integer prize = list.remove(0);
                    boxList.add(prize);
                }
            }
            Thread.sleep(5);
        }
        if (boxList.size() == 0) {
            return null;
        } else {
            return Collections.max(boxList);
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list, 10, 5, 20, 50, 100, 200, 500, 800, 2, 80, 300, 700);

        MyCallable mc = new MyCallable(list);

        FutureTask<Integer> ft1 = new FutureTask<>(mc);
        FutureTask<Integer> ft2 = new FutureTask<>(mc);

        Thread t1 = new Thread(ft1, "抽奖箱1");
        Thread t2 = new Thread(ft2, "抽奖箱2");

        t1.start();
        t2.start();

        int max1 = ft1.get();
        int max2 = ft2.get();

        if (max1 > max2) {
            System.out.println("在此次抽奖过程中" + t1.getName() + "中产生了最大奖项，该奖项的奖金为" + max1 + "元");
        } else if (max2 > max1) {
            System.out.println("在此次抽奖过程中" + t2.getName() + "中产生了最大奖项，该奖项的奖金为" + max2 + "元");
        }
    }
}
```


**什么是包**？

包就是文件夹，用来管理各种不同功能的Java类，方便后期代码维护。

**包名规则**：公司域名反写+包的作用，需要全部英文小写，见名之意。

**使用其他类的规则**

使用其他类时，需要使用全类名。

import	com.XXXX.XXXX

使用同一包中的类时，不需要导包。

使用 java.lang 包时，不需要导包。

其他情况都需要导包

如果同时使用两个包中的同名类，需要用全类名。



**final 能修饰什么？**

方法	表明该方法是最终方法，不能被重写 

类	表明该类是最终类，不能被继承

变量	叫做常量，只能被赋值一次

如果当前的方法是一种规则，不希望被别人修改，就用final修饰

final	修饰基本数据类型：记录的值不能被改变

final	修饰引用数据：记录的地址值不能被改变，内部的属性值还是可以改变的

![image-20241014213712287](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20241014213712287.png)

输出结果：李四 24

![image-20241014214029823](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20241014214029823.png)

输出结果：10 20 30 40 50 


## 第四章：

### 作业：

1.完善程序：

代码：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        //创建Scanner对象
        //System.in表示标准化输入，也就是键盘输入
        Scanner sc = new Scanner(System.in);
        //利用hasNextXXX()判断是否还有下一输入项
        int a = 0;
        int b = 0;
        int c = 0;
        if (sc.hasNext()) {
            a = sc.nextInt();
        }

        if (sc.hasNext()) {
            b = sc.nextInt();
        }

        if (sc.hasNext()) {
            c = sc.nextInt();
        }

        if(a==0 || b==0 || c==0) {
            System.out.println("输入不能为0");
            System.exit(-1);
        }

        MyNumber obj1, obj2, obj3;
        //从这里开始，基于swap函数，完善你的程序
        obj1 = new MyNumber();
        obj2 = new MyNumber();
        obj3 = new MyNumber();
        obj1.num = a;
        obj2.num = b;
        obj3.num = c;
        if(obj1.num > obj2.num){
            swap(obj1, obj2);
        }
        if(obj2.num > obj3.num){
            swap(obj2, obj3);
        }
        if(obj1.num > obj2.num){
            swap(obj1, obj2);
        }
        System.out.println(obj1.num + "," + obj2.num + "," + obj3.num);
        //程序结束
    }

    public static void swap(MyNumber m, MyNumber n) {
        if(m.num > n.num) {
            int s = m.num;
            m.num = n.num;
            n.num = s;
        }
    }
}

class MyNumber {
    int num;
}
```

运行截图：

![](data:image/png;base64,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)



### 笔记：

+ #### 面向对象思想：

  + 对象=属性+方法

  + 类是规范、定义，对象是类的具体实现

  + 类可以继承：子类可以继承父类所有内容(不能直接访问private成员)，子类必须通过父类的方法才能访问父类的私有变量。

    

+ #### 面向对象语言的主要特点：

  + 识认性、类别性、多态性、继承性

  + 世界是由对象和对象之间的协作而组成的

  + 对象包含属性/成员变量和行为/成员方法

  + 对象可以通过继承来实现一样的属性和行为

    

+ #### java类和对象：

  + A obj = new A();

  + 没有两个对象是完全一样的

  + 类是规范，对象是根据规范产生出的实例

  + 基本型别赋值是拷贝赋值，对象赋值是reference赋值

  + 产生一个对象，99%是用new关键字，还有1%是用克隆和反射

  + 类成员变量有初值，但函数临时变量必须初始化

    

+ #### 构造函数

  + Java构造函数的名称必须和类名一样，且没有返回值 

  + Java有构造函数，但是没有析构函数

  + 构造函数是制造对象的过程，析构函数是清除对象的过程·每个变量都是有生命周期的，它只能存储在离它最近的一对{}中

  + 当变量被创建时，变量将占据内存;当变量消亡时，系统将回收内存

  + Java具有内存自动回收机制的，当变量退出其生命周期后，JVM会自动回收所分配的对象的内存。所以不需要析构函数来释放内存。

  + 对象回收效率依赖于垃圾回收器GC(Garbage Collector) ,其回收算法关系到性能好坏，是JVM的研究热点。

  + 每个Java类都必须有构造函数。

  + 如果没有显式定义构造函数，Java编译器自动为该类产生一个空的无形参构造函数。如果已经有了显式的有参构造函数，编译器就不会越俎代庖了。

  + 每个子类的构造函数的第一句话，都默认调用父类的无参数构造函数super(，除非子类的构造函数第一句话是super，而且super语句必须放在第一条。(本条规则在后续继承时会再提到)

    

+ #### 信息隐藏原则

  + 面向对象有一个法则:信息隐藏
    + 类的成员属性，是私有的private;
    + 类的方法是公有public的，通过方法修改成员属性的值。
  + 打个比方:朋友再熟悉，也不会到他抽屉直接拿东西，而是通过他的公开接口来访问、修改东西。
  + 类成员是私有private的
  + get和set方法是公有public的，统称为getter和setter
  + 外界对类成员的操作只能通过get和set方法
## 第十章：

### 作业：

1.完成汇率和金额排序：

代码：

```java
import java.util.Arrays;
import java.util.Scanner;

class Currency {
    private String name;        //货币名称
    private int originalValue;  //原始值
    private int value;          //转换为人民币后的值

    public static String[] CURRENCY_NAME = { "CNY", "HKD", "USD", "EUR" };
    public static int[] CURRENCY_RATIO = { 100, 118, 15, 13 };

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getOriginalValue() {
        return originalValue;
    }

    public void setOriginalValue(int originalValue) {
        this.originalValue = originalValue;
    }

    public int getValue() {
        return value;
    }

    public void setValue(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return getName() + getOriginalValue();
    }
}

class HKD extends Currency implements Comparable<Currency> {
    public HKD(int originalValue){
        setOriginalValue(originalValue);
        for (int i = 0; i < 4; i++){
            if(CURRENCY_NAME[i].equals("HKD")){
                setName("HKD");
                setValue(getOriginalValue() / CURRENCY_RATIO[i] * 100);
            }
        }
    }

    @Override
    public int compareTo(Currency c) {
        if(getValue() > c.getValue()) {
            return 1;
        } else {
            return -1;
        }
    }
    // 实现你的构造函数与Comparable中的接口
}

class USD extends Currency implements Comparable<Currency> {
    public USD(int originalValue){
        setOriginalValue(originalValue);
        for (int i = 0; i < 4; i++){
            if(CURRENCY_NAME[i].equals("USD")){
                setName("USD");
                setValue(getOriginalValue() / CURRENCY_RATIO[i] * 100);
            }
        }
    }

    @Override
    public int compareTo(Currency c) {
        if(getValue() > c.getValue()) {
            return 1;
        } else {
            return -1;
        }
    }
    // 实现你的构造函数与Comparable中的接口
}

class EUR extends Currency implements Comparable<Currency> {
    public EUR(int originalValue){
        setOriginalValue(originalValue);
        for (int i = 0; i < 4; i++){
            if(CURRENCY_NAME[i].equals("EUR")){
                setName("EUR");
                setValue(getOriginalValue() / CURRENCY_RATIO[i] * 100);
            }
        }
    }

    @Override
    public int compareTo(Currency c) {
        if(getValue() > c.getValue()) {
            return 1;
        } else {
            return -1;
        }
    }
    // 实现你的构造函数与Comparable中的接口
}

public class Main {
    public static void main(String[] args) {
        Currency[] cs = new Currency[3];
        //初始化
        Scanner sc = new Scanner(System.in);
        //利用hasNextXXX()判断是否还有下一输入项
        int a = 0;
        int b = 0;
        int c = 0;
        if (sc.hasNext()) {
            a = sc.nextInt();
            cs[0] = new HKD(a);
        }

        if (sc.hasNext()) {
            b = sc.nextInt();
            cs[1] = new USD(b);
        }

        if (sc.hasNext()) {
            c = sc.nextInt();
            cs[2] = new EUR(c);
        }
        //初始化结束

        //请补充排序
        Arrays.sort(cs);

        //请补充输出结果
        for(Currency cc : cs){
            System.out.println(cc);
        }
    }
}
```

运行截图：

![](data:image/png;base64,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)



### 笔记：

#### 数组：

+ 数组：是一个存放多个数据的容器（容器，就是一种可以放东西的设备，比如一口锅。从广义上说，以前的变量定义也是一个容器，例如，int a = 5; a实际上是一个容器，a里面放着5）
  + 数据是同一种类型
  + 所有的数据是线性规则排列
  + 可通过位置索引来快速定位访问数据
  + 需明确容器的长度

+  Java数组定义和初始化

  ```java
  int a[]; //a还没有new操作实际上是null，也不知道内存位置
  int[] b; //b还没有new操作实际上是null，也不知道内存位置
  int[] c = new int[2]; //c有2个元素，都是0
  c[0] = 10; c[1] = 20; //逐个初始化
  int d[] = new int[]{0,2,4};//d有3个元素，0，2，4，同时定义和初始化
  int d1[]= {1,3,5};//d1有3个元素，1，3，5同时定义和初始化
  //注意声明变量时候没有分配内存，不需要指定大小，以下是错误示例：
  int e[5];
  int[5] f;
  int[5]g = new int[5];
  int h[5] = new int[5];
  ```

+ 数组索引
  + 数组的length属性标识数组的长度
  + 从0开始，到length - 1
  + int[a] = new int[5]; //a[0]~a[4], not a[5]; a.length是5
  + 数组不能越界访问，否则会报ArrayIndexOutOfBoundsException异常

+ 数组遍历：两种方法

  ```java
  //需要自己控制索引位置
  for(int i = 0; i < d.length; i++)
  {
  	system.out.print1n(d[i]);
  }
  //无需控制索引位置
  for(int e : d)
  {
  	system.out.println(e);
  }
  ```

+ 多维数组：

  + 数组的数组
  + 存储是按照行存储原则

  ```java
  //规则数组
  int a[][] = new int[2][3];
  //不规则数组
  int b[][];
  b = new int[3][];
  b[0] = new int[3];
  b[1] = new int[4];
  b[2] = new int[5];
  
  int k = 0;
  for(int i=0; i < a.length; i++)
  {
      for(int j = 0; j < a[i].length; j++)
      {
      	a[i][j] = ++k;
      }
  }
  for(int[] items : a)
  {
      for(int item : items)
      {
     		system.out.print(item + ",");
      }
      system.out.print1n();
  }
  ```

  

#### JCF：

+ 基础介绍：
  + 容器：能够存放数据的空间结构
    + 数组 / 多维数组，只能线性存放
    + 列表 / 散列集 / 树 / …….
  + 容器框架：为表示和操作容器而规定的一种标准体系结构
    + 对外的接口：容器中所能存放的抽象数据类型
    + 接口的实现：可复用的数据结构
    + 算法：对数据的查找和排序
  + 容器框架优点：提高数据存取效率，避免程序员重复劳动
  + C+＋的STL，Java的JCF

+ 特点：

  + Java 1.1和以前的数据结构
    - Vector，Stack，Hashtable，Enumeration等

  + Java1.2和以后，JCF集合框架
    + 功能更强大
    + 易于学习
    + 接口和实现分离，多种设计模式设计更灵活
    + 泛型设计

+ 接口：

  + 早期接口Enumeration
  + JCF的集合接口是Collection
    - add，contains，remove，size
    - iterator

  + JCF的迭代器接口Iterator
    + hasNext
    + next
    + remove

+ 主要类：

  + JCF主要的数据结构实现类
    + 列表（List，ArrayList，LinkedList）
    + 集合（Set，HashSet，TreeSet，LinkedHashSet）
    + 映射（Map，HashMap，TreeMap，LinkedHashMap）

  + JCF主要的算法类
    + Arrays：对数组进行查找和排序等操作
    + Collections：对Collection及其子类进行排序和查找操作

+ 总结：
  + 容器框架的作用
  + JCF主要数据结构
    + 列表
    + 集合
    + 映射



#### 列表List：

+ 介绍：

  + List：列表
    + 有序的Collection
    + 允许重复元素
    + {1，2，4，{5，2}，1，3}

  + List 主要实现
    + ArrayList（非同步的）
    + LinkedList（非同步）
    + Vector（同步）

+ ArrayList：
  +  以数组实现的列表，不支持同步
  + 利用索引位置可以快速定位访问
  + 不适合指定位置的插入、删除操作
  + 适合变动不大，主要用于查询的数据
  + 和Java数组相比，其容量是可动态调整的
  + ArrayList在元素填满容器时会自动扩充容器大小的50%

+ LinkedList：
  + 以双向链表实现的列表，不支持同步
  + 可被当作堆栈、队列和双端队列进行操作
  + 顺序访问高效，随机访问较差，中间插入和删除高效
  + 适用于经常变化的数据

+ Vector（同步）
  + 和ArrayList类似，可变数组实现的列表
  + Vector同步，适合在多线程下使用
  + 原先不属于JCF框架，属于Java最早的数据结构，性能较差
  + 从JDK1.2开始，Vector被重写，并纳入到JCF
  + 官方文档建议在非同步情况下，优先采用ArrayList




#### 集合Set：

+ 特性：
  + 确定性：对任意对象都能判定其是否属于某一个集合
  + 互异性：集合内每个元素都是不相同的，注意是内容互异
  + 无序性：集合内的顺序无关

+ 集合接口：
  + HashSet（基于散列函数的集合，无序，不支持同步）
  + TreeSet（基于树结构的集合，可排序的，不支持同步）
  + LinkedHashSet（基于散列函数和双向链表的集合，可排序的，不支持同步）

+ HashSet

  + 基于HashMap实现的，可以容纳null元素，不支持同步
    + Set s = Collections.synchronizedSet(new HashSet(.….));

  + add添加一个元素
  + clear清除整个HashSet
  + contains判定是否包含一个元素
  + remove删除一个元素size大小
  + retainAll计算两个集合交集

+ LinkedHashSet

  + 继承HashSet，也是基于HashMap实现的，可以容纳null元素
  + 不支持同步
    + Set s = Collections.synchronizedSct(new LinkedHashSet(...));

  + 方法和HashSet基本一致
    + add，clear，contains，remove，size

  + 通过一个双向链表维护插入顺序

+ TreeSet

  + 基于TreeMap实现的，不可以容纳null元素，不支持同步
    + SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...);

  + add添加一个元素

  - clear清除整个TreeSet
  - contains 判定是否包含一个元素
  - remove删除一个元素  size 大小
  - 根据compareTo方法或指定Comparator排序

+ 比较：

  + HashSet，LinkedHashSet，TreeSet的元素都只能是对象

  + HashSet和LinkedHashSet判定元素重复的原则

    + 判定两个元素的hashCode返回值是否相同，若不同，返回false
    + 若两者hashCode相同，判定equals方法，若不同，返回false; 否则返回true。

    - hashCode和equals方法是所有类都有的，因为Object类有

  + TreeSet判定元素重复的原则
    + 需要元素继承自Comparable接口
    + 比较两个元素的compareTo方法



#### 映射Map：

+ 介绍：

  + Map映射
    + 数学定义：两个集合之间的元素对应关系。
    + 一个输入对应到一个输出
    + {1，张三}，{2，李四}，{Key，Value}，键值对，K-V对 

  + Java中Map
    + Hashtable（同步，慢，数据量小）
    + HashMap（不支持同步，快，数据量大）
    + Properties（同步，文件形式，数据量小）

+ Hashtable
  - K-V对，K和V都不允许为null
  - 同步，多线程安全
  - 无序的
  - 适合小数据量
  - 主要方法：clear，contains / containsValue，containsKey，get，put，remove，size

+ HashMap

  + K-V对，K和V都允许为null
  + 不同步，多线程不安全
    + Map m = Collections.synchronizedMap(new HashMap(...));

  + 无序的
  + 主要方法：clear，containsValue，containsKey，get，put，remove，size

+ LinkedHashMap
  + 基于双向链表的维持插入顺序的HashMap

+ TreeMap
  + 基于红黑树的Map，可以根据key的自然排序或者compareTo方法进行排序输出

+ Properties
  + 继承于Hashtablc
  + 可以将K-V对保存在文件中
  + 适用于数据量少的配置文件
  + 继承自Hashtable的方法：clear，contains / containsValue，containsKey，get，put，remove，size
  + 从文件加载的load方法，写入到文件中的store方法
  + 获取属性getProperty，设置属性setProperty




#### 工具类：

+ JCF中工具类

  + 不存储数据，而是在数据容器上，实现高效操作
    + 排序
    + 搜索

  + Arrays类
  + Collections类

- Arrays：处理对象是数组
  - 排序：对数组排序，sort / parallelSort。
  - 查找：从数组中查找一个元素，binarySearch。
  - 批量拷贝：从源数组批量复制元素到目标数组，copyOf。
  - 批量赋值：对数组进行批量赋值，fill。
  - 等价性比较：判定两个数组内容是否相同，equals。

+ Collections：处理对象是Collection及其子类
  + 排序：对List进行排序，sort
  + 搜索：从List中搜索元素，binarySearch
  + 批量赋值：对List批量赋值，fill。
  + 最大、最小：查找集合中最大 / 小值，max，min
  + 反序：将List反序排列，reverse

+ 对象比较

  + 对象实现Comparable接口（需要修改对象类）

    - compareTo方法
    - 大于返回1，==返回0，小于返回-1

    + Arrays和Collections在进行对象sort时，自动调用该方法

  + 新建Comparator（适用于对象类不可更改的情况）

    + compare方法
      + 大于返回1，==返回0，小于返回-1

    + Comparator比较器将作为参数提交给工具类的sort方法

## 第三章：

### 作业：

1.输出properties文件中的值：

代码：

```java
import java.util.Locale;
import java.util.ResourceBundle;

public class GetValue{
    public static void main(String[] args)  {
        Locale myLocale = Locale.getDefault();
        System.out.println(myLocale);

        ResourceBundle  universityResourceBundle= ResourceBundle.getBundle("msg_zh",myLocale);
        System.out.println(universityResourceBundle.getString("university"));

        ResourceBundle nameResourceBundle = ResourceBundle.getBundle("msg_zh_CN",myLocale);
        System.out.println(nameResourceBundle.getString("name"));

        myLocale= new Locale("en","US");
        nameResourceBundle = ResourceBundle.getBundle("msg_en_US",myLocale);
        System.out.println(nameResourceBundle.getString("name"));
    }
}
```

运行截图：

![](data:image/png;base64,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)

### 笔记：

#### Java字符编码：

+ 字符编码

  + 字符编码

    + 字符：0，a，我，①，……
    + 计算机只用0和1，1 bit(0 或者1)
    + ASCII码(Amcrican Standard Code for Information Interchangc)
    + 美国信息交换标准代码，奠定计算机编码基础
    + 用一个字节(1 Byte=8 bits)来存储a-z，A-Z，0-9和一些常用符号
    + 用于显示英语及西欧语言
    + 回车键(13,00001101)，0(48,00110000)，A(65,01000001)，a(97,01100001)
    + ASCII编码采用1 Byte，8 bits，最多256个字符
    + ASCII无法适应其他地方，如汉字数量有十几万
    + 扩展编码(加字节)
      - ISO8859(1-15)西欧语言
      - GB2132，GBK，GB18030 ASCII+中文
      - Big5 ASCI＋繁体中文
      - Shift_JIS ASCHI+日文
      - ......

    + Unicode 编码

  + 中文编码

    + GB2312，1980年发布，7445个字符(6763个简体字)，包括拉丁字母、希腊字母、日文平假名及片假名字母、俄语西里尔字母等682个符号
    + GBK，1995年发布，21886个汉字和符号，包括GB2312和Big 5
    + GB18030(2000,2005两个版本)，70244个汉字和符号，包括GBK和GB2312

    - Big 5，繁体中文
    - GB18030 > GBK > GB2312

  + Unicode(字符集)
    
    + 目标：不断扩充，存储全世界所有的字符
  + 编码方案
    + UTF-8，兼容ASCII，变长(1-4个字节存储字符)，经济，方便传输
    + UTF-16，用变长(2-4个字节)来存储所有字符
    + UTF-32，用32bits(4个字节)存储所有字符

  + ANSI编码
    - Windows上非Unicode的默认编码(Windows code pages)
    - 在简体中文Windows操作系统中，ANSI 编码代表GBK编码
    - 在繁体中文Windows操作系统中，ANSI编码代表Big5
    - 记事本默认是采用ANSI保存
    - ANSI编码文件不能在兼容使用

  + 源文件编码：采用UTF-8编码
    + Eclipse，右键java文件，属性，resource，选择UTF-8
    + Eclipse，右键项目，属性，resource，选择UTF-8

  + 程序内部采用UTF-16编码存储所有字符(不是程序员控制)
  + 和外界(文本文件)的输入输出尽量采用UTF-8编码
    
  + 不能使用一种编码写入，换另外一种编码读取
    
  + 通过CharsetTest.java，TxtReadUTF8.java，TxtWriteUTF8.java来了解Java的字符编码

+ 总结
  + 了解字符编码的分类
  + 了解Java的字符编码和文件的输入输出



#### Java国际化编程：

+ 国际化编程

  + Internationalization，缩写为i18n
  + 多语言版本的软件
    + 套软件，多个语言包
    + 根据语言设定，可以切换显示文本

  + Java是第一个设计成支持国际化的编程语言
    + java.util.ResourceBundle用于加载一个语言_ 国家语言包
    + java.util.Locale定义一个语言_国家
    + java.text.MessageFormat用于格式化带占位符的字符串
    + java.text.NumberFormat用于格式化数字/金额
    + java.text.DateFormat用于格式化日期时间
    + java.time.format.DateTimcFormatter用于格式化日期时间(后4个Format参见《Java核心技术》第8章)

+ Locale类

  + Locale(zh_CN, en_US,…)
    + 语言，zh, en等
    + 国家/地区，CN，US等
    + 其他变量(variant)(几乎不用)
  + Locale方法
    + getAvailableLocales()返回所有的可用Locale
    + getDefault()返回默认的Locale

+ 语言文件
  + 语言文件
    + 一个Properties文件(参见《Java核心技术》第十章)
    + 包含K-V对，每行一个K-V，例如: age=20
    + 命名规则
      + 包名+语言+国家地区.properties,(语言和国家地区可选)
      + message.properties
      + mcssage_zh.propertics
      + message_zh_CN.propertics
    + 存储文件必须是ASCII码文件
    + 如果是ASCII以外的文字，必须用Unicode的表示\uxxxx
    + 可以采用native2ascii.exe (%JAVA_HOME%\bin目录下)进行转码

+ ResourceBundle类

  + ResourceBundle
    + 根据Locale要求，加载语言文件(Properties文件)
    + 存储语言集合中所有的K-V对
    + getString(String key)返回所对应的value

  + ResourceBundle根据key找value的查找路径
    + 包名_ 当前Locale语言_ 当前Locale国家地区_ 当前Locale变量(variant)
    + 包名_ 当前Locale语言_ 当前Locale国家地区
    + 包名_ 当前Locale语言
    + 包名_ 默认Locale语言_ 默认Locale国家地区_ 默认Locale变量(variant)
    + 包名_ 默认Locale语言_ 默认Locale国家地区
    + 包名_ 默认Locale语言
    + 包名

+ 其他国际化
  + 日期/时间国际化
    + DateTimeFormatter和Locale的结合
  + 数字/金额国际化
    + NumberFormat和Locale结合

+ 总结
  + Java国际化总结
    - ResourceBundlc和Locale类
    - Properties文件的制作和native2ascii的转化



#### Java高级字符串处理：

+ 大纲
  + 正则表达式
  + 其他字符串操作
    + 集合字符串互转
    + 字符串转义
    + 变量名字格式化
    + 从字符串到输入流

+ 正则表达式
  + 如何识别给定字符串为合法的邮箱地址
    + a@b.com
    + a@@b.com
    + a@b
    + @a.com
    + a@b@c.com
  + 如何认定一个字符串满足一定的规律
  + 正则表达式(Regular Expression)
    + 规则表达式，计算机科学的一个基础概念
    + 用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”
    + ^[A-Za-z]+$，代表着一个字符串，只能由26英文字母组成
    + 作用：
      + 测试字符串内的模式
      + 识别/替换文本
      + 提取文本
  + 正则表达式独立于特定语言(Java, Perl, Python,PHP…)
  + 正则表达式的匹配模板
    + 定界符
    + 原子
    + 特殊功能字符(元字符)
    + 模式修正符
+ Java的正则表达式
  + java.util.regex包
    - Pattern正则表达式的编译表示
     - compile编译一个正则表达式为Pattern对象
    - matcher 用Pattern对象匹配一个字符串，返回匹配结果
    - Matcher
     - Index Methods(位置方法) // start(), start(int group), end), end(int group) Study
    - Methods(查找方法) // lookingAt(, find(, find(int start), matches(Replacement
    - Methods(替换方法) //replaceAll(String replacement)

+ 其他字符串操作
  + 字符串和集合互转
    + [1,2,3]，“1,2,3”
  + 字符串转义
    + 对关键字符转义
  + 变量名字格式化
    + 名字驼峰命名
  + 字符串输入流
    + 将字符串转为一个输入流
    + 输入流可以定义为Scanner，这是Online Judge的实现原理

+ 总结
  + 灵活使用正则表达式
  + 多使用第三方库:Apache Commons Lang. Guava等
    + github.com
    - mvnrepository.com
    - www.open-open.com
    - ......
## 第十一章：

### 作业：

1.统计单词：

代码：

```java
import java.io.*;
import java.util.*;

class Operate {
    public static void main(String[] args) throws IOException {
        HashMap<String, Integer> map = new HashMap<>();
        String line;

        BufferedReader br = new BufferedReader(new FileReader("C:\\a.txt"));
        while ((line = br.readLine()) != null) {
            String[] words = line.split(" ");
            for (String str : words) {
                if(str.equals("==============")){
                    continue;
                }
                map.put(str, map.getOrDefault(str, 0) + 1);
            }
            map.remove("");
        }

        ArrayList<Record> records = new ArrayList<>();
        for (Map.Entry<String, Integer> e : map.entrySet()) {
            records.add(new Record(e));
        }

        Collections.sort(records);

        BufferedWriter bw = new BufferedWriter(new FileWriter("C:\\b.txt"));
        bw.write("============\n");
        for (Record record : records) {
            bw.write(record.toString());
            bw.newLine();
        }
        bw.write("============");

        bw.flush();
        bw.close();
    }
}

class Count {
    private String word;
    private int cnt;

    public int getCnt() {
        return cnt;
    }

    public void setCnt(int cnt) {
        this.cnt = cnt;
    }

    public String getWord() {
        return word;
    }

    public void setWord(String word) {
        this.word = word;
    }

    @Override
    public String toString() {
        return getWord() + "," + getCnt();
    }
}

class Record extends Count implements Comparable<Count> {

    public Record(Map.Entry<String, Integer> e) {
        setWord(e.getKey());
        setCnt(e.getValue());
    }

    @Override
    public int compareTo(Count c) {
        if (c.getCnt() > getCnt()) {
            return 1;
        } else {
            return -1;
        }
    }
}
```

截图：

b.txt的内容：

![](data:image/png;base64,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)

### 笔记：

#### 文件系统及java文件基本操作：

+ 文件概述
  + 文件系统是由OS（操作系统）管理的
  + 文件系统和Java进程是平行的，是两套系统
  + 文件系统是由文件夹和文件递归组合而成
  + 文件目录分隔符
    + Linux / Unix用 / 隔开
    + Windows用 \ 隔开，涉及到转义，在程序中需用 / 或 \\ \ 代替
    + 文件包括文件里面的内容和文件基本属性
    + 文件基本属性：名称、大小、扩展名、修改时间等

+ tips：

  + 常见的字符转义
    + \n等价于换行；
    + \r等价于回车；
    + \t等价于tab键

  + 安装软件时，安装路径最好是没有空格和中文，以免引起安装失败。

+ Java文件类File

  + java.io.File是文件和目录的重要类（JDK6及以前是唯一）
    + 目录也使用File类进行表示

  + File类与OS无关，但会受到OS的权限限制
  + 常用方法
    + createNewFile，delete，exists，getAbsolutePath，getName，getParent，getPath，isDirectory，isFile，length，listFiles，mkdir，mkdirs

  + 注意：File不涉及到具体的文件内容，只涉及属性

+ Java NIO（NI0包，全名为Non-Blocking I/O，俗称New IO。）
  + Java 7提出的NIO包，提出新的文件系统类
    + Path，Files，DirectoryStrcam，FileVisitor，FileSystem
    + 是java.io.File的有益补充
      + 文件复制和移动
      + 文件相对路径
      + 递归遍历目录
      + 递归删除目录
      + ......




#### java IO包概述：

+ 介绍：
  + 文件系统和Java是两套系统
  + Java读写文件，只能以(数据)流的形式进行读写
  + java.io包中
    + 节点类：直接对文件进行读写
    + 包装类
      + 转化类：字节 / 字符 / 数据类型的转化类
      + 装饰类：装饰节点类

+ 基础概念：
  + 字节：byte，8bit，最基础的存储单位
  + 字符：a，10000，我
  + 数据类型：3，5.25，abcdef
  + 文件是以字节保存，因此程序将变量保存到文件需要转化

+ 类介绍：
  + 节点类：直接操作文件类
    - InputStream，OutputStream（字节）
    + FileInputStream，FileOutputStream
    - Reader，Writer（字符）
    + FileReader，FileWriter

  + 转换类：字符到字节之间的转化
    + InputStreamReader：文件读取时字节，转化为Java能理解的字
    + OutputStreamWriter：Java将字符转化为字节输入到文件中

  + 装饰类：装饰节点类
    + DataInputStream，DataOutputStream：封装数据流
    + BufferedInputStream，BufferOutputStream：缓存字节流
    + BufferedReader，BufferedWriter：缓存字符流




#### 文本文件读写：

+ 基本介绍：

  + 文件类型
    + 一般文本文件（若干行字符构成的文件），如txt等
    + 一般二进制文件，如数据文件dat
    + 带特殊格式的文本文件，如xml等
    + 带特殊格式二进制文件，如doc，ppt等

  + 文件是数据的一个容器（口袋）
  + 文件可以存放大量的数据
  + 文件很大，注定Java只能以流形式依次处理
  + 从Java角度理解
    + 输出：数据从Java到文件中，写操作
    + 输入：数据从文件到Java中，读操作

  + 文本文件读写
    + 输出文本字符到文件中
    + 从文件中读取文本字符串

+ 操作：

  + 写文件

    + 先创建文件，写入数据，关闭文件
    + FileOutputStream，OutputStreamWriter，BufferedWriter
    + BufferWriter
      + write
      + newLine

    - try-resource语句，自动关闭资源
    - 关闭最外层的数据流，将会把其上所有的数据流关闭

  + 读文件

    + 先打开文件，逐行读入数据，关闭文件
    + FileInputStream，InputStreamWriter，BufferedReader
    + BufferReader
      + readLine

    - try-rcsourcc语句，自动关闭资源
    - 关闭最外层的数据流，将会把其上所有的数据流关闭




#### 二进制文件读写：

+ 基础介绍：

  + 二进制文件
    + 狭义上说，采用字节编码，非字符编码的文件
    + 广义上说，一切文件都是二进制文件
    + 用记事本等无法打开 / 阅读

  + 二进制文件读写
    + 输出数据到文件中
    + 从文件中读取数据

+ 操作：

  + 写文件

    + 先创建文件，写入数据，关闭文件

    - FileOutputStream，BufferedOutputStream，DataOutputStream
    - DataOutputStream
      + flush
      + write / writeBoolean / writeByte / writeChars / writeDouble / writeInt / WriteUTF / …

    + try-resource语句，自动关闭资源
    + 关闭最外层的数据流，将会把其上所有的数据流关闭

  + 读文件

    + 先打开文件，读入数据，关闭文件

    - FileInputStream，BufferedInputStream，DataInputStream
    - DataInputStream
    + read / readBoolean / readChar / readDouble / readFloat / readInt / readUTF / …

    + try-resource语句，自动关闭资源
    + 关闭最外层的数据流，将会把其上所有的数据流关闭




#### Zip文件读写：

+ 基础介绍：

  + 压缩包：zip，rar，gz……

  + Java zip包支持Zip和Gzip包的压缩和解压

  + zip文件操作类：java.util.zip包中

    + java.io.InputStream，java.io.OutputStream的子类

    - ZipInputStream，ZipOutputSream压缩文件输入 / 输出流
    - ZipEntry压缩项

+ 压缩
  + 单个 / 多个压缩
    + 打开输出zip文件
    + 添加一个ZipEntry
    + 打开一个输入文件，读数据，向ZipEntry写数据，关闭输入文件
    + 重复以上两个步骤，写入多个文件到zip文件中
    + 关闭zip文件

+ 解压
  + 单个 / 多个解压
    + 打开输入的zip文件-获取下一个ZipEntry
    + 新建一个目标文件，从ZipEntry读取数据，向目标文件写入数据，关闭目标文件
    + 重复以上两个步骤，从zip包中读取数据到多个目标文件-关闭zip文件

+ 总结
  + Java支持Zip和Gzip文件解压缩
  + 重点在Entry和输入输出流向，无需关注压缩算法


---
title: Java-IO
date: 2022-05-05 09:09:12
tags: ["java","Java-IO"]
categories: ["java"]
---
* File类：它的一个对象，代表一个文件或文件夹。
  * 相对路径：相较于某个路径下的路径。
    * IDEA中：用JUnit中的单元测试，相对路径即为当前Module下。用Main()方法，相对路径即为当前project下
    * Eclipse中：单元测试方法和Main()方法，相对路径都为当前project下
  * 绝对路径：包含盘符在内的文件或文件目录的路径
  * File.separator 动态分隔符
  * renameTo(File dest) 把文件重命名为指定的文件路径，且dest不能在硬盘中存在

* IO：inputstream/outputstream        **gbk 中文占2个字节 utf-8 中文占3个字节**

  * 按操作数据单位分：字节流（8bit）适合非文本文件（.jpg, .mp3, .mp4, .doc, .ppt, .pdf）。对于文本文件，只要不在内存中显示，是不会出现乱码的。如：文本文件的复制

    ​                                   字符流（16bit）适合文本文件（.txt, .java, .c）。不能用字符流操作非文本文件

  * 按流的角色分：节点流：直接作用在文件上；处理流：包在已有的流的基础上

  * 字节流抽象类：inputstream/outputstream      **使用byte[]数组**

  * 字符流抽象类：Reader/Writer     **使用char[]数组**

![](D:\笔记文件\picture\流的体系结构.PNG)

* FileReader：
  
* read()返回读入的一个字符。如果到达文件末尾，返回-1。使用finally来关闭流
  
* FileWriter：
  * new FileWriter(file)/new FileWriter(file,false) 对原有文件覆盖式写入
  * new FileWriter(file,true) 不覆盖原有文件

* 缓冲流：在内部提供了一个缓冲区，提高了流的读取、写入速度
  * 关闭流时，先关缓冲流，再关文件流。说明：在关闭外层流（缓冲流）时，内层流（文件流）会自动关闭。
  * BufferedReader 的readLine()方法返回值不包含换行符。可使用BufferedWriter 的newLine()方法
  * DEFAULT_BUFFER_SIZE=8192=8x1024=8kb        默认缓冲区的大小8KB

* 转换流：用于字节流与字符流之间的转换。**属于字符流**。

  * InputStreamReader        将字节输入流转换为字符输入流
  * OutputStreamWriter       将字符输出流转换为字节输出流

* 解码：字节、字节数组---->字符数组、字符串

* 编码：字符数组、字符串---->字节、字节数组

  ```java 
  File file1 = new File("hello.txt");
  File file2 = new File("hello-1.txt");

  FileInputStream fis = new FileInputStream(file1);
  FileOutputStream fos = new FileOutputStream(file2);

  //hello.txt文件保存时使用什么字符集，此处就使用什么字符集
  InputStreamReader isr = new InputStreamReader(fis,"utf-8");
  //此处使用的字符集没有限制
  OutputStreamWriter osw = new OutputStreamWriter(fos,"gbk");
  char[] chars = new char[10];
  int len;
  while((len=isr.read(chars)!=-1))
      ows.writer(chars,0,len);
  isr.close();
  ows.close();
  ```

  

* 操作步骤：1.File类的实例化 2.流的实例化 3.读入/写出的操作 4.资源的关闭

* 标准输入流：System.in 字节流

* 标准输出流：System.out

  * System类的setIn(InputStream is) / setOut(PrintStream ps) 方法重新指定输入和输出的流

* 打印流：PrintStream、PrintWriter。

  * 提供了一系列重载的print()和println()方法，用于多种数据类型的输出
  * System.out返回的是PrintStream的实例

* 数据流：DataInputStream和DataOutputStream用于读取或写入基本数据类型和String类型。例如：将这些类型的数据写入文件

```java
DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.txt"));

dos.writeUTF("zhangsan");
dos.flush();//刷新时数据写入文件，不使用文件也能正常写入
dos.writeInt(20);
dos.flush();

dos.close();
```

```java
DataInputStream dis = new DataInputStream(new FileInputStream("data.txt"));
//需按写入的顺序读取
String a=dos.readUTF();
int b=dos.readInt();

dos.close();
```

* 序列化机制：将Java对象转化成二进制数据
* 对象流：用于存储基本数据类型和对象的处理流。不能序列化static和transient修饰的成员变量
  * 序列化：将内存中的Java对象保存在磁盘中或者通过网络传输出去，使用ObjectOutputStream
  * 反序列化：将磁盘文件中的对象还原为内存中的Java对象，使用ObjectInputStream

```java
ObjectOutputStream oos = ObjectOutputStream(new FileOutputStream("data.dat"));

oos.writeObject(new String("Hello Word"));
oos.flush();
//User类首先需要实现Serializable接口，其次需要定义static final long serialVersionUID
//如果不定义serialVersionUID这个常量，当我们修改了User类，没有重新进行序列化操作，而进行反序列化操作时会发生错误
oos.writeObject(new User("zahngsan",21));
oos.flush();

oos.close();
```

```java
ObjectInputStream ois = ObjectInputStream(new FileInputStream("data.dat"));

Object obj = ois.readObject();
User user = (User)ois.readObject();
System.out.println((String)obj);
System.out.println(user);
ois.close();
```

* 随机存取文件流：RandomAccessFile既可以作为一个输入流，也可以作为一个输出流。
  * 如果RandomAccessFile作为输出流，对已经存在的文件，则会对**文件的内容**进行覆盖。[默认从头开始覆盖]
  * seek(int n)方法 将指针调到n位置。
    * seek(new File(hello.txt).length())    //实现文本内容追加

```java
RandomAccessFile raf1 = new RandomAccessFile(new File("a.jpg","r"));//只读
RandomAccessFile raf2 = new RandomAccessFile(new File("a1.jpg","rw"));

byte[] bytes = new byte[1024];
int len;
while((len = raf1.read(bytes))!=-1)
    raf2.write(bytes,0,len);
raf1.close();
raf2.close();
```

* 字节数组流：ByteArrayInputStream：把流中的内容写到一个字节数组中

  ​                       ByteArrayOutputStream：把数据写入字节数组流中。


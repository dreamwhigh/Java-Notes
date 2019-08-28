## 慕课网 JAVA IO 学习笔记

### 1.1 文件编码

gbk 编码：中文占用 2 个字节，英文占用 1 个字节；

utf-8 ：中文占用 3 个字节，英文占用 1 个字节

Java 采用双字节编码 utf-16be 

utf-16be ：中文占用 2 个字节，英文占用 2 个字节

字节序列是某种编码时，将字节序列转换为字符串时，需要指定对应的编码方式，否则会出现乱码。

### 2.1 File 类常用 API

java.io.File 类只用于表示文件和目录的信息，但是它不表示文件的内容。

常用 API

列出目录下所有文件

### 3.1 RandomAccessFile

支持随机访问文件，可以访问文件的任意位置；

提供对文件内容的访问，包括读和写文件操作。

Java 文件模型：硬盘上的文件是 byte 存储的，是数据的集合

- 打开文件

两种模式：`rw` 读写、`r` 只读

存在一个文件指针，打开文件时指针在开头 point = 0

```java
RandomAccessFile raf = new RandomAccessFile(file,"rw")
```

- 写方法，只写一个字节（后八位），之后指针指向下一个位置

```java
raf.write(int)
```

- 读文件

```java
int b = raf.read();
```

- 关闭

```java
raf.close();
```

### 字节流

#### 文件输入流 InputStream

抽象了应用程序读取数据的方式

##### 流末尾

EOF 表示 End；读到 -1 即读到 EOF（文件尾）

##### 读取

```java
//in 为一个输入流对象
int b = in.read()//读取一个字节无符号填充到 int 的低八位
in.read(byte[] buf)//读取数据填充到一个字节数组
in.read(byte[] buf,int start,int length)//读取数据到 buf 数组从 start 开始到后面 length 长度的位置
```



##### 写入

```java
//out 为一个输出流对象
out.write(int b)//写出 b 的低八位到输出流
out.write(byte[] buf)
out.write(byte[] buf,int start,int length)//字节数组 buf 从start 位置开始写 length 长度的字节到输出流
```



#### FileInputStream

具体实现了在文件上读取数据
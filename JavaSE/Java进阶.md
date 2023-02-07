# Java进阶

此处记录Java进阶中的零碎知识

## IO流

IO指的是:输入输出

### 流的分类

#### 字节流

以byte为单位读,能读写所有文件

字符流

以字符为单位读,只能读写纯文本文件

### Java中的IO流

- 字节流

1. java.io.**InputStream** 字节输入流
2. java.io.**OutputStream** 字节输出流

- 字符流

1. java.io.**Reader** 字符输入流
2. java.io.**Writer** 字符输出流

所有流有close()方法,使用完后记得关闭

所有输出流都有flush()方法,清空管道残留数据,不使用可能会导致数据丢失.

## Stream

用于提升容器collection使用的效率

### 流的获取

- collections.stream()借助集合类的stream()方法

- 通过具体的集合获取,例如

  ```java
  List<Integer> list=new ArrayList();
  list.add(1);
  Stream<Integer> stream=list.stream();
  ```

注意Map可以使用Entry对象获取流

### 流的操作

非终结方法

- filter()筛选
- mapper()映射
- concat()把两个流合并成一个流 //Stream的静态方法
- skip()/limit()跳过/截取前几个元素

终结方法

- count()计数
- foreach()迭代

### 流的收集

使用Stream.collect()方法把流收集到容器中.


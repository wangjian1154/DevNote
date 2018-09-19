# 热门Java面试题

### List、Set、Map的区别
+ List、Set都实现了Collection接口。Map不是Collection的子类或接口的实现类，他就是一个接口
+ List:
  + 1.可以有重复的对象
  + 2.可以插入多个null的元素
  + 3.它是一个有序的容器，存储了每个元素的插入顺序，输出的顺序就是插入顺序
  + 4.List接口常见的实现类有ArrayList、LinkedList和Vector。ArrayList使用最多，它提供了使用索引随意访问;而LinkedList则对于经常需要添加、删除元素的场合更合适。  
+ Set:
 + 1.不允许重复的对象
 + 2.无序容器，无法保证每个元素的存储顺序。TreeSet通过Comparator或者Comparable维护了一个排序顺序
 + 3.只允许有一个Null的元素
 + 4.Set接口最流行的几个实现类是HashSet、LinkedHashSet以及TreeSet。HashSet是基于HashMap的，TreeSet实现了SortedSet接口，所以TreeSet是一个根据compare和compareTo进行定义的有序容器
+ Map:
  + 1.Map是一个接口，它不是一个Collection的子类或者接口的实现类
  + 2.Map的每个Entry都持有两个对象，也就是一个键和一个值。Map的键必须唯一，值可以是相同或不同的
  + 3.TreeMap也通过Comparator或者Comparable维护了一个排序顺序
  + 4.Map里可以有任意个Null值，但是只能有一个Null键
  + 4.Map里可以有任意个Null值，但是只能有一个Null键
  + 5.Map最流行的几个实现类是HashMap、LinkedHashMap、HashTable和TreeMap。常用的是HashMap和TreeMap

### HashSet是如何保证不重复的
+ HashSet是哈希表结构，当一个元素要存入HashSet集合时，首先通过自身的hashCode算出一个值，通过这个值查找元素在集合中的位置，如果没有就存入。如果有，就需要调用equals方法进行比较，如果equals返回为真，证明这两个元素相等，就不存

### 强引用 、软引用、 弱引用、虚引用
+ 强引用:
  + 只要引用存在，垃圾回收器永不回收
  ```java
  String str = "abc";
  List<String> list = new Arraylist<String>();
  list.add(str);
  ```
  在list集合里的数据不会释放，即使内存不足也不会
+ 软引用:
  + 非必须引用，内存溢出之前进行回收
  ```java
  Object obj = new Object();
  SoftReference<Object> sf = new SoftReference<Object>(obj);
  obj = null;
  sf.get();//有时候会返回null
  ```
+ 弱引用  
  + 如果一个对象具有弱引用，在GC线程扫描内存区域的过程中，不管当前内存足够与否，都会回收内存
  ```java
  //弱引用  
  WeakReference<User>weakReference=new WeakReference<User>(new User());  
  ```
+ 虚引用
  + 如果一个对象仅持有虚引用，在任何时候都有可能会被回收。虚引用于弱应用、软引用的区别在于:虚引用必须和引用队列联合使用。虚引用主要用来跟踪对象，被垃圾回收的活动。
  ```java
  //虚引用  
  PhantomReference<User> phantomReference=new PhantomReference<User>(new User(),new ReferenceQueue<User>());  
  ```

### int和Integer的区别  
+ int的包装类是Integer，字面量在-128到127之间，Integer不会new对象，比较的是数值，所以下面打印的是ture和false
  ```java
    Integer a = 100, b = 100, c = 128, d = 128;

    System.out.println(a==b);//ture
    System.out.println(c==d);//false
  ```

### String、StringBuilder和StringBuffer的区别
+ String类中使用字符数组保存字符串，别去使用了final修饰符，所以String是不可变的只读字符串
+ StringBuffer中很多方法都带有synchronized关键字，所以它是线程安全的，执行效率比StringBuilder要低一些
String适用于少量字符串操作的情况
StringBuilder适用于单线程下载字符缓冲区进行大量操作的情况
StringBuffer使用多线程下在字符缓冲区进行大量操作的情况

### short s1 = 1; s1 = s1 + 1;有错吗？short s1 = 1,s1 += 1;有错吗？
+ 第一个s1+1,因为s1是short类型,1是int类型不能使用short类型接收
+ 第二个+=是Java中的运算符，内部进行了强制类型转换，所以正确

### Http和Https的区别
+ Https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用
+ Http是超文本传输协议，信息是明文传输，Https这是具有安全性的ssl加密传输协议
+ Http默认端口是80，Https默认端口是443
+ http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

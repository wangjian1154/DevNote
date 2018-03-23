## 热门Java面试题

#### List、Set、Map的区别
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

#### HashSet是如何保证不重复的
+ HashSet是哈希表结构，当一个元素要存入HashSet集合时，首先通过自身的hashCode算出一个值，通过这个值查找元素在集合中的位置，如果没有就存入。如果有，就需要调用equals方法进行比较，如果equals返回为真，证明这两个元素相等，就不存

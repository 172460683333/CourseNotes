# Collections

#### List

List集合是一个元素有序（每个元素都有对应的顺序索引，第一个元素索引为0）、且可重复的集合。

##### **ArrayList**

- 操作时间复杂度：查询，修改O(1)，在末尾添加元素，不扩容O(1)，扩容时间取决于system.arraycopy()，最坏为O(n)，删除指定元素O(n)。 

- 扩容机制：ArrayList包含了两个重要的对象：`elementData`和 `size`，elementData是一个Object[], 用于储存添加到ArrayList中的元素，size是elementData的容量，默认为10，可以在创建时赋值。如果容量超出，ArrayList会创建一个长度为size*1.5+1的新数组，并使用system.arrayCopy（）将原数组复制到新数组中。

- 初始容量默认10

- 线程不安全，多线程中使用copyOnWriteArrayList。


##### Linkedlist

- 操作时间复杂度：查询，修改非首尾O(n)，在末尾增删元素O(1)，在指定位置增删元素O(n)

- 线程不安全



**ArrayList 和 LinkList的区别**

- ArrayList 内部使用的动态数组来存储元素，LinkedList 内部使用的双向链表来存储元素，这也是 ArrayList 和 LinkedList 最本质的区别。
- Linklist需要储存节点的引用，消耗的内存更多。
- 插入，删除操作较多时使用Linklist，查询，修改操作较多时使用ArrayList，linkedlist同时也是queue的实现类，经常使用linkedlist作为队列使用。


#### Queue

Queue是java中实现队列的接口，Queue的实现类有LinkedList,ArrayDeque,PriorityQueue。

##### ArrayDeque

ArrayDeque是基于动态数组实现的双端队列

ArrayDeque不是线程安全的。 
ArrayDeque不可以存取null元素，因为系统根据某个位置是否为null来判断元素的存在。 
当作为栈使用时，性能比Stack好；当作为队列使用时，性能比LinkedList好。 

##### PriorityQueue

PriorityQueue是基于堆实现的优先队列，可自定义比较器，增删改查的时间复杂度都是O(log(n))

#### Map

##### **HashMap**

- HashMap的继承自实现java.util.Map的abstractMap。
- HashMap通过数组+链表+红黑树（jdk1.8，链表长度>8时转化为红黑树）实现，哈希表中有一个哈希桶数组，数组中储存的是node对象，node向外延伸形成链表或者红黑树。
- key可以为null但是不能重复，value值可以为null，也可以重复。
- HashMap效率高的原因在于它查询键值对位置的哈希算法和扩容机制，比如查询一个键值对的位置，首先会调用key的hasHashCode（）方法来获取key的哈希码，再通过哈希算法计算得到key在哈希桶数组中的索引，然后再到索引对应的链表或者红黑树中找到键值对。哈希算法的是高位运算+取模运算，前者将哈希码的高十六位与自身做异或操作，后者将高位运算的结果对哈希桶的长度取模。
- **链表在长度大于8时会尝试扩容,如果数组长度小于64则扩容,否则将链表转化为红黑树, 长度小于6时转回链表**
- hashMap开销大的地方在于扩容，每次哈希表中容纳的键值对超过哈希桶数组的长度乘以负载因子（默认0.75）时就会触发扩容，新建一个容量为原来两倍大小数组。
- jdk7以前初始容量是16,jdk8改为初始0,第一次put时扩容为16
- 线程不安全，在多线程中要使用concurrentHashmap。

**链表长度为什么超过8要转为红黑树？**

- 默认的是链表结构，并不是一开始就是红黑树结构，因为链表比红黑数占用的空间较少；
- hash冲突导致链表长度真正达到8的概率极小，约为一千万分之一，同时也考虑到红黑树查询比链表快；

**哈希内部解决冲突的方法有哪些**

1. 开放定址法，存在冲突时再找一个空位，具体方法有线性探测法（h(x)=(Hash(x) +i)mod (Hashtable.length)，索引加一探测新位置是否被占，取模是为了防止越界），平方探测法
2. 再哈希法，换一个哈希函数再算一次
3. 链地址法，把冲突元素存到链表中（即hashMap的处理方式）

**concurrentHashmap**

- 底层数据结构和解决hash冲突和HashMap相同,但是不允许空值和空键，否则会抛出异常；
- 实现线程安全的原理是划分多个segment数据段，每一个数据段都相当于一个hashMap，且都加了独立的ReentrantLock，所以在并发操作时每个数据段互不影响，jdk8把给segment加锁换成了对node节点加锁,降低了锁的粒度,提升了效率.
- 并发扩容,concurrentHashmap利用多个线程对原始数组进行分片,每个线程负责各自的数据迁移
- Size()方法的并发优化,如果线程竞争不激烈,使用CAS进行数据累加,否则维护一个数组来更新元素个数.

#### Set

储存元素唯一,无序的集合

##### **HashSet**

本质上是一个hashmap,只不过所有key的value都是同一个object

##### TreeSet

基于红黑树的集合,可以对储存的元素进行排序

##### LinkHashSet

会保存元素插入的顺序

#### Hashtable

- 底层数据结构为数组+链表
- key和value都不能为null
- 初始容量默认11,扩容时新长度为原来的两倍+1
- 所有方法均有synchronized修饰,线程安全但效率低

#### 面试题

##### ArrayList怎么加锁√8

- 使用Collections.SynchronizedList
- 使用CopyOnWriteArrayList
- 对add()方法加锁
- *使用ThreadLocal变量,保证线程封闭性(将ArrayList放到线程本地操作)

##### HashMap,HashTable,HashSet的区别

实现接口:HashMaps实现Map接口,HashTable实现Map接口和Dictionary抽象类,HashSet实现Set接口

先比较HashMap和HashTable

- HashMap和HashTable都是基于数组+链表实现的,储存的元素也都是key-value键值对
- 两者的hash算法不同,HashTable直接使用hashcode对数组长度取模,HashMap则是先对hashcode进行了高位运算再对数组长度取模
- HashMap的默认初始容量是16,扩容容量为原来的两倍,HashTable默认初始容量11,扩容容量为原来的两倍加1
- HashMap线程不安全,效率高,HashTable相反

再来看HashSet,它实际上是一个HashMap,所有的Value都是同一个Object,dddd.
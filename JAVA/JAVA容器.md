# 容器基础
## JAVA容器有哪些
Java中的容器(Containers)指的是用来存储和操作数据的类或接口。Java容器主要包含在java.util 包中，它们提供了丰富的方法来管理和操作数据集合。Java容器通常分为两大类:==集合(Collections)和映射(Maps)==
![[Pasted image 20260518160251.png]]
## 集合(Collections)
![[Pasted image 20260518160400.png]]
### List(列表):有序可重复
![[Pasted image 20260518160540.png]]
元素有明确的顺序(插入顺序)，允许存储重复元素，可通过索引访问元素。
#### ArrayList
![[Pasted image 20260518160747.png]]
- 底层是==动态数组(容量可自动扩容)==，默认初始容量为10，扩容时通常变为原来的1.5倍。
- 优势:随机访问效率高(get(intindex)时间复杂度为O(1))。
- 劣势:插入/删除元素(尤其是中间位置)效率低(需要移动大量元素，时间复杂度为0(n))。
- 适用场景:频繁读取数据、少量插入删除的场景(如数据查询列表)
#### LinkedList
![[Pasted image 20260518160912.png]]
- 底层是双向链表，每个节点存储元素和前后节点的引用。
- 优势:插入/删除元素(尤其是首尾位置)效率高(只需修改节点引用，时间复杂度为O(1))。
- 劣势:随机访问效率低(需从头/尾遍历，时间复杂度为O(n))。
- 适用场景:频繁插入删除、较少随机访问的场景(如实现队列、栈等数据结构
#### Vector
- 与ArrayList类似，但所有方法都被synchronized 修饰，因此是线程安全的。
- 劣势:线程安全带来额外开销，性能低于ArrayList，现代开发中较少使用(可通过Collections.synchronizedList() 替代)
#### Stack
- 继承自Vector，实现“后进先出(LIFO)"的栈结构，提供push()(入栈)、pop()(出栈)、peek()(查看栈顶)等方法。
- 注意:Java官方更推荐使用 Deque 的push()/pop()方法替代 Stack(因为Stack 继承 Vector 存在设计缺陷)
### Set(集):无序不重复
核心特点:元素无重复(依赖equals和hashcode方法判断唯一性)，默认不保证顺序(除LinkedHashSet)
![[Pasted image 20260518161351.png]]
#### HashSet
- 底层基于哈希表(HashMap)实现，元素存储在哈希表的桶中。
- 关键:元素需正确实现 hashCode()和equals()方法(否则可能导致重复元素或查找异常)。
- 特点:添加、删除、查询元素的平均时间复杂度为O(1)，但不保证迭代顺序(可能随扩容变化)。
- 适用场景:需要==快速去重、不关心顺序的场景==(如存储唯一标识集合)。
#### LinkedHashSet
- 继承自HashSet，底层在哈希表基础上增加了一条双向链表，记录元素的插入顺序。
- 特点:迭代时按插入顺序返回元素，性能略低于HashSet(维护链表有额外开销)。
- 适用场景:需要==去重且保留插入顺序的场景==(如日志记录的去重顺序)。
#### TreeSet
- 底层是红黑树(一种自==平衡二叉查找树==)，元素会按“自然顺序”或自定义比较器(Comparator)排序。
- 特点:元素有序(排序后的顺序)，添加、删除、查询的时间复杂度为O(logn)。
- 要求:元素必须可比较(要么实现comparable 接口，要么创建时传入Comparator)
- 适用场景:需要==对元素排序的场景==(如排行榜、范围查询)。
红黑树首先是**二叉查找树（BST）**，满足：
	   10
	   /     \
	8    20
	/  \     /  \
   3     15       30
左子树所有节点值 < 根节点值 < 右子树所有节点值
**中序遍历结果**：`3, 8, 10, 15, 20, 30`（天然升序）
### Queue(队列):先进先出(FIFO)
主要用于存储“等待处理”的元素，遵循先进先出原则(特例:优先级队列按优先级排序)
#### LinkedList作为队列
- 实现了 Queue 接口，通过add()/offer()(入队)、remove()/poll()(出队)、element()/peek()(查看队首)等方法操作。
- 无容量限制(理论上可无限添加元素，直到内存溢出)。
#### PriorityQueue
- 底层是优先级堆(默认是==小顶堆(父节点的值 ≤ 子节点的值==))，元素按优先级排序(自然顺序或比较器定义)。
- 特点:队首始终是优先级最高的元素，入队/出队时间复杂度为O(logn)。
- 注意:不允许插入null元素，且元素必须可比较(同TreeSet要求)。
- 适用场景:需要==按优先级处理任务的场景==(如任务调度器)。
### Deque(双端队列):两端均可操作
允许在队列的头部和尾部同时进行插入、删除操作，可同时作为“栈”(LIFO)和“队列”(FIFO)使用。
#### ArrayDeque
![[Pasted image 20260518172024.png]]
底层是==循环数组==，容量自动扩容(通常翻倍)。
- 优势:==比LinkedList更高效(数组结构减少节点引用开销)==，添加/删除首尾元素时间复杂度为O(1)。
- 劣势:不支持null元素。
- 适用场景:作为栈或双端队列的首选(替代 Stack和LinkedList作为队列)。
#### LinkedList作为双端队列
- 实现了Deque 接口，支持两端操作，但性能略低于ArrayDeque(链表节点维护开销)
- 允许插入null元素(但不推荐，可能导致操作异常)
### 框架的整体结构
Java容器框架的顶层接口主要有:
- Collection:所有单列集合的根接口(List、Set、Queue都继承自它)，定义了添加、删除、遍历等通用方法。
- Map:键值对集合的根接口(如HashMap、TreeMap等，未在上述分类中，因为是双列集合)，存储“键-值”映射，键唯一，值可重复。
此外，集合框架中还有一些工具类(如Collections)，提供排序、同步化、查找等静态方法，方便集合操作(例 Collections.sort(list) 可对 List排序)
掌握这些集合的特性，能在实际开发中根据场景选择最合适的实现类，提升程序性能和可读性。
## 映射(Maps)
![[Pasted image 20260518165009.png]]
![[Pasted image 20260518165021.png]]

映射提供了一个键值对的集合，Java映射主要包括以下几种类型:
1. ==HashMap==:基于哈希表实现。HashMap 使用哈希表来存储键值对，因此==插入、删除和查找==操作都非常快(平均O(1))。HashMap 不保证键值对的顺序，并且允许null键和null值。
2. ==LinkedHashMap==:基于哈希表和链表实现。LinkedHashMap 继承了 HashMap 的优点，并且通过链表==维护==键值对的==插入顺序或访问顺序==。
3. ==TreeMap==:基于红黑树实现。TreeMap 提供了==排序功能==，键值对按照键的自然顺序或通过比较器排序。插入、删除和查找操作的时间复杂度为O(logn)。
4. IdentityHashMap:基于哈希表实现，使用== 操作符而非.equals()方法来比较键。
5. WeakHashMap:基于哈希表实现，键是弱引用，允许垃圾回收器回收不再使用的键。
6. ==ConcurrentHashMap==:基于哈希表实现，并通过==分割锁技术实现线程安全==。ConcurrentHashMap 提供了==高并发下==的高性能读写操作。
## Java容器哪些是线程安全的?哪些是非线程安全的?
Java中的容器类可以根据其是否==支持多线程环境下的安全性==分为==线程安全==和==非线程安全==两类。线程安全意味着在==并发环境中==，容器==能够正确处理多个线程的访问==，==不会出现数据不一致的情况==。非线程安全则意味着在并发环境下，需要==采取额外的同步措施==来确保数据的一致性。
![[Pasted image 20260518180705.png]]
Java中线程安全的容器主要包括 Vector 、 HashTable 、 Collections.synchronizedList 、ConcurrentHashMap等，非线程安全的容器包括ArrayList、HashMap、HashSet等。
### 线程安全容器
1. Vector:所有方法通过synchronized修饰，线程安全但性能较低。
2. HashTable:基于synchronized实现，线程安全但性能较差。（全表加锁）
3. Collections.synchronizedList/Set/Map:将非线程安全集合包装为线程安全版本
4. ConcurrentHashMap:支持并发访问，性能优于HashTable。（分段锁）
### 非线程安全容器
1. ArrayList/LinkedList:未同步，多线程修改可能导致数据错误。
2. HashMap/HashSet:未同步，多线程读写可能导致数据污染。
3. StringBuilder:非线程安全，需使用StringBuffer(同步版本)
### 替代方案
- CopyOnWriteArrayList:写时复制，适合读多写少的场景。
- ConcurrentSkipListMap/ ConcurrentSkipListSet : 支持有序并发访问。
## JAVA容器key和value是否能为空？

| 集合类                   | Key         | Value       | 父类          | 说明              |
| --------------------- | ----------- | ----------- | ----------- | --------------- |
| **Hashtable**         | ❌ 不允许为 null | ❌ 不允许为 null | Dictionary  | 线程安全            |
| **ConcurrentHashMap** | ❌ 不允许为 null | ❌ 不允许为 null | AbstractMap | 锁分段技术（JDK8:CAS） |
| **TreeMap**           | ❌ 不允许为 null | ✅ 允许为 null  | AbstractMap | 线程不安全           |
| **HashMap**           | ✅ 允许为 null  | ✅ 允许为 null  | AbstractMap | 线程不安全           |
在Java中，不同的集合类对于null值的支持与否主要是==基于设计目的和使用场景==来考虑的。下面我将分别解释每个集合类对null值支持的规定及其背后的原因:
### Hashtable
- Key 和 Value:都不允许为 null
- 原因:Hashtable 是一个古老的集合类，它的设计目标是为了提供线程安全的操作，并且与早期的Java版本兼容。==不允许null作为键或值==的主要原因是==避免在多线程环境下出现不确定的行为。例如==，==如果允许null作为键，那么当多个线程尝试获取null键对应的值时，可能会导致混淆，因为无法确定哪个线程添加了该null键==。此外，Hashtable的设计倾向于严格性，不允许任何可能引起异常的情况发生。
### ConcurrentHashMap
- Key和Value:都不允许为null
- 原因:ConcurrentHashMap 是为了在高并发环境下提供高效的读写操作而设计的。它使用了更细粒度的锁定机制(如==锁分段==技术，在JDK8中进一步优化为CAS无锁算法)，这使得它在多线程环境下的性能优于Hashtable。==不允许 null作为键或值的原因与Hashtable类似==，主要是为了避免在高并发场景下可能出现的不确定性以及潜在的错误。
### TreeMap
- Key:不允许为 null;Value:允许为 null
- 原因:TreeMap 是基于红黑树实现的有序映射表。==不允许null作为键==是因为TreeMap需要通过键来进行排序，而==null无法与其他对象进行比较==，这会导致NullPointerException。然而，TreeMap允许null作为值，这是因为值并不影响元素的排序，因此不会引起任何问题。
### HashMap
- Key和Value:允许为null
- 原因:HashMap是最常用的非线程安全的映射表实现。它==允许null作为键或值==，主要是为了提高灵活性。例如，允许null作为键可以方便地表示“不存在“或“未知”的状态;允许null作为值则可以在某些场景下用于标记某些特定的状态。但是，需要注意的是，HashMap中==只能有一个键为null的条目，而值为null的条目则可以有多个==。
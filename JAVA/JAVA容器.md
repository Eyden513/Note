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
# List
## ArrayList和Array的区别
1. 动态vs固定大小
- Array:数组是固定大小的数据结构。一旦创建，其大小就无法改变。如果你需要增加或减少数组中的元素数量，你需要创建一个新的数组并将原有数组中的元素复制过去。
- ArrayList:ArrayList 是一个可动态调整大小的列表。它可以随着元素的增加或减少自动调整其容量，因此不需要手动管理大小。
2. 泛型支持
- Array:原生的数组不支持泛型，但是在Java 5之后，你可以使用泛型来指定数组中的元素类型，但这主要是编译时的检查。
- ArrayList:ArrayList支持泛型，可以指定列表中元素的类型，这样可以避免在运行时进行类型转换，并且提高了类型安全。
3. 方法和功能
- Array:数组本身没有提供很多方法来操作其内容。你需要手动实现添加、删除、搜索等功能。
- ArrayList:ArrayList 提供了大量的方法来操作列表，如 add()、remove()、indexof()、contains()等，使得操作列表变得更加方便。
4. 内存分配
- Array:数组在创建时分配一块连续的内存空间。
- ArrayList:ArrayList 使用一个动态数组来存储元素，当需要更多空间时，它会创建一个新的更大的数组并将旧数组中的元素复制过来。
## ArrayList 和 LinkedList 的区别
![[Pasted image 20260519161430.png]]
1. 底层数据结构
- ArrayList:基于动态数组实现。
- LinkedList:基于双向链表实现。
2. 随机访问
- ArrayList:支持快速的随机访问(0(1))，因为你可以通过索引直接访问元素。
- LinkedList:不支持快速的随机访问(O(n))，因为需要从头节点开始遍历到指定位置。
3. 插入和删除
- ArrayList:插入和删除操作相对较慢(O(n)，特别是在列表中间位置时，因为需要移动后续元素。
- LinkedList:插入和删除操作非常快(O(1))，因为你只需要改变前后节点的指针即可。
4. 内存占用
- ArrayList:内存占用相对较少，因为只需存储元素本身。
- LinkedList:内存占用较多，因为每个节点不仅存储元素，还需要存储前后节点的引用。
## ArrayList的扩容机制
![[Pasted image 20260519161617.png]]
ArrayList 在创建时会有一个初始容量。当添加的元素超过了当前容量时，ArrayList 会自动扩容。扩容的具体机制如下:
1. 初始容量:默认情况下，ArrayList 的初始容量为10。你可以通过构造函数指定一个初始容量。
2. 扩容策略:当ArrayList的大小超过其容量时，它会创建一个新的数组，并将旧数组中的所有元素复制到新数组中。新的数组的容量通常是旧容量的1.5倍(即增加50%)
3. 扩容过程
- ensureCapacityInternal 方法用于确保容量足够。
- grow方法负责实际的扩容操作。
- 新的数组会通过 Arrays.copyof 方法创建，并将==旧数组中的元素复制到新数组==中。0
- 最后，ArrayList ==的引用会指向新的数组==。
# HashSet、LinkedHashSet、TreeSet 的区别
## HashSet
- 底层数据结构:Hashset 是基于哈希表实现的。它使用哈希码来确定元素的位置，并通过哈希表来存储元素。Hashset 内部使用了一个HashMap来存储元素，其中元素作为键(key)，值为PRESENT(一个静态对象)。
- 特点:HashSet 不允许重复元素，并且不保证元素的任何特定顺序。
- 性能:插入、删除和查找操作的平均时间复杂度为0(1)。
- 应用场景:当需要==快速判断元素是否存在==，且不关心元素顺序时，可以选择使用 Hashset。
## LinkedHashSet
- 底层数据结构:LinkedHashset 是基于哈希表和双向链表实现的。它在 ==Hashset的基础上增加了双向链表==来维护元素的插入顺序。
- 特点:LinkedHashset 也不允许重复元素，但它保证了元素的插入顺序(即元素按照插入的顺序排列)
- 性能:插入、删除和查找操作的平均时间复杂度接近O(1)，但由于需要维护链表，性能略低于 Hashset
- 应用场景:当需要==保持元素的插入顺序，并且需要快速判断元素是否存在==时，可以选择使用LinkedHashSet
## TreeSet
- 底层数据结构:Treeset 是基于红黑树实现的。它提供了排序功能，并通过红黑树来存储元素。
- 特点:TreeSet也不允许重复元素，并且元素按照自然顺序或通过比较器排序。
- 性能:插入、删除和查找操作的时间复杂度为0(logn)。
- 应用场景:当==需要对元素进行排序，并且需要快速判断元素==是否存在时，可以选择使用Treeset

# Map相关
## HashMap 和HashSet的区别
![[Pasted image 20260519162133.png]]
1. 数据结构
- HashMap:HashMap是一个键值对映射容器，它使用哈希表来存储键值对。每个键值对由一个键(key)和一个值(value)组成。
- HashSet:Hashset是一个集合容器，它不允许重复元素，并且不保证元素的顺序。Hashset 内部使用了一个 HashMap来存储元素，其中元素作为键(key)，值为PRESENT。
2. 元素
- HashMap:HashMap 存储的是键值对，每个键必须是唯一的，但值可以重复。
- HashSet:HashSet存储的是单一元素，不允许重复元素。
3. 键值对vs单一元素
- HashMap:存储的是键值对，每个键值对由一个键和一个值组成。
- HashSet:存储的是单一元素，每个元素都是唯一的。
4. 存储方式
- HashMap:键值对存储在哈希表中，键用于哈希计算。
- HashSet:元素存储在哈希表中，元素自身用于哈希计算。
## HashMap 和 Hashtable 的区别
HashMap和Hashtable是Java中常用的哈希表实现，但两者之间有几个关键的区别:
- 线程安全性:Hashtable 是线程安全的，它内部的方法使用了synchronized 关键字来确保多线程环境下的安全性。而HashMap并没有提供同步机制，因此在多线程环境中使用时需要手动同步。
- 允许null键和null 值:HashMap 允许一个null键和多个nul1值,而Hashtable 不允许 null键或null值。
- 历史和过时的方法:Hashtable 出现在早期的JDK版本中，并且包含了一些过时的方法，如clone()和contains()。而HashMap 在JDK1.2中引入，没有这些过时的方法，使用起来更加现代。
- 性能:由于 Hashtable 的方法是同步的，使用synchronized加锁，因此在多线程环境中的性能较低。而HashMap 在单线程环境中性能更高。
## HashMap 和 ConcurrentHashMap 的区别
![[Pasted image 20260519162513.png]]
HashMap和ConcurrentHashMap 都是哈希表实现，但ConcurrentHashMap 是专门为高并发环境设计的:
- 线程安全性:HashMap 是非线程安全的，而ConcurrentHashMap 是线程安全的，它使用了分割锁技术来实现并发性能。
- 性能:在==单线程环境中，HashMap的性能较高，但是多线程环境下会出现并发问题==。但在==多线程环境中ConcurrentHashMap 由于其分块锁技术，能够提供更好的并发性能==。
- 内部实现:HashMap使用一个没有加锁，而ConcurrentHashMap将哈希表分成多个段(segments)，每个段有自己的锁，这样可以并发地对不同段进行操作。
- 方法:ConcurrentHashMap 提供了更多的并发操作方法，如 putIfAbsent，remove 等，这些方法在多线程环境中更有优势。
## HashMap如何解决哈希冲突的
![[Pasted image 20260519162807.png]]
HashMap通过链表来解决哈希冲突，当多个元素的哈希值相同，导致它们映射到同一个桶(bucket)时，HashMap会将这些元素放在一个链表中。每个桶就是一个链表的头部，而链表中的每个节点存储一个键值对。
## HashMap为什么线程不安全?如何实现线程安全?
HashMap是非线程安全的，主要原因在于它的扩容机制和内部操作==没有进行同步==。当==多个线程同时进行写操作==时，可能会出现以下问题:
- 并发修改异常:多个线程同时进行写操作时，可能会导致HashMap的内部结构变得不一致，引发ConcurrentModificationException.
- 数据不一致:如果多个线程同时修改HashMap，可能会导致数据不一致。
为了实现线程安全，可以采取以下几种方式:
- 使用 ==synchronized 关键字==:可以在方法上加synchronized 关键字，但这会影响性能。
- 使用 ==Collections.synchronizedMap== :可以将 HashMap 包装成一个线程安全的映射(对所有方法加同步锁)。
- 使用==ConcurrentHash==Map:ConcurrentHashMap 是专门为高并发环境设计的线程安全映射。
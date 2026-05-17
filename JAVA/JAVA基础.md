# JAVA语法

## JAVA和C++的区别
| 维度       | Java              | C++                      |
| :------- | :---------------- | :----------------------- |
| **内存管理** | 自动垃圾回收（GC）        | 手动管理（new/delete）或智能指针    |
| **跨平台**  | 依赖JVM（一次编写，到处运行）  | 需为不同平台单独编译，linux、windows |
| **性能**   | 有JVM开销，适合大多数场景    | 直接编译为机器码，性能更高            |
| **语法**   | 纯面向对象，单继承         | 支持多范式，多继承                |
| **应用场景** | 企业级开发、Android、大数据 | 系统软件、游戏、嵌入式系统            |
| **安全性**  | 严格检查（如数组越界）       | 需手动规避指针风险                |
## 成员变量和局部变量的区别
- 示例代码
```
class A{
    int a = 0; //成员变量
    int staticA = 0; //静态成员变量

    public void fun(int f){
        int b = 1; //局部变量
        int c= a + b;
    }

    public static void main(){
        A objA = new A();
        objA.fun(3);
    }

}
```
- 示例图
![[Pasted image 20260517145352.png]]
Java中的成员变量和局部变量在定义位置、作用域、生命周期和默认值等方面存在显著差异。以下是它们的主要区别及示例:

| 维度       | 成员变量（实例变量/类变量）                                        | 局部变量                   |
| :------- | :---------------------------------------------------- | :--------------------- |
| **定义位置** | 类中，方法、构造器或代码块外部                                       | 方法、构造器、代码块或参数列表中       |
| **作用域**  | 整个类，可被类中所有方法访问                                        | 仅限于定义它的方法、构造器或代码块      |
| **生命周期** | 实例变量随对象创建而存在，随对象销毁而消失<br>类变量（`static`）随类加载而存在，随类卸载而消失 | 方法/代码块执行时创建，执行结束后销毁    |
| **默认值**  | 有默认值（如 `int` 为 `0`，对象为 `null`）                        | **没有默认值**，必须显式初始化后使用   |
| **修饰符**  | 可使用 `public`、`private`、`static` 等修饰                   | 一般不使用访问修饰符（如 `public`） |
- 示例代码
```
public class VariableDemo {
    // 成员变量（实例变量）
    private int instanceVar;        // 无初始值，默认0
    private String instanceStr;     // 默认null

    // 成员变量（类变量，static）
    public static double classVar = 10.5;  // 有初始值

    // 方法func
    public func(int param) {
        // param 是局部变量（构造器参数）
        int localVar=0;
    }

}
```
## JAVA几种基本数据类型
![[Pasted image 20260517203800.png]]
### 基本类型
基本类型直接存储值，并且是内置的语言类型。Java的基本类型有以下几种:
- 整型
byte:8位有符号整数，范围从-128到127。
short:16位有符号整数，范围从-32768到32767。
int:32位有符号整数，范围从-2^31到2^31-1。
long:64位有符号整数，范围从-2^63到2^63-1。使用L或|作为后缀。
- 浮点类型
float:单精度32位IEEE754浮点数。
double:双精度64位IEEE754浮点数，默认的浮点数类型。
- 字符类型
char:16位Unicode字符。
- 布尔类型
boolean:只有两个可能的值，true和false。
### 引用类型
![[Pasted image 20260517204122.png]]
引用类型不是直接存储值，而是存储对对象的引用。这些类型包括:
- 类(Class):用户定义的对象类型。
- 接口(Interface):定义了一组行为规范，没有实现细节。
- 数组(Array):存储固定大小的同类型元素的有序集合。
- 枚举(Enum):一种特殊的类，用来枚举一组常量。
## 什么是自动装箱和自动拆箱
![[Pasted image 20260517204219.png]]
自动装箱(Auto-boxing)和自动拆箱(Auto-unboxing)是Java 5引入的特性，它们简化了基本数据类型和其对应的包装类之间的转换。
### (1)自动装箱(Auto-boxing)
自动装箱是指将基本数据类型自动转换为其对应的包装类对象的过程。这使得你可以直接将一个基本数据类型赋值给一个包装类类型的变量，而无需显式地使用包装类的构造函数或value0f方法。例如:
```
int a = 10;
Integer b=a;/自动装箱
```
在这个例子中，a是一个int 类型的变量，将其赋值给Integer类型的变量b 时，Java会自动调用Integer.valueof(a) 方法来创建一个 Integer 对象。
### (2)自动拆箱(Auto-unboxing)
自动拆箱则是相反的过程，即将包装类对象转换为基本数据类型。当你有一个包装类对象并且需要将其值作为一个基本类型使用时，自动拆箱会自动发生。例如:
```
Integer b=10;//自动装箱
int a=b;//自动拆箱
```
在这个例子中，b 是一个Integer对象，当将其赋值给int类型的变量a时，Java会自动调用b.intValue()方法来获取基本类型 int的值。
### (3)性能考虑
虽然自动装箱和自动拆箱让代码更加简洁，但在性能方面需要注意。包装类对象是在堆上分配的，因此频繁地创建包装类对象可能会导致更多的垃圾收集活动。对于大量的数据处理，应尽量使用基本数据类型以提高性能。此外，由于自动装箱创建的是对象，所以当涉及到null值时，包装类对象可以为null，而基本类型则不能。总的来说，==自动装箱和自动拆箱使得Java编程更加便捷，但在性能敏感的应用中需要谨慎使用。==
## JAVA中值传递和引用传递的区别
![[Pasted image 20260517204702.png]]在Java中，方法参数的传递实际上总是值传递(pass-by-value)，而不是引用传递(pass-by-reference)。这是因为Java中的方法参数总是接收实际参数的一个副本。具体来说，对于基本类型，传递的是值的副本;对于对象，传递的是引用的副本。下面详细解释这两者的区别:
### (1)基本类型(值传递)
当我们将基本类型(如int、float、char 等)作为参数传递给方法时，实际上是传递了这些值的副本，这意味着在方法内部对参数的任何更改都不会影响到原始值。
`public class ValuePassExample {`
	`public static void main(String[] args) {
			int x=10;`
			changeValue(x);`
			System.out.println("x的值是:+x);//输出“x的值是:10”
	   }`
	`public static void changeValue(int y) {`
			`y = 20;`
			`System.out.println("方法内部y 的值是:"+y);//输出"方法内部y的值是:20"`
	  }
在这个示例中，changevalue 方法尝试修改传入的int 类型参数y的值。但是，由于传递的是x的副本，所以在方法内部对y 的修改不会影响到main 方法中的x 的值。
### (2)对象(引用传递)
当我们传递对象作为参数时，实际上是传递了对象引用的副本,这意味着在方法内部可以修改对象的状态，但不能改变对象本身的引用。
```
public class ReferencePassExample {
    public static void main(String[] args) {
        MyClass obj = new MyClass(10);
        changeObject(obj);
        System.out.println("obj 的值是：" + obj.getValue()); // 输出 "obj 的值是：20"
    }

    public static void changeObject(MyClass obj) {
        obj.setValue(20);
        System.out.println("方法内部 obj 的值是：" + obj.getValue()); // 输出 "方法内部 obj 的值是：20"
    }
}
```
在这个示例中，changeobject 方法接收一个MyClass 类型的对象引用。虽然传递的是引用的副本，但是我们可以通过这个副本引用修改对象的状态(即value 的值)。因此，当changeobject 方法修改了对象的状态后，main 方法中的对象obj也会反映出这种变化。
### (3)总结
- 基本类型:传递的是值的副本，因此在方法内部对参数的修改不会影响到原始值。
- 对象:传递的是引用的副本，因此在方法内部可以修改对象的状态，但不能改变对象本身的引用。
需要注意的是，在Java中，对象本身并不是被传递的，而是传递了指向对象的引用。因此，如果你试图在方法内部重新为参数引用赋值一个新的对象，那么这个新的对象不会影响到原来的对象。修改引用示例:
```
public class ChangeReferenceExample {
    public static void main(String[] args) {
        MyClass obj = new MyClass(10);
        changeReference(obj);
        System.out.println("obj 的值是：" + obj.getValue()); // 输出 "obj 的值是：10"
    }

    public static void changeReference(MyClass obj) {
        obj = new MyClass(20); // 改变局部变量 obj 的引用，不影响外部的 obj
        System.out.println("方法内部 obj 的值是：" + obj.getValue()); // 输出 "方法内部 obj 的值是：20"
    }
}

class MyClass {
    private int value;

    public MyClass(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```
在这个示例中，尽管changeReference方法内部改变了 obj的引用，但这并没有影响到main方法中的obj的值，因为它只是改变了方法内部的局部变量obj的引用。
## 深拷贝和浅拷贝的区别
![[Pasted image 20260517211952.png]]
### (1) 浅拷贝(Shallow Copy)
浅拷贝是指创建一个新的对象，并将==原对象的引用类型成员变量直接赋值给新对象==。这意味着新对象的引用类型成员变量仍然指向原对象的成员变量所指向的对象。因此，对新对象的引用类型成员变量所做的任何改变都会影响到原对象。
### (2)深拷贝(Deep Copy)
深拷贝是指创建一个新的对象，并且==递归地复制原对象的所有成员变量==(包括引用类型成员变量)。这意味着新对象的引用类型成员变量指向的是原对象成员变量所指向对象的一个全新的拷贝。因此，对新对象的引用类型成员变量所做的任何改变都不会影响到原对象。
### (3)浅拷贝示例
```
import java.util.Arrays;

class ShallowCopyExample {
    public static void main(String[] args) {
        // 创建原始对象
        MyClass original = new MyClass(new int[]{1, 2, 3});

        // 浅拷贝
        MyClass shallowCopy = original;

        // 修改拷贝对象
        shallowCopy.array[0] = 100;

        System.out.println("Original: " + Arrays.toString(original.array));
        System.out.println("Shallow copy: " + Arrays.toString(shallowCopy.array));
    }
}

class MyClass {
    int[] array;

    public MyClass(int[] array) {
        this.array = array;
    }
}
```
### (4)深拷贝示例
```
import java.util.Arrays;

class DeepCopyExample {
    public static void main(String[] args) {
        // 创建原始对象
        MyClass original = new MyClass(new int[]{1, 2, 3});

        // 深拷贝
        MyClass deepCopy = new MyClass(Arrays.copyOf(original.array, original.array.length));

        // 修改拷贝对象
        deepCopy.array[0] = 100;

        System.out.println("Original: " + Arrays.toString(original.array));
        System.out.println("Deep copy: " + Arrays.toString(deepCopy.array));
    }
}

class MyClass {
    int[] array;

    public MyClass(int[] array) {
        this.array = array;
    }
}
```
通过以上示例可以看出，浅拷贝只是简单地复制了对象的引用，而深拷贝则是创建了一个完全独立的新对象。
# JAVA对象
## = =和equals的区别
```
class A {
    int value = 0;
}

class B {
    int value = 1;
}

void main() {
    A objA = new A();
    B objB = new B();
    if (objA == objB) {
        // 比对的内存地址
    }

    if (objA.equal(objB)) {
        // 对比内容
    }
}
```
= =和equals()都是用来比较两个对象之间的相等性，但它们有不同的用途和行为:
- = =:这是一个运算符，用于比较两个对象的引用==是否指向同一个内存地址==。换句话说，检查的是两个对象是否是同一个对象。==对于基本类型，== ==比较的是它们的值是否相等==。
- equals():这是一个方法，定义在 object 类中，==用于比较两个对象的内容是否相等==。默认情况下，Object 类中的equals()方法实际上也是使用== 运算符来比较对象的引用。但是，很多类(如Integer等)会重写 equals()方法，使其能够比较对象的内容而不是引用。
## hashcode()和equals()关系
hashcode()和 equals()方法在Java中紧密相关，尤其是在实现自定义类时。这两者的关系如下:
- 当两个对象通过==equals()方法判断为相等时==，它们的==hashcode()值必须相同==。这是hashcode()方法的一个重要约定。
- 反过来，如果两个对象的hashcode()值相同，它们不一定相等。hashcode()的值相同仅仅意味着这两个对象可能是相等的，但还需要通过equals()方法来最终确认。
## 为什么要重写hashcode和equals
重写 hashcode()和equals()方法的原因主要有以下几点:
- 一致性:确保equals()方法返回true 的两个对象具有相同的hashcode()值，这是hashCode()方法的合同之一。
- 容器性能:当对象被用作哈希表(如HashMap 或Hashset)的键时，正确的hashCode()方法可以提高容器的性能。如果hashcode()方法没有正确实现，可能会导致哈希冲突增加，从而降低性能。
==哈希表（如 `HashMap`、`HashSet`）的查找流程是：==
1. ==先计算 key 的 hashCode() → 定位到桶（bucket）==
2. ==在该桶内用 equals() 比较 → 找到精确匹配的对象==
==**如果 `equals()` 相等但 `hashCode()` 不同**：两个"逻辑相等"的对象会被散列到**不同的桶**==
- 对象比较:重写equals()方法可以让类按照自定义的规则来比较对象是否相等，这对于业务逻辑非常重要。
```
String a = new String("hello");
String b = new String("hello");

// 物理层面：两个不同的对象，内存地址不同
System.out.println(a == b);      // false

// 逻辑层面：内容相同，业务上视为相等
System.out.println(a.equals(b)); // true
```
# String
String在Java中是一个不可变的类，它用于表示文本字符串。String对象一旦创建，其内容就不能被改变。String类本身是最终类(final)，因此不能被继承。
## String存储原理
### (1)String底层使用什么类型?
String类底层使用byte[](Java 9及以上)或char[](Java 9以前)来存储字符串数据。这是因为字符串本质上是一系列字符的集合，而字符可以用char类型表示。在Java 9及以后版本中，使用byte[]加上coder 数组的方式可以更好地支持Unicode编码。
### (2) String/StringBuffer区别

![[Pasted image 20260517214052.png]]
String和 StringBuffer的主要区别在于可变性:
- String是不可变的，一旦创建就不能修改其内容。每次对String对象的修改都会创建一个新的String对象。
- StringBuffer是可变的，可以对其内容进行修改而不创建新的对象。StringBuffer类提供了许多方法来修改字符串，如append()、insert()、delete()等。
- StringAppender
另外，StringBuffer方法是线程安全的(synchronized)，这意味着它可以在多线程环境下安全地使用，而String对象本身由于不可变性天然就是线程安全的。
## 什么是字符串常量池?
字符串常量池是==一个特殊的缓存机制==，用于存储字符串字面量(literal)。当使用字符串字面量创建字符串时(如Stringa=“hello”;)，JVM会在字符串常量池中查找是否存在相同的字符串，如果存在则返回池中的引用，否则会在池中创建一个新的字符串对象并返回其引用。这种机制有助于节省内存并提高性能，特别是在多次创建相同字符串的情况下。
## new String("ABC")和String a="abc"区别?
![[Pasted image 20260517214302.png]]
- new String(”ABC"”):这种方式通过new关键字创建了一个新的string对象，即使字符串常量池中已经存在相同的字符串，也会在堆上创建一个新的对象。这意味着每次使用这种方式创建字符串时都会生成一个新的对象。
- Stringa="abc”:这种方式通过字符串字面量来创建字符串。JVM会在字符串常量池中查找是否存在相同的字符串，如果存在，则直接返回池中的引用;如果不存在，则在池中创建一个新的字符串对象并返回其引用。这种方式不会创建额外的对象，除非字符串常量池中没有相同的字符串。
通过上述描述可以看出，String类的设计旨在提供高效、安全的字符串操作，而stringBuffer则提供了一个可变的字符串解决方案，适用于需要频繁修改字符串内容的场景。字符串常量池机制则有助于优化内存使用，避免重复创建相同的字符串对象。
# 异常
## Error和Exception区别
- Error:Error类及其子类表示==程序无法处理的错误==，通常是严重的问题，如JVM自身的问题、资源耗尽等。Error通常是致命的，不应该被程序捕获和处理，因为它们通常表明程序无法继续执行下去。常见的Error包括outofMemoryError、StackOverflowError 等。
- Exception:Exception类及其子类表示==程序可以处理的异常情况==。这些异常通常可以==通过编程手段来预防或处理==。Exception可以进一步分为受检异常(Checked Exception)和非受检异常(UncheckedException)
## 受检异常和非受检异常
- 受检异常(Checked Exception):这些异常==必须在编译时处理==。如果方法可能抛出受检异常，那么要么在方法中==捕获==并处理它，要么在方法==签名中声明该异常==，以便调用者知道可能发生的异常。典型的受检异常包括IOException、 SQLException 等。
- 非受检异常(Unchecked Exception):这些异常在==编译时不需要特别处理==。它们通常是由于==程序逻辑错误==引起的,如 NullPointerException、 ArrayIndexoutofBoundsException 等。 非受检异常继承自RuntimeException 类。
异常发生
   │
   ├─► 受检异常（Checked）─────► 编译器立刻拦住："你打算怎么办？"
   │                              ① try-catch 自己解决
   │                              ② throws 甩锅给上层
   │
   └─► 非受检异常（Unchecked）──► 编译器放行："运行时再看吧"
                                  运行时崩了才暴露问题
## JAVA异常处理机制
Java异常处理机制主要包括以下几个组成部分:
- try块:包含可能抛出异常的代码段。
- catch块:处理try块中抛出的异常。一个try块可以跟随一个或多个catch块，每个catch块可以处理
- 不同类型的异常。
- throw语句:手动抛出一个异常。
- throws关键字:声明一个方法可能抛出的受检异常。
- finally块:无论是否发生异常，finally块中的代码都会被执行。通常用于释放资源，如关闭文件或数据库连接。
## Java 中final、 finally和finalize的区别
| 关键字          | 作用                                                         | 示例/特点                                                    |
| ------------ | ---------------------------------------------------------- | -------------------------------------------------------- |
| `final`      | 修饰类/方法/变量，使其不可变（类不能被继承、方法不能被重写、变量成为常量）。                    | `final int MAX = 10;`<br>`final class A {}`              |
| `finally`    | `try-catch` 结构中必须执行的代码块（无论是否发生异常），常用于资源释放。                 | `try { ... } catch { ... } finally { closeResource(); }` |
| `finalize()` | 是 `Object` 类的 `protected` 方法，垃圾回收前由 JVM 调用（**已过时，不推荐使用**）。 | 子类可重写该方法释放资源，但行为不可控，建议用 `try-with-resources`。            |
核心差异:
- final是修饰符，用于限制变更;
- finally是异常处理的代码块，确保执行;
- finalize()是对象生命周期的回调，已被弃用。
## finally总是会被执行吗
finally块几乎总是会被执行，但也有例外情况:
- 正常执行:如果try或catch块中的代码正常执行完毕，finally块会执行。
- 抛出异常:如果try或catch块中抛出了异常，并且该异常没有被捕获，finally块仍会执行。
- 系统退出:如果在try或catch块中调用了System.exit(int)方法来终止程序，那么finally块不会被执行。
- JVM崩溃:如果JVM本身崩溃，finally块也不会被执行。
总的来说，finally块是非常可靠的，可以用来确保某些清理工作得以完成，例如关闭文件、释放锁或清理资源。但在上述特殊情况下，finally块可能不会被执行。因此，在编写代码时应考虑这些特殊情况，并采取适当的措施来保证资源的正确释放。
### 1. String 为什么设计成 final 的？

----

### 2. 简述 Java 的异常处理机制.

**异常的概念**

在 Java 语言规范中，所有异常都是 Throwable 类或者其子类的实例。

Throwable 有两大直接子类，第一个是 Error，涵盖程序不应该捕获的异常，当触发 Error 时，程序的执行状态已经无法恢复，需要终止线程甚至终止虚拟机。

第二子类是 Exception，涵盖程序可能需要捕获并且处理的异常，如 NullPointerException。Exception 包含检查异常和非检查异常，对于检查异常，需要在程序中显式地捕获，或者在方法中用 throws 关键字标注，比如 ClassNotFoundException。

**处理异常的方式**

**抛出异常**：抛出异常可分为显式和隐式两种。显式抛异常的主体是应用程序，它指的是在程序中使用 throw 关键字，手动将异常抛出。

隐式抛异常的主体是 Java 虚拟机，它指的是 Java 虚拟机在执行过程中，碰到无法继续执行的异常状态，自动抛出异常。比如在执行读取数组操作时，发现输入的索引值时负数，故而抛出数组越界异常（ArrayindexOutOfBoundsException）。

**捕获异常**：捕获异常涉及三种代码块：

  1. **try 代码块**：用来标记需要进行异常监控的代码

  2. **catch 代码块**：用来捕获在 try 块中触发的某种执行类型的异常。try 代码块后面可以跟着多个 catch 代码块来捕获不同的异常（也可以在一个 catch 语句中通过 ｜ 来捕获不同的异常），Java 虚拟机会从上至下进行异常匹配来进行处理，因此前面的 catch 代码块所捕获的异常类型不能覆盖后面的，否则编译器会报错。

  3. **finnally 代码块**：跟在 try 块和 catch 块之后，用来声明一段必定执行的代码。它的设计初衷是为了避免跳过某些关键的清理代码，例如关闭已打开的系统资源。

     在程序正常执行的情况下，finally 块会在 try 块之后执行，如果 try 块触发异常并且没有被捕获，finally 块会直接运行并且在运行之后抛出该异常。

     如果异常被 catch 代码块捕获，finally 块则在 catch 块之后运行。在某些情况下 catch 块也触发了异常，finally 块同样会运行，并会抛出 catch 代码块触发的异常。在极端情况下，finally 代码块也触发了异常，则会中断当前 finally 块的代码并抛出异常。

----

### 3. 为什么实现 equals() 必须先实现 hashCode() 方法？

1. 如果两个对象相同则其 hashCode 一定相等；但是 hashCode 相等时，两个对象却不一定相同
2. 为了提高程序的执行效率才实现了 `hashCode` 方法，先进行 `hashCode` 比较，如果不同就没有必要进行 `equals` 比较了，这样大大减少了 equals 的使用次数，从而提高程序的执行效率。

所以如果只重写 `equals()` 不重写 `hashCode()` 方法可能会出现逻辑错误

**重写 equals() 方法注意事项**

- 自反性：对于任意不为 `null`的引用值 x，`x.equals(x)` 一定是`true`，自己和自己比较一定返回 `true`
- 对称性：对于任意不为`null`的引用值`x`和`y`，当且仅当`x.equals(y)`是`true`时，`y.equals(x)`也是`true`。
- 传递性：对于任意不为`null`的引用值`x`、`y`和`z`，如果`x.equals(y)`是`true`，同时`y.equals(z)`是`true`，那么`x.equals(z)`一定是`true`。
- 一致性：对于任意不为`null`的引用值`x`和`y`，如果用于equals比较的对象信息没有被修改的话，多次调用时`x.equals(y)`要么一致地返回`true`要么一致地返回`false`。
- 对于任意不为`null`的引用值`x`，`x.equals(null)`返回`false`。

**重写 hashCode() 方法注意事项**

- 在一个 Java 应用执行期间，如果一个对象提供给 equals 做比较的信息没有被修改的话，该对象多次调用`hashCode()`方法，该方法必须始终如一返回同一个整型数值。
- 如果两个对象根据`equals()`方法是相等的，那么调用二者各自的`hashCode()`方法必须返回相同结果
- 并不要求根据`equals()`方法不相等的两个对象，调用各自的`hashCode()`方法必须产生不同结果。

### 4. String name = new String("abc") 创建了几个对象？

```java
String str1 = "abc"; // 在常量池中
String str2 = new String("abc"); // 在堆上
```

<img src="https://imgconvert.csdnimg.cn/aHR0cDovL3d3dy5jaG91cGFuZ3hpYS5jb20vd3AtY29udGVudC91cGxvYWRzLzIwMjAvMDgvc3RyaW5nMS5qcGc?x-oss-process=image/format,png" alt="image" style="zoom: 50%;" />

当直接赋值时，字符串 "abc" 会被存储在常量池中，只有一份，此时赋值操作等于是创建 0 个或 1 个对象，取决于常量池中是否已经存在字符串 "abc"。

通过 `String s = new String("abc")` 方式创建字符串对象时，会将等号右边的 "abc" 常量放在常量池中，先在堆中创建一个 String 对象，该对象的内容指向常量池的 "abc" 。此时创建是 1 个或者 2 个对象，取决于常量池中是否存在字符串 "abc"。

### 5. 创建对象有几种方式？

**使用 new 关键字**：是最常见也是最简单的一种创建对象的方式

```java
Object obj = new Object();
```

**使用反射**

使用 Class 类的 newInstance() 方法创建对象，调用无参构造函数创建对象。1.9 以后已过时。

```java
Object obj = Object.class.newInstance();
```

使用 Constructor 类的 newInstance() 方法，可以调用有参数的和私有的构造函数。

```java
Object obj = Object.class.getConstructor().newInstance();
```

**使用 clone**

对于实现 Cloneable 接口的对象可以调用 `clone()` 方法克隆一个新的对象。

`clone()` 方法是浅拷贝的，也就是说在拷贝时如果成员变量是引用类型的话成员变量是不会进行拷贝的。如果需要实现深拷贝，需要覆盖 `clone()` 方法，同时将成员变量也进行克隆，如果成员变量还引用了其它对象，被引用的对象也需要进行单独拷贝。所以实现深拷贝是比较麻烦的，尤其是在引用第三方对象的时候，由于第三方对象没有实现 `clone()` 方法，是不能进行深拷贝的。

**反序列化**

序列化和反序列化主要用于对象的持久化和远程传输，一个对象要想进行序列化和反序列化，需要实现 Serializable 接口。

```java
public class Test {

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // 序列化
        File file = new File("user");
        FileOutputStream fos = new FileOutputStream(file);
        ObjectOutput oos = new ObjectOutputStream(fos);
        oos.writeObject(new User("Tom"));
        oos.flush();

        // 反序列化
        FileInputStream fis = new FileInputStream(file);
        ObjectInputStream ois = new ObjectInputStream(fis);
        User user = (User) ois.readObject();
        System.out.println(user.getName());
    }
}

class User implements Serializable {

    private String name;

    public User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

#### 1.6. 获取 Class 对象有几种方式？有什么区别？

三种方式都返回 Class对象，不同之处在于：

1. 类名.class：只做类的加载，不做初始化工作
2. `Class.forName("类名")`：加载类并做类的静态初始化，需要进行异常捕获，可能会抛出 `ClassNotFoundException`
3. 对象实例调用 `getClass()` 方法：对类进行加载并进行静态初始化、实例化

#### 1.7. Object 类有哪些方法？

1. `getClass()`：获取实例对应的类对象
2. `hashCode()`：返回一个整型数值，默认返回内存地址，可以通过 JVM 参数配置
3. `equals()`：比较两个对象是否相等，默认使用 == 进行比较，比较的是内存地址
4. `clone()`：克隆方法，实现了 Cloneable 接口的类可以调用该方法，否则会抛出 CloneNotSupportedException
5. `toString()`：返回一个 String 字符串，是对对象的描述。默认返回格式如下：对象的 **class 名称 + @ + hashCode** 的十六进制字符串。
6. `notify()`：随机唤醒一个在该对象等待池的某个线程
7. `notifyAll()`：唤醒在该对象等待池的所有线程，优先级高的线程竞争到对象锁的概率大
8. `wait()`：使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁
9. `finalize()`：主要用于在 GC 的时候再次被调用。当垃圾回收器要宣告一个对象死亡时，至少要经过两次标记过程：如果对象在进行可达性分析后发现没有和 GC Roots 相连接的引用链，就会被第一次标记，并且判断是否执行 finalize( ) 方法，如果对象覆盖 finalize( )方法且未被虚拟机调用过，那么这个对象会被放置在 F-Queue 队列中，并在稍后由一个虚拟机自动建立的低优先级的 Finalizer 线程区执行触发 `finalizer()` 方法，但不承诺等待其运行结束。 作用是为了给对象最后一次逃脱的机会。

### 8. JDK8 新特性及实际应用.

1. 函数式接口：就是说函数可以作为另一个函数的参数。函数式接口，要求接口中**有且仅有一个抽象方法**，因此经常使用的Runnable，Callable接口就是典型的函数式接口。可以使用`@FunctionalInterface`注解，声明一个接口是函数式接口。
2. Lambda 表达式：就是匿名函数，可以作为参数直接传递给调用者
3. Stream API：是对集合对象功能的增强，提供了各种非常便利、高效的操作
4. 新的时间和日期的 API
5. 其它新特性：如 HashMap、ConcurrentHashMap的结构变化
6. 52fc21008dc6030ec46a240b36475d429f11b484

### 9. 快速失败和安全失败.

**快速失败**

迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个 modCount 变量。集合在被遍历期间如果内容发生变化，就会改变 modCount 的值。每当迭代器遍历下一个元素之前，都会检测 modCount 变量是否为 expectedmodCount 值，是的话就返回遍历；否则抛出异常，终止遍历。java.util 包下的集合类都是快速失败的，不能在多线程下发生并发修改（迭代过程中被修改）。

**安全失败**

采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。由于迭代时是对原集合的拷贝进行遍历，所以在遍历过程中对原集合所作的修改并不能被迭代器检测到，所以不会触发ConcurrentModificationException。基于拷贝内容的优点是避免了ConcurrentModificationException，但同样地，迭代器并不能访问到修改后的内容，也就是说迭代器遍历的是开始遍历那一刻拿到的集合拷贝，在遍历期间原集合发生的修改迭代器是不知道的。java.util.concurrent 包下的容器都是安全失败，可以在多线程下并发使用，并发修改。
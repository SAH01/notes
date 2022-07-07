Java面试题总结

----

# 一、Java基础

## 1）Java有没有goto？

> goto是C语言中的，通常与条件语句配合使用，可用来实现条件转移， 构成循环，跳出循环体等功能。Java保留了这个关键字但是没有使用。

## 2）&和&&的区别？

> &和&&都表示逻辑与的关系，同真则真，有假则假。
>
> ==&&具有短路的功能，即如果第一个表达式为 false，则不再计算第二个表达式。==if(x == 33 & ++y>0) y 会增长，if(x ==33 && ++y>0) 不会增长
>
> & 是按位与运算符。当&操作符两边的表达式不是 boolean 类型时，&表示按位与操作。

## 3）静态变量和实例变量的区别？

> 在语法定义上的区别：静态变量前要加 static 关键字，而实例变量前则不加。
> 在程序运行时的区别：实例变量属于某个对象的属性，必须创建了实例对象，其中的实例变量才会被分配空间，才能使用这个实例变量。静态变量不属于某个实例对象，而是属于类，所以也称为类变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间， 静态变量就可以被使用了。

## 4）是否可以从一个 static 方法内部发出对非 static 方法的调用？

> 不可以。因为非 static 方法是要与对象关联在一起的，必须创建一个对象后，才可以在该对象上进行方法调用，而 static 方法调用时不需要创建对象，可以直接调用。也就是说，当一个 static 方法被调用时，可能还没有创建任何实例对象，如果从一个 static 方法中发出对非 static 方法的调用，那个非 static 方法是关联到哪个对象上的呢？

## 5）Math类的三个取整方法

> Math 类中提供了三个与取整有关的方法：**ceil、floor、round**，这些方法的作用与它们的英文名称的含义相对应，例如，ceil 的英文意义是天花板，该方法就表示向上取整，Math.ceil(11.3) 的结果为 12,Math.ceil(-11.3)的结果是-11；floor 的英文意义是地板，该方法就表示向下取整， Math.ceil(11.6)的结果为 11,Math.ceil(-11.6)的结果是-12；最难掌握的是==round 方法，它表示“四舍五入”，算法为Math.floor(x+0.5)，即将原来的数字加上0.5 后再向下取整==，所以，Math.round(11.5)的结果为 12，Math.round(-11.5)的结果为-11。

## 6）请说出作用域 public，private，protected，以及不写时的区别（不写是friendly）

![image-20220520204219523](Java面试题总结/image-20220520204219523.png)

## 7）Overload 和 Override 的区别。

> 1. 重写 Override 表示子类中的方法可以与父类中的某个方法的名称和参数完全相同，通过子类创建的实例对象调用这个方法时，将调用子类中的定义方法，这相当于把父类中定义的那个完全相同的方法给覆盖了，这也是面向对象编程的多态性的一种表现。
> 2. 在使用重载时只能通过不同的参数样式。例如，**不同的参数类型，不同的参数个数，不同的参数顺序**（当然，同一方法内的几个参数类型必须不一样，例如可以是 fun(int,float)，但是不能为 fun(int,int)）
> 3. 被覆盖和被重载的方法都不能是private。

## 8）构造器 Constructor 是否可被 override?

> 构造器 Constructor 不能被继承，因此不能重写 Override，但可以被重载 Overload。

## 9）面向对象的特征有哪些方面？

> 面向对象的编程语言有==封装、继承 、抽象、多态==等 4 个主要的特征。
>
> 1. 封装：实现软件部件的“高内聚、低耦合”，防止程序相互依赖性而带来的变动影响。在面向对象的编程语言中，对象是封装的最基本单位，把对同一事物进行操作的方法和相关的方法放在同一个类中，把方法和它操作的数据放在同一个类中。私有的属性，公有的方法。
>
>     例子：面向对象的封装性，即将对象封装成一个高度自治和相对封闭的个体，==对象状态（属性）由这个对象自己的行为（方法）来读取和改变。==一个更便于理解的例子就是，司机将火车刹住了，刹车的动作是分配给司机，还是分配给火车，显然，应该分配给火车，因为司机自身是不可能有那么大的力气将一个火车给停下来的，只有火车自己才能完成这一动作，火车需要调用内部的离合器和刹车片等多个器件协作才能完成刹车这个动作，**司机刹车的过程只是给火车发了一个消息，通知火车要执行刹车动作而已。**
>
> 2. 抽象：抽象就是找出一些事物的相似和共性之处，然后将这些事物归为一个类，==这个类只考虑这些事物的相似和共性之处==，并且会忽略与当前主题和目标无关的那些方面，将注意力集中在与当前目标有关的方面。
>
> 3. 继承：==在定义和实现一个类的时候，可以在一个已经存在的类的基础之上来进行，把这个已经存在的类所定义的内容作为自己的内容，并可以加入若干新的内容，或修改原来的方法使之更适合特殊的需要==，这就是继承。继承是子类自动共享父类数据和方法的机制，这是类之间的一种关系， 提高了软件的可重用性和可扩展性。
>
> 4. 多态：多态是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即==一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。==

## 10）Java中实现多态的机制是什么？

> ==父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方法在运行期才动态绑定==，就是引用变量所指向的具体实例对象的方法，也就**是内存里正在运行的那个对象的方法，而不是引用变量的类型中定义的方法。**



---



## 11）abstract class 和 interface 有什么区别？

> - 含有 abstract 修饰符的 class 即为抽象类，==abstract 类不能创建的实例对象。==含有 abstract 方法的类必须定义为 abstract class，abstract class 类中的方法不必全是抽象的（也就是说可以有一个抽象类，里面没有抽象方法），但是**不可以是private**的。==abstract class 类中定义抽象方法必须在具体(Concrete)子类中实现==，所以，不能有抽象构造方法或抽象静态方法（但是可以有普通静态方法和普通构造方法）。==如果的子类没有实现抽象父类中的所有抽象方法，那么子类也必须定义为 abstract 类型。==
>
> - 接口（interface）可以说成是抽象类的一种特例，==接口中的所有方法都必须是抽象的。==接口中的方法定义默认为 public abstract 类型，接口中的成员变量类型默认为 public static final。
>
> - 简言之，抽象类本质是是类，可以有普通类有的一些东西，但是接口比抽象类还要抽象，接口中只能有公开的未实现的方法。
>
>     一个类可以实现多个接口但是只能继承一个抽象类。接口不可以有普通成员变量，但是可以有静态成员变量，接口不可以有静态方法。接口不可以有构造方法。
>
> - **总结**：接口主要是给出一些方法，如果某个类实现了这个接口，这些功能就会加到这个类上。
>
>     ​			抽象类本质上还是类，只不过这个类不清楚自己到底干了什么没干什么，因为他自身存在没有实现完的东西需要留给继承它的子类去做。

## 12）什么是native方法？

> 简单地讲，一个Native Method就是一个java调用非java代码的接口。
>
> - Java程序中声明native修饰的方法，类似于abstract修饰的方法，只有方法签名，没有方法实现。编译该java文件，会产生一个.class文件。
> - 使用javah编译上一步产生的class文件，会产生一个.h文件。
> - 写一个.cpp文件实现上一步中.h文件中的方法。
> - 将上一步的.cpp文件编译成动态链接库文件.dll。
> - 最后就可以使用System或是Runtime中的loadLibrary()方法加载上一步的产生的动态连接库文件了。

## 13）谈一谈对String的理解？

> 基本数据类型包括 byte、int、char、long、float、double、boolean 和 short。
> **java.lang.String 类是 final 类型的，因此不可以继承这个类、不能修改这个类。**为了提高效率节省空间，我们应该用 StringBuffer 类

## 14）数组有没有 length()这个方法? String 有没有 length()这个方法？

> ==数组没有 length()这个方法，有 length 的属性。==String 有有 length()这个方法。

## 15）这条语句一共创建了多少个对象：String s="a"+"b"+"c"+"d";

```java
String s1 = "a"; 
String s2 = s1 + "b"; 
String s3 = "a" + "b";
System.out.println(s2 == "ab"); 
System.out.println(s3 == "ab");
```

> 第一条语句打印的结果为 false，第二条语句打印的结果为 true，==这说明 javac 编译可以对字符串常量直接相加的表达式进行优化，不必要等到运行期去进行加法运算处理，而是在编译时去掉其中的加号，直接将其编译成一个这些常量相连的结果。==
> 题目中的第一行代码被编译器在编译时优化后，相当于直接定义了一个”abcd”的字符串，所以， **上面的代码应该只创建了一个 String 对象。**写如下两行代码，
> String s = "a" + "b" + "c" + "d"; System.out.println(s == "abcd");
> 最终打印的结果应该为 true。

## 16）try {}里有一个 return 语句，那么紧跟在这个 try 后的 finally {}里的 code 会不会被执行，什么时候被执行，在 return 前还是后?

```java
package com.test;

public class Test1 {
    public static void main(String[] args) {
        try {
            System.out.println(new Test1().testname());
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    public String testname() throws Exception {
        String t = "";

        try {
            t = "try";
            System.out.println("try");
            return t;
        } catch (Exception e) {
            // result = "catch";
            t = "catch";
            return t;
        } finally {

            System.out.println("finally");
            // return t = "finally";
        }
    }
}
打印结果如下：
try
finally
try
将finally中的注释放开，打印结果如下：
try
finally
finally
```

> 参考：https://www.cnblogs.com/xh_Blog/p/6518620.html
>
> 结论：finally中的代码肯定会执行，但是会先执行try中的代码，如果try中有return，那么return的东西会先放到函数栈中，然后再执行finally中的代码，
>
> ①、如果finally中也有return，则会直接返回并终止程序，函数栈中的return不会被完成！；
>
> ②、==如果finally中没有return，则在执行完finally中的代码之后，会将函数栈中的try中的return的内容返回并终止程序；==
>
> catch同try;

## 17）error 和 exception 有什么区别?

> error 表示恢复不是不可能但很困难的情况下的一种严重问题。比如说内存溢出。==error不可能指望程序能处理这样的情况。==exception 表示一种设计或实现问题。也就是说，它表示如果程序运行正常，从不会发生的情况。

## 18）Java 中的异常处理机制的简单原理和应用。

> - 异常是指 java 程序==运行时（非编译）==所发生的非正常情况或错误。
>
> Java 对异常进行了分类，不同类型的异常分别用不同的 Java 类表示，所有异常的根类为java.lang.Throwable，Throwable 下面又派生了两个子类：**Error 和 Exception**，==Error 表示应用程序本身无法克服和恢复的一种严重问题，程序只有死的份了==，例如，说内存溢出和线程死锁等系统问题。==Exception 表示程序还能够克服和恢复的问题，其中又分为系统异常和普通异常==，系统异常是软件本身缺陷所导致的问题，也就是软件开发人员考虑不周所导致的问题，软件使用者无法克服和恢复这种问题，但在这种问题下==（系统异常）==**还可以让软件系统继续运行或者让软件死掉**，例如，数组脚本越界（ArrayIndexOutOfBoundsException），空指针异常（NullPointerException）、类转换异常（ClassCastException）；==普通异常（程序不应该死掉）==是运行环境的变化或异常所导致的问题，是用户能够克服的问题，例如，网络断线，硬盘空间不够，发生这样的异常后，程序不应该死掉。
>
> - ==java 为系统异常和普通异常提供了不同的解决方案，编译器强制普通异常必须 try..catch 处理或用 throws 声明继续抛给上层调用方法处理，==所以**普通异常也称为 checked 异常**，而系统异常可以处理也可以不处理，所以，编译器不强制用 try..catch 处理或用 throws 声明，所以**系统异常也称为 unchecked 异常**。

## 19）请写出你最常见到的 5 个 runtime exception。

![img](Java面试题总结/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwNDE3NDk5,size_16,color_FFFFFF,t_70.png)

> ​	参考：https://blog.csdn.net/qq_20417499/article/details/80222820
>
> 1. ClassNotFoundException找不到类异常
>     当应用试图根据字符串形式的类名构造类，而在遍历CLASSPAH之后找不到对应名称的class文件时，抛出该异常。
> 2. ArrayIndexOutOfBoundsException数组下标越界异常
>     当使用的数组下标超出数组允许范围时，抛出该异常。
> 3. NullPointerException空指针异常类
>     String s=null;
>     int size=s.size();
>     当应用程序试图在需要对象的地方使用 null 时，抛出异常。
> 4. ArithmeticException算术异常类
>     int a=5/0;
>     一个整数“除以零”时，抛出异常。
> 5. ClassCastException类型强制转换异常
>     Object x = new Integer(0);
>     System.out.println((String)x);
>     当试图将对象强制转换为不是实例的子类时，抛出该异常。



----



## 20）什么是线程安全？

> **含义：**当多个线程访问某个方法时，不管你通过怎样的调用方式或者说这些线程如何交替的执行，我们在主程序中不需要去做任何的同步，这个类的结果行为都是我们设想的正确行为，那么我们就可以说这个类时线程安全的。
> **如果一段代码可以保证多个线程访问的时候正确操作共享数据，那么它是线程安全的。**

## 21）ArrayList 和 Vector 的区别？

> 共同点：这两个类都实现了 List 接口（List 接口继承了 Collection 接口），他们都是有序集合，即存储在这两个集合中的元素的位置都是有顺序的，相当于一种动态的数组，我们以后可以按位置索引号取出某个元素，并且其中的数据是允许重复的。
>
> ==区别：==
>
> 1. 同步性：Vector 是线程安全的，也就是说是它的方法之间是线程同步的，而 ArrayList 是线程序不安全的，它的方法之间是线程不同步的。
> 2. 数据增长：==当存储进它们里面的元素的个数超过了容量时，Vector 增长原来的一倍，ArrayList 增加原来的 0.5 倍。==

## 22）HashMap 和 HashTable 的区别？

> HashMap 是 HashTable 的轻量级实现（非线程安全的实现），他们都完成了 Map 接口，主要区别在于 HashMap 允许空（null）键值（key）,由于非线程安全，在只有一个线程访问的情况下， 效率要高于 HashTable。
>
> 1. HashMap 不是线程安全的 HashTable 实现了线程同步
> 2. ==HashMap 允许出现null的key或者value但是HashTable 不允许==

## 23）Collection 简述？

> ==为了保存数量不确定的数据，以及保存具有映射关系的数据（也被称为关联数组）==，Java提供了集合类。集合类主要负责保存、盛装其他数据，因此集合类也被称为容器类。Java 所有的集合类都位于 java.util 包下，提供了一个表示和操作对象集合的统一构架，包含大量集合接口，以及这些接口的实现类和操作它们的算法。
>
> ==集合类和数组不一样，数组元素既可以是基本类型的值，也可以是对象（实际上保存的是对象的引用变量），而集合里只能保存对象==（实际上只是保存对象的引用变量，但通常习惯上认为集合里保存的是对象）。**其实你在添加list.add(1)的时候 基本数据类型的1被自动装箱成为了Integer类型。**
>
> Java 集合类型分为 Collection 和 Map，它们是 Java 集合的根接口，这两个接口又包含了一些子接口或实现类。图 1 和图 2 分别为 Collection 和 Map 的子接口及其实现类。

> ![image-20220522143738741](Java面试题总结/image-20220522143738741.png)

## 24）Collection 和 Collections 的区别？

> Collection 是集合类的上级接口，继承与他的接口主要有 Set 和 List.
> ==Collections 是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。==
>
> 类比Object和Objects

## 25）== 和 equals 的区别是什么？

> **一、对象类型不同**
>
> 1、equals()：是超类Object中的方法。
>
> 2、==：是操作符。
>
> **二、比较的对象不同**
>
> 1、equals()：用来检测两个对象是否相等，即两个对象的内容是否相等。
>
> 2、==：用于比较引用和比较基本数据类型时具有不同的功能，具体如下：
>
> （1）、基础数据类型：比较的是他们的值是否相等，比如两个int类型的变量，比较的是变量的值是否一样。
>
> （2）、引用数据类型：比较的是引用的地址是否相同，比如说新建了两个User对象，比较的是两个User的地址是否一样。
>
> **三、运行速度不同**
>
> 1、equals()：没有 == 运行速度快。
>
> 2、== 运行速度比equals()快，因为 == 只是比较引用。
>
> ==总结：equals方法比较的是对象的值是否相等，而 == 比较的是引用是否相等，也就是说，如果当两个对象的值一样但是引用地址不一样的时候，用equals比较是true但是用 == 就是false。==
>
> 补充：**Java有 5种引用类型（对象类型）：类 接口 数组 枚举 标注**

## 26）你所知道的集合类都有哪些？主要方法？

> Set，大概的方法是 add,remove, contains；
>
> 对于 map，大概的方法就是put,remove，contains 等，List 类会有 get(int index)这样的方法，因为它可以按顺序取元素，而 Set 类中没有 get(int index)这样的方法。
>
> List 和 Set 都可以迭代出所有元素，迭代时先要得到一个iterator 对象，所以，Set 和 list 类都有一个 iterator 方法，用于返回那个 iterator 对象。
>
> map 可以返回三个集合，一个是返回所有的 key 的集合，另外一个返回的是所有 value 的集合，再一个返回的 key 和 value 组合成的 EntrySet 对象的集合，map 也有 get 方法，参数是 key，返回值是 key 对应的 value。

## 27）列举几个你常用的类、接口和包

> 1. Java常用的类：BufferedReader BufferedWriter ，FileReader FileWirter  ，Date， Class， 、String
> 2. java常用的接口：List ，Map ，Set，HttpServletRequest ，HttpServletResponse ，Servlet
> 3. java常用的包：java.lang ，java.io ，java.util ，java.sql ，javax.servlet
>
> - **java.lang**：该包提供了Java语言进行程序设计的基础类，它是默认导入的包。该包里面的Runnable接口和Object、Math、String、StringBuffer、System、Thread以及Throwable类需要重点掌握，因为它们应用很广。
> - **java.util**：该包提供了包含集合框架、遗留的集合类、事件模型、日期和时间实施、国际化和各种实用工具类（字符串标记生成器、随机数生成器和位数组）。
> - **java.io**：该包通过文件系统、数据流和序列化提供系统的输入与输出。
> - **java.sql**：该包提供了使用Java语言访问并处理存储在数据源（通常是一个关系型数据库）中的数据API。
> - **javax.servlet**： 包含了一定数量的类和接口，这些类和接口描述和定义了一个servlet程序和运行时环境的合同。这运行时环境提供给遵循 servlet容器的类的实例。



----



## 28）Java 栈和堆的区别

> ==1 栈：为编译器自动分配和释放，如函数参数、局部变量、临时变量等等
> 2 堆：为成员分配和释放，由程序员自己申请、自己释放。否则发生内存泄露。典型为使用new申请的堆内容。==
> 3 静态存储区：内存在程序编译的时候就已经分配好，这块内存在程序的整个运行期间都存在。它主要存放静态数据、全局数据和常量。



# 二、JavaWeb



## 29）解释一下什么是 servlet

> servlet 有良好的生存期的定义，包括==加载和实例化、初始化、处理请求以及服务结束==。这个生存期由 javax.servlet.Servlet 接口的 init,service 和 destroy 方法表达。

## 30）描述一下 servlet 生命周期

> Servlet 被服务器实例化后，容器运行其 init 方法，请求到达时运行其 service 方法，service 方法自动派遣运行与请求对应的 doXXX 方法（doGet，doPost）等，当服务器决定将实例销毁的时候调用其 destroy 方法。

## 31）servlet基本架构

```java
public class ServletName extends HttpServlet {
public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

	}
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

	}
}
```

## 32）SERVLET API 中 forward() 与 redirect()的区别？

> 前者仅是容器中控制权的转向，在客户端浏览器地址栏中不会显示出转向后的地址；
>
> ==后者则是完全的跳转，浏览器将会得到跳转的地址，并重新发送请求链接。==这样，从浏览器的地址栏中可以看到跳转后的链接地址。

## 33）jsp 有哪些动作?作用分别是什么?

> 1. jsp:include：在页面被请求的时候引入一个文件。
>
> 2. jsp:useBean：寻找或者实例化一个 JavaBean。
>
> 3. jsp:setProperty：设置 JavaBean 的属性。
>
> 4. jsp:getProperty：输出某个 JavaBean 的属性。
>
> 5. jsp:forward：把请求转到一个新的页面。
>
> 6. jsp:plugin：根据浏览器类型为 Java 插件生成 OBJECT 或 EMBED 标记



---



# 三、数据库

## 34）JdbcUtils (mysql)

```java
import java.io.FileInputStream;
import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.net.URLDecoder;
import java.sql.*;
import java.util.Properties;
public class JdbcUtils {
    /**
     *  创建一个jdbc的工具类，简化创建连接和释放资源的操作
     */
    private static String url;
    private static String user;
    private static String password;

    //创建静态代码块，在类加载的时候执行一次，用来读取配文件
    static {
        try {
            //创建properties集合类
            Properties properties = new Properties();
            //注意，用类加载器来动态获取src下的配置文件路径
            ClassLoader classLoader = JdbcUtils.class.getClassLoader();
            URL resource = classLoader.getResource("jdbc.properties");
            String path1 = resource.getPath();
            //用urlDecoder来解决中文jdbc中文路径乱码的问题
            String path = URLDecoder.decode(path1, "utf-8");
            System.out.println(path);
            //加载配置文件
           // properties.load(new FileReader("src/jdbc.properties"));
            //用path来动态的获取路径
            properties.load(new FileReader(path));
            //获取信息
             url = properties.getProperty("url");
             user = properties.getProperty("user");
             password = properties.getProperty("password");
            String driver = properties.getProperty("driver");

            //注册驱动
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }

    //1.定义创建连接的方法
    public  static Connection getConnection(){
        try  {
            System.out.println("url"+url);
            Connection connection = DriverManager.getConnection(url, user, password) ;
            return connection;
        }catch (Exception e){
            return null;
        }

    }
    //2.定义释放资源的方法

    public static void close(Statement statement ,Connection connection){
        if (statement!=null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(connection!=null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }
    //3.释放资源方法重载
    public static void close(ResultSet rs ,Statement statement ,Connection connection){
        if (rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (statement!=null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(connection!=null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }
}
```

## 35）Jdbc 连接 oracle

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
public class dbUtil {
  public static Connection getConnection(){
	  Connection conn=null;
	  
	  try {
		String url="jdbc:oracle:thin:@127.0.0.1:1521:orcl";
		  String user="scott";
		  String password="tiger";
		  
		  Class.forName("oracle.jdbc.driver.OracleDriver");//加载数据驱动
		  conn = DriverManager.getConnection(url, user, password);// 连接数据库
		  
	} catch (ClassNotFoundException e) {
		e.printStackTrace();
		System.out.println("加载数据库驱动失败");
	}catch(Exception e){
		e.printStackTrace();
		System.out.println("连接数据库失败");
	}
	  return conn;
  }
  public static void close(Connection conn, PreparedStatement ps, ResultSet rs){
	  try {
		if(rs!=null){
			  rs.close();
		  }
	} catch (SQLException e) {
		e.printStackTrace();
	}
	  
	  try {
			if(ps!=null){
				  ps.close();
			  }
		} catch (SQLException e) {
			e.printStackTrace();
		}
	  
	  try {
			if(conn!=null){
				  conn.close();
			  }
		} catch (SQLException e) {
			e.printStackTrace();
		} 
  }
}
```

## 36）大数据量下的分页解决方法

```sql
"select * from students order by id limit " + pageSize*(pageNumber-1) + "," + pageSize;
```

## 37）ORM是什么，有什么作用？

> - 对象关系映射（Object Relational Mapping，简称ORM）模式是一种为了解决面向对象与关系数据库存在的互不匹配的现象的技术。简单的说，==ORM是通过使用描述对象和数据库之间映射的元数据，将程序中的对象自动持久化到关系数据库中。==
>
> - **ORM是面向对象程序设计语言和关系数据库发展不同步时的中间解决方案。采用ORM框架后，应用程序不再直接访问底层数据库，而是以面向对象的方式来操作持久化对象（创建、修改、删除等），而ORM框架则将这些面向对象的操作转换成底层的SQL操作。**
>
>     ===
>
>     作者：Dcl_Snow
>     链接：https://www.jianshu.com/p/90c4ca01824d
>     来源：简书
>     著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
>
>     ===

# 四、J2EE

## 38）C/S 与 B/S 区别

> **１．硬件环境不同**
> C/S 一般建立在专用的网络上, 小范围里的网络环境, 局域网之间再通过专门服务器提供连接和数据交换服务.
> B/S 建立在广域网之上的, 不必是专门的网络硬件环境,例与电话上网, 租用设备. 信息自己管理. 有比 C/S 更强的适应范围, 一般只要有操作系统和浏览器就行
> **２．对安全要求不同**
> C/S 一般面向相对固定的用户群, 对信息安全的控制能力很强. 一般高度机密的信息系统采用 C/S 结构适宜. 可以通过 B/S 发布部分可公开信息.
> B/S 建立在广域网之上, 对安全的控制能力相对弱, 可能面向不可知的用户。
>
> **３．对程序架构不同**
> C/S 程序可以更加注重流程, 可以对权限多层次校验, 对系统运行速度可以较少考虑.
> B/S 对安全以及访问速度的多重的考虑, 建立在需要更加优化的基础之上. 比 C/S 有更高的要求 B/S 结构的程序架构是发展的趋势, 从 MS 的.Net 系列的 BizTalk 2000 Exchange 2000 等, 全面支持网络的构件搭建的系统. SUN 和IBM 推的JavaBean 构件技术等,使 B/S 更加成熟.
> **４．软件重用不同**
> C/S 程序可以不可避免的整体性考虑, 构件的重用性不如在 B/S 要求下的构件的重用性好
>
> B/S 对的多重结构,要求构件相对独立的功能. 能够相对较好的重用.就入买来的餐桌可以再利用,而不是做在墙上的石头桌子
> **５．系统维护不同**
> C/S 程序由于整体性, 必须整体考察, 处理出现的问题以及系统升级. 升级难. 可能是再做一个全新的系统
> B/S 构件组成,方面构件个别的更换,实现系统的无缝升级. 系统维护开销减到最小.用户从网上自己下载安装就可以实现升级.
> **６．处理问题不同**
> C/S 程序可以处理用户面固定, 并且在相同区域, 安全要求高需求, 与操作系统相关. 应该都是相同的系统
> B/S 建立在广域网上, 面向不同的用户群, 分散地域, 这是 C/S 无法作到的. 与操作系统平台关系最小.
> **７．用户接口不同**
> C/S 多是建立的 Window 平台上,表现方法有限,对程序员普遍要求较高
> B/S 建立在浏览器上, 有更加丰富和生动的表现方式与用户交流. 并且大部分难度减低, 减低开发成本.
> **８．信息流不同**
> C/S 程序一般是典型的中央集权的机械式处理, 交互性相对低
> B/S 信息流向可变化, B-B B-C B-G 等信息、流向的变化, 更像交易中心。

## 39）WEB服务器和应用服务器的区别

> **应用服务器处理业务逻辑，web服务器则主要是让客户可以通过浏览器进行访问，处理HTML文件，web服务器通常比应用服务器简单。**
> **WEB服务器**:Apache、IIS、Nginx（也是反向代理服务器）
> **应用服务器**:Tomcat、Weblogic、Jboss
>
> ==举例：==一般来说，大的站点都是将Tomcat与Apache的结合，Apache负责接受所有来自客户端的HTTP请求，然后将Servlets和JSP的请求转发给Tomcat来处理。Tomcat完成处理后，将响应传回给Apache，最后Apache将响应返回给客户端。

# 五、MyBatis

## 40）谈谈 MyBatis

> Mybatis 是一个半自动化的 ORM 框架，它对 jdbc 的操作数据库的过程进行封装，使得开发者只需要专注于 SQL 语句本身，而不用去关心注册驱动，创建 connection 等，Mybatis 通过 xml 文件配置或者注解的方式将要执行的各种 statement 配置起来，==并通过 java 对象和 statement 中的sql 进行映射成最终执行的 sql 语句，最后由 Mybatis 框架执行 sql 并将结果映射成 java 对象并返回。每个 MyBatis 应用程序主要都是使用 SqlSessionFactory 实例的，一个 SqlSessionFactory 实例可以通过 SqlSessionFactoryBuilder 获得。==SqlSessionFactoryBuilder 可以从一个 xml 配置文件或者一个预定义的配置类的实例获得。
> Mybatis 分为三层
> （1） API 接口层：提供给外部使用的接口 API
> （2） 数据处理层：负责具体的 SQL
> （3） 基础支撑层：负责最基础的功能支撑，如连接管理，事务管理，配置加载和缓存处。理

## 41）Mybatis 的优点

> - 基于 SQL 语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL 写在 XML 里，解除 sql 与程序代码的耦合，便于统一管理；提供 XML 标签，支持编写动态 SQL 语句，并可重用。
> - 与 JDBC 相比，减少了 50%以上的代码量，消除了 JDBC 大量冗余的代码，不需要手动开关连接；
> - 很好的与各种数据库兼容（因为 MyBatis 使用 JDBC 来连接数据库，所以只要 JDBC 支持的数据库 MyBatis 都支持）。
>     能够与 Spring 很好的集成；
> - 提供映射标签，支持对象与数据库的 ORM 字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

## 42）Mybatis 的优点

> 1. Sql 语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写 Sql 语句的功底有一定要求。
> 2. 对性能的要求很高，或者需求变化较多的项目，如互联网项目，MyBatis 将是不错的选择。

## 43）Mybatis 的编程过程是怎样的

> 1. 创建 SqlSessionFactory
> 2. 通过 SqlSessionFactory 创建 SqlSession
> 3. 通过 sqlsession 执行数据库操作
> 4. 调用 sqlsession.commit()提交事务
> 5. 调用 sqlsession.close()关闭会话

## 44）Mybatis 中#和$的区别？

> 1. ${}是 Properties 文件中的变量占位符，它可以用于标签属性值和 sql 内部，属于静态文本替换
>
> 2. #{}是 sql 的参数占位符，Mybatis 会将 sql 中的#{}替换为?号，在 sql 执行前会使用PreparedStatement 的参数设置方法，按序给 sql 的? 号占位符设置参数值。
>
> 3. #方式能够很大程度防止 sql 注入。
>     $方式无法防止 Sql 注入。
>     $方式一般用于传入数据库对象，例如传入表名。
>
> 4. 为什么 # 可以防止SQL注入？参考作者：https://www.cnblogs.com/coder-who/
>
>     - 1.什么是SQL注入
>
>         答：SQL注入是通过把SQL命令插入到web表单提交或通过页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL指令。
>
>         　　注入攻击的本质是把用户输入的数据当做代码执行。
>
>         　　举例如: 表单有两个用户需要填写的表单数据，用户名和密码，如果用户输入admin(用户名)，111(密码)，若数据库中存在此用户则登录成功。SQL大概是这样
>
>         　　　　　　SELECT * FROM XXX WHERE userName = admin and password = 111
>
>         　　　　　但若是遭到了SQL注入，输入的数据变为 admin or 1 =1 # 密码随便输入，这时候就直接登录了，SQL大概是这样
>
>         　　　　　　SELECT * FROM XXX WHERE userName = admin or 1 = 1 # and password = 111 ,因为 # 在sql语句中是注释，将后面密码的验证去掉了，而前面的条件中1 = 1始终成立，所以不管密码正确与否，都能登录成功。
>
>         2.mybatis中的#{} 为什么能防止sql注入，${}不能防止sql注入
>
>         答: ==#{}在mybatis中的底层是运用了PreparedStatement 预编译，传入的参数会以 ? 形式显示，因为sql的输入只有在sql编译的时候起作用，当sql预编译完后，传入的参数就仅仅是参数，不会参与sql语句的生成，而${}则没有使用预编译，传入的参数直接和sql进行拼接，由此会产生sql注入的漏洞。==
>
>     - **再次理解sql预编译前后传参数的区别？**参考作者：https://blog.csdn.net/weixin_46099269
>
>         - select * from user where uid=#{id} and password=#{pwd};
>             ==这时数据库就会进行预编译，并进行一个缓存动作，缓存一条这样的语句：
>             select * from user where uid=? and password=?;
>             当我们调用这条语句，并实际向#{id}中的id传了一个值 “deftiii” or 1=1# 时，不需要在编译，数据库会直接找对应的表中有没有名字是 “deftiii” or 1=1# 的用户，而不再有编译sql语句的过程。==

## 45）使用 MyBatis 的 mapper 接口调用时有哪些要求？

> - Mapper==接口方法名==和 mapper.xml 中定义的每个 ==sql 的 id== 相同
> - Mapper 接口方法的==输入参数类型==和 mapper.xml 中定义的==每个 sql 的 parameterType 的类型==相同
> - Mapper 接口方法的==输出参数类型==和 mapper.xml 中定义的每个 ==sql 的 resultType== 的类型相同
> - Mapper.xml 文件中的 ==namespace== 即是 ==mapper 接口的类路径==。

## 46）简述 Mybatis 的 Xml 映射文件和 Mybatis 内部数据结构间的映射关系？

> - **Mybatis 将所有 Xml 配置信息都封装到 All-In-One 重量级对象 Configuration 内部。**
> - **<parameterMap>标签会被解析为 ParameterMap 对象，其每个子元素会被解析为ParameterMapping 对象。**
> - **<resultMap>标签会被解析为 ResultMap 对象，其每个子元素会被解析为ResultMapping 对象。** 
> - 每一个<select> 、<insert> 、<update> 、<delete> 标签均会被解析为MappedStatement 对象，标签内的 sql 会被解析为 BoundSql 对象。

## 47）Mybatis 的 Xml 映射文件中，不同的 Xml 映射文件，id 是否可以重复？

> 可以！
>
> ==不同的 Xml 映射文件，如果配置了 namespace，那么 id 可以重复；如果没有配置 namespace， 那么 id 不能重复。==

## 48）谈谈Mybatis 缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。（==缓存的本质就是把你查询过的东西暂时保存一下，如果两次查询都用的同一个session或者是同一个namespace，那么同样的sql语句就不用多次执行了。==）

    **一级缓存和二级缓存**

    - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
    - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
    - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存
        - ![image-20220523172911174](Java面试题总结/image-20220523172911174.png)

#### 一级缓存失效的四种情况

1. 没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！sqlSession不同。
2. sqlSession相同，查询条件不同
   1.   User user = mapper.queryUserById(1);
   2.  User user2 = mapper2.queryUserById(2);
3. sqlSession相同，两次查询之间执行了增删改操作！
4. sqlSession相同，手动清除一级缓存
   - session.clearCache();		//手动清除缓存

![img](Java面试题总结/kuangstudy203221f0-73d7-4d4c-bb81-84b1af9a63db.png)



#### 小结

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- 查出的数据都会被默认先放在一级缓存中
- 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中



## 49）Mybatis 分页

> 1. limit分页
>
> 2. RowBounds 进行分页，非常方便，不需要在 sql 语句中写 limit，即可完成分页功能。但是由于它是在 sql 查询出所有结果的基础上截取数据的，所以在数据量大的sql中并不适用，它更适合在返回数据结果较少的查询中使用。
>
>     最核心的是在 mapper 接口层，传参时传入 RowBounds(int offset, int limit) 对象，即可完成分页。
>
> 3. Mybatis分页插件PageHelper

2. **RowBounds的使用**

    mapper 接口层代码如下

​		List<Book> selectBookByName(Map<String, Object> map, RowBounds rowBounds);
​		调用如下

​		List<Book> list = bookMapper.selectBookByName(map, new RowBounds(0, 5));
​		原文链接：https://blog.csdn.net/wsjzzcbq/article/details/83447948

3. **PageHelper的使用**

    PageHelper参考自：https://www.jianshu.com/p/50fcd7f127f0

    在service中,先开启分页，然后把查询结果集放入PageInfo中。

    ```java
    public PageInfo listUserByPage(int pageNum, int pageSize) {
            PageHelper.startPage(pageNum, pageSize);
            List<UserVo> userVoList=userMapper.listUser();
            PageInfo pageInfo=new PageInfo(userVoList);
            return pageInfo;
        }
    ```

​	PageHelper.startPage(pageNum, pageSize);这句非常重要，这段代码表示分页的开始，意思是从第pageNum页开始，每页显示	pageSize条记录。

​	PageInfo这个类是插件里的类，这个类里面的属性会在输出结果中显示,
使用PageInfo这个类,你需要将查询出来的list放进去

----



# 六、Hibernate

## 50）简述一下 hibernate 的开发流程

> 第一步：加载 hibernate 的配置文件，读取配置文件的参数(jdbc 连接参数，数据 库方言，hbm 表与对象关系映射文件)
> 第二步：创建 SessionFactory 会话工厂(内部有连接池)
> 第三步：打开 session 获取连接，构造 session 对象(一次会话维持一个数据连接， 也是一级缓存)
> 第四步：开启事务
> 第五步：进行操作
> 第六步：提交事务
> 第七步：关闭 session(会话)将连接释放第八步：关闭连接池

## 51）Hibernate 和 JDBC 对比

==共同点==：1.Java数据库操作中间件，线程不安全，显式事务处理。

==不同点==：

1. JDBC 是 SUN 公司提供一套操作数据库的规范，而Hibernate 是一个基于 jdbc 的主流持久化框架，==对 JDBC 访问数据库的代码做了封装。==
2. 使用的SQL语言不同：JDBC 使用的是基于关系型数据库的标准 SQL 语言，==Hibernate 使用的是 HQL(Hibernate query language)语言。==
3. 操作的对象不同：JDBC 操作的是数据，将数据通过 SQL 语句直接传送到数据库中执行，==Hibernate 操作的是持久化对象，由底层持久化对象的数据更新到数据库中。==
4. 数据状态不同：
5. JDBC 操作的数据是“瞬时”的，变量的值无法与数据库中的值保持一致， 而 Hibernate 操作的数据是可持久的，即==持久化对象的数据属性的值是可以跟数据库中的值保持一致的。==

## 52）说说 hibernate 的三种状态之间如何转换？

hibernate 的三种状态是瞬时状态、持久状态、托管状态：

> 比如有一个 User 实体类和一张 User 表。当 new 了一个 user 对象，但没有开启事务。此时 user就处于==瞬时状态==，与数据库的数据没有任何联系，
>
> 当开启事务后，执行了 session.save()方法后，session 缓存中存放了该 user 对象，而数据库中也有相应的这条数据，此时就转换为==持久状态==。
>
> 当事务提交后，session 被销毁。session 缓存中就没有 user 对象，而数据库表中有相应记录，此时为==托管状态==。

## 53）如何搭建一个 Hibernate 的环境

> 1. 先导入 jar 包与配置文件、hibernate 启动 session 的工具类。
> 2. 在配置文件中配置数据库的基本信息与数据库方言
> 3. 进行测试，先创建实体类和数据库中的表。创建映射文件，命名规则是 实体类名.hbm.xml。位置要与实体类同一包下。在映射文件中配置 实体类与数据库表之间的映射关系。在hibernate.cfg.xml 配置文件中添加映射文件的路径。
> 4. 通过 hibernate 的工具类创建 sessionfactory，通过工厂创建 session 对象，通过 session 开启事务， 进行数据操作后，事务提交。

# 七、Struts2

## 54）Struts2 执行流程

> - 客户端发送请求，请求到达服务端，由 struts 的核心控制器==ActionServlet==拦截请求。
>
> - 核心控制器调⽤ action 映射器匹配请求路径和映射路径，判断映射路径是否存在。
>
> - 核心控制器调⽤ actionProxy 代理器，actionProxy 代理调⽤配置管理器，配置管理器解析
>     struts.xml，匹配要 访问的 action，返回结果给 actionProxy 。
>
> - actionProxy 代理调⽤对应的 action，执⾏业务逻辑操作，调⽤之前执⾏⼀系列的拦截器(封装请求参 数，数据校验等操作)。
>
> - action 返回 string 字符串，配置管理器确定返回结果，倒着执⾏⼀系列的拦截器。
>
> - 返回结果给客户端。
>
> （图片来自：https://www.programminghunter.com/）

![img](Java面试题总结/bb8ae6858c477965a471f82c4bcad6bc.jpeg)



## 55）列举 Struts2 中引入的一些有用的注释？

> @Action 创建动作类
> @Actions 为多个动作配置单个类
> @Namespace 和@Namespaces 用于创建不同的模块
> @Result 用于结果页面
> @ResultPath 用于配置结果页面位置

## 56）SpringMVC 和 Struts2 的区别？

1. **Struts2 是类级别的拦截**， 一个类对应一个 request 上下文，**SpringMVC 是方法级别的拦截**， 一个方法对应一个 request 上下文，而方法同时又跟一个 url 对应，所以说从架构本身上SpringMVC 就容易实现 restful url。
2. 由于 **Struts2** 需要针对每个 request 进行封装，把 request，session 等 servlet 生命周期的变量封装成一个一个 Map，供给每个 Action 使用，并保证线程安全，所以在原则上，**是比较耗费内存的**。
3. **拦截器实现机制**上，Struts2 有以自己的 interceptor 机制，SpringMVC 用的是独立的 AOP方式
4. SpringMVC 的入口是 servlet，而 Struts2 是 filter 
    - 区别：
        1. servlet流程是短的，url传来之后，就对其进行处理，之后返回或转向到某一自己指定的页面。它主要用来在业务处理之前进行控制；
        2. filter流程是线性的，url传来之后，检查之后，可保持原来的流程继续向下执行，被下一个filter，servlet接收等，而servlet处理之后，不会继续向下传递。filter功能可用来保持流程继续按照原来的方式进行下去，或者主导流程，而servlet的功能主要是用来主导流程。
        3. filter可用来进行字符编码的过滤，检查用户是否登陆的过滤，禁止页面缓存等。
            （此处引用自）原文链接：https://blog.csdn.net/weixin_42669555/article/details/81049423
5. **SpringMVC 集成了 Ajax**，使用非常方便，只需一个注解@ResponseBody 就可以实现，然后直接返回响应文本即可。
6. **Spring MVC 和 Spring 是无缝的。**
7. **设计思想上**，Struts2 更加符合 OOP 的编程思想， SpringMVC 就比较谨慎，在 servlet 上扩展。
8. SpringMVC **开发效率**（几乎可以认为0配置）和**性能**高于 Struts2。



---



# 八、Spring

## 57）什么是 Spring 的依赖注入

==**IOC（ Inversion of Control ）的⼀个重点是在系统运行中,动态的向某个对象提供它所需要的其他对象。**==

其中依赖注入（DI Dependency Injection）是实现IOC的一种方式。

a.接口注入

b.setter方法注入

c.构造方法注入

d.注解方式注入

> ==平常我们 new 一个实例，这个实例的控制权是我们程序员，而控制反转是指 new 实例工作不由我们程序员来做而是交给 spring 容器来做。==

## 58）Spring 中的设计模式

a. 单例模式——spring 中两种代理方式，若目标对象实现了若干接口， spring 使用 jdk 的java.lang.reflect.Proxy-Java 类代理。若目标兑现没有实现任何接口，spring 使用 CGLIB 库生成目标类的子类。单例模式——在 spring 的配置文件中设置 bean 默认为单例模式。
b. 模板方式模式——用来解决代码重复的问题。比如：RestTemplate、JmsTemplate、JpaTemplate
c. 前端控制器模式——spring 提供了前端控制器 DispatherServlet 来对请求进行分发。
d. 试图帮助（viewhelper）——spring 提供了一系列的 JSP 标签，高效宏来帮助将分散的代码整合在试图中。
e. 依赖注入——贯穿于 BeanFactory/ApplacationContext 接口的核心理念
f. 工厂模式——在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用同一个接口来指向新创建的对象。Spring 中使用 beanFactory 来创建对象的实例。

## 59）怎样开启注解装配？

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在 Spring 配置文件中配置<context:annotation-config/>元素。

## 60）Spring 的常用注解

```java 
@Required：该注解应用于设值方法
@Autowired：该注解应用于有值设值方法、非设值方法、构造方法和变量。
@Qualifier：该注解和@Autowired 搭配使用，用于消除特定 bean 自动装配的歧义。
```

## 61）简单解释一下 Spring 的 AOP

==AOP （ AspectOrientedProgramming ），即 面 向 切 面 编 程 ， 可 以 说 是 OOP（ObjectOrientedProgramming，面向对象编程）的补充和完善。==

> **OOP 引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。不过 OOP 允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。**
>
> 在 OOP 设计中，它导致了大量代码的重复，而不利于各个模块的重用。AOP 技术恰恰相反，它利用一种称为"横切" 的技术，==剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。==所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

## 62）Spring 的通知是什么？有哪几种类型？

> 通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过 SpringAOP 框架触发的代码段。Spring 切面可以应用五种类型的通知：
> 1）before：前置通知，在一个方法执行前被调用。
> 2）after:在方法执行之后调用的通知，无论方法执行是否成功。
> 3）after-returning:仅当方法成功完成后执行的通知。
> 4）after-throwing:在方法抛出异常退出时执行的通知。
> 5）around:在方法执行之前和之后调用的通知。



# 九、SpringMVC

## 63）SpringMVC 的流程

> a.用户向服务器发送请求，请求被 SpringMVC ==前端控制器 DispatchServlet 捕获==；
>
> b.DispatcherServlet 对请求 URL 进行解析，得到请求资源标识符（URL），然后根据该 URL 调用 HandlerMapping 将请求映射到处理器 HandlerExcutionChain；
> c.DispatchServlet 根据获得 Handler 选择一个合适的 HandlerAdapter 适配器处理；
> d.Handler 对数据处理完成以后将返回一个 ModelAndView（）对象给 DisPatchServlet;
> e.Handler 返回的 ModelAndView()只是一个逻辑视图并不是一个正式的视图，DispatcherSevlet 通过 ViewResolver 试图解析器将逻辑视图转化为真正的视图 View ;
> h.DispatcherServle 通过 model 解析出 ModelAndView()中的参数进行解析最终展现出完整的 view 并返回给客户端;

## 64）SpringMVC 的主要组件

> ==前 端 控 制 器 DispatcherServlet== ，作 ⽤ ：接 受 请 求 、响 应 结 果 相 当 于 转 发 器 ， 有 了 DispatcherServlet 就减少了其他组件之间的耦合度。
> ==处理器映射器 HandlerMapping==，作⽤：根据请求的 URL 来查找 Handler。
> ==处理器适配器 HandlerAdapter==，注意：在编写 Handler 的时候要按照 HandlerAdapter 要求的 规则去编写，这样适配器 HandlerAdapter 才可以正确的去执⾏ Handler。
> ==处理器 Handler(需要程序员开发)==。
>
> ==视图解析器 ViewResolver==。
> ==视图 View(需要程序员开发)==。

## （65）SpringMVC 的核心4入口类是什么？Struts1,Struts2 的分别是什么？

**SpringMVC 的 是 DispatcherServlet** 

**Struts1 的是 ActionServlet** 

**Struts2 的是 StrutsPrepareAndExecuteFilter**

# 十、SpringBoot

## （66）SpringBoot 简介

> Spring Boot 是 Spring 开源组织下的子项目，是 Spring 组件一站式解决方案，它的产⽣简化了框架的使⽤，所谓简化，是指简化了 使用 Spring 的难度，简省了繁重的配置，**提供了各种启动器，开发者能快速上手**，所以 SpringBoot 是⼀个服务于框架的框架，服务范围是简化配置⽂件。Spring Boot 优点，如：
> （1）独立运行（2）简化配置（3）自动配置（4）无代码生成和 XML 配置（5）应用监控（6） 上手容易

## （67）SpringBoot 默认启动方式是什么？

运⾏带有 mian ⽅法类。
类 上 需 要 加 ==@SpringBootApplication 注 解== ， main ⽅ 法 中 使 ⽤
**SpringApplication.run(类名.class，args);⾃动加载 application.properties ⽂件。**

## （68）SpringBoot 的配置⽂件有哪几种格式？它们有什么区别？

1. properties 和 yml，它们的区别主要是书写格式不同。

![image-20220527105924589](Java面试题总结/image-20220527105924589.png)

3. yml 格式不⽀持@PropertySource 注解导⼊配置。



## （69）如何在自定义端口上运行 Spring Boot 应用程序？

**为了在自定义端口上运行 Spring Boot 应用程序，您可以在 application.properties 中指定端口。server.port = 8090** 

## （70）SpringBoot 的核心注解是哪个？它主要由哪几个注解组成的？

> **启动类上⾯注解是@SpringBootApplication**，它也是 SpringBoot 的核⼼注解，主要包含 了以下 3 个注解： 包 括 @ComponentScan ， @SpringBootConfiguration，@EnableAutoConfiguration。
>
> @**EnableAutoConfiguration** 的作⽤启动⾃动的配置， @EnableAutoConfiguration 注解就是SpringBoot 根据你添加的 jar 包来配置你项⽬的默认配置，⽐如根据 spring-boot-starter-web， 来判断你项⽬是否添加了 webmvc 和 tomcat，就会⾃动帮你配置 web 项⽬中所需要的默 配置。
>
> @**ComponentScan** 扫 描 当 前 包 及 其 ⼦ 包 下 被 @Component ， @Controller ， @Service ， @Repository 注解标记的类并纳⼊ spring 容器中进⾏管理。
>
> @**SpringBootConfiguration** 继承⾃@Configuration，⼆者功能也⼀直，标注当前类是配置类，并会将当前类内声明的⼀个或多个以@Bean 注解标记的⽅法的实例纳⼊到 spring 容器中 并且实例名就是⽅法名。

## （71） 你如何理解 SpringBoot的Starters

> ==Starters可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，==
> 你可以一站式集成Spring及其他技术，而不需要到处找示例代码和依赖包。
> 如你想使用Spring 访问数据库，只要加入springboot-starter-data-jpa 启动器依赖就能使用了。Starters包含了许多项目中需要用到的依赖，它们能快速持续的运行，都是一系列得到支持的管理传递性依赖。
>
> 原文链接：https://blog.csdn.net/m0_51684972/article/details/110928657
>
> JPA （Java Persistence【坚持】 API）Java**持久化**API。是一套Sun公司Java官方制定的**ORM** 方案,是规范，是标准 。

## （72）springboot 中常用的 starter 的组件有哪些？

```java
spring-boot-starter-parent 	//boot 项目继承的父项目模块. 
spring-boot-starter-web 	//boot 项目集成 web 开发模块.
spring-boot-starter-tomcat  //boot 项目集成 tomcat 内嵌服务器. 
spring-boot-starter-test 	//boot 项目集成测试模块.
mybatis-spring-boot-starter //boot 项目集成 mybatis 框架.
spring-boot-starter-jdbc 	//boot 项目底层集成 jdbc 实现数据库操作支持.
其他诸多组件，可到 maven 中搜索，或第三方 starter 组件到 github 上查询
```










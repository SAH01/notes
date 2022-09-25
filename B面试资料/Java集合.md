# Java集合

为了保存数量不确定的数据，以及保存具有映射关系的数据（也被称为关联数组），Java 提供了集合类，也称为容器，位于 java.util 包下。

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/20191124140050653.png)



Iterator接口和Iterable接口没有实现和继承的关系，这两个接口的关系在于Iterable可以返回一个Iterator实例对象。

> （1）forEach()方法：解决 for 循环依赖下标的问题，为高效遍历更多的数据结构提供了支持。
>
> （2）spliterator()方法：提供了一个可以并行遍历元素的迭代器，以适应现在cpu多核时代并行遍历的需求。
>
> （3）iterator()方法：可以返回一个Iterator实例对象。
>
> ​	其中1和2是Java1.8之后新加的方法。

```java
	default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
```

**关于forEach方法：**

>  只要想使用foreach循环遍历，就必须正确地实现Iterable接口

foreach循环==遍历对象的时候底层是使用迭代器进行迭代的==，其最终被**编译器转为了对Iterator.next()的调用**。

foreach循环==遍历数组的时候将其转型成为对每一个数组元素的循环引用==。


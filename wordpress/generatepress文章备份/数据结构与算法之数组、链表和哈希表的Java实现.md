# 数据结构与算法之数组、链表和哈希表的Java实现

# 1、数组

类型固定、长度固定

连续的内存空间

顺序存储、随机读取

查询快、新增删除慢。**最好初始化的时候就指定数组大小。**这样就可以避免一定的数组扩容出现的内存消耗。

-----------

```java
import java.util.Arrays;
import java.util.Iterator;

/**
 * @author Administrator
 * @date 2022-09-11 16:56
 * 实现一个数组
 */
public class MyArray<E> implements Iterable<E> {

	private Object[] elementData;   // Object存放数据

	public MyArray(int capacity)    // 构造方法 初始化容量大小
	{
		// 指定长度 初始化数组 new 出一块空间
		elementData = new Object[capacity];
	}

	/**
	 * 直接添加新元素
	 * @param element
	 * @return
	 */
	public boolean add (E element)
	{
		int size = elementData.length;  // 获取当前数组大小
		int newCapacity = size+1;   // 扩容+1
		// 此处发生性能消耗，新增数据时，需要扩容，整体数据需要复制迁移，实际上arraylist是1.5扩容！
		elementData = Arrays.copyOf(elementData,newCapacity); // 把旧的空间复制一份到新的空间并+1
		elementData[size]=element;
		return true;
	}

	/**
	 * set 方法 根据索引位置新增元素
	 * @param index
	 * @param element
	 * @return
	 */
	public E set (int index ,E element)
	{
		E oldValue = (E) elementData[index];    // 获取旧位置的元素值
		elementData[index] = element;           // 新值覆盖旧值
		return oldValue;          // 返回旧值

	}
	public E get (int index)
	{
		return (E) elementData[index]; // 返回对应索引位置的值
	}

	@Override
	public Iterator<E> iterator(){
		return new MyIterator();
	}

	class MyIterator implements Iterator<E>{
		int index = 0;
		@Override
		public boolean hasNext() {
			return index != elementData.length;
		}

		@Override
		public E next() {
			return (E) elementData[index++];    // 返回下一个元素值并+1
		}

		@Override
		public void remove() {
			//
		}
	}

	public static void main(String[] args) {
		MyArray<String> myArray= new MyArray<String>(10);   // 初始化一个容量为10的数组
		myArray.set(0,"q");
		myArray.set(2,"w");
		myArray.add("新增");
		Iterator<String> iterator = myArray.iterator();     // 使用迭代器
		while (iterator.hasNext()){
			System.out.println(iterator.next());
		}
	}
}
```

![image-20220911183429296](https://s2.51cto.com/images/blog/202209/12132955_631ec3d32865c59508.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 1.1、关于arraylist初始容量和扩容

ArrayList 新增元素的方法有两种，一种是直接将元素加到数组的末尾，另外一种是添加元素到任意位置。

arraylist默认构造器，在不指定大小的时候默认容量为 ==10==。

在超出容量之后，每次扩容为==当前容量大小的1.5倍+1==。

## 1.2、关于迭代器

集合的顶层接口`Collection`继承`Iterable`接口。在`Iterable接口`中有一个`Iterator方法`，它返回一个`Itertator对象`。

```java
public interface Iterable<T> {
    /**
     * Returns an iterator over elements of type {@code T}.
     *
     * @return an Iterator.
     */
    Iterator<T> iterator();
}
```

```java
public interface Iterator<E> {
    boolean hasNext();
    
    E next();
    
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }
}
```

迭代器遍历中**调用集合revome()方法触发异常** java.util.ConcurrentModificationException 集合中并发修改的异常.

因为迭代器只负责遍历，它使用的仍然是集合本身的数据，在List集合实现的时候**数组的长度size会因为remove发生变化的，同时元素的索引值也会因为remove( )方法的调用而发生变化**。那么在遍历的时候的remove就需要对这个点进行复刻，而且如果在迭代器里使用了List原生的remove方法，那么就会引起数值不同步的问题。

在`ArrayList`集合的`iterator()`方法中，是通过返回`Itr`对象来获得迭代器的。`Itr`是`ArrayList`的一个内部类，它实现了`Iterator`接口，*代码如下：*

```java
   private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        Itr() {}

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
```

注意以下的三个属性：

| cursor           | 索引下标，表示下一个可以访问的元素的索引，默认值为 0 |
| ---------------- | ---------------------------------------------------- |
| lastRet          | 索引下标，表示上一个元素的索引，默认值为 **-1**      |
| expectedModCount | 对集合修改的次数，初始值为 **0**                     |

我们知道：**List的add和remove调用会增加modCount的值。**也就是这两个操作会被计入对集合的修改次数。

在迭代器的源码中，有一个方法是用来判断 modCount 和 expectModCount 的值是否相等的，其中modCount的值来自List，expectModCount 是迭代器内定义的变量。那为什么要这么设计呢？

==因为arraylist是线程不安全的。==

结合iterrator的next方法，我们可以看到，**如果没有这个校验**，**某个线程删除了list的一个元素，此时next方法不知道size变更了，依然去取数组里的数据，会导致数据为null或ArrayIndexOutOfBoundsException异常等问题。**

>  ConcurrentModificationException发生在Iterator( )和next( )方法实现中，每次调用都会检查容器的结构是否发生变化，目的是为了避免共享资源而引发的潜在问题。
>
>  观察HashMap和ArrayList底层Iterator#next(), 可以看到fast-fail只会增加或者删除（非Iterator#remove()）抛出异常；改变容器中元素的内容不存在这个问题（主要是modCount没发生变化）。
>
>  在单线程中使用迭代器，对非线程安全的容器，但是只能用Iterator和remove；否则会抛出异常。
>
>  在多线程中使用迭代器，可以使用线程安全的容器来避免异常。
>
>  使用普通的for循环遍历，效率虽然比较低下，但是不存在ConcurrentModificationException异常问题，用的也比较少。

所以说如果在使用迭代器的时候，用到了List自带的remove方法，那么modCount改变了，但是迭代器内定义的变量expectedCount却没有改变，这样就会被抛出异常。

综上：我们在使用迭代器的时候，不要混用List本身的remove方法。

----

## 1.3、为什么在调用remove之前要先调用next

当使用Iterator迭代访问Collection集合元素时，Collection集合里的元素不能被改变，**只有通过Iterator的remove()方法删除上一次next()方法返回的集合元素才可以**；否则会引发java.util.ConcurrentModificationException异常。

查看next方法的源码可以看到 `return (E) elementData[lastRet = i];`这样一行代码，这行代码表示next方法在让数组下标cursor向后移动一位的同时，还会把lastRet的值变成当前返回的元素下标，这样remove方法就可以根据这个下标完成对元素的删除。

# 2、哈希表

## 2.1、哈希冲突

冲突位置，把数据构建为链表结构。

**装载因子=哈希表中的元素个数 / （散列表）哈希表的长度**

装载因子越大，说明链表越长，性能就越低，那么哈希表就需要扩容，把数据迁移到新的哈希表中！

数据会经过两层比较：

一个是对哈希值的比较 使用hashcode（）方法

另一个是对key值的比较，使用equals（）方法

**如果两个对象相同，hashcode一定相同。**
**但是hashcode相同，两个对象不一定相同。**

>  ==哈希冲突是指，不同的key经由哈希函数，映射到了相同的位置上，造成的冲突。==

```java
/**
 * @author Administrator
 * @date 2022-09-09 14:28
 */
public class MyHashTable<K,V> {
	private Node<K, V>[] table;    // 实例化一个Node数组 Node数组本身又是个链表

	private static class Node<K,V>{
		final int hash; //hash值
		final K key;    
		V value;
		Node<K,V> next; //下一个节点 kv对

		public Node(int hash, K key, V value, Node<K, V> next) {
			this.hash = hash;
			this.key = key;
			this.value = value;
			this.next = next;
		}
	}

	public MyHashTable(int capacity) {
		table = (Node<K,V>[]) new Node[capacity];
	}
	public void put (K key,V value){
		// 1. 计算哈希值
		int hash = hash(key);
		// 2. 计算数组的索引值
		int i = (table.length-1) & hash;
		// 3. 把当前的kv对 存到Node里
		Node<K,V> node = new Node (hash,key,value,null);
		// 4. 根据索引下标 拿到目前table里的数据 然后进行对比
		Node<K,V> kvNode = table[i];

		if(kvNode == null ){
			// 如果为空 则说明当前位置没有数据 可以直接存
			table[i] = node;
			return;
		}
		// 如果有值 就去看 key是否一样
		if(kvNode.key.equals(key))
		{
			//  如果一样 就覆盖
			kvNode.value = value;
		}
		else{
			// 如果不一样就用链表存起来
			kvNode.next = node;
		}
	}
	public V get (K key){
		int hash = hash(key);
		int i = (table.length - 1 ) & hash;
		Node<K,V> node = table[i];
		if(node == null)
		{
			return null;
		}
		Node<K,V> newNode = node;      // 做条件判断
		// 如果存在这个值 而且存在hash冲突 那就需要去循环这个链表 直到找到那个key
		while (node.next != null)
		{
			if (newNode.key.equals(key)){
				// 这就说明找到了 直接跳出循环
				break;
			}else{
				newNode=newNode.next;   // 如果找不到key 就一直往链表的后面找
			}
		}
		return newNode.value;       // 最后返回这个值
	}

	/**
	 * 计算哈希值
	 * @param key
	 * @return
	 */
	static final int hash(Object key){
		int h;
		return (key == null)? 0 : (h = key.hashCode()) ^ (h >>> 16);
	}

	public static void main(String[] args) {
		MyHashTable<String,String> hashTable = new MyHashTable<String, String>(5);
		hashTable.put("key1","ycw");
		hashTable.put("key2","ycw2");
		hashTable.put("key1","ycw3");
		System.out.println(hashTable.get("key1"));
		System.out.println(hashTable.get("key2"));
	}
}
```

## 2.2、hashmap

### 扩容

	在填装因子(loaderFactor) > 0.75 时进行扩容

1. 创建新的数组，长度newLength是原来的2倍（将哈希表的存储空间增加为原来的两倍）。
2. 遍历旧列表中取出元素的key之后，重新进行hash计算 newndex = (newLength- 1) & hash。
3. 重新插入元素。

--------

# 3、链表

MyLinkedList 有一个头指针，一个尾指针，还有链表长度size

内有两个类，一个是实现了Iterator接口的迭代器类，另一个是Node类，其中Node数据结构中，==除了数据，还要有前一个Node和后一个Node变量。

`双向循环链表` 

代码如下：

```java
import java.util.Iterator;

/**
 * 双向循环链表
 * @author Administrator
 * @date 2022-09-11 22:57
 */
public class MyLinkedList<E> implements Iterable<E>{
	private Node<E> last;   // 尾指针
	private Node<E> first;  // 头指针
	private int size;       // 链表长度 每次插入要 +1


	@Override
	public Iterator<E> iterator() {
		return new MyIter() ;   // 返回一个迭代器
	}

	/**
	 * 实现Iterator接口的迭代器类
	 */
	class MyIter implements Iterator<E>{
		int index = 0;      // 从0开始遍历
		@Override
		public boolean hasNext() {
			return index != size;  //如果为真 就是还有下一个
		}

		@Override
		public E next() {
			return get(index++);    //　通过get方法得到链表的item值
		}

		@Override
		public void remove() {

		}
	}

	/**
	 * 节点类
	 * @param <E>
	 */

	private static class Node<E>{
		E item;         // 元素值
		Node<E> prev;   // 前一个Node
		Node<E> next;   // 后一个Node
		public Node(Node<E> prev,E item,Node<E> next){
			this.item=item;
			this.prev=prev;
			this.next=next;
		}
	}

	public boolean addLast (E element)
	{
		// 声明一个不变的尾结点
		final Node<E> l = last;
		// 把item装入一个Node里 下面开始插入
		Node<E> newNode = new Node<E>(l,element,null);
		// 先把尾结点指向新插入的结点
		last = newNode;   // 新插进来的就是最后一个
		// 如果是这是第一个元素 那么头尾结点其实都是这个新结点
		if (l == null)
		{
			first = newNode;     // 把这个新结点赋值给头结点
		}else{
			l.next = newNode;   // 如果不是新的 那么就正常指向下一个
		}
		size ++;            // 链表长度 +1
		return true;
	}

	public E set (int index , E element)
	{
		// 先判断index在哪
		Node<E> x = findNode(index);
		E oldValue = x.item;
		x.item = element;
		return oldValue;        // 返回修改前的item值
	}

	/**
	 * 找到指定索引上的Node并返回
	 * @param index
	 * @return
	 */
	private Node<E> findNode(int index){
		if(index < (size >> 1) )    // 如果index索引 小于链表总长的一半 那就从前往后找 直到index位置
		{
			Node<E> x = first;
			for(int i = 0 ; i < index; i++){
				x = first.next;
			}
			return x;
		}
		// 如果index索引 大于链表总长的一半 那就从后往前找 直到index位置
		Node<E> x = last;
		for(int i = size-1 ;i > index; i--){
			x = last.prev;
		}
		return x;
	}

	/**
	 * 获得值
	 * @param index
	 * @return
	 */
	public E get(int index)
	{
		
		return findNode(index).item;
	}

	public static void main(String[] args) {
		MyLinkedList<String> myLinkedList = new MyLinkedList<String>();
		myLinkedList.addLast("aaa1");
		myLinkedList.addLast("aaa2");
		myLinkedList.addLast("aaa3");
		myLinkedList.addLast("aaa4");
		myLinkedList.addLast("aaa5");
		myLinkedList.set(0,"set");
		myLinkedList.forEach (s -> {
			System.out.println(s);
		});

		Iterator myIter = myLinkedList.iterator();
		while (myIter.hasNext()){
			System.out.println(myIter.next());
		}
	}
}
```

![image-20220912114731965](https://s2.51cto.com/images/blog/202209/12132954_631ec3d2252f520062.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 3.1、forEach

forEach( )方法是java8新增的一个遍历方法，它被定义在`java.lang.Iterable` 接口中。

List、Map、Set、Stream等接口都实现了这个方法，所以可以直接使用这些类的实例化对象调用forEach方法遍历元素。

**比如遍历hashMap可以这么写：**

```java
 items.forEach((k,v)->System.out.println("Item : " + k + " Count : " + v));
```

**遍历List可以这么写：**

```java
myList.forEach(s -> System.out.println(s)); 
```
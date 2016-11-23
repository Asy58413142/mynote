### 什么是稀疏数组Sparse Array ###

所谓稀疏数组，就是数组中大部分的内容值都未被使用(或都为零)，在数组中仅有少部分的空间使用。因此造成空间内存的浪费，为了节省内存空间，并且不影响数组中原有内容值，我们可以采用一种压缩的方式表示稀疏数组的内容。

假设有一个9*7的数组，其内容如下：

![](http://i.imgur.com/jaRDjW7.jpg)

在此数组中，共有63个空间，但却只使用了5个元素，造成58个元素空间的浪费。以下我们就使用稀疏数组重新来定义这个数组：

![](http://i.imgur.com/9keleNK.jpg)

其中在稀疏数组中第一部分所记录的是原数组的列数以及元素使用的个数、第二部分所记录的是原数组中元素的位置和内容值。

经过压缩之后，原来需要声明大小为63的数组，而使用压缩后，只需要声明大小为6*3的数组，仅需18个存储空间。


### Android中的稀疏数组 ###

手机软件的开发不同于PC软件的开发，因为手机性能相对有限，内存也有限，所谓“寸土寸金”，可能稍有不慎，就会导致性能的明显降低。Android为了方便开发者，特意在android.util这个包中提供了几个提高效率的工具类，比如之前用过的LruCache类，还有稀疏数组工具类：SparseArray，SparseBooleanArray和SparseIntArray。

总体来说，它们都类似map这样key-value的存储方式，但是由于查找的算法不一样。因此效率也各不同。但要明白，没有说哪个一定是最好的。只有根据不同需求在不同场景去应用，才能获取较优的结果。


#### SparseArray

    package android.util;  
  
	import com.android.internal.util.ArrayUtils;  
	  
	/** 
	 * SparseArrays 利用integer去管理object对象。不像一个正常的object对象数组，它能在索引数中快速的查找到所需的结果。（这 
	 * 句话是音译，原意是能在众多索引数中“撕开一个缺口”，为什么原文这么表达？下面会慢慢说清楚。）它比HashMap去通过Integer索引 
	 * 查找object对象时在内存上更具效率,不仅因为它避免了用来查找的自动“装箱”的keys，并且它的数据结构不依赖额外的对象去 
	 * 各个映射中查找匹配。 
	 *  
	 * SparseArrays map integers to Objects.  Unlike a normal array of Objects, 
	 * there can be gaps in the indices.  It is intended to be more memory efficient 
	 * than using a HashMap to map Integers to Objects, both because it avoids 
	 * auto-boxing keys and its data structure doesn't rely on an extra entry object 
	 * for each mapping. 
	 * 
	 * 请注意，这个容器会保持它的映射关系在一个数组的数据结构中，通过二分检索法驱查找key。（这里我们终于知道，为何这个工具类中， 
	 * 提供的添加映射关系的操作中，key的类型必须是integer。因为二分检索法，将从中间“切开”，integer的数据类型是实现这种检索过程的保证。） 
	 *  
	 * 如果保存大量的数据，这种数据结构是不适合的，换言之，SparseArray这个工具类并不应该用于存储大量的数据。这种情况下，它的效率 
	 * 通常比传统的HashMap更低，因为它的查找方法并且增加和移除操作（任意一个操作）都需要在数组中插入和删除（两个步骤才能实现）。 
	 *  
	 * 如果存储的数据在几百个以内，它们的性能差异并不明显，低于50%。 
	 *  
	 * （OK，那么光看Android官方的介绍我们就有初步结论了，大量的数据我们相对SparseArray会优先选择HashMap，如果数据在几百个这个数目， 
	 *  那么选择它们任意一个去实现区别不大，如果数量较少，就选择SparseArray去实现。 其实如果我们理解了二分法，就很容易了SparseArray的 
	 *  实现原理，以及SparseArray和HashMap它们之间的区别了。） 
	 *  
	 * <p>Note that this container keeps its mappings in an array data structure, 
	 * using a binary search to find keys.  The implementation is not intended to be appropriate for 
	 * data structures 
	 * that may contain large numbers of items.  It is generally slower than a traditional 
	 * HashMap, since lookups require a binary search and adds and removes require inserting 
	 * and deleting entries in the array.  For containers holding up to hundreds of items, 
	 * the performance difference is not significant, less than 50%.</p> 
	 * 
	 *   
	 * 为了提高性能，这个容器包含了一个实现最优的方法：当移除keys后为了立刻使它的数组紧密，它会“遗留”已经被移除（标记了要删除）的条目（entry） 。 
	 * 所被标记的条目（entry）（还未被当作垃圾回收掉前）可以被相同的key复用，也会在垃圾回收机制当作所有要回收的条目的一员被回收，从而使存储的数组更紧密。 
	 *  
	 * （我们下面看源码就会发现remove()方法其实是调用delete()方法的。印证了上面这句话所说的这种优化方法。 
	 * 因为这样，能在每次移除元素后一直保持数组的数据结构是紧密不松散的。） 
	 *  
	 * 垃圾回收的机制会在这些情况执行：数组需要扩充，或者映射表的大小被恢复，或者条目值被重新检索后恢复的时候。 
	 *   
	 * <p>To help with performance, the container includes an optimization when removing 
	 * keys: instead of compacting its array immediately, it leaves the removed entry marked 
	 * as deleted.  The entry can then be re-used for the same key, or compacted later in 
	 * a single garbage collection step of all removed entries.  This garbage collection will 
	 * need to be performed at any time the array needs to be grown or the the map size or 
	 * entry values are retrieved.</p> 
	 * 
	 * 当调用keyAt(int)去获取某个位置的key的键的值，或者调用valueAt(int)去获取某个位置的值时，可能是通过迭代容器中的元素 
	 * 去实现的。 
	 * 
	 * <p>It is possible to iterate over the items in this container using 
	 * {@link #keyAt(int)} and {@link #valueAt(int)}. Iterating over the keys using 
	 * <code>keyAt(int)</code> with ascending values of the index will return the 
	 * keys in ascending order, or the values corresponding to the keys in ascending 
	 * order in the case of <code>valueAt(int)<code>.</p> 
	 */  
	public class SparseArray<E> implements Cloneable {  
	    //...  
	}


####这里总结下几个重要的点：
	1.SparseArray的原理是二分检索法，也因此key的类型都是整型。
	2.HashMap和SparseArray比较）当存储大量数据（起码上千个）的时候，优先选择HashMap。如果只有几百个，用哪个区别不大。如果数量不多，优先选择SparseArray。
	3.SparseArray有自己的垃圾回收机制。（当数量不是很多的时候，这个不必关心。）

####接着将里面的主要方法列出来：

    private int index = 1;  
    private String value = "value";  
      
    public void testSparseArray()  
    {  
        //创建一个SparseArray对象  
        SparseArray<String> sparseArray = new SparseArray<String>();  
          
        //向sparseArray存入元素value，key为index  
        sparseArray.put(index, value);  
          
        //这个方法本质也是利用put(key, value)去存入数据  
        sparseArray.append(index, value);  
          
          
        sparseArray.indexOfKey(index);  
        //查找value所在的位置，如果不存在，则返回-1  
        sparseArray.indexOfValue(value);  
          
          
          
        //更新某个key的值  
        sparseArray.setValueAt(index, value);  
          
          
          
        //获取index所对应的值，没有则返回null  
        sparseArray.get(index);  
        //获取index所对应的值，没有则返回自定义的默认值"default-value"  
        sparseArray.get(index,"default-value");  
          
          
          
        //删除index对应的元素  
        sparseArray.delete(index);  
        //移除，本质也是调用delete(int)方法  
        sparseArray.remove(index);  
          
          
          
        //清空所有数据  
        sparseArray.clear();  
          
    }


### SparseBooleanArray和SparseIntArray ###

SparseBooleanArray和SparseIntArray，其实看名字也知道，它们跟SparseArray极其类似，只是存储类型加以限制了。SparseBooleanArray只能存储boolean值，而SparseIntArray只能存储integer类型的值。它们也同样实现了Cloneable接口，可以直接调用clone方法，也同样是以二分法为依据。而其他的主要方法也是一样的。下面以SparseBooleanArray为简单例子写出主要的方法，从方法看出，两者和SparseArray的确是灰常类似的。SparseIntArray的代码就不再贴出来了，因为都一样的。我们在使用的过程中举一反三，会用一个，其他2个也就会用了呢。

    public void testSparseBooleanArray()  
    {  
          
	//      SparseBooleanArray sparseBooleanArray = new SparseBooleanArray();  
        //这种创建方式可以设置容器的大小  
        SparseBooleanArray sparseBooleanArray = new SparseBooleanArray(5);  
          
          
        //存入数据，同样有两种方法  
        sparseBooleanArray.put(int, boolean);  
          
        sparseBooleanArray.append(int, boolean);  
          
        //根据key获取对应的boolean值，没有则返回false  
        sparseBooleanArray.get(key);  
        //跟上面类似,valueIfKeyNotFound是自定义的假设不存在则返回的默认值  
        sparseBooleanArray.get(key, valueIfKeyNotFound);  
          
        //获取第5个位置的键值  
        sparseBooleanArray.keyAt(5);  
        //获取第5个元素的值  
        sparseBooleanArray.valueAt(5);  
          
        //删除某个key的元素  
        sparseBooleanArray.delete(key);  
        //清除所有  
        sparseBooleanArray.clear();  
          
    }

那么SparseBooleanArray和SparseIntArray也和SparseArray一样，存储不是太多的数据，它们都是作为比HashMap更好的选择。但数据是死的，功能也是死的，实现方式是灵活的。条条大路通罗马。我们不能一概而论说谁好谁差，放在具体的场景，才能选择更高效也更合乎成本的实现方式。


[Android中SparseArray<E>优化集合类详解](http://www.jianshu.com/p/a58015986bb9)


[Android开发中高效的数据结构用SparseArray代替HashMap](http://www.open-open.com/lib/view/open1429598959573.html)


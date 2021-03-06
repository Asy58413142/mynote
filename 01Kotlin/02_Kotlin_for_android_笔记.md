## 配置 ##

1.在AS里安装好Kotlin插件之

2.新建一个项目Project

3.在MainActivity中按Ctrl+Shift+Alt+K将java代码convet成Kotlin代码

4.随便将MainActivity做一下改变(哪怕一个空格)，AS就会在上面给你弹出让你Config Kotlin的按钮，点击

5.AS会自动配置Project的gradle.build和Module的gradle.build文件

6.如果你要用到kotlin-android-extensions插件，在你的Module的gradle.build文件中加上  apply plugin: 'kotlin-android-extensions'即可


## 类和函数 ##

### 定义一个类 ###

如果你想定义一个类，你只需要使用class关键字。

	class MainActivity{
	
	}

它有一个默认唯一的构造器。我们会在以后的课程中学习在特殊的情况下创建其它额外的构造器，但是请记住大部分情况下你只需要这个默认的构造器。你只需要在类名后面写上它的参数。**如果这个类没有任何内容可以省略大括号**：

	class Person(name: String, surname: String)

**那么构造函数的函数体在哪呢？你可以写在init块中**：

	class Person(name: String, surname: String) {
	    init{
	        ...
	    }
	}


### 类继承 ###

默认任何类都是基础继承自Any（与java中的Object类似），但是我们可以继承其它类。所有的类默认都是不可继承的（final），所以我们只能继承那些明确声明open或者abstract的类：

    open class Animal(name: String)
	class Person(name: String, surname: String) : Animal(name)	

当我们只有单个构造器时，我们需要在从父类继承下来的构造器中指定需要的参数。这是用来替换Java中的super调用的。

### 函数 ###

函数（我们Java中的方法）可以使用fun关键字就可以定义:

	fun onCreate(savedInstanceState: Bundle?) {
	}

如果你没有指定它的返回值，它就会返回Unit，与Java中的void类似，但是Unit是一个真正的对象。你当然也可以指定任何其它的返回类型：

	fun add(x: Int, y: Int) : Int {
	    return x + y
	}

然而**如果整个函数体和返回的结果可以使用一个表达式计算出来**，你可以不使用括号而是使用等号：

    fun add(x: Int,y: Int) : Int = x + y

函数体就是x + y的操作，返回值也是这个。

返回值类型也可以省略，因为Kotlin有**类型推断功能Type inference**，它会根据返回值x + y自动推断出类型是int：

    fun add(x: Int, y: Int) = x + y

### 构造方法的参数和函数的参数 ###

Kotlin中的参数与Java中有些不同。如你所见，我们先写参数的名字再写它的类型：

	fun add(x: Int, y: Int) : Int {
	    return x + y
	}

我们**可以给参数指定一个默认值使得它们变得可选**，这是非常有帮助的。这里有一个例子，在Activity中创建了一个函数用来toast一段信息：

	fun toast(message: String, length: Int = Toast.LENGTH_SHORT) {
	    Toast.makeText(this, message, length).show()
	}

第二个参数（length）指定了一个默认值。这意味着你调用的时候可以传入第二个值或者不传，这样可以避免你需要的重载函数：

    toast("Hello")
	toast("Hello", Toast.LENGTH_LONG)

kotlin写的这个toast方法与下面的Java代码是一样的(一个顶多个用，避免重载)：

	void toast(String message){
	}
	
	void toast(String message, int length){
	    Toast.makeText(this, message, length).show();
	}

再看看这个例子：

	fun niceToast(message: String,
	                tag: String = javaClass<MainActivity>().getSimpleName(),
	                length: Int = Toast.LENGTH_SHORT) {
	    Toast.makeText(this, "[$className] $message", length).show()
	}

我增加了第三个默认值是类名的tag参数。如果在Java中总数开销会以几何增长。现在可以通过以下方式调用：

	toast("Hello")
	toast("Hello", "MyTag")
	toast("Hello", "MyTag", Toast.LENGTH_SHORT)

而且甚至还有其它选择，因为你可以使用参数名字来调用，这表示你可以通过在值前写明参数名来传入你希望的参数：

	toast(message = "Hello", length = Toast.LENGTH_SHORT)

更神奇的是，你调用该方法的时候不用按照参数的顺序传参：

	toast(length = Toast.LENGTH_SHORT, tag = "ireya", message = "hello")

如果你没有写 参数名 = ，那么你就得在指定的参数位置传相应的参数。另外，要保证调用的时候的参数名和函数声明时候的参数名一致(AS会提示，很方便)

## 编写第一个类 ##

### 创建一个Layou ###

    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:id="@+id/activity_main"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    tools:context="cn.wang.kotlin.MainActivity">
	
	    <android.support.v7.widget.RecyclerView
	        android:id="@+id/recyclerview"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent">
	
	    </android.support.v7.widget.RecyclerView>
	
	</FrameLayout>

在MainActivity.kt中：

    val linearLayoutManager = LinearLayoutManager(this)
    linearLayoutManager.orientation = OrientationHelper.VERTICAL
    recyclerview.layoutManager = linearLayoutManager

RecyclerView的LayoutManager通过属性的方式被设置，而不是通过setter方法。

对象实例化：

	对象实例化也是与Java有些不同的。Kotlin去掉了new关键字。这时构造函数仍然会被调用，你传了几个参数就会去调用相应的构造函数。


### The Recycler  Adapter ###

我们同样需要一个RecyclerView的Adapter，该RecyclerView的每个条目都只是简单的包含一个TextView，我们直接用代码生成TextView即可。

增加一个ForecastListAdapter.kt的Kotlin文件，包括以下代码：


    /**
	 * KotlinDemo
	 * Created by Wang on 2016/11/24 14:51.
	 *
	 * 该Adapter类的主构造函数接收一个List<String>类型的items参数，并继承自RecyclerView.Adapter，泛型是我们的ViewHolder类型
	 **/
	class ForecastListAdapter(val items: List<String>) : RecyclerView.Adapter<ForecastListAdapter.ViewHolder>() {
	
	    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
	    //        将第position个数据设置给相应条目的textview
	        holder.textview.text = items[position]
	    }
	
	    /**
	     * getItemCount方法，返回值为items.size，根据Kotlin的类型推断Type Refernece可知，返回值类型为int
	     */
	    override fun getItemCount() = items.size
	
	    /**
	     * 同上，返回值类型是ViewHolder类型，这里直接用代码创建了一个TextView对象，并作为创建ViewHolder对象的参数。
	     */
	    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int) = ViewHolder(TextView(parent.context))
	
	
	    /**
	     * ViewHolder类，继承自RecyclerView.ViewHolder，主构造器接收一个TextView类型的参数，并用val声明，表示该变量是
	     * ViewHolder类的成员变量，修饰符是public final。
	     * 最后使用textview去调用父类的构造器，即相当于java中的super()
	     */
	    class ViewHolder(val textview: TextView) : RecyclerView.ViewHolder(textview)
	}


回到MainActivity，现在简单地创建一系列的String放入List中，然后使用创建分配Adapter实例。

    val items = listOf(
                "Mon 6/23 - Sunny - 31/17",
                "Tue 6/24 - Foggy - 21/8",
                "Wed 6/25 - Cloudy - 22/17",
                "Thurs 6/26 - Rainy - 18/11",
                "Fri 6/27 - Foggy - 21/10",
                "Sat 6/28 - TRAPPED IN WEATHERSTATION - 23/18",
                "Sun 6/29 - Sunny - 20/7"
        )

        recyclerview.adapter = ForecastListAdapter(items)

## 变量和属性 ##

在Kotlin中，**一切都是对象**。没有像Java中那样的原始基本类型。这个是非常有帮助的，因为我们可以使用一致的方式来处理所有的可用的类型。


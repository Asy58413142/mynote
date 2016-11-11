![](http://i.imgur.com/5pSsgxw.png)

想不想快速搞懂Android Studio中build.gradle的配置，想的话就先看看Gradle的粑粑Groovy吧。

## Groovy是什么 ##

- 基于JVM的语言，是Java中的一员
- 弱类型的动态语言

## Groovy的优势 ##

- 构建在Java语言之上，并结合了Python、Ruby、Smalltalk等语言的强大功能。
- 对Java开发者来说学习成本很低，无缝集成Java代码和类库。
- 既可用作面向对象语言，又可作为脚本语言使用。
- 引入了DSL（领域定义语言）概念，简单易读，方便维护。
- 支持函数式编程、运算符重载、闭包等特性。
- 可编译为Java字节码，在可运行Java的地方，就可运行Groovy。

## Groovy的数据类型 ##

Groovy是弱类型的动态语言，定义变量的时候可以不指定变量的类型。定义变量的时候使用def关键字，也可以省略，建议写上。

    def variable1 = 1    //可以不指定数据类型
	println variable1.getClass()
	def variable2 = "this is a string"    //不用在一行代码的末尾加;
	variable1 = 'change the type of variable1'    //动态改变变量的类型
	println variable1.getClass()
	
	variable1 = true
	println variable1.getClass()

输出结果：

    class java.lang.Integer
	class java.lang.String
	class java.lang.Boolean

也可以在定义变量的时候明确定义它的类型：

	def int x = 1    //在定义变量的时候也可以直接指定其类型
	x = 'haha'    //如果明确的指定了它的类型之后，就不能改变它的类型了，
	println x    //否则就会包 GroovyCastException

可以看到，variable1变量起初是Integer类型的，给它赋值一个String的值之后，它就变成了String类型了，这就是弱类型动态语言。

另外，Groovy中"一切皆对象"，基本类型都是以他们在Java中的包装类存在的。

### 字符串 ###

    ''：java.lang.String类型，除了转义的字符外，其它字符会原样输出，例如：'Hello \'Groovy\''的结果是Hello 'Groovy'
	"":这就是GString类型,除了基本的String所支持的转义之外，还支持变量、表达式的转义，例如："Hello $Groovy"，会引入变量Groovy的值(基本的String类型不支持)。GString还支持表达式的转义,例如："2+3=${2+3}"，输出结果是 2+3=5

#### 多行字符串 ####

	'''Hello
	World
	Groovy'''会按照原有的排版类型输出结果，包括换行符。

	"""Hello
	World
	$Groovy"""不但支持多行字符串，还支持变量的转义。

## 函数 ##

函数定义可以不指定参数的类型，也可以不指定返回值类型，在不指定返回值类型的时候，函数的最后一行表达式的结果就是返回值。

    def function(arg1,arg2){	//函数定义的时候可以不指定参数类型，调用的时候可以传入任意类型的参数
	    arg1+arg2	//因为该函数没有指定返回值类型，所以最后一样表达式的值就是返回值
	}
	
	def result=function 1,"test"	//调用函数的时候可以不加()
	println result

输出结果：

	1test
上面的 function 函数，如果你调用的时候传入 1,2，那么返回值就是3

## 容器类 ##

### List ###


List类

变量定义i:List变量由[]定义，比如


	def aList=[5,'string',true]    //List由[]定义，其中的元素可以是任何对象，没有泛型，也不安全

变量存取：可以直接通过索引存取，而且不用担心索引越界。如果索引超过当前链表长度，List会往该索引添加元素

	assert aList[1]=='string'	//true
	
	println "the 1 elememnt of aList is "+aList[1]	//string
		
	println "the 3 element of aList is "+aList[3]	//角标为3的元素按理说已经越界了，但是这里不会报越界异常，而是返回null
	
	println 'The length of aList: '+aList.size	//结果是3，因为取元素的时候即使越界了，也不会自动扩展list的长度
	
	aList[50]=true	//给角标为50的元素赋值为 true，这个时候aList的长度就会被扩展了，从0到50，即长度为51
	
	println 'the 50 element of aList is '+aList[50]	//true
	println aList[100]	//null
	
	println 'The length of aList: '+aList.size	//51
	
	println 'The elements of aList are: $aList'	//因为是''，所以会原样输出 $aList
	println "The elements of aList are:  $aList"	//因为是""，所以会转义，输出aList中的每个元素

### Map ###

变量定义：

	def aMap = ['key1':'value1','key2':true]

Map由[:]定义，冒号左边是键，冒号右边是值。键只能是String类型，值可以是任意类型。

    def aNewMap = [key1:"value",key2:true] //其中的key1和key2默认被处理成字符串"key1"和"key2"

不过Key要是不使用引号包起来的话，也会带来一定混淆，比如

	def key1="wowo"
	def aConfusedMap=[key1:"who am i?"]

aConfuseMap中的key1到底是"key1"还是变量key1的值“wowo”？显然，答案是字符串"key1"。如果要是"wowo"的话，则aConfusedMap的定义必须设置成：

	def aConfusedMap=[(key1):"who am i?"]

Map中元素的存取更加方便，它支持多种方法：

	println aMap.keyName    <==这种表达方法好像key就是aMap的一个成员变量一样
	println aMap['keyName'] <==这种表达方法更传统一点
	aMap.anotherkey = "i am map"  <==为map添加新元素

### Range ###

Range是Groovy对List的一种拓展，变量定义和大体的使用方法如下：

    def aRange = 1..5  <==Range类型的变量 由begin值+两个点+end值表示左边这个aRange包含1,2,3,4,5这5个值

如果不想包含最后一个元素，则
	
	def aRangeWithoutEnd = 1..<5  <==包含1,2,3,4这4个元素
	println aRange.from
	println aRange.to



## 闭包 ##

Groovy中闭包叫Closure，它是一种数据类型，代表了一段可执行的代码。其形式如下：

    def aClosure = {//闭包是一段代码，所以需要用花括号括起来..  
	    String param1, int param2 ->  //这个箭头很关键。箭头前面是参数定义，箭头后面是代码  
	    println"this is code" //这是代码，最后一句是返回值，  
	   //也可以使用return，和Groovy中普通函数一样  
	}  

简而言之，Closure的定义格式是：

    def xxx = {paramters -> code}  //或者  
	def xxx = {无参数，纯code}  这种case不需要->符号

闭包定义好后，要调用它的方法就是：

闭包对象.call(参数)  或者更像函数指针调用的方法：

闭包对象(参数)  

比如：

    aClosure.call("this is string",100)  或者  
	aClosure("this is string", 100) 

如果闭包没定义参数的话，则隐含有一个参数，这个参数名字叫it，和this的作用类似。it代表闭包的参数。

比如：

	def greeting = { "Hello, $it!" }
	assert greeting('Patrick') == 'Hello, Patrick!'

等同于：

	def greeting = { it -> "Hello, $it!" }
	assert greeting('Patrick') == 'Hello, Patrick!'

但是，如果在闭包定义时，采用下面这种写法，则表示闭包没有参数！

	def noParamClosure = { -> true }

这个时候，我们就不能给noParamClosure传参数了！

	noParamClosure ("test")  <==报错喔！

#### Closure的使用 ####

一般Closure可以作为一个参数传递给一个方法，就像Java中的回调接口一样。

先看下面的例子，看不懂没关系，下面有详细的解释：


    def aClosure = {arg1,arg2 ->	//定义一个闭包对象
		println 'I am from a closure '+arg1+" "+arg2
	}
	
	def testClosure(arg1,arg2,Closure closure){		//定义一个方法，接受三个参数，最后一个参数是闭包对象
		closure arg1,arg2
	}
	
	testClosure "canshu1","canshu2",aClosure	//调用上面的方法



- 1、首先是定义一个闭包对象，这个应该没什么问题。
- 2、第二块代码是定义一个方法，方法中参数的类型可以不声明，所以才会有arg1、arg2这样的形式存在。然后第三个参数是一个闭包对象。在方法体内去调用闭包对象的call方法，因为call方法可以省略，所以可以写成

		closure(arg1,arg2)

又因为括号也可以省略，所以就有了最终版：

		closure arg1,arg2

- 3、最关键的，第三块代码；调用之前定义好的函数，这里有一个规则，就是：如果一个函数的最后一个参数是Closure对象，那么小括号不用写。"canshu1"和"canshu2"分别作为这个函数的第一个和第二个参数，第三个参数就是之前定义好的Closure对象。在这个Closure中，接受两个参数:arg1、arg2。我们在调用testClosure方法的时候将"canshu1"和"canshu2"传递给了该函数，在该函数内部，又将这两个参数传递给了closure对象，所以Closure对象中接受到的参数分别是： arg1 = "canshu1",arg2 = "canshu2"。所以上面的例子运行结果就是：

    	I am from a closure canshu1 canshu2

如果上面的解释看清楚明白了，那么接着往下：

在Java中，我们使用接口回调的时候，习惯使用匿名内部类，上面的使用方式中aClosure相当于一个内部类，因为我们定义了，并用一个aClosure接受了。那么如何实现匿名内部类的效果呢：

原理一样，将aClosure对应的Closure对象不声明，直接使用到方法中即可：


    def testClosure(arg1,arg2,Closure closure){
		closure arg1,arg2
	}
	
	
	testClosure "canshu1","casnhu2",{
		arg1,arg2 ->
		println 'I am from a Closure '+arg1+' '+arg2
	}

- 第一块代码和上面的一样，同样是声明一个方法，最后一个参数是一个Closure对象
- 第二块代码是直接调用该方法，至于第三个参数，我们不声明，直接使用，就相当于Java中的匿名内部类。这个解释对于java开发人员应该能明白了。运行结果和上面的一样。


如果上面的这个能懂，那么下面这个也能懂了：

    def testClosure(Closure closure){
	    closure("hello","world")
	}
	
	testClosure {
	    arg1,arg2 ->
	    println 'I am from a Closure '+arg1+' '+arg2
	}

运行结果为：

	I am from a Closure hello world

- 这个和上面的例子差不多，区别就是testClosure方法这次只接受了一个参数，而且还是Closure对象。


继续往下看：

    def testClosure(Closure closure){
	    closure()
	}
	
	testClosure{
	    println 'I am from a Closure '
	}


传送门：

build.gradle文件中：

    defaultConfig {
        applicationId "cn.wang.unittest"
        minSdkVersion 15
        targetSdkVersion 24
        versionCode 1
        versionName "1.0"
	}

你是不是也看懂了呢？

applicationId的意思是调用setApplicationId("cn.wang.unittest")方法并传递参数。其他类似。

到此为止，如果想跟深入的了解[Groovy](http://www.groovy-lang.org/index.html)和[Gradle](https://gradle.org/)，请持续关注更新，近期会将更全面的Groovy和Gradle知识更新上来。如果英文可以的话，请点击链接传送官网。

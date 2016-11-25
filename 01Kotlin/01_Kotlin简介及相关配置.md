## Lambda ##

[Java8 lambda表达式10个示例](http://www.importnew.com/16436.html)

## 简单教程 ##

[Kotlin搞起来——1.实际使用，安利与相关配置](http://blog.csdn.net/coder_pig/article/details/53035505)

[Kotlin搞起来——2.基础知识](http://blog.csdn.net/coder_pig/article/details/53117127)

[Kotlin搞起来——3.集合与它的操作符们](http://blog.csdn.net/coder_pig/article/details/53164424)

[Kotlin搞起来——4.类与对象](http://blog.csdn.net/coder_pig/article/details/53190422)


## 概括教程 ##

[Kotlin: Java 6 废土中的一线希望](https://realm.io/cn/news/droidcon-michael-pardo-kotlin/)

## 详细中文教程 ##

[Kotlin-For-Android-Developers](https://github.com/wangjiegulu/kotlin-for-android-developers-zh/blob/master/SUMMARY.md)




### 简介 ###

Kotlin是JetBrains公司设计开发并开源的基于JVM的新的编程语言，有些类似于IOS中的Swift，使用Kotlin会使我们的程序变的更加简洁，安全，让我们可以在低版本JDK中使用Java 8的一些新特性，新怎的一些新特性如下：


[Kotlin: Java 6 废土中的一线希望](https://realm.io/cn/news/droidcon-michael-pardo-kotlin/)

- Lambda表达式
- 数据类(Data Classes)
- 函数字面量和内联函数(Function literals & inline functions)
- 函数扩展(Extension functions)
- 空安全(Null safety)
- 智能转换(Smart casts)
- 字符串模板(String templates)
- 主构造函数(Primary constructors)
- 类委托(Class delegation)
- 类型推断(Type inference)
- 单例(Singletons)
- 声明点变量(Declaration-site variance)
- 区间表达式(Range expressions)

首先看一下声明是Lambda表达式：

简单理解：用来替代匿名内部类的一种手段，比如，你原先创建一个线程并启动的代码可能是：

![](http://i.imgur.com/CcGAb0r.png)

而使用了Lambda表达式，你只需要一句代码：

![](http://i.imgur.com/LvhxpEM.png)

很简洁，再写一个点击按钮事件弹出Toast的事件：

![](http://i.imgur.com/bwrGAOl.png)

同样，很简洁，除此之外，你还可以用Lambda表达式来遍历列表：

![](http://i.imgur.com/5ox23C5.png)

这些都是Java 8提供的新特性，除此之外，还有函数式接口(FP)、流API(Stream)等，但是遗憾的是，即便你加入了retrolambda，但Android Studio中也只是支持函数式接口(匿名内部类的替换)，你连forEach都调用不了。

关于Lambda表达式参见内容：[Java8 lambda表达式10个示例](http://www.importnew.com/16436.html)


使用Kotlin，即可以在AS上使用Java 8的一些新特性。

另外Kotlin有一个很重要的特性就是：Null-safety，Null安全

在实际开发中，遇到过最多的bug莫过于：NullPointerException，某个引用(变量)出现了我们没有预料到的为null的情况，然后引起了程序Crash！对此，我们最常见的改bug方式，就是在出现空指针的地方加上一个判空，

    if(xxx ！= null){//执行代码}

对此，相信大部分开发者和我一样，对此都深恶痛绝，写这种判空就像贴狗皮膏药一样，本来自己觉得写得很好看的代码格局就被破坏了，心里肯定是不爽的。如果使用了Kotlin，就可以省掉判空这一步，因为Kotlin直接在源头上就限制了：

#### 所有变量声明默认：不可为空 ####

如果你**对不可为空的引用赋null值**的话，编译器会直接提示错误：

![](http://i.imgur.com/VY6Gcpu.png)

而如果你想让这个变量**可为空**的话，需要在类型后面手动**增加一个问号**：

![](http://i.imgur.com/YUcjcoX.png)

编译器虽然通过了，没提示错误，但是如果你按照上面那样直接写，也是会空指针的，童谣还是需要判空：**if(str != null)**，另外若果你**做了判空**的话，这个引用是会直接转换成：**不可为空的类型！**你可能会吐槽，这和上面我们做的那种有什么区别。其实可以更简洁些，你只需要在应用后面加上一个**问号**，即可完成判空操作：

![](http://i.imgur.com/7YO0mNC.png)

只有在前面的str**不为null**的情况下，才会去调用后面的toString()，否则，什么都不做！

除了这个**?(问号，安全访问操作符)**外，你还可以使用**！！**操作符，强制编译器执行可空类型引用时**跳过限制检查**，但是如果引用为空，还是会报空指针的哦。

![](http://i.imgur.com/tNSw9eN.png)

相信使用了Kotlin，你程序中因为NullPointerException而引起的Crash会少很多！

### Kotlin “代码简洁炫酷，逼格爆炸” ###

我们用java代码写一个java bean：

![](http://i.imgur.com/wm7me3B.png)

写构造方法，然后写一堆get、set方法，尽管这些不用我们写，只需按下Alt+Insert，然后自动生成就好，如果 你用Kotlin的话，你只需下面一句：

![](http://i.imgur.com/7uqml9p.png)

恩，就是这么简单，构造方法，get、set等方法都会在编译的时候自动生成。其中 className(参数列表) 这个就是Kotlin中的主构造器(Primary Constructor)，这里的参数可加 var或val可不加，如果不加的话就是局部变量，加上var或val就是成员变量。


### 扩展函数 ###

基本每个项目中我们都会有一个Utils包，里面都是一些常用的工具类：

![](http://i.imgur.com/cRaWp43.png)

然后你用到的时候需要：ResourcesUtil.getScreenWidth(Context context)传参调用，
 
如果你用了Kotlin，你只需要建立一个放扩展函数的工具类，然后在这里写扩展函数。

![](http://i.imgur.com/AZZsdrb.png)

然后，在所有地方，你可以直接调用

![](http://i.imgur.com/LoOt28O.png)

![](http://i.imgur.com/ITrS0sG.png)

这里Any类型是Kotlin中所有类的父类，类似于Java中的Object类，在.前面加上一个类型，代表的意思是，允许在这个类型中执行这个函数，比如如果改成Activity，那么在Activity类型的类里，就可以直接用这个扩展函数，而其他非Activity类型的类则不可用！


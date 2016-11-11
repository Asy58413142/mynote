##Getting Started With Gradle And Android Studio

Google刚开始介绍Gradle和Android Studio的时候,宗旨是让代码复用、创建构建变量、配置以及自定义构建过程变的简单，他们想要一个很好的IDE但是又不能让构建系统依赖IDE。在命令行中使用Gradle去构建一个项目和在Android Studio中使用Gradle构建项目能有相同的结果。

## Android Studio ##

目前大部分开发者用的开发工具都是Android Studio，在此不再赘述。

## 简单的Gradle配置 ##

当你用Android Studio创建一个项目的时候，IDE会自动帮你在项目的根目录下生成一个build.gradle文件，在该文件中就可以配置Gradle了，项目目录下的build.gradle是用来配置当前项目目录下所有Modules公用的一些配置信息。在项目中可能有一个或者多个Module，在每个Module的目录下，也会有一个build.gradle文件，该文件中配置的内容只能供当前的Module使用。

build.gradle不是使用xml写的，而是用一种DSL语言(Domain Specific Language)，它是基于Groovy的，Groovy是一种基于JVM的动态语言。

虽然它是基于Groovy的，并不意味着你如果想学好Gralde，就必须要先弄明白Groovy，因为Groovy是基于JVM的一种动态语言，只要你能熟练运用java，就能很轻松的看懂Groovy。但是如果你想创建自己的Task或者Plugin，那么你就需要深入的了解Groovy了。其实，使用java语言去写也可以，Groovy能无缝的认识java语言。

### Projects and Tasks ###

Gralde中最重要内容就是Project and  task。每一次构建都是由最少一个Project组成的，每一个Project包含一个或多个Tasks。每一个build.gradle代表一个Project，Task就是定义在build.gradle这个脚本中的内容。当初始化一次构建过程的时候，Gradle基于这个build.gradle文件去聚合所在的Project和Task对象。一个Task包含一系列有顺序的行为对象(Action Object),一个行为对象就是一块可执行的代码，就像java中的一个方法。

## Build Lifecycle 一次构建的生命周期 ##

执行一次构建就是去执行相应的task，然后被执行的Task有时又会依赖其他的Task，如果有依赖的task，那么就需要先执行依赖的task，然后才能去执行最终的task。为了简化整个构建过程，构建工具创建了一个动态的工作流模型，规定：所有的tasks必须要一个接一个的去执行，并且不能循环。一旦一个task执行完了，那么在本次build process中就不会再次被执行。没有依赖的task会先被执行。其中依赖关系在你配置build.gradle的时候就已经形成了。

一个build过程分为以下三个阶段：

- Initialization初始化阶段：该阶段Project对象被创建，如果项目中含有多个Modules，并且每个Module下面都有一个build.gradle文件，那么就会被创建多个project对象。
- Configuration配置阶段：这个阶段，构建脚本被执行，它会为每个project对象创建并配置相应的tasks。
- Execution执行阶段：在这个阶段，Gralde会去决定哪些个tasks应该被执行，指定哪个task应该依赖一些参数去执行。


### The Build Configuration File 项目根目录下的build.gradle文件配置###

一定要有build.gradle文件，才能让gradle去构建一个项目，要不然它也不知道怎么去构建啊。一个最基本的Android项目构建文件需要以下必要元素：

    buildscript {
		repositories {
			jcenter()
		}
		dependencies {
			classpath 'com.android.tools.build:gradle:1.2.3'
		}
	}

一次构建过程就是在这里配置的，repositories这个代码块中，JCenter的仓库被配置为这个构建脚本的依赖，Gradle允许你覆盖重写，比如Maven或者你自己本地的仓库，只要上面有Gralde就可以。

dependencies代码块中配置了Gradle的Android插件在Maven仓库上的地址，该插件提供构建Android项目的所有东西，每一个Android项目的build.gradle文件中都应该有：

	apply plugin 'com.android.application'

上面这行代码的意思就是引入Gradle的Android插件。

插件的作用是扩展Gralde的功能，引入一个插件就可以去定义相应的属性并且可以使用该插件中定义好的task去执行构建过程。

	如果你构建一个Library，那么你需要引入 'com.android.library'，而不是'com.android.application'并且两者不可同时引入，否则会报错。


当使用了Android插件的时候，只能被Android构建程序执行的task会生成，
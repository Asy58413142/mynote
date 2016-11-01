#UnitTest
	单元测试是项目开发中很重要的一部分，测试能够让你在正式发布之前验证其正确性、功能的表现是否符合设计的要求以及APP的使用性能。

Android Studio使得单元测试变的很简单，只需要几个简单的步骤，就可以新建一个JUnit测试，JUnit测试是运行在本地的JVM上的。当然，也可以使用[Mockito](https://github.com/mockito/mockito)来测试Android相关的API调用，以及[Espresso](https://developer.android.com/topic/libraries/testing-support-library/index.html#Espresso)或者[UI Automator](https://developer.android.com/topic/libraries/testing-support-library/index.html#UIAutomator)来测试UI。

##Android Studio中测试的种类和其所在的目录
你所创建的Test类的type决定了该类要放在哪个目录下，Android Studio提供了两个目录用来创建不同的Test类：

- 本地单元测试(Local Unit Test)

		所在路径：module-name/src/test/java/.

将Android Studio的Project pane设置成Project视图，在你的module-name/src/目录下有一个test目录，在test/java/<package-name>/目录下创建的测试类就是Local Unit Test，这里的单元测试都是运行在本地的JVM上的。当你要测试的内容没有依赖到Android Framework或者在你模拟Android Framework的依赖的时候，使用本地测试可以很大程度上减少测试时间，因为运行在本地JVM上的速度相对是很快的。上一句没看明白没关系，说白了/test目录下的单元测试只能没有涉及到Android API的java代码。

![](http://i.imgur.com/ojmzTW1.png)


- 指令测试(Instrumented Test)

		所在路径：module-name/src/androidTest/java/.

同样，将Android Studio的Project pane设置成Project视图，在你的module-name/src/目录下有一个androidTest目录，在/androidTest/java/<package-name>/目录下的单元测试就是Instrumented test，这里的单元测试是要运行在Android操作系统上的，也就是说要么运行在真机上，要么运行在模拟器上，不能运行在JVM里了。这里的测试类可以调用[指令集API](https://developer.android.com/reference/android/app/Instrumentation.html)，在这里可以调用APP的上下文对象Context。在什么情况下会用到Instrumented Test呢？就是在需要自动测试集成的功能性UI效果的时候，这些测试会自动的完成UI效果测试。

因为instrumented test需要build成一个apk才能运行，所以它们必须要有自己的AndroidManifest.xml文件，但是Android Studio中的Gradle已经帮你完成了，只不过在项目的资源集中看不到，当然你也可以自己新建一个manifest.xml文件用来指定test需要的minSdkVersion或者其他需求，在运行的时候Gradle会将多个manifest文件merge(合并)成一个manifest文件。不过这些都没有必要，知道就可以。

![](http://i.imgur.com/inco3c3.png)

##创建新的Test

不管是要新建一个本地单元测试(Local test)还是指令测试(Instrumented test)，你可以按照下面的步骤来为一个class或者一个method创建一个test类：

- 1.打开你要测试的java文件(或者包含你要测试的method的java文件)
- 2.将光标放到类名或者method方法名上，然后按Ctrl+Shift+T
- 3.在弹出的菜单中，点击 Create New Test
- 4.在Create Test对话框中，编辑需要修改的条目，并选中需要测试的方法，点击OK
- 5.然后在Choose Destination Directory对话框中，选择目录：包含androidTest的目录是创建一个指令集测试，test的目录是创建一个local test，如下图。然后点击ok

![](http://i.imgur.com/KNoeEAv.png)

或者是你纯手动创建test测试文件：

- 1.打开Android Studio的Project pane，并选择Project视图
- 2.打开src目录，然后，如果想创建local test就打开test目录，如果想创建instrumented test，就打开androidTest目录，然后右键->new->Java Class.
- 3.填好类名，点击ok

![](http://i.imgur.com/SvyKPWJ.png)

最重要的，请确认你的module下的build.gradle添加了对测试包的dependencies：

    dependencies {
	    // Required for local unit tests (JUnit 4 framework)
	    testCompile 'junit:junit:4.12'
	
	    // Required for instrumented tests
	    androidTestCompile 'com.android.support:support-annotations:24.0.0'
	    androidTestCompile 'com.android.support.test:runner:0.5'
	}


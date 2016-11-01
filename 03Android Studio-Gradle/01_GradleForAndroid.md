[官网介绍](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Basic-Project)，英文可以直接跳转，不用往下看。

##Gradle简介

Gradle是DSL语言，大家都知道，它有什么优势看官网即可，重点是知道Gradle怎么用。语法请移步[官方介绍](http://google.github.io/android-gradle-dsl/current/)，或者去寻找其爸爸 Groovy，强烈建议学习Groovy。

##Basic Project Setup(基本的项目配置)

用过Android Studio的都知道，你新建一个项目都会自动生成一个Project根目录下的build.gradle和一个Module下的build.gradle，其中项目根目录下的build.gradle配置的信息对该工程项目下的所有module都适用，每个Module下的build.gradle只对其所在的Module有效。

###Simple build files(简单的构建文件)

最简单的android项目(这里的项目指的是Project不是Module)都应该有下面的一个build.gradle，也就是根目录下的build.gradle文件：

    // Top-level build file where you can add configuration options common to all sub-projects/modules.

	buildscript {
	    repositories {
	        jcenter()
	    }
	    dependencies {
	        classpath 'com.android.tools.build:gradle:2.2.2'
	
	        // NOTE: Do not place your application dependencies here; they belong
	        // in the individual module build.gradle files
	    }
	}
	
	allprojects {
	    repositories {
	        jcenter()
	    }
	}
	
	task clean(type: Delete) {
	    delete rootProject.buildDir
	}


- buildscript：构建脚本，它配置了这个项目使用的gradle的信息。repositories{}声明了它使用jcenter上的仓库，dependencies{}配置了Maven上的gradle版本。这个一般不用修改。

- allprojects{}：声明该项目下的所有Module的共同属性，都依赖jcenter上的仓库。当然也可以不在根项目中配置，那么就要分别在所有Module下的build.gradle进行配置了。

- task clean：同上，声明了所有Module的clean任务都是一样的，就是删除根目录下的buildDir目录。

###Module下面的build.gradle文件

新建一个Module，AS会自动为其生成一个build.gradle文件，内容大致如下：

    apply plugin: 'com.android.application'
	
	android {
	    compileSdkVersion 24
	    buildToolsVersion "24.0.2"
	
	    defaultConfig {
	        applicationId "cn.wang.myapplication"
	        minSdkVersion 15
	        targetSdkVersion 24
	        versionCode 1
	        versionName "1.0"
	
	        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	
	    }
	    buildTypes {
	        release {
	            minifyEnabled false
	            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	        }
	    }
	}
	
	dependencies {
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
	        exclude group: 'com.android.support', module: 'support-annotations'
	    })
	    compile 'com.android.support:appcompat-v7:24.0.0-beta1'
	    testCompile 'junit:junit:4.12'
	}

一个一个看

- apply plugin:'com.android.application'声明了这是一个Android项目。
- android{}:对该android项目进行配置。
	- compileSdkVersion 编译使用到的sdk版本，一般选最高的即可
	- buildToolsVersion 构建工具的版本
	- defaultConfig{} 默认的配置，当一些配置信息在别的地方没有配置的时候，就会取默认配置里面的参数。
		- applicationId 项目的唯一id
		- minSdkVersion 最低支持版本
		- targetSdkVersion 目标sdk版本
		- versionCode 项目版本号
		- versionName 项目版本名
		- testInstrumentationRunner 测试需要用到的指令集

	-buildTypes{} 构建的类型，系统默认有release和debug，如果你不配置这两项也会有。如果你配置了，就会用你的。待会会仔细讲这个配置。
	-dependencies{} 该module用到的依赖。


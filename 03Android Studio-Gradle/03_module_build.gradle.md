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
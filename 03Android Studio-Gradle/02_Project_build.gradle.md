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
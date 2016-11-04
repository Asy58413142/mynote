#Android Studio中的Build Variant

所谓的Build Variant，就是让你能够把你的产品做release的分类：比如下面的分类：

| Variants | 描述 | 内容 |
| :--------- | :------- | :------- |
| dev | develop version | 内部开发版本，所有的api连接到内部服务器 |
| PB | public beta | 公开测试版，连接生产服务器，并且包含一些log信息 |
| PUB | public release | 公开发布版，连接生产服务器，但是移除了log信息 |

这个时候就需要打开Module下的build.gradle文件，在defaultConfig中进行配置，然后加入这几个productFlavors：

	defaultConfig {
	        applicationId "cn.wang.unittest"
	        minSdkVersion 15
	        targetSdkVersion 23
	        versionCode 1
	        versionName "1.0"
	//        默认的app_label资源  ,使用的时候R.string.app_label或@string/app_label
	        resValue("string", "app_label", "default_name")
	//        默认的IS_INTERNAL，如果为true表示使用内部服务器，false表示使用生产服务器
	        buildConfigField("boolean", "IS_INTERNAL", "true")
	//        默认的SHOW_LOG，true表示需要打log，false表示不打log
	        buildConfigField("boolean", "SHOW_LOG", "true")
	        /*单元测试*/
	        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	        signingConfig signingConfigs.sign
	    }

    productFlavors {
        /*内部开发使用版本，用来测试,
        *app_label是DEV_APP，
        *IS_INTERNAL未定义，所以会使用defaultConfig里面的IS_INTERNAL的默认值true
        *SHOW_LOG为true，这个配置变量可以用来判断是否打log
        */
        DEV {
            versionName "My APP_dev"
            resValue("string", "app_label", "DEV_APP")
            buildConfigField("boolean", "SHOW_LOG", "true")
        }
        /*发布的测试版本，
        *app_label是PUBETA_APP，
        * IS_INTERNAL为false，表示不能使用内部服务器，
        * SHOW_LOG为true，表示测试版需要打log
        * */
        PUBETA {
            versionName "My APP_PUBETA"
            resValue("string", "app_label", "PUBETA_APP")
            buildConfigField("boolean", "SHOW_LOG", "true")
            buildConfigField("boolean", "IS_INTERNAL", "false")
        }
        /*发布的正式版本
        *app_label未定义，所以会使用defaultConfig里面的app_label的默认值default_name，
        * IS_INTERNAL为false，表示不能使用内部服务器，
        * SHOW_LOG为false，表示正式版不需要打log
        * */
        PUB {
            versionName "My APP"
            buildConfigField("boolean", "SHOW_LOG", "false")
            buildConfigField("boolean", "IS_INTERNAL", "true")
        }
    }


注释写的很详细了，resValue就是会生成资源文件，这里的resValue("string","","")表示会生成一个string资源，如果是resValue("color","","")就会相应的生成一个color资源，其他同理，使用方式也一样。

buildConfigField()会生成一个BuildConfig类中的一个成员，使用的时候直接调用即可：

    例如：BuildConfig.SHOW_LOG; //因为咱们定义的时候SHOW_LOG为boolean类型，所以使用的时候也要用boolean类型接受。

	boolean show_log=BuildConfig.SHOW_LOG;

	if(show_log){
		//打印log
	} else {
		//不打印log
	}

是不是很方便。

那么，咱们在productFlavors中声明了那么多不同的版本有什么用呢？既然你知道了buildConfigField和resValue的用法了，那么productFlavors的功能就体现出来了：

你在productFlavors中配置了若干个版本之后，点击Sync之后，Gradle会自动为你生成若干个版本的Variant，在哪里看呢？在Android Studio的左下角，有一个build variant：

![](http://i.imgur.com/bWNp6Lo.png)

然后找到你的那个项目，这里是02_unittest，右边一栏的下拉框中，就是生成的各个版本，这个版本的生成规则是：

	buildTypes + productFlavors

严格来说是productFlavors + buildTypes，因为productFlavors中配置的版本名字在前，buildTypes中配置的构建类型在后。为了容易观察，这里再贴出buildTypes中配置的构建类型：

    buildTypes {
        release {
            minifyEnabled false
            versionNameSuffix "_release"
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.sign
        }
        _test {
            versionNameSuffix "_test"
            applicationIdSuffix ".test"
            signingConfig signingConfigs.sign
        }
    }

上文说过，buildTypes中可以配置若干个构建类型，但是必须要有release和debug，如果你没有配置，系统也会给你自动生成。上面的配置中有release和_test,加上隐藏的debug，也就是3个。然后productFlavors中的每个版本去匹配buildTypes中的每个构建类型，3*3=9，所以会生成9个build Variants。

注：上面说的 buildTypes + productFlavors用+号是说明生成的build variant的名字的生成规则。

那么生成这么多build Variants有什么用呢，它的特性就是buildTypes和productFlavors的结合，举例说明：

	PUBETA_test这个variant构建之后生成的包，它的特性有：

	首先是PUBETA这个productFlavors的特性：

	1、versionName 是 "My APP_PUBETA"
    2、app_label资源的值是PUBETA_APP
	3、SHOW_LOG的值为true，表示打印log
	4、IS_INTERNAL的值为false，表示使用生产服务器

	然后是_test这个buildTypes的特性：

	1、versionNameSuffix:版本名加后缀 _test
	2、applicationIdSuffix：包名加后缀 .test
	3、打这个包需要签名(除了debug外，其他的buildTypes都需要签名才能运行，否则gradle会报错)
	

##运行不同的build variant

很简单，打开build variant，选中你要运行的那个variant，然后点击 run 按钮，或者Shift+F10即可，如果不是debug版本的，需要签名，不要忘了哦，要不然跑步起来。

![](http://i.imgur.com/jvFEPbZ.png)
	
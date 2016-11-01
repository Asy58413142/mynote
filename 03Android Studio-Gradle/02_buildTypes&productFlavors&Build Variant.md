#Android Studio中Gradle的buildTypes

buildTypes：构建类型，系统默认有两个release和debug，你不配置也会存在。然后就是可以自定义，像我的：

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

其中release就是发布的正式版本，debug你不配置就用系统默认的，_test就是自定义的(感觉这个类型有release和debug就够了，自定义也没什么别的用)。

每个类型中的参数配置，以release为例：

- minifyEnabled：是否进行混淆
- versionNameSuffix：版本名的后缀，如果versionName是 1.0，那么加上后缀就是1.0_release
- proguardFiles：混淆文件
- signingConfig：签名

###关于签名

	android {
	    signingConfigs {
	        sign {
	            keyAlias 'test'
	            keyPassword 'jerry'
	            storeFile file('C:/Users/97760/Desktop/test.jks')
	            storePassword 'jerry'
	        }
	    }
	}

signingConfigs{}也是在android{}下面进行配置，如果不知道怎么配置，按Ctrl+Alt+Shift+S进行设置：

![](http://i.imgur.com/OxRCfT3.png)

如果没有.jks，点击build->Generate Signed APK...生成一个即可。配置完之后点击ok，在android{}里面就会自动生成一个signingConfigs配置。

在buildTypes就可以使用，使用规则就是signingConfig signingConfigs.sign


#Android Studio中的Build Variant

所谓的Build Variant，就是让你能够把你的产品做release的分类：比如下面的分类：

| Variants | 描述 | 内容 |
| :--------- | :------- | :------- |
| dev | develop version | 内部开发版本，所有的api连接到内部服务器 |
| PB | public beta | 公开测试版，连接生产服务器，并且包含一些log信息 |
| PUB | public release | 公开发布版，连接生产服务器，但是移除了log信息 |

这个时候就需要打开Module下的build.gradle文件，然后加入这几个productFlavors：



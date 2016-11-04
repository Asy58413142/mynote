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

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
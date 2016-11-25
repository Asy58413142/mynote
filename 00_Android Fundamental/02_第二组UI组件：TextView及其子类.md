## 文本框(TextView)与编辑框(EditText)的功能和用法 ##

TextView直接继承了View，它还是EditText、Button两个UI组件类的父类。TextView的作用就是在界面上显示文本。

从功能上来看，TextView其实就是一个文本编辑器，只是Android关闭了它的编辑功能。如果想要定义一个可以编辑内容的文本框，则可以使用它的子类：EditText，EditText允许用户编辑文本框中的内容。

TextView还派生了一个CheckedTextView，CheckedTextView增加了一个checked状态，可以通过setChecked(boolean)和isChecked()方法来改变、访问该组件的checked状态。除此之外，该组件还可以通过setCheckMarkDrawable()方法来设置它的勾选图标。

不仅如此，TextView还派生出了Button类。

TextView及其子类类图：


![](http://i.imgur.com/zwjYo8k.png)

虽然TextView有android:editable属性，但是该属性已经废弃了，应该用android:inputType属性替代。即使给一个TextView组件设置了该属性，也是无法输入内容的。因为TextView的getDefaultEditable方法返回的是false，而Edittext的getDefaultEditable方法返回的是true。

TextView提供了大量的XML属性，这些XML属性大部分不仅支持TextView，而且可适用于它的子类(EditText和Button)


#### 先看看TextView的技巧 ####

**字符串资源里变量替换**  其实这个也不算是TextView的技巧，适用于所有使用strings.xml中资源的view。

因为Android拒绝硬编码，我们可以把字符串放到xml中当做资源使用，假设有下面的情况：程序中要使用的字符串如 第345页，345 是变量，我们总不能用两个字符串资源吧，如：

    <string name="di">第</string>  
    <string name="page">页</string> 

在程序中拼接字符串。那么我们可能会想，在我们学习C的时候，有%s这样说额神奇的符号，可以替换变量的格式化操作符。

其实在android也有这样的东西，那就是XLIFF，全称叫做XML **本地化数据交换格式**，英文全称XML Localization Interchange File Format。

用法也很简单，如：

    <string name="page">第%1$s页</string>

程序中只要给变量赋值就可以了，如：

    String page = getString(R.string.page,"345");

那么要是有多个变量呢，如 第345页24行？这也好办，如下：

    <string name="page">第%1$s页%2$s行</string>  
    String page = getString(R.string.page,"345","24");

程序运行的时候，%1$s这样的格式化操作符会自动被相应的参数替代。


**SpannableString使用**

实例代码：


    @Override
    protected void onCreate (@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_textview);


        textview = (TextView) findViewById(R.id.textview);

        //创建SpannableString对象,内容不可修改
        SpannableString spannableString = new SpannableString("字体测试字体绝对大小相对大小缩放大小" +
                "前景颜色背景颜色字体样式下划线删除线上标X2下标Y3链接" +
                "字体外表图图图图");


        //使用setSpan()方法设置标记对象
        //使用removeSpan()解绑标记对象
        //public void setSpan (Object what, int start, int end, int flags)
        //绑定特定的标记对象（Object what）到起始为start，终止为end的text对象上(可以重叠使用)
        //设置字体TypefaceSpan()
        //设置字体大小(绝对值)AbsoluteSizeSpan()
        //设置字体大小(相对值)RelativeSizeSpan()
        //设置字体大小(相对值,默认字体宽度的倍数)ScaleXSpan()
        //设置字体的前景色ForegroundColorSpan()
        //设置字体的背景色BackgroundColorSpan()
        //设置字体的样式StyleSpan()
        //设置下划线UnderlineSpan()
        //删除线StrikethroughSpan()
        //上标SuperscriptSpan()
        //下标SubscriptSpan()
        //链接URLSpan()-->需要添加setMovementMethod方法附加响应
        //字体外观设置（依次包括字体名称，字体大小，字体样式，字体颜色，链接颜色)TextAppearanceSpan()
        //项目符号BulletSpan()，文字前面加个小点点 ·
        //图片ImageSpan()	就是加个bitmap

        //设置字体(default,default-bold,monospace,serif,sans-serif)
        spannableString.setSpan(new TypefaceSpan("serif"),0,4,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new AbsoluteSizeSpan(30,true),4,10,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new StrikethroughSpan(),4,10,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new RelativeSizeSpan(0.5f),10,14,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new ScaleXSpan(1.5f),14,18,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new ForegroundColorSpan(Color.BLUE),18,22,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new BackgroundColorSpan(Color.YELLOW),22,26,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new StyleSpan(Typeface.BOLD),26,30,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new UnderlineSpan(),30,33,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new StrikethroughSpan(),33,36,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new SuperscriptSpan(),39,40,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new SubscriptSpan(),43,44,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new URLSpan("http://www.baidu.com"),44,46,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);

        Drawable drawable = getResources().getDrawable(R.mipmap.ic_launcher);
        drawable.setBounds(0,0,drawable.getIntrinsicWidth(),drawable.getIntrinsicHeight());

        spannableString.setSpan(new ImageSpan(drawable),50,54,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        spannableString.setSpan(new BulletSpan(BulletSpan.STANDARD_GAP_WIDTH,Color.RED),0,spannableString.length(),Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);

	//        设置该方法使文本的超链接起作用
        textview.setMovementMethod(LinkMovementMethod.getInstance());
        textview.setText(spannableString);
    }


运行结果：

![](http://i.imgur.com/lD9oh7T.png)



**SpannableString的另外一种用法：点击textview中特定的字符串进行页面跳转**


    @Override
    protected void onCreate (@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_textview);


        textview = (TextView) findViewById(R.id.textview);

        SpannableString spanStr = new SpannableString("使用该软件，即表示您同意该软件的使用条款和隐私政策");
        //设置下划线文字
        spanStr.setSpan(new UnderlineSpan(), 16, 20, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        //设置文字的单击事件
        spanStr.setSpan(new ClickableSpan() {

            @Override
            public void onClick (View widget) {

                startActivity(new Intent(TextViewActivity.this, FrameLayoutActivity.class));
            }
        }, 16, 20, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        //设置文字的前景色
        spanStr.setSpan(new ForegroundColorSpan(Color.GRAY), 16, 20, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        //设置下划线文字
        spanStr.setSpan(new UnderlineSpan(), 21, 25, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        //设置文字的单击事件
        spanStr.setSpan(new ClickableSpan() {

            @Override
            public void onClick (View widget) {

                startActivity(new Intent(TextViewActivity.this, GridLayoutActivity.class));
            }
        }, 21, 25, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        //设置文字的前景色
        spanStr.setSpan(new ForegroundColorSpan(Color.parseColor("#6655ff")), 21, 25, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);

	//        同样，设置该方法让点击事件生效
        textview.setMovementMethod(LinkMovementMethod.getInstance());
        textview.setText(spanStr);
    }


运行结果：

![](http://i.imgur.com/aGNsu9Q.png)

点击“使用条款”和“隐私政策”会跳转到相应的activity中。

如果不想要下划线，可以重写ClickableSpan的updateDrawState方法：


    spanStr.setSpan(new ClickableSpan() {

            /**
             * 重写该方法，并调用ds.setUnderlineText(false);去掉下划线
             */
            @Override
            public void updateDrawState (TextPaint ds) {
                super.updateDrawState(ds);
                ds.setUnderlineText(false);
            }

            @Override
            public void onClick (View widget) {

                startActivity(new Intent(TextViewActivity.this, GridLayoutActivity.class));
            }
        }, 21, 25, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);


TextView的属性介绍：


- android:autoLink属性，对应于setAutoLinkMask(int);方法，设置是否当文本为URL链接/email/电话号码/map时，文本显示为可点击的链接。可选值(none/web/email/phone/map/all)。

这个很简单，在xml属性中属性android:autoLink="all"，在程序中TextView.setAutoLinkMask(Linkify.ALL);;

说下参数：

	Linkify.EMAIL_ADDRESS -- 仅识别出 TextView 中的 Email 在址，标识为超链接，点击后会跳到 Email，发送邮件给此地址
	
	Linkify.PHONE_NUMBERS -- 仅识别出 TextView 中的电话号码，标识为超链接，点击后会跳到 Dialer，Call 这个号码
	
	Linkify.WEB_URLS-- 仅识别出 TextView 中的网址，标识为超链接，点击后会跳到 Browser 打开此 URL
	
	Linkify.ALL -- 这个选项是识别出所有系统所支持的特殊 Uri，然后做相应的操作



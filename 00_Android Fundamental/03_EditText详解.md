### EditText的输入方式  ：InputType ###

EditText下面这些属性都废弃了：

- android:autoText:改用autoLink属性
- android:capitalize:改用inputType属性，值对应textCapCharacters、textCapSentences、textCapWords
- android:editable:改用inputType属性
- android:inputMethod:改用inputType属性
- android:numeric:改用InputType属性，值对应number、numberSigned、numberDecimal、numberPassword
- android:password:改用inputType，值对应textWebPassword、textVisiblePassword、textPassword、numberPassword
- android:phoneNumber:改用inputType，值对应phone
- android:singleLine:改用maxLines，值对应1


可以看到EditText的inputType属性是多么强大，接下来看看inputType属性都有哪些值：

intpuType属性主要是用来控制软键盘开启状态的，例如number值控制软键盘开启时就是数字键盘。

- android:inputType="none" 不能输入，对EditText无效，EditText本来就是要输入内容的
- android:inputType="text" 任意字符串
- android:inputType="textCapCharacters" 字母大写
- android:inputType="textCapWords" 首字母大写
- android:inputType="textCapSentences" 仅第一个字母大写
- android:inputType="textAutoCorrect" 自动完成
- android:inputType="textAutoComplete" 自动完成
- android:inputType="textMultiLine" 多行输入
- android:inputType="textImeMultiLine" 输入法多行（如果支持）
- android:inputType="textNoSuggestions" 不提示
- android:inputType="textUri" 网址
- android:inputType="textEmailAddress" 电子邮件地址
- android:inputType="textEmailSubject" 邮件主题
- android:inputType="textShortMessage" 短讯
- android:inputType="textLongMessage" 长信息
- android:inputType="textPersonName" 人名
- android:inputType="textPostalAddress" 地址
- android:inputType="textPassword" 密码
- android:inputType="textVisiblePassword" 可见密码
- android:inputType="textWebEditText" 作为网页表单的文本
- android:inputType="textWebEmailAddress" 网页邮箱地址
- android:inputType="textWebPassword" 网页表单样式的密码
- android:inputType="textFilter" 文本筛选过滤
- android:inputType="textPhonetic" 拼音输入 //数值类型
- android:inputType="number" 数字
- android:inputType="numberPassword" 只能输入数字的密码，输入以后以*显示
- android:inputType="numberSigned" 带符号数字格式
- android:inputType="numberDecimal" 带小数点的浮点格式
- android:inputType="phone" 拨号键盘
- android:inputType="datetime" 时间日期
- android:inputType="date" 日期键盘
- android:inputType="time" 时间键盘

InputType属性有时并不能限制用户只能输入某些字符(number系列的属性可以很好的限制)，例如textEmailAddress值本来应该限制用户只能输入英文字母(a-zA-Z)、数字(0-9)、@和.这些字符，但是输入法很多输入法厂商对此没有加上限制，顶多是在弹出的输入法上方多出一排：.、@、.com、.cn、.net之类的字符串，但是仍然可以输入其他字符，例如()&^%^^%$之类的，还可以输入中文：

![](http://i.imgur.com/aPKSCtv.png)

那怎么限制用户不能输入中文和其他特殊字符呢？

可以使用EditText的digits属性，该属性表示EditText能接受的字符集合。通过配置此属性，就可以实现只允许用户输入指定的字符。看下面几个场景：

1. 场景一：只允许输入数字 
android:digits="0123456789" 
2. 场景二：只允许输入数字和英文字母 
android:digits="0123456789abcdefghigklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ" 
3. 场景三：只允许输入数字，英文字母和@.两个符号 
android:digits="0123456789abcdefghigklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ@."

使用android:digits属性有几个需要注意的地方。

1. android:digits属性中的字符顺序没有影响，例如只允许输入数字时，可以用android:digits="0123456789"，也可以用android:digits="9876543210"。 
2. android:digits属性中允许有重复字符，有重复字符在使用效果上没有影响。例如android:digits="aaaAAA"和android:digits="aA"效果是一样的。 
3. android:digits属性只能设置允许输入哪些字符，不能设置禁止输入哪些字符，也就是说android:digits属性只能设置白名单，不能设置黑名单。


#### 接下来再详细的说下inputType属性


带有中文输入法的手机，通常输入法会有三种类型的软键盘：中文键盘，英文键盘和数字键盘，不同键盘类型之间可以切换。虽然android:digits属性可以让EditText中只允许输入指定类型的字符，但并不能控制软键盘弹出时的类型。例如在设置android:digits只允许输入数字时，可能弹出的软键盘是英文键盘或中文键盘，需要用户点击切换到数字键盘后才可以输入；在设置android:digits只允许输入英文和数字时，可能弹出的软键盘是中文键盘，同样需要用户点击切换到英文键盘和数字键盘后才可以输入。如果能在软键盘显示时就直接跳到对应键盘类型，不需要用户切换就更好了。 

inputType属性就是来设置输入框类型的。

当设置android:inputType为number时，输入法开启时会自动切换到数字键盘。

这里列举上述两种场景下的android:inputType配置。

1. 场景一：只允许输入数字  
android:inputType="number"。实际上，当inputType设置为number后，EditText中就只能输入数字了，那么android:digits="0123456789"也不用设置了。 
如果android:inputType="number"和android:digits同时设置，则实际可以输入的字符以android:digits配置为准。例如android:digits="01234567+-"，则可以输入的字符为"01234567+-"，这时如果同时设置了android:inputType="number"，可以达到自动切换到数字键盘的目的。 

2. 场景二：只允许输入数字，英文字母和@.两个符号  
android:inputType="textEmailAddress"。 
最后再吐槽下android在android:inputType参数上的设计，android并没有将各个android:inputType参数对应的功能全部实现在android框架中，部分参数的功能完全交给输入法去实现，还有一部分参数的功能一半在android内部实现，一半交给输入法实现。由于android输入法的开放性，android本身对输入法开发商又没有任何约束，这导致了各个android:inputType参数在不同输入法上的表现千差万别。例如textCap…这几个参数，android原本希望用它来控制输入字母的大小写，但大多数输入法都没有完全实现这几个选项中对大小写的设定，有些只实现了textCapCharacters，有些只是将软键盘调整为英文键盘，有些则干脆什么也不做。textEmailAddress有些输入法会在软键盘上方额外增加几个邮箱中常用的符号，有些则只是显示英文键盘。


### EditText输入的文字为密码形式的另外一种设置方式：

除了在xml文件中为EditText组件指定相应的InputType之外，还可以用代码的形式设置EditText输入的文字为密码形式：

    通过设置EditText的setTransformationMethod()方法来实现隐藏密码或这显示密码。

	editText.setTransformationMethod(PasswordTransformationMethod.getInstance());//设置密码为不可见。

### EditText限制输入字符长度以及将小写字母转换成大写字母：

可以在xml文件中设置maxLength属性来限制输入字符的长度

还可以在代码中设置：

    editText.setFilters(new InputFilter[]{newInputFilter.LengthFilter(100)});

在xml文件中设置inputType="textCapCharacters"，有时并不生效(我测试的对很多手机都不生效)，怎么解决呢？

还是在代码中设置：

    edittext1.setFilters(new InputFilter[]{new InputFilter.AllCaps()});

EditText的setFilters方法接受一个InputFilter类型的数组，我们还可以这样写：

    edittext1.setFilters(new InputFilter[]{new InputFilter.LengthFilter(10),new InputFilter.AllCaps()});//既限定输入长度为10，又将小写字母全部转换成大写字母

其中LengthFilter和AllCaps类都是InputFilter的静态内部类，都实现了InputFilter接口。

### InputFilter接口 ###

先打一下基础，EditText继承自TextView，90%的功能跟TextView是一致的，只重写了几个方法。所以EditText的大部分功能都是在TextView中实现的，具体逻辑也都是在TextView中实现的。

InputFilter是系统提供的一个接口，里面只有一个方法filter(),用于过滤输入/插入的字符串，返回值为CharSequence。filter()方法的参数如下：

    public CharSequence filter(CharSequence source, int start, int end,
                               Spanned dest, int dstart, int dend);
参数介绍：
- source：即将要输入/插入到EditText中的内容
- start：source的起始位置，一般为0(暂时没有发现其他值的情况)
- end：source的结束位置，一般也等于source的长度
- dest:EditText中已经有的内容
- dstart：插入/输入的位置
- dend：dstart+即将输入的内容的长度

最后两个参数很重要。

TextView类的setText()方法中，会调用filter()方法，得到过滤后的字符串，setText方法源码如下：

注：setText()方法有很多重载方法，但是最终都会调用下面这个。这个方法很重要，下面所有的分析都会经过这里，只需要看关键逻辑处的注释。

    // mText：发生变化前TextView中的内容
	// text：将要设置的新内容
	private void setText(CharSequence text, BufferType type,
                         boolean notifyBefore, int oldlen) {
        if (text == null) {
            text = "";
        }

        if (!isSuggestionsEnabled()) {
            text = removeSuggestionSpans(text);
        }

        if (!mUserSetTextScaleX) mTextPaint.setTextScaleX(1.0f);

        if (text instanceof Spanned &&
            ((Spanned) text).getSpanStart(TextUtils.TruncateAt.MARQUEE) >= 0) {
            if (ViewConfiguration.get(mContext).isFadingMarqueeEnabled()) {
                setHorizontalFadingEdgeEnabled(true);
                mMarqueeFadeMode = MARQUEE_FADE_NORMAL;
            } else {
                setHorizontalFadingEdgeEnabled(false);
                mMarqueeFadeMode = MARQUEE_FADE_SWITCH_SHOW_ELLIPSIS;
            }
            setEllipsize(TextUtils.TruncateAt.MARQUEE);
        }

        // 使用InputFilter处理text
        int n = mFilters.length;
        for (int i = 0; i < n; i++) {
            CharSequence out = mFilters[i].filter(text, 0, text.length(), EMPTY_SPANNED, 0, 0);
            if (out != null) {
                text = out;
            }
        }

        if (notifyBefore) {
            if (mText != null) {
                oldlen = mText.length();
                // 通知调用TextWatcher的beforeTextChanged()方法
                sendBeforeTextChanged(mText, 0, oldlen, text.length());
            } else {
                sendBeforeTextChanged("", 0, 0, text.length());
            }
        }

        boolean needEditableForNotification = false;

        if (mListeners != null && mListeners.size() != 0) {
            needEditableForNotification = true;
        }

        // 如果是EditText，就new一个Editable
        if (type == BufferType.EDITABLE || getKeyListener() != null ||
                needEditableForNotification) {
            createEditorIfNeeded();
            Editable t = mEditableFactory.newEditable(text);
            text = t;
            setFilters(t, mFilters);
            InputMethodManager imm = InputMethodManager.peekInstance();
            if (imm != null) imm.restartInput(this);
        } else if (type == BufferType.SPANNABLE || mMovement != null) {
            text = mSpannableFactory.newSpannable(text);
        } else if (!(text instanceof CharWrapper)) {
            text = TextUtils.stringOrSpannedString(text);
        }

        if (mAutoLinkMask != 0) {
            Spannable s2;

            if (type == BufferType.EDITABLE || text instanceof Spannable) {
                s2 = (Spannable) text;
            } else {
                s2 = mSpannableFactory.newSpannable(text);
            }

            if (Linkify.addLinks(s2, mAutoLinkMask)) {
                text = s2;
                type = (type == BufferType.EDITABLE) ? BufferType.EDITABLE : BufferType.SPANNABLE;
                mText = text;
                if (mLinksClickable && !textCanBeSelected()) {
                    setMovementMethod(LinkMovementMethod.getInstance());
                }
            }
        }

        mBufferType = type;
        // 用新内容替换旧内容
        mText = text;

        if (mTransformation == null) {
            mTransformed = text;
        } else {
            mTransformed = mTransformation.getTransformation(text, this);
        }

        final int textLength = text.length();

        if (text instanceof Spannable && !mAllowTransformationLengthChange) {
            Spannable sp = (Spannable) text;

            final ChangeWatcher[] watchers = sp.getSpans(0, sp.length(), ChangeWatcher.class);
            final int count = watchers.length;
            for (int i = 0; i < count; i++) {
                sp.removeSpan(watchers[i]);
            }

            if (mChangeWatcher == null) mChangeWatcher = new ChangeWatcher();

            sp.setSpan(mChangeWatcher, 0, textLength, Spanned.SPAN_INCLUSIVE_INCLUSIVE |
                       (CHANGE_WATCHER_PRIORITY << Spanned.SPAN_PRIORITY_SHIFT));

            if (mEditor != null) mEditor.addSpanWatchers(sp);

            if (mTransformation != null) {
                sp.setSpan(mTransformation, 0, textLength, Spanned.SPAN_INCLUSIVE_INCLUSIVE);
            }

            if (mMovement != null) {
                mMovement.initialize(this, (Spannable) text);
                if (mEditor != null) mEditor.mSelectionMoved = false;
            }
        }

        if (mLayout != null) {
            checkForRelayout();
        }

        // 通知调用TextWatcher的onTextChanged()方法
        sendOnTextChanged(text, 0, oldlen, textLength);
        onTextChanged(text, 0, oldlen, textLength);

        // 通知view刷新 
      notifyViewAccessibilityStateChangedIfNeeded(AccessibilityEvent.CONTENT_CHANGE_TYPE_TEXT);

        if (needEditableForNotification) {
            // 通知调用TextWatcher的afterTextChanged()方法
            sendAfterTextChanged((Editable) text);
        }

        if (mEditor != null) mEditor.prepareCursorControllers();
    }


mFilters是一个InputFilter数组，很好理解，因为需要设置一个或多个过滤器。通过for循环，把text按照所有过滤条件全部过滤一遍，最终得到“合格”的text。

知道了InputFilter是如何起作用的，那么剩下的就是搞清楚filter()方法中的各个参数的含义，写出自己需要的InputFilter。

接下来看看系统提供的InputFilter类。
    

先看下AllCaps怎么实现该方法的：


    public static class AllCaps implements InputFilter {

        public CharSequence filter(CharSequence source, int start, int end,
                                   Spanned dest, int dstart, int dend) {
			//遍历每个字符，将小写转换成大写
            for (int i = start; i < end; i++) {
                if (Character.isLowerCase(source.charAt(i))) {
                    char[] v = new char[end - start];
                    TextUtils.getChars(source, start, end, v, 0);
                    String s = new String(v).toUpperCase();

                    if (source instanceof Spanned) {
                        SpannableString sp = new SpannableString(s);
                        TextUtils.copySpansFrom((Spanned) source,
                                                start, end, null, sp, 0);
                        return sp;
                    } else {
                        return s;
                    }
                }
            }
			//如果return结果为null，表示原样输入，不做任何操作
            return null; // keep original
        }
    }

从代码不难发现，当字符串source中包含小写字母时，将其转换为大写字母然后返回，从而实现AllCaps。

再来看看InputFilter.LengthFillter的实现原理

    public CharSequence filter(CharSequence source, int start, int end, Spanned dest,
                int dstart, int dend) {

			//keep表示还能输入多少字符
            int keep = mMax - (dest.length() - (dend - dstart));
			//如果keep小于等于0了，表示不能再输入了，直接返回""空串
            if (keep <= 0) {
                return "";
            } else if (keep >= end - start) {
				//如果还能输入，就返回null，返回null表示原样输入
                return null; // keep original
            } else {
                keep += start;
                if (Character.isHighSurrogate(source.charAt(keep - 1))) {
                    --keep;
                    if (keep == start) {
                        return "";
                    }
                }
                return source.subSequence(start, keep);
            }
        }


再来看看EditText是如何使用这个InputFilter的，其实InputFilter是在TextView中用到的，在TextView的setText()方法中

    int n = mFilters.length;  
	for (int i = 0; i < n; i++) {  
	    CharSequence out = mFilters[i].filter(text, 0, text.length(), EMPTY_SPANNED, 0, 0);  
	    if (out != null) {   
	        text = out;  
	    }  
	}  
	...

    sendOnTextChanged(text, 0, oldlen, textLength);  
	onTextChanged(text, 0, oldlen, textLength);

遍历所有的InputFilter，如果filter后的结果不是null，更新text变量。这意味着filter返回null就代表保持原文本不变（如上，在InputFilter.LengthFilter中看到的）。所有InputFilter后的结果就是最终显示在TextView中的文本。

了解了InputFilter的原理，就知道如何对TextView/EditText的文本做“手脚”了。

  
	1.不响应所有输入的特殊字符（@#$%^&）
	2.显示最多只能输入100个大写字母（使用InputFilter.LengthFilter和InputFilter.Allcaps的组合）
	3.将输入的某个字符自动替换为其他字符

系统为我们提供了几个常用的Filter，看看系统提供的实现了InputFilter的类：


    edittext1.setFilters(new InputFilter[]{
	//                new DateKeyListener(),
	// 只能输入 '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '/', '-', '.'等表示日期的字符，但是并不是直接弹出数字键盘，而是弹出英文键盘


	//                new TimeKeyListener(),
	// 只能输入 '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'm', 'p', ':'等表示时间的字符，但是并不是直接弹出数字键盘，而是英文键盘


	//                new DateTimeKeyListener(),
	// 只能输入 '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'm', 'p', ':', '/', '-', ' '(包含空格)
	// 等表示日期时间的字符，但是并不是直接弹出数字键盘，而是英文键盘


	//                new DigitsKeyListener(true,true),
	// 只能输入数字，有两个参数，第一个boolean表示能否输入正负号(正负号只能在第一个字符处输入),第二个boolean表示能否输入小数点(小数点只能输入一个)
	// ，如果使用DigitsKeyListener的无参构造表示两个都是false,同样该方式并不是直接弹出数字键盘，而是英文键盘


                new DialerKeyListener()//只能输入'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '#', '*',
                // '+', '-', '(', ')', ',', '/', 'N', '.', ' ', ';'
        });

可以看出，InputFilter只负责过滤输入，即限定用户只能输入哪些字符，并不能确定软键盘开启时的类型，如果要设置软键盘开启时的类型，还是需要EditText的setInputType方法，或者InputType属性。

InputFilter说了那么多，也差不多了，应该知道其原理了，那么我们自己定义一个InputFilter来玩玩：

我们先定义一个规则，如果待输入的内容满足规则，就让filter方法返回null(原样输入)，如果不满足规则，就返回""(空串，表示什么也不输入)。

知道了规则，就可以直接写代码了：

    class MyFilter implements InputFilter {

        private final Pattern pattern;

        /**
         * 构造方法中直接传入一个规则
         * @param pattern 正则规则
         */
        public MyFilter (Pattern pattern) {
            if (pattern == null) {
                throw new IllegalArgumentException("MyFilter 类需要一个 规则");
            }
            this.pattern = pattern;
        }

        /**
         * 为方便使用，还可以直接传入一个字符串类型的规则
         * @param patternString 
         */
        public MyFilter (String patternString) {
            this(Pattern.compile(patternString));
        }

        @Override
        public CharSequence filter (CharSequence source, int start, int end, Spanned dest, int dstart, int
                dend) {
            Matcher matcher = pattern.matcher(source);
            //如果不匹配，返回""，表示不输入
            if (!matcher.matches()) {
                return "";
            }
	//            如果匹配，返回null，表示原样输入
            return null;
        }
    }

使用：

    edittext1.setFilters(new InputFilter[]{
				//这里传入的规则是\d,表示只能输入数字，更多的Pattern请查看java文档Pattern类。
                new MyFilter("\\d")
		}
运行之后EditText只能输入数字，其他输入无效，不再演示。

[InputFilter另外一个例子。
](http://blog.csdn.net/longfu2012/article/details/7699535)

上面说了限定EditText输入长度和输入内容除了代码之外还可以使用maxLength和inputType属性，这些属性是怎么生效的呢？

TextView的构造方法中，会通过TypedArray获取各种属性，用于初始化。下面为构造方法源码片段（只贴出主要逻辑）：

    // 读取控件参数
	switch (attr) {
	    case com.android.internal.R.styleable.TextView_maxLength:
	        maxlength = a.getInt(attr, -1);
	    break;
	}
	
	.................
	...... 略 .......
	.................
	
	// 该方法的主要逻辑在下面贴出
	setInputType(inputType, true);
	
	.................
	...... 略 .......
	.................
	
	// 设置过滤器（注意：不管有没有设置maxLength属性，都会调用setFilters()方法）
	if (maxlength >= 0) {
	    setFilters(new InputFilter[] { new InputFilter.LengthFilter(maxlength) });
	} else {
	    setFilters(NO_FILTERS);
	}

setInputType()方法源码片段如下（只贴出主要逻辑）：

    if (cls == EditorInfo.TYPE_CLASS_NUMBER) {
	    // 如果inputType = number，就创建DigitsKeyListener
	    // DigitsKeyListener继承自NumberKeyListener，而NumberKeyListener实现了InputFilter接口
	    input = DigitsKeyListener.getInstance(
	                    (type & EditorInfo.TYPE_NUMBER_FLAG_SIGNED) != 0,
	                    (type & EditorInfo.TYPE_NUMBER_FLAG_DECIMAL) != 0);
	}
	
	// 把上面创建的DigitsKeyListener赋值给成员变量mKeyListener
	mEditor.mKeyListener = input;

可以看到，对于android:maxLength属性来说，就是直接通过setFilters()方法设置LengthFilter。如果代码中调用了setFilters()方法重新设置过滤器，android:maxLength属性就会失效。



### 另外一种控制输入的方式： TextWatcher ###

先看TextView源码中的addTextChangedListener()：

    public void addTextChangedListener(TextWatcher watcher) {
        if (mListeners == null) {
            mListeners = new ArrayList<TextWatcher>();
        }
        mListeners.add(watcher);
    }

这里可看到，是可以添加多个TextWatcher的，典型的观察者模式，TextView会在文字发生变化时，遍历集合中的TextWatcher，发送通知，如下：

    private void sendBeforeTextChanged(CharSequence text, int start, int before, int after) {
        if (mListeners != null) {
            final ArrayList<TextWatcher> list = mListeners;
            final int count = list.size();
            for (int i = 0; i < count; i++) {
                list.get(i).beforeTextChanged(text, start, before, after);
            }
        }

        // The spans that are inside or intersect the modified region no longer make sense
        removeIntersectingNonAdjacentSpans(start, start + before, SpellCheckSpan.class);
        removeIntersectingNonAdjacentSpans(start, start + before, SuggestionSpan.class);
    }

只列举了sendBeforeTextChanged方法，另外两个方法sendOnTextChanged()、sendAfterTextChanged()类似。

那什么时候会调用sendBeforeTextChanged()方法呢？ 
有两种情况会使TextView里面的内容发生变化，从而通知监听器，第一种就是setText()方法，第二种就是从键盘输入。

Google对于“改变字符串”的设计理念就是“替换”。如果是删内容，就是用空字符串替换需要删除的字符串；如果是增加内容，就是用新字符串替换空字符串。所以要先搞清楚下面几个概念： 

1. 原内容：发生改变前TextView中的内容； 
2. 被替换内容起点坐标：编辑一段内容时，有可能是直接添加新内容，也有可能是选中一段原有内容，用新内容把它替换掉； 
3. 被替换内容的长度：如果是直接添加新内容，被替换内容的长度就是0； 
4. 新增加的内容：对于setText()来说，就是方法中的参数，对于键盘输入来说，就是键盘输入的内容


再来分析这两种情况。

#### 情况一：setText() ####

setText()的源码在上面已经贴出，并写了注释。

通过分析，大概可以得出如下结论：（通过键盘输入的源码分析可以确认该结论）

    // s：原内容
	// start：被替换内容起点坐标，因为setText()是将原内容全部替换掉，所以起点是0
	// count：被替换内容的长度，因为setText()是将原内容全部替换掉，所以就是mText.length()
	// after：新增加内容的长度
	public void beforeTextChanged(CharSequence s, int start, int count, int after) {
	}
	
	// s：发生改变后的内容
	// start：被替换内容的起点坐标
	// before：被替换内容的长度
	// count：新增加的内容的长度
	public void onTextChanged(CharSequence s, int start, int before, int count) {
	}
	
	// s：发生改变后的内容
	public void afterTextChanged(Editable s) {
	}

#### 情况二：键盘输入 ####

TextView的构造方法中，会获取android:text属性的值，调用setText()方法设置初始内容。其中，就会判断BufferType的类型，如果是EditText，就会创建Editable（此段逻辑见上面setText()源码）。

最终new出SpannableStringBuilder对象，SpannableStringBuilder实现了Editable、Appendable接口。Appendable提供了一个接口（有三个重载的）：append()，用来把新内容（来自键盘输入）添加到原内容中。所以我们去SpannableStringBuilder里看看append()方法的具体实现。

三个重载的接口，就有三个具体实现，但原理都一样，最终都会调用replace()方法。下面以其中一个append()实现来分析：

    // 键盘输入有两种：一种是正常输入；另一种是先选中一段内容，再从键盘输入，新内容会替换掉选中的内容；
	// 这个方法是正常输入时调用
	public SpannableStringBuilder append(CharSequence text, int start, int end) {
	    // length就是插入点的位置
	    int length = length();
	    // 最终都会调用replace()方法来“增加”内容。从命名可以看出，Google对于字符串改变的设计思路就是“替换”，如果是删内容，就是用空内容替换原内容，如果是增加内容，就是用新内容替换某个内容
	    return replace(length, length, text, start, end);
	}
	
	public SpannableStringBuilder replace(final int start, final int end,
	            CharSequence tb, int tbstart, int tbend) {
	        checkRange("replace", start, end);
	
	    // 与setText()一样，都会对新增内容进行过滤
	    int filtercount = mFilters.length;
	    for (int i = 0; i < filtercount; i++) {
	        CharSequence repl = mFilters[i].filter(tb, tbstart, tbend, this, start, end);
	
	        if (repl != null) {
	            tb = repl;
	            tbstart = 0;
	            tbend = repl.length();
	        }
	    }
	
	    // 由于是正常键盘输入，end等于start，所以origLen等于0
	    final int origLen = end - start;
	    // 新增内容的长度
	    final int newLen = tbend - tbstart;
	
	    if (origLen == 0 && newLen == 0 && !hasNonExclusiveExclusiveSpanAt(tb, tbstart)) {
	        return this;
	    }
	
	    TextWatcher[] textWatchers = getSpans(start, start + origLen, TextWatcher.class);
	    // 通知TextWatcher调用beforeTextChanged()方法，逻辑跟TextView中的一样，就不再贴代码了
	    sendBeforeTextChanged(textWatchers, start, origLen, newLen);
	
	    boolean adjustSelection = origLen != 0 && newLen != 0;
	    int selectionStart = 0;
	    int selectionEnd = 0;
	    if (adjustSelection) {
	        selectionStart = Selection.getSelectionStart(this);
	        selectionEnd = Selection.getSelectionEnd(this);
	    }
	
	    change(start, end, tb, tbstart, tbend);
	
	    if (adjustSelection) {
	        if (selectionStart > start && selectionStart < end) {
	            final int offset = (selectionStart - start) * newLen / origLen;
	            selectionStart = start + offset;
	
	            setSpan(false, Selection.SELECTION_START, selectionStart, selectionStart,
	                        Spanned.SPAN_POINT_POINT);
	            }
	        if (selectionEnd > start && selectionEnd < end) {
	            final int offset = (selectionEnd - start) * newLen / origLen;
	            selectionEnd = start + offset;
	
	            setSpan(false, Selection.SELECTION_END, selectionEnd, selectionEnd,
	                        Spanned.SPAN_POINT_POINT);
	        }
	    }
	
	    // 通知TextWatcher调用onTextChanged()、afterTextChanged()方法。可以看到，这两个方法是一起调用的，这点跟setText()有点细微差别，总体来说是一样的
	    sendTextChanged(textWatchers, start, origLen, newLen);
	    sendAfterTextChanged(textWatchers);
	
	    sendToSpanWatchers(start, end, newLen - origLen);
	
	    return this;
	}


## InputFilter、TextWatcher小结：



- 使用InputFilter对字符串进行控制、过滤。


- 尽量不要在TextWatcher的onTextChanged()方法中对文字进行过滤，然后再调用setText()方法重置字符串，效率明显比InputFilter低。


- 如果一定要在TextWatcher的onTextChanged()方法中调用setText()方法，注意防止死循环。因为setText()方法又会回调onTextChanged()方法，会形成死循环。


- TextWatcher主要功能是进行监听，从Google对该类的命名就可以看出来。

[InputFilter、TextWatcher详解](http://www.itdadao.com/articles/c15a735409p0.html)





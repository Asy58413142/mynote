## EditText调出软键盘的action设置 ##

在Android发开过程中，有时候需要对EditText的软键盘进行监听。 
当点击软键盘回车位置按键的时候，需要实现 完成、前进、下一项、搜索、发送或其他功能，这就需要开发者对软键盘回车的点击事件进行捕捉。 

比如在登录界面，需要用户在输入密码之后点击软键盘回车直接登录，不必再去点击屏幕上的登录按钮。我们就可以在密码使用的EditText设置 android:imeOptions=”actionDone”，然后对EditText设置OnEditorActionListener监听，当捕捉到用户点击完成时，调用登录方法即可。（IME英文全称Input Method Editors，中文名称输入法编辑器）


使用android:imeOptions属性的时候，一定要对EditText设置 android:inputType 或者 设置 android:singleline=”true”。

EditText的imeOptions属性用来设置软键盘最右下角一个按键的类型，看看imeOptions属性有多少值：

- actionDone:完成
- actionSearch：搜索
- actionGo：前进
- actionNext：下一项
- actionNone:无动作
- actionPrevious：上一项
- actionSend：发送
- actionUnspecified:未指定
- flagForceAscii：
- flagNavigateNext：
- flagNavigatePrevious：
- flagNoAccessoryAction：
- flagNoEnterAction：
- flagNoExtractUi：
- flagNoFullscreen：
- normal:

在xml文件中指定相应的imeOptions值之后在Activity中监听action的点击事件：


    editText.setOnEditorActionListener(new TextView.OnEditorActionListener() {
            @Override
            public boolean onEditorAction (TextView v, int actionId, KeyEvent event) {
                Log.e(TAG, "onEditorAction: event:   " + event);
                Log.e(TAG, "onEditorAction: v:   " + v);
                Log.e(TAG, "onEditorAction: v.getImeActionId() :  " + v.getImeActionId());
                Log.e(TAG, "onEditorAction: v.getImeOptions(): " + v.getImeOptions());
                Log.e(TAG, "onEditorAction: ----------------------------------------------");
                if (actionId == EditorInfo.IME_ACTION_DONE) {
                    //Do Something done,例如 执行登录操作。。。
                    Toast.makeText(TextViewActivity.this, "done", Toast.LENGTH_SHORT).show();
                } else if (actionId == EditorInfo.IME_ACTION_SEARCH) {
                    //Do Something search，例如 执行搜索操作。。。
                    Toast.makeText(TextViewActivity.this, "search", Toast.LENGTH_SHORT).show();
                }
	//        系统对IME_ACTION_DONE做了相关操作，即如果是DONE，点击之后隐藏软键盘，如果返回true就不会调用系统的这个功能，所以要返回false
                return false;
            }


部分第三方的输入法，对EditorInfo支持的不一样，有的功能实现了，但是对应的图标没有修改过来，有的干脆功能就没有实现。比如谷歌自己的输入法，就不支持actionPrevious的图标和功能，而百度输入法的小米专用版中，actionPrevious功能虽然实现了，但是图标仍然显示的是回车的图标。

对EditText指定不同的imeOptions之后，就需要实现OnEditorActionListener 中的onEditorAction()方法，然后根据不同的动作执行进行响应。 
对于actionDone、actionNext和actionPrevious，系统都自己进行了部分处理。 

- actionDone：隐藏输入法 
- actionNext：跳到下一个EditText 
- actionPrevious：跳到上一个EditText

然并卵！！！ 

原因在于，在TextView源码的方法onEditorAction()中：

    public void onEditorAction(int actionCode) {
        final Editor.InputContentType ict = mEditor == null ? null : mEditor.mInputContentType;
        if (ict != null) {
            if (ict.onEditorActionListener != null) {
				//调用我们自己实现的onEditorAction方法，如果该方法返回true，直接return，不会走下面的默认操作
                if (ict.onEditorActionListener.onEditorAction(this,
                        actionCode, null)) {
                    return;
                }
            }

            // This is the handling for some default action.
            // Note that for backwards compatibility we don't do this
            // default handling if explicit ime options have not been given,
            // instead turning this into the normal enter key codes that an
            // app may be expecting.
			//如果是ACTION_NEXT，点击之后光标跳转到下一个EditText处
            if (actionCode == EditorInfo.IME_ACTION_NEXT) {
                View v = focusSearch(FOCUS_FORWARD);
                if (v != null) {
                    if (!v.requestFocus(FOCUS_FORWARD)) {
                        throw new IllegalStateException("focus search returned a view " +
                                "that wasn't able to take focus!");
                    }
                }
                return;

			//如果是ACTION_PREVIOUS，点击之后光标跳转到上一个EditText处
            } else if (actionCode == EditorInfo.IME_ACTION_PREVIOUS) {
                View v = focusSearch(FOCUS_BACKWARD);
                if (v != null) {
                    if (!v.requestFocus(FOCUS_BACKWARD)) {
                        throw new IllegalStateException("focus search returned a view " +
                                "that wasn't able to take focus!");
                    }
                }
                return;

			//如果是ACTION_DONE，点击之后隐藏软键盘
            } else if (actionCode == EditorInfo.IME_ACTION_DONE) {
                InputMethodManager imm = InputMethodManager.peekInstance();
                if (imm != null && imm.isActive(this)) {
					//隐藏软键盘
                    imm.hideSoftInputFromWindow(getWindowToken(), 0);
                }
                return;
            }
        }

        ViewRootImpl viewRootImpl = getViewRootImpl();
        if (viewRootImpl != null) {
            long eventTime = SystemClock.uptimeMillis();
            viewRootImpl.dispatchKeyFromIme(
                    new KeyEvent(eventTime, eventTime,
                    KeyEvent.ACTION_DOWN, KeyEvent.KEYCODE_ENTER, 0, 0,
                    KeyCharacterMap.VIRTUAL_KEYBOARD, 0,
                    KeyEvent.FLAG_SOFT_KEYBOARD | KeyEvent.FLAG_KEEP_TOUCH_MODE
                    | KeyEvent.FLAG_EDITOR_ACTION));
            viewRootImpl.dispatchKeyFromIme(
                    new KeyEvent(SystemClock.uptimeMillis(), eventTime,
                    KeyEvent.ACTION_UP, KeyEvent.KEYCODE_ENTER, 0, 0,
                    KeyCharacterMap.VIRTUAL_KEYBOARD, 0,
                    KeyEvent.FLAG_SOFT_KEYBOARD | KeyEvent.FLAG_KEEP_TOUCH_MODE
                    | KeyEvent.FLAG_EDITOR_ACTION));
        }
    }

系统会首先判断用户实现的方法ict.onEditorActionListener.onEditorAction(this, actionCode, null)的返回值，一旦返回true，会立即return，因此系统的处理被直接跳过。 

如果想自己实现部分功能，然后其他的基本操作由系统完成，那就在实现方法onEditorAction（）是返回false。

## 搞定软键盘 ##

### 软键盘显示原理 ###

软键盘的本质是什么？软键盘其实是一个Dialog。

InputMethodService为我们的输入法创建了一个Dialog，并且将该Dialog的Window的某些参数(如Gravity)进行了设置，使之能够在底部或者全屏显示。当我们点击输入框时，系统对活动主窗口进行调整，从而为输入法腾出相应的空间，然后将该Dialog显示在底部，或者全屏显示。

### 软键盘显示的调整 ###

Android定义了一个属性，名字为windowSoftInputMode，这个属性用于设置Activity主窗口与软键盘的交互模式，用于避免软键盘遮挡内容的问题。我们可以在AndroidManifest.xml中对Activity进行设置。如：android:windowSoftInputMode="stateUnchaged|adjustPan"。

该属性可选的值有两部分，一部分为软键盘的状态控制，控制软键盘是隐藏还是显示，另一部分是Activity窗口的调整，以便腾出空间展示软键盘。android:windowSoftInputMode的属性设置必须是下面中的一个值，或一个"state"值加一个"adjust"值的组合，各个值之间用|分开。

- stateUnspecified-未指定状态：当我们没有设置android:windowSoftInputMode属性的时候，软键盘默认采用的就是这种交互方式，系统会根据界面采取相应的软键盘的显示模式。
- stateUnchanged-不改变状态：当前界面的软键盘状态，取决于上一个界面的软键盘状态，无论是隐藏还是显示。
- stateHidden-隐藏状态：当设置该状态时，软键盘总是被隐藏，不管是否有输入的需求。
- stateAlwaysHidden-总是隐藏状态：当设置该状态时，软键盘总是被隐藏，和stateHidden不同的是，当我们条状到下个界面，如果下个界面的软键盘是显示的，而我们再次回来的时候，软键盘就会隐藏起来。
- stateVisible-可见状态：当设置为这个状态时，软键盘总是可见的，即使再界面上没有输入框的情况下也可以强制弹出来。
- stateAlwaysVisible-总是显示状态：当设置为这个状态时，软键盘总是可见的，和stateVisivble不同的是，当我们跳转到下一个界面，如果下个界面软键盘是隐藏的，而我们再次回来的时候，软键盘就会显示出来。
- adjustUnspecified-未指定模式：是指软键盘与界面的显示内容之间的显示关系。当我们没有设置这个值的时候，这个选项也是默认的设置模式。这这种情况下，系统会根据界面选择不同的模式。
- adjustResize-调整模式：该模式下窗口总是调整屏幕的大小用于保证软键盘的显示空间，这个选项不能和adjustPan同时使用，如果这两个属性都没有被设置，系统会根据窗口中的布局自动选择其中一个。
- adjustPan-默认模式：该模式下通常不会调整来保证软键盘的空间，而是采取了另外一种策略，系统会通过布局的移动，来保证用户要进行输入的输入框肯定在用户的视野范围里面，从而让用户可以看到自己输入的内容。


一个页面中如果有很多输入框，当输入最下面的输入框时，上面的输入框可能因为软键盘的弹出而被顶上去，如果界面不支持滚动的话，就不可以通过滚动去查看上面的输入，所以为了方便用户，在有很多输入框的界面中要使用ScrollView包括起来。


## 自动弹出软键盘 ##

有时候需要一进入Activity后就自动弹出软键盘，可以通过设置一个时间函数来实现，具体写法如下： 


    new Timer().schedule(new TimerTask() {
            @Override
            public void run () {
                InputMethodManager inputMethodManager = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
                inputMethodManager.toggleSoftInput(0,InputMethodManager.HIDE_NOT_ALWAYS);
            }
        },1000);


## 不自动弹出软键盘 ##

有时进入Activity后不希望系统自动弹出软键盘，我们可以按照下面的方法来实现： 

方法一： 

在AndroidMainfest.xml中选择那个activity，设置windowSoftInputMode属性为adjustUnspecified|stateHidden


    <activity Android:name=".Main"
            Android:label="@string/app_name"
            Android:windowSoftInputMode="adjustUnspecified|stateHidden"
            Android:configChanges="orientation|keyboardHidden">
            <intent-filter>
                <action Android:name="android.intent.action.MAIN" />
                <category Android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
	</activity>

方法二： 


让EditText失去焦点，使用EditText的clearFocus方法


    EditText edit=(EditText)findViewById(R.id.edit);
              edit.clearFocus();

方法三： 

强制隐藏Android输入法窗口

    EditText edit=(EditText)findViewById(R.id.edit); 
    InputMethodManager imm = (InputMethodManager)getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.hideSoftInputFromWindow(edit.getWindowToken(),0);




## 软键盘无法顶起页面 ##

开发中有个需求是将页面底部的一个按钮顶起，但是开发时发现Android5.0以后的版本设置了adjustResize属性后无法成功顶起。纠结了好久，最后在stackoverflow找到解决方案，那就是在根布局上加上fitsSystemWindow=”true”即可。


    <activity
			加上下面一句，让主界面自动调整
            android:windowSoftInputMode="adjustResize"
            android:name=".uiview.TextViewActivity"/>

如果加上上面的一句之后不好使，就在根布局上加上fitsSystemWindow=”true”

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    	android:layout_width="match_parent"
    	android:layout_height="match_parent"
    	android:fitsSystemWindows="true"
	/>    


![](https://github.com/JerryIreya/mynote/blob/master/00_Android%20Fundamental/images/softInputMode.jpg?raw=true)

[彻底搞定Android开发中软键盘的常见问题](http://blog.csdn.net/mynameishuangshuai/article/details/51567357)


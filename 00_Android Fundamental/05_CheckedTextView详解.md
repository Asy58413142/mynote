### CheckedTextView是什么 ###

CheckedTextView是TextView的子类，从名字来看，是可以被选中的一个TextView，类似CheckBox


![](http://i.imgur.com/kmBRT3B.png)

只不过是CheckBox的选框在左，文字在右。CheckedTextView的选框在右，文字在左。


使用很简单：


在xml文件中使用CheckedTextView

    <CheckedTextView
        android:gravity="center"
        android:id="@+id/checkedtextview"

		//下面这行为CheckedTextView设置右边的小框框
        android:checkMark="?android:listChoiceIndicatorMultiple"
        android:layout_width="match_parent"
        android:text="可勾选的textview"

		//默认选中状态
        android:checked="true"
        android:layout_height="wrap_content"/>


在java代码中使用

    final CheckedTextView checkedTextView = (CheckedTextView) findViewById(R.id.checkedtextview);

        //给CheckedTextView设置点击事件
        checkedTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick (View v) {
                //如果已经是选中状态，给它设置成未选中状态，反之亦然。
                if (checkedTextView.isChecked()) {
                    checkedTextView.setChecked(false);
                } else
                    checkedTextView.setChecked(true);

				//上面的if--else语句相当于下面这行代码
				checkedTextView.toggle();//将CheckedTextView的状态反转，true就设置成false，false就设置成true
            }
        });

使用CheckedTextView实现点击改变右侧箭头朝向

    final CheckedTextView checkedTextView2 = (CheckedTextView) findViewById(R.id.checkedtextview2);

        //使用CheckedTextView实现点击改变右侧箭头朝向
        checkedTextView2.setCheckMarkDrawable(android.R.drawable.arrow_down_float);
        checkedTextView2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick (View v) {
                if (((CheckedTextView) v).isChecked())
					//改成向上的箭头
                    checkedTextView2.setCheckMarkDrawable(android.R.drawable.arrow_up_float);
                else
					//改成向下的箭头
                    checkedTextView2.setCheckMarkDrawable(android.R.drawable.arrow_down_float);
                //最后别忘了将CheckedTextView的状态反转
                checkedTextView2.toggle();
            }
        });



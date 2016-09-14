#Android Material学习
##
* 界面跳转动画代码：
##
    startActivity(new Intent(this,SecondActivity.class), ActivityOptions.makeSceneTransitionAnimation(this).toBundle()); 
* 文字输入提示
##
    <android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <EditText
            android:id="@+id/edt_password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="password"/>
    </android.support.design.widget.TextInputLayout>
* 对输入框中的内容进行校验
##
    public void onClick(View v) {
             hideKeyboard();
     String username = usernameWrapper.getEditText().getText().toString();
     String password = usernameWrapper.getEditText().getText().toString();
          if (!validateEmail(username)) {
                 usernameWrapper.setError("Not a valid email address!");
           } else if (!validatePassword(password)) {
                 passwordWrapper.setError("Not a valid password!");
           } else {
                 usernameWrapper.setErrorEnabled(false);
                 passwordWrapper.setErrorEnabled(false);
                 doLogin();
           }
    }
* ToolBar默认是居右，改变成居中。
##
    由于toolbar有一个16dp的content inset。
    app:contentInsetStart="0dp"
    //toolbar的这个属性就是让居右为0dp完全实现让标题栏居中
    //当然还要添加如下：
     <TextView
          android:textColor="@android:color/white"
          android:text="首页"
          android:gravity="center"
          android:layout_width="match_parent"
          android:layout_height="match_parent" />
    //这样标题栏自然就会居中

* CoordinatorLayout
###介绍：
#######CoordinatorLayout使得子view之间知道了彼此的存在，一个子view的变化可以通知到另一个子view，CoordinatorLayout 所做的事情就是当成一个通信的桥梁，连接不同的view，使用 Behavior 对象进行通信。比如：在CoordinatorLayout中使用AppBarLayout，如果AppBarLayout的子View（如ToolBar、TabLayout）标记了app:layout_scrollFlags滚动事件,那么在CoordinatorLayout布局里其它标记了app:layout_behavior的子View（LinearLayout、RecyclerView、NestedScrollView等）就能够响应（如ToolBar、TabLayout）控件被标记的滚动事件。
1. 隐藏和显示AppBar
##
    <android.support.design.widget.AppBarLayout
          android:layout_width="match_parent"
         android:layout_height="wrap_content"
        android:fitsSystemWindows="true">
    //scrollFlags：来确定其行为
        <android.support.v7.widget.Toolbar
            android:id="@+id/myToolBar"
            android:layout_width="match_parent"
            android:layout_height="56dp"
            android:background="@color/colorAccent"
            android:elevation="4dp"
            app:title="haha"
            app:logo="@drawable/tool_bar_1"
            app:layout_scrollFlags="scroll|enterAlways"
            />
    </android.support.design.widget.AppBarLayout>
    //behavior 来执行并实现其行为
    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        /> 

----------

###layout_scrollFlags：
* scroll: 想滚动就必须设置这个， 没有设置，view将被固定在屏幕顶部
* enterAlways:实现quick return效果, 当向下移动时，立即显示View（比如Toolbar)
* enterAlwaysCollapsed:当你的View已经设置minHeight属性又使用此标志时，你的View只能以最小高度进入，只有当滚动视图到达顶部时才扩大到完整高度。
* exitUntilCollapsed:向上滚动时收缩View，但可以固定Toolbar一直在上面。
###注意：
* AppBarLayout必须作为Toolbar的父布局容器
* AppBarLayout是支持手势滑动效果的，不过需要跟CoordinatorLayout配合使用
* 为了AppBar可以滚动，需要在CoordinatorLayout里面,放一个带有可滚动的View，只支持NestedScrollView和RecycleView，不支持listview
* 为了AppBar可以滚动，给需要滑动的组件（Toolbar、CollapsingToolbarLayout）设置 app:layout_scrollFlags属性; 给滑动的组件设置app:layout_behavior属性

----------
* CollapsingToolbarLayout

###介绍：
#####CollapsingToolbarLayout作用是提供了一个可以折叠的Toolbar，它继承至FrameLayout，给它设置layout_scrollFlags，它可以控制包含在CollapsingToolbarLayout中的控件(如：ImageView、Toolbar)在响应layout_behavior事件时作出相应的scrollFlags滚动事件(移除屏幕或固定在屏幕顶端)。

    <?xml version="1.0" encoding="utf-8"?>
    <android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/frameLayout"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
          tools:context="com.liyi.loadmorelistview.MyChildActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="250dp"
        android:fitsSystemWindows="true">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsingToolbarLayout"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:expandedTitleMarginEnd="64dp"
            app:expandedTitleMarginStart="48dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            app:contentScrim="#a0000000">

            <ImageView
                android:src="@drawable/ic_1"
                android:scaleType="centerCrop"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                app:layout_collapseParallaxMultiplier="0.5"
                app:layout_collapseMode="parallax"/>


            <android.support.v7.widget.Toolbar
                android:id="@+id/myToolBar"
                android:layout_width="match_parent"
                android:layout_height="56dp"
                android:elevation="4dp"
                app:title="haha"
                app:layout_collapseMode="pin"
                />
        </android.support.design.widget.CollapsingToolbarLayout>

    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        >

        ...

    </android.support.v4.widget.NestedScrollView>


    </android.support.design.widget.CoordinatorLayout>


----------
1、在CollapsingToolbarLayout中
contentScrim - 设置当CollapsingToolbarLayout完全折叠(收缩)后的背景颜色
expandedTitleMarginStart - 设置扩张时候(还没有收缩时)title向左填充的距离。

     layout_scrollFlags="scroll|exitUntilCollapsed"
###
2、在ImageView控件中：
#####layout_collapseMode
* pin - 当CollapsingToolbarLayout完全收缩后，Toolbar还可以保留在屏幕上。
* parallax - 在内容滚动时，CollapsingToolbarLayout中的View（比如ImageView)也可以同时滚动，实现视差滚动效果，通常和layout_collapseParallaxMultiplier(设置视差因子)搭配使用。
#####layout_collapseParallaxMultiplier(视差因子) - 设置视差滚动因子，值为：0~1。
###
3、Toolbar控件中：

* layout_collapseMode(折叠模式)：为pin。
#####监听appBarLayout
使用OnOffsetChangedListener
#
  * verticalOffset==0时，说明是展开的；
  * verticalOffset<0，开始收缩了；
  * |verticalOffset|==H(AppBarLayout-ToolBar-StatusBar)，已经完全收缩了

代码如下：

    appBarLayout.addOnOffsetChangedListener(new AppBarLayout.OnOffsetChangedListener() {
        @Override
        public void onOffsetChanged(AppBarLayout appBarLayout, int verticalOffset) {
            if (Math.abs(verticalOffset) >= mCollapsingToolbarLayout.getHeight() - mToolbar.getHeight()-statusBarHeight) {
                //toolBar在这里收缩了
            }
        }
    });

      ---

    @Override
    public void onWindowFocusChanged(boolean hasFocus) {
        super.onWindowFocusChanged(hasFocus);
        Rect frame = new Rect();
        getWindow().getDecorView().getWindowVisibleDisplayFrame(frame);
        // 状态栏高度
         statusBarHeight = frame.top;
    }


----------





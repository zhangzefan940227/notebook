1. Android启动流程拆解：init  zygote  systemserver  launcher  activity
2. Android 属性动画最全解析
3. C++之旅 第三版
4. www.programmercarl.com
5. leetcode 刷题顺序、按标签分类、科学刷题(https://blog.csdn.net/fengyuyeguirenenen/article/details/125099023?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170174413116800186543421%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170174413116800186543421&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-125099023-null-null.142^v96^pc_search_result_base7&utm_term=LeetCode%E5%88%B7%E9%A2%98%E9%A1%BA%E5%BA%8F&spm=1018.2226.3001.4187)

# View事件体系源码分析
整体上，点击事件是按照Activity->Window(PhoneWindow)->DecorView->View顺序进行分发的。

**首先看Activity的dispatchTouchEvent()方法**
```java
public boolean dispatchTouchEvent(MotionEvent ev) {
    //当用户执行任何交互操作时，系统会自动调用该方法。主要是为了使屏幕保持交互状态等效果。
    if (ev.getAction() == MotionEvent.ACTION_DOWN) {
        //通知应用程序，用户正在与应用程序进行交互
        onUserInteraction(); 
    }
    //通过Window分发事件，如果Window中有子View消耗了点击事件，则返回true，不再执行Activity的onTouchEvent方法
    if (getWindow().superDispatchTouchEvent(ev)) {
        return true;
    }
    return onTouchEvent(ev);
}
```
**接着看getWindow().superDispatchTouchEvent()**
```java
// Activity的attach方法中，实例化了PhoneWindow对象
// mWindow = new PhoneWindow(this, window, activityConfigCallback);
public Window getWindow() {
    return mWindow;
}

// Window实际上是将分发事件交给了DecorView这个根ViewGroup进行
// DecorView实例化是通过setContentView()->installDecor()->generateDecor()创建
public boolean superDispatchTouchEvent(MotionEvent event) {
    return mDecor.superDispatchTouchEvent(event);
}

//此处开始事件分发的基本流程
public boolean superDispatchTouchEvent(MotionEvent event) {
    return super.dispatchTouchEvent(event);
}
```


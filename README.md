1. Android启动流程拆解：init  zygote  systemserver  launcher  activity
2. Android 属性动画最全解析
3. C++之旅 第三版
4. www.programmercarl.com
5. leetcode 刷题顺序、按标签分类、科学刷题(https://blog.csdn.net/fengyuyeguirenenen/article/details/125099023?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170174413116800186543421%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170174413116800186543421&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-125099023-null-null.142^v96^pc_search_result_base7&utm_term=LeetCode%E5%88%B7%E9%A2%98%E9%A1%BA%E5%BA%8F&spm=1018.2226.3001.4187)
6. https://www.zhihu.com/question/595810238

# View事件分发源码分析
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
**接下来就是ViewGroup中的事件分发流程**
```java
public boolean dispatchTouchEvent(MotionEvent ev) {
        if (mInputEventConsistencyVerifier != null) {
            mInputEventConsistencyVerifier.onTouchEvent(ev, 1);
        }

        // If the event targets the accessibility focused view and this is it, start
        // normal event dispatch. Maybe a descendant is what will handle the click.
        if (ev.isTargetAccessibilityFocus() && isAccessibilityFocusedViewOrHost()) {
            ev.setTargetAccessibilityFocus(false);
        }

        boolean handled = false;
        if (onFilterTouchEventForSecurity(ev)) {
            final int action = ev.getAction();
            final int actionMasked = action & MotionEvent.ACTION_MASK;

            // Handle an initial down.
            // 当接收到Down事件后，View会清除之前正在处理的事件并重置View处理事件的初始状态
            if (actionMasked == MotionEvent.ACTION_DOWN) {
                // Throw away all previous state when starting a new touch gesture.
                // The framework may have dropped the up or cancel event for the previous gesture
                // due to an app switch, ANR, or some other state change.
                cancelAndClearTouchTargets(ev);
                resetTouchState();
            }

            // Check for interception.
            // 是否需要拦截事件标志位，默认false，说明ViewGroup默认不拦截事件
            final boolean intercepted;
            if (actionMasked == MotionEvent.ACTION_DOWN
                    || mFirstTouchTarget != null) {
                // FLAG_DISALLOW_INTERCEPT标志是由子View给父View设置的标志位，一旦设置，父View就无法拦截除Down之外的其他事件了
                final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
                if (!disallowIntercept) {
                    intercepted = onInterceptTouchEvent(ev);
                    ev.setAction(action); // restore action in case it was changed
                } else {
                    intercepted = false;
                }
            } else {
                // There are no touch targets and this action is not an initial down
                // so this view group continues to intercept touches.
                intercepted = true;
            }

          ...
        return handled;
    }
}
```
**当ViewGroup不拦截事件时，事件会向下分发由它的子View进行处理**
```java
public boolean dispatchTouchEvent(MotionEvent ev) {
    ...
    // buildTouchDispatchChildList()构建触摸事件分发的子视图列表
    final ArrayList<View> preorderedList = buildTouchDispatchChildList();
    // 是否开启自定义绘制顺序，false表示使用默认绘制顺序
    final boolean customOrder = preorderedList == null && isChildrenDrawingOrderEnabled();
    final View[] children = mChildren;
    for (int i = childrenCount - 1; i >= 0; i--) {
        // 获取子View列表
        final int childIndex = getAndVerifyPreorderedIndex(
                childrenCount, i, customOrder);
        // 遍历子View列表
        final View child = getAndVerifyPreorderedView(
                preorderedList, children, childIndex);

        // If there is a view that has accessibility focus we want it
        // to get the event first and if not handled we will perform a
        // normal dispatch. We may do a double iteration but this is
        // safer given the timeframe.
        // 获取子View的索引和视图，并进行有效性验证
        if (childWithAccessibilityFocus != null) {
            if (childWithAccessibilityFocus != child) {
                continue;
            }
            childWithAccessibilityFocus = null;
            i = childrenCount;
        }

        // 判断子元素能否接收到点击事件
        if (!child.canReceivePointerEvents() // 子元素是否在播放动画
                || !isTransformedTouchPointInView(x, y, child, null)) { // 点击事件是否落在子元素区域内
            ev.setTargetAccessibilityFocus(false);
            continue;
        }

        newTouchTarget = getTouchTarget(child);
        if (newTouchTarget != null) {
            // Child is already receiving touch within its bounds.
            // Give it the new pointer in addition to the ones it is handling.
            newTouchTarget.pointerIdBits |= idBitsToAssign;
            break;
        }
        resetCancelNextUpFlag(child);
        // 通过dispatchTransformedTouchEvent分发触摸事件，会调用子View的dispatchTouchEvent
        if (dispatchTransformedTouchEvent(ev, false, child, idBitsToAssign)) {
            // Child wants to receive touch within its bounds.
            mLastTouchDownTime = ev.getDownTime();
            if (preorderedList != null) {
                // childIndex points into presorted list, find original index
                for (int j = 0; j < childrenCount; j++) {
                    if (children[childIndex] == mChildren[j]) {
                        mLastTouchDownIndex = j;
                        break;
                    }
                }
            } else {
                mLastTouchDownIndex = childIndex;
            }
            mLastTouchDownX = ev.getX();
            mLastTouchDownY = ev.getY();
            newTouchTarget = addTouchTarget(child, idBitsToAssign);
            alreadyDispatchedToNewTouchTarget = true;
            break;
        }

        // The accessibility focus didn't handle the event, so clear
        // the flag and do a normal dispatch to all children.
        ev.setTargetAccessibilityFocus(false);
    }
...
    return handled;
}
```

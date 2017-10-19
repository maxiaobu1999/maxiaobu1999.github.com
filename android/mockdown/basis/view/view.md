##view总结

* [view系统级绘制](https://maxiaobu1999.github.io/html5/heima/README.html)
    * activity有个window对象，PhoneWindow是唯一实现类，内部类DecorView是顶级父view，viewRootImpl负责windowManagerService与DecorView进行交互
    * decorView   PhoneWindow的内部类 通过decorView实现消息传递
    * Android应用程序调用SurfaceFlinger服务把经过测量、布局和绘制后的Surface渲染到显示屏幕上。
    * SurfaceFlinger：Android系统服务，负责管理Android系统的帧缓冲区，即显示屏幕。
    * Surface：Android应用的每个窗口对应一个画布（Canvas），即Surface，可以理解为Android应用程序的一个窗口。
    * Android应用程序的显示过程包含了两个部分（应用侧绘制、系统侧渲染）、两个机制（进程间通讯机制、显示刷新机制）
* [view树的绘制流程](https://maxiaobu1999.github.io/html5/heima/README.html)
    * view的绘制由ViewRoot的实现类完成，三个步骤measure，layout，draw从viewRoot的performTraversals()开始执行，顶级view是decorView，
        ，view 树的遍历方式是遍历一代子 view，
        如果有viewGroup实例，就递归遍历这个viewGroup的子元素，一直到整个view树遍历完成为止
    * ViewRoot是连接WindowManager与DecorView的纽带，View绘制流程的三大步（measure、layout、draw）都是通过ViewRoot完成的。
    当Activity对象被创建完毕后，会将DecorView添加到Window中（Window是对窗口的抽象，其具体实现类是phone Window，DecorView是一个窗口的顶级容器View，其本质是一个FrameLayout），同时会创建ViewRootImpl（ViewRoot的实现类）对象，
    并将ViewRootImpl与DecorView建立关联。关于ViewRoot，我们只需要知道它是联系GUI管理系统和GUI呈现系统的纽带。View的绘制流程从ViewRoot的performTraversals方法开始，经过measure、layout、draw三
    个过程完成对一个View的绘制工作。peformTraversal方法内部会调用measure、layout、draw这三个方法，这三个方法内部又分别调用onMeasure、onLayout、onDraw方法。
         在onMeasure方法中View会对其所有的子元素执行measure过程，此时measure过程就从父容器"传递"到了子元素中，接着子元素会递归的对其子元素进行measure过程，如此反复完成对整个View树的遍历。onLayout与onDraw过程的执行流程与此类似。
         measure过程决定了View的测量宽高，这个过程结束后，就可以通过getMeasuredHeight和getMeasuredWidth获得View的测量宽高了；
         layout过程决定了View在父容器中的位置和View的最终显示宽高，getTop等方法可获取View的top等四个位置参数（View的左上角顶点的坐标为(left, top), 右下角顶点坐标为(right, bottom)），getWidth和getHeight可获得View的最终显示宽高（width = right - left；height = bottom - top）。
         draw过程决定了View最终显示出来的样子，此过程完成后，View才会在屏幕上显示出来。
    * 三个主要流程measure layout draw，这三个流程以责任链的方式依次调用，来完成整个view树的绘制，
    view 树的绘制是遍历一代子view，如果有view group的实例那么由这个view group继续变遍历完成view树的绘制，
    view 树的顶层view是decorView，绘制工作主要由viewRoot的实现类完成，这两个都是window的成员变量，
    虽然每个activity都有window对象，但是view的绘制流程与activity的生命周期没有关联
    * activity都有一个window对象，在phoneWindow中有两个重要成员DecorView和ViewRoot，DoctorView本身是一个桢布局，是window中的顶级view；view的绘制主要由viewRoot
        完成，在ViewRoot的performTravels（）执行对DecorView的测量布局绘制流程，拿测量来说，通过for循环遍历DoctorView的子 view，在循环体中判断每一个view是不是viewGroup的实例
        如果是，就继续测量该view group的子 view，通过这种方式完成整个view树的测量。布局绘制的流程与测量的过程基本上一样，
    * 绘制工作是由ViewRoot完成的，依次执行测量布局绘制三个过程，通常顶级视图是DecorView，ViewRoot首先会遍历DecorView的子视图，如果子视图
    * 从ViewRootImpl的performTraversals方法开始，在performTraversals方法中会调用performMeasure、performLayout、performDraw三个方法来遍历完成整棵视图树的绘制。

        //-----------------获取 activity中的所有view  
        private void getAllViews(Activity act) {  
            List<View> list = getAllChildViews(act.getWindow().getDecorView());  
        }  
      
        private List<View> getAllChildViews(View view) {  
            List<View> allchildren = new ArrayList<View>();  
            if (view instanceof ViewGroup) {  
                ViewGroup vp = (ViewGroup) view;  
                for (int i = 0; i < vp.getChildCount(); i++) {  
                    View viewchild = vp.getChildAt(i);  
                    allchildren.add(viewchild);  
                    //再次 调用本身（递归）  
                    allchildren.addAll(getAllChildViews(viewchild));  
                }  
            }  
            return allchildren;  
        }
* [measure](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 开始viewGroup 遍历子视图  调用子视图的measure（），根据父的measureSpec和子的paramLayout决定子视图的大小 
    * ViewGroup.LayoutParams
        * match_parent 不包括padding值
    * MeasureSpec 测量规格  32位int值
        * 高两位   测量模式   低30位记录测量大小
        * EXACTLY 精确值模式  对应android:layout_width=”50dp”
        * AT_MOST 最大值模式 父容器指定了一个可用大小specSize  只能是specSize中指定的大小，对应wrap_content
        * UNSPECIFIED 不确定模式，父容器不对View有任何限制，一般不会用到
    * measure（）中调用 onMeasure（）       
    * onMeasure（）
        * int widthMeasureSpec  宽的测量规格
        * setMeasureDimension（）  实现onMeasure的方法  必须调用
* [layout](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 空实现
* [draw](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 两个容易混淆的方法 
        * invalidate() 大小没有变化，会draw（） ，不会调用layout（）
        * requestLayout（） 会调用draw（） 不会layout（）
* [事件分发机制](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 三个主要方法dispatch、intercept、onTouch，事件由系统包装成MotionEvent进行传递，
     应用层最先接收到的是ViewRoot，第一个dispatch()在decorView中然后调用activity的dispatch（）
    ；之后事件会依次向子view传递，再都不拦截的情况下，会由子到父的向上调用onTouch()
    * [view基础](https://maxiaobu1999.github.io/html5/heima/README.html)
        * View的位置
            ![view位置](http://pic.92to.com/201612/23/c6a5e36cd2864841abbcdcd2a691b68e_th.jpg)
        * MotionEvent   触摸事件 
        * TouchSlop     最小滑动距离
        * VelocityTracker  速度追踪
        * GestureDetector 手势检测
        * Scroller        弹性滑动，有过渡效果
    * [view滑动](https://maxiaobu1999.github.io/html5/heima/README.html)
        * scrollTo()/scrollBy()  滑动到指定位置/相对当前位置滑动 view本身不移动，不能超过view范围，内容移动点击有效
        * 使用动画   view动画只有影像移动，不适用交互eg：拖动
        * LayoutParams  交互多的使用
    * [弹性滑动](https://maxiaobu1999.github.io/html5/heima/README.html)
        * scroller 只有内容滑动，但是调用invalidate computeScroll空方法实现重绘，根据时间算距离
        * onAnimationUpdate()中根据每一帧百分比算距离
        * 使用handler延时滑动
    * [为什么会有事件分发机制](https://maxiaobu1999.github.io/html5/heima/README.html)
        * view是树形结构的，view可能会重叠在一起，多个view会相应同一点击，这个事件给谁
    * [三个重要的事件分发的方法](https://maxiaobu1999.github.io/html5/heima/README.html)
        * dispatchTouchEvent
        * onInterceptTouchEvent
            * activity和view没有
        * onTouchEvent
    * [事件分发的流程](https://maxiaobu1999.github.io/html5/heima/README.html)
    * activity->PhoneWindow->DecorView->ViewGroup->...->View
        * 屏幕点击事件首先传递给activity->view的实现管理类->通过内部类DecorView进行消息传递
        ->外面最大的父容器viewGroup->依次传递给子view->依次反转回activity
        ![事件分发](http://upload-images.jianshu.io/upload_images/966283-b9cb65aceea9219b.png?imageMogr2/auto-orient/strip%7CimageView2/2)
    
##view总结

* [view系统级绘制](https://maxiaobu1999.github.io/html5/heima/README.html)
    * phoneWindow window的唯一实现类 view最顶层的管理容器
    * decorView   PhoneWindow的内部类 通过decorView实现消息传递
    * Android应用程序调用SurfaceFlinger服务把经过测量、布局和绘制后的Surface渲染到显示屏幕上。
    * SurfaceFlinger：Android系统服务，负责管理Android系统的帧缓冲区，即显示屏幕。
    * Surface：Android应用的每个窗口对应一个画布（Canvas），即Surface，可以理解为Android应用程序的一个窗口。
    * Android应用程序的显示过程包含了两个部分（应用侧绘制、系统侧渲染）、两个机制（进程间通讯机制、显示刷新机制）
* [view树的绘制流程](https://maxiaobu1999.github.io/html5/heima/README.html)
    *从ViewRootImpl的performTraversals方法开始，在performTraversals方法中会调用performMeasure、performLayout、performDraw三个方法来遍历完成整棵视图树的绘制。

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
![Handler类图](http://img-1253423006.costj.myqcloud.com/Handler%E7%B1%BB%E5%9B%BE.jpg)

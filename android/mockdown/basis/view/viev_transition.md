##事件分发总结

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

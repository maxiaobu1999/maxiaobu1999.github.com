##handler总结
    作用：与用户交互的界面
*   [什么是handler](https://maxiaobu1999.github.io/html5/heima/README.html)
    * Android message机制的上层接口
    * 通过发送和处理Message和Runnable对象来关联相应线程的MessageQueue
    * 可以让对应的Message和Runnable再未来的某个时间点进行相应处理
    * 让自己想要处理的耗时操作放在子线程，让更新ui的操作放在主线程
*   [handler的使用方法](https://maxiaobu1999.github.io/html5/heima/README.html)
    * post(runnable)
    * sendMessage(message)
*   [handler机制原理](https://maxiaobu1999.github.io/html5/heima/README.html)
    * Looper线程独有，prepare()创建looper关联thread，创建MessageQueue，loop()读取MessageQueue当中的消息，dispatchMessage()中调用handler.handleMessage()
    * 读到消息后交给Handler处理，发送消息&处理消息 这货sThreadLocal。get（）持有mLooper
*   [handler引起的内存泄露以及解决办法](https://maxiaobu1999.github.io/html5/heima/README.html)
    * mHandler.removeCallbacks();
    * static weak
![Handler类图](http://img-1253423006.costj.myqcloud.com/Handler%E7%B1%BB%E5%9B%BE.jpg)
 
##handler总结
    作用：与用户交互的界面
*   [什么是handler](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 当创建handler时，会从当前进程中取出Looper对象，也就持有了messageQueue对像，handler调用queue的添加message时，queue会调用native方法通知looper开始轮询，在message中获取handler对象调用handlerMessage()
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
##handlerThread总结
* handlerThread 产生背景
    * 开启thread自线程进行耗时操作
    * 多次创建和销毁线程是很销毁系统资源的
    * 
* handlerThread是什么
    * handler+Thread+looper
    * 是一个thread内部有looper
* handlerThread特点
    * HandlerThread本质上是一个线程类，它继承了Thread
    * handlerThread有自己的内部Looper对象，可以进行looper循环
    * 通过获取HandlerThread的looper对象传递给Handler对象，可以在handleMessage方法中执行异步任务
    * 优点是不会阻塞，减少了对性能的消耗，缺点是不能同时惊醒多任务的处理，需要等待进行处理。处理效率低。
    * 与线程池注重并发不同，HandlerThread是一个串行队列，HandlerThread背后只有一个线程
* 源码解析
##IntentService
* IntentService是什么
    * IntentService是继承并处理异步请求的一个类，在IntentService内有一个工作线程来处理耗时操作，
    启动IntentService的方式和启动传统的Service一样，同时，当任务执行完后，IntentService会自动
    停止，而不需要手动控制或stopSelf()。另外，可以启动IntentService多次，而每一个耗时操作会以工作
    队列的方式在IntentService的onHandleIntent回调方法中执行，并且，每次只会执行一个工作线程，执行完
    第一个在执行第二个
    * 本质上是一种Service，继承自Service并且本身就是一个抽象类
    * 它内部通过HandlerThread和Handler实现有操作
* IntentService使用方法
    * MyIntentService extend IntentService
    * 构造MyIntentService（"线程名称"）
    * activity 中设置回调
    * onHandleIntent(Intent)  处理耗时操作
    * 处理完成后执行回调方法，更新ui
* IntentService 源码分析
    *本质上就是一个封装了handlerThread和Handler的异步框架
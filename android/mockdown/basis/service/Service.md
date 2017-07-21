##Activity总结
    作用：与用户交互的界面
*   [Service是什么](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 在后台执行长时间操作而没有界面的组件
*   [Service和thread的区别](https://maxiaobu1999.github.io/html5/heima/README.html)
    * thread轻量级进程，是程序执行流的最小单元
*   [Service的生命周期方法](https://maxiaobu1999.github.io/html5/heima/README.html)
    * onBind 绑定服务是调用
    * onCreate 首次创建时调用 只调用一次
    * onStartCommand 每次通过startService()方法启动Service时都会调用  被系统干死后再复活，也会调用，但是intent为null
*   [Service的应用场景，以及和thread的区别](https://maxiaobu1999.github.io/html5/heima/README.html)

*   [开启Service的两种方式以及区别](https://maxiaobu1999.github.io/html5/heima/README.html)
    * startService
        * 定义一个类继承service
        * 在Manifest中配置该Service
        * 使用Context的startService(Intent)方法启动该Service
        * 不在使用时，调用stopService(Intent)方法停止该服务 
    * bindService
        * 总结
            * service与activity绑定 可以交互
            * 可以跨进程交互
           
        * 使用
            * 创建BindService服务端,继承service，创建实现IBinder接口的实例并提供公共方法给客户端调用
            * OnBind()回调方法返回此Binder实例。
            * 在客户端中，创建ServiceConnection类，从onServiceConnected()回调方法接收Binder，并使用提供的方法调用绑定服务
       
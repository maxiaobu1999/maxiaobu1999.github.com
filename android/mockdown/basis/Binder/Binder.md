##Binder总结
    作用：与用户交互的界面
*   [Linux内核基础知识](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 进程隔离/虚拟地址空间
        * 给每个进程一个虚拟地址空间，每个进程相当于独享操作系统，进程间无法互相访问
        * 系统调用 用户空间可以通过系统调用访问内核部分程序，为的是不让访问部分内核
        * Binder驱动(理解为USB驱动)
*   [Binder通讯模型](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 通常意义上讲，Binder指的是一种通信机制
    * 对于Server进程来讲，Binder指的是Binder本地对象/对于Client来说，Binder指的是Binder代理对象
    * 对于传输过程而言，Binder是可以进行跨进程传递的对象
*   [Binder通讯机制原理](https://maxiaobu1999.github.io/html5/heima/README.html)
    * Service端再ServiceManager的表中注册提供的a()方法
    * Client端先去ServiceManager中查询
    * Service返回一个代理对象及a空方法给binder
    * binder根据代理对象中的空方法去Service中调用a方法
    * Service把结果返回给ServiceManager，sm再把结果返回给client
*   [为什么使用Binder](https://maxiaobu1999.github.io/html5/heima/README.html)
*   [AIDL](https://maxiaobu1999.github.io/html5/heima/README.html)
     
![binder机制图](http://img-1253423006.costj.myqcloud.com/binder.png)
##Binder总结
    作用：与用户交互的界面
*   [Linux内核基础知识](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 进程隔离/虚拟地址空间
        * 给每个进程一个虚拟地址空间，每个进程相当于独享操作系统，进程间无法互相访问
        * 系统调用 用户空间可以通过系统调用访问内核部分程序，为的是不让访问部分内核
        * Binder驱动(理解为USB驱动)
*   [Android进程分类](https://maxiaobu1999.github.io/html5/heima/README.html)
    * Zygote进程
      这个是Android框架的主要进程，所有的App进程以及系统服务进程SystemServer都是由Zygote进程Fork出来的
    * 系统服务进程SystemServer
    * App的主进程
        每一个App的运行都是在一个独立的进程，进程的名字就是App的packagename，这些进程都是从Zygote进程Fork出来的，并受AMS（ActivityManagerService）管理  
    * App的辅助进程
      可以允许App有多个进程，在AndroidManifest.xml里边配置android:process属性，就可以开启多进程，这些进程名字都是packagename:name这种，以区分是属于哪个App，我一般称之为辅助进程。但这些进程也都跟主进程一样，也是从Zygote进程Fork出来的，并受AMS管理
    * Native进程
      Android除了使用Java，还有NDK，可以使用C/C++去开发，然后这里也是可以Fork出进程的，我一般称之为Native进程，Native进程可以不受AMS管理，自由度很大    
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
##不知道分到哪总结
*   [xml解析](https://maxiaobu1999.github.io/html5/heima/README.html)
    * DOM解析
        * 整个文档解析成Document对象加载进内存，操作方便，占内存大
    * PULL解析 Android专用
        * 遍历解析器的eventType，一边解析一边处理数据，parser.next()触发下一事件
        * 主动触发事件
    * SAX  
        * 与PULL基本类似
        * 自动触发事件
*   [进程&&线程总结](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 进程
        * 每个app运行时前首先创建一个进程，该进程是由Zygote fork出来的，用于承载App上运行的各种Activity/Service等组件。进程对于上层应用来说是完全透明的，这也是google有意为之，让App程序都是运行在Android Runtime。大多数情况一个App就运行在一个进程中，除非在AndroidManifest.xml中配置Android:process属性，或通过native代码fork进程。
          
    * 线程
        * 线程对应用来说非常常见，比如每次new Thread().start都会创建一个新的线程。该线程与App所在进程之间资源共享，从Linux角度来说进程与线程除了是否共享资源外，并没有本质的区别，都是一个task_struct结构体，在CPU看来进程或线程无非就是一段可执行的代码，CPU采用CFS调度算法，保证每个task都尽可能公平的享有CPU时间片。
            
##性能优化总结
*   [anr](https://developer.android.com/topic/performance/vitals/anr.html)
    * 当应用的UI线程被阻塞时间过长，ANR会被触发，如果app在前台，系统回显示应用未响应对话框
        后台ANR需显示dialog，开启开发者模式showAllANR
    * 促发条件：
        前台有activity，input事件或broadcastReceiver 5秒内未响应
        前台无activity，BroadcastReceiver大量事件内未处理完
        非文档，service也会造成ANR
    * 鉴定问题
        * 主线程设计IO操作
        * 主线程处理长时间运算
        * 主线程对其他进程做同步binder请求，其他进程长时间未return
        * 线程死锁
    * 解决
        * 使用Thread或HandleThread时设定优先级
        * 使用Asynctask处理耗时操作  
        * 使用handler处理工作线程耗时操作
        * 在生命周期回调方法中避免避免代码
*   [oom](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 当前内存加上我们申请的内存资源超过Dalvik虚拟机的最大内存，抛出Out Of Memory异常
    * bitmap
        * 显示合适的尺寸
        * 及时释放内存 Java jni各一块，java不能回收c那块，需recycle()是jni那块内存
        * 图片压缩  BitmapFactory.Options() 不加载原图进内存
        * inBitmap属性 使用之前bitmap使用的内存
        * 捕获OutOfMemoryError，不推荐，压死骆驼最后一根草
    * 其他
        * listView convertView/lru(三级缓存) 
        * 避免在onDraw()创建对象，造成内存抖动，也可能oom
        * 谨慎使用多进程 虽然内存多了bug也多
        * 请求更大内存
*   [bitmap](https://maxiaobu1999.github.io/html5/heima/README.html)
    * recycle 清理native内存及java引用，不立即回收标记为dead，bitmap不可使用会抛异常，官方注释不建议调用 
        * 3.0之后内存 Java jni各一块，
        * 3.0前都存在堆中，因为不稳定，所以建议recycle()      
    * LruCache(LRU算法)  
        * 最近最少使用 没满往栈里放，满了栈底开始移除，栈里有移到上面，没有放栈顶
        * 内部使用LinkHashMap实现
        * trimToSize()把用的最久的缓存对象清除，并添加新的缓存对象
        * safeSizeOf()计算大小并从缓存中去掉
        * put() get() 把新对象加进去 取出来
    * 计算inSampleSize 
        * BitmapFactory.Options()算出缩放比例
    * 三级缓存    
        * 网络 本地 内存 
*   [UI卡顿](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 60fps->16ms
        * 一秒60帧 每16ms更新一次页面
    * overdraw
        * 同一帧绘制多次 多背景重叠
    * UI卡顿原因
        * ui线程中轻微耗时操作
        * layout过于复杂，无法在16ms内完成渲染
        * 同一时间动画执行过多
        * view过度绘制，背景不要重叠
        * View频繁的触发measure、layout，引起view频繁渲染
        * 内存频繁触发gc，暂时阻塞渲染操作
        * 冗余的资源及逻辑等导致加载和执行缓慢
        * anr ui卡顿就是轻量版的anr
    * 总结
        * 布局优化<include />重用、<merge />占位父布局、<ViewStub />使用时加载
            尽量不要冗余嵌套及复杂布局，用gone
        * 列表及Adapter优化
        * 背景和图片等内存分配优化，减少不必要的背景，图片需处理
*   [内存泄漏](https://maxiaobu1999.github.io/html5/heima/README.html)
    * java内存泄露基础知识
        * 无用对象持续占有内存或无用对象的内存得不到及时释放，从而造成的内存空间的浪费 
        * java内存分配策略
            * 静态储存区（方法区）
                * 存放静态数据、全局变量
                * 程序编译时已经分配好了，静态存储区的变量在程序运行期间都存在
            * 栈区
                * 方法体内定义的基本类型变量与对象的引用变量（地址）存放在栈区，超过变量作用域释放内存；
                  空间可其他方法重新使用  
                * 因为内置于处理器中所以效率高
            * 堆区    
                * 存放new出来的对象和数组,java垃圾回收器管理
        * java如何管理内存的
            * 
        * java中的内存泄露
    * Android内存泄露
        * 单例
        * 匿名内部类  非静态内部类会持有外部类引用
        * handler    looper -》message-》handler-》是aty匿名内部类
        * 避免使用static成员变量   
            * 生命周期与app一至，在后台时内存不会释放，后台进程占内存多的先死
        * 资源未关闭
            * broadcast Cursor，Stream没有close
        * AsyncTask造成内部泄露，调用cancel（）
        * WebView单独线程，持有activity引用，类似匿名内部类
             * ViewGroup中放WebView，动态add到Activity
             * 持有activity弱引用
             * activity停止时remove掉，再destroy掉WebView
    * 解决方法
        * leakCanary     检测内存泄露
        * Android Monitor  分析运行时的内存文件
*   [内存管理](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 内存管理机制概述
        * 分配机制：操作系统为每个进程分配合理大小内存，保证程序能够正常运行，不会内存不够使用，或者占用太多内存
        * 回收机制：内存不足时，会回收再分配内存资源机制，保证新的进程正常运行，杀死占有进程的机制    
    * Android内存管理机制
        * 进程分类
            * Zygote进程
              这个是Android框架的主要进程，所有的App进程以及系统服务进程SystemServer都是由Zygote进程Fork出来的
            * 系统服务进程SystemServer
            * App的主进程
                每一个App的运行都是在一个独立的进程，进程的名字就是App的packagename，这些进程都是从Zygote进程Fork出来的，并受AMS（ActivityManagerService）管理  
            * App的辅助进程
              可以允许App有多个进程，在AndroidManifest.xml里边配置android:process属性，就可以开启多进程，这些进程名字都是packagename:name这种，以区分是属于哪个App，我一般称之为辅助进程。但这些进程也都跟主进程一样，也是从Zygote进程Fork出来的，并受AMS管理
            * Native进程
              Android除了使用Java，还有NDK，可以使用C/C++去开发，然后这里也是可以Fork出进程的，我一般称之为Native进程，Native进程可以不受AMS管理，自由度很大，本文暂且不讲     
        * 分配机制
            * 先给小内存，慢慢增大，根据机型有最大值
        * 倾向于杀死能够回收更多内存的进程
    * 内存管理目标
        * 更少的占用内存
        * 在合适的时候，合理的释放系统资源
        * 在系统内存紧张的情况下，能释放掉大部分不重要的资源，来为Android系统提供可用的内存
        * 在生命周期中，保存还原重要数据，以至于系统能正确的恢复应用
    * 内存优化的方法
        * 当service完成任务后，尽量停止他
        * 系统内存紧张时，会调用OnTrimMemory()回调（类似onCreate），释放掉一些不重要的资源，如动态生成的View，图片缓存，Fragment等；    
        * 滥用Bitmap  合适大小  recycle()清c中缓存 软引用  lruCache算法
        * 使用针对内存优化过的数据容器 
            * SparseArray（key只能是int）、ArrayMap用来代替HashMap 数据量不大，最好在千级以内 
            * 如果key的类型已经确定为int类型，那么使用SparseArray，因为它避免了自动装箱的过程，如果key为long类型，它还提供了一个LongSparseArray来确保key为long类型时的使用
            * 如果key类型为其它的类型，则使用ArrayMap
            * 少用枚举
        * 避免使用依赖注入框架
        * 使用ZIP对齐APK
        * 使用多进程 把消耗大内存过大或长期运行的模块移入单独进程     
*   [冷热启动](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 冷启动
        * 应用启动前，系统中没有该应用的任何进程信息，创建新进程分配给app
    * 热启动
        * 按返回键退出应用，然后在进程被杀死前重启动，启动时进程保持在后台
    * 冷启动优化
        * 减少布局复杂性 使用viewStub 按需加载
        * 减少onCreate()工作量
        * 不在application中使用静态变量
*   [不要用static储存数据](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 进程杀死会初始化
*   [sp安全问题](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 不能跨进程同步
    * 文件过大 解析大sp文件时会创建大量临时对象
*   [内存对象序列化](https://maxiaobu1999.github.io/html5/heima/README.html)
    * Serializeble java的 序列化时会产生大量临时变量
    * parcelable   Android的  要在磁盘上存储的数据不能用Parcelable


![binder机制图](http://frodoking.github.io/img/android/okhttp_request_process.png)
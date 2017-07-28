##BroadcastReceiver总结
    作用：与用户交互的界面
*   [广播的定义](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 发送一个Intent 可被多个receiver接受
*   [广播的场景](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 同一app内具有多个进程的不同组件之间的消息通信
    * 不同app之间的组件之间的消息通信
*   [广播的种类](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 普通 Normal BroadcastReceiver:ContextBroadReceiver
    * 有序 System BroadcastReceiver:Context.sendOrderedBroadcast
    * 本地 LocalBroadcast:只在自身APP内传播  采用handler 其他binder
*   [实现广播-receiver](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 静态注册:注册完成就一直运行 app挂了依然可以接收广播
    * 动态注册:跟随activity的生命周期
*   [广播的实现机制](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 自定义广播接收者BroadcastReceiver,并复写onReceiver()
    * 通过Binder机制向AMS(Activity Manager Service)进行注册
    * 广播发送者通过Binder机制向AMS发送广播
    * AMS查找符合相应条件(IntentFilter/Permission等)的BroadcastReceiver,将广播发送到BroadcastReceiver(一般情况下是Activity)相应的消息循环队列中
    * 消息循环执行拿到此广播，回调BroadcastReceiver中的onReceive()
    * 只在APP内部传播,不必担心数据泄露
    * 比全局广播更高效 (Local内部采用handler机制)
*   [LocalBroadcastManager详解](https://maxiaobu1999.github.io/html5/heima/README.html)
    * sendBroadcast()实质是handler sendMessage() 高效 与其他广播方式不同
    * 两个map 一个list 储存关系和对象
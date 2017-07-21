##BroadcastReceiver总结
    作用：与用户交互的界面
*   [WebView常见的一些坑](https://maxiaobu1999.github.io/html5/heima/README.html)
    * api16之前 addJavascriptInterface()漏洞，利用反射执行任意java对象
    * WebView动态写在其他容器中，要先remove掉WebView，再调用WebView的removeAllView()和Disdory()
    * jsBridge  native与js交互的中间层
    * WebViewClient.onPageFinished 页面加载完成时回调，正在加载的网页产生跳转，该方法会调用无数次，
    -->用这个WebChromeClient.OnProgressChanged
    * 后台耗电，WebView单独开线程，没销毁会增加耗电
    * webView硬件加速导致的页面渲染问题  产生页面白块或闪烁
*   [关于WebView的内存泄露问题](https://maxiaobu1999.github.io/html5/heima/README.html)
    * WebView单独线程，持有activity引用，类似匿名内部类
    * 独立进程，会涉及进程间通信
    * 解决办法
        * ViewGroup中放WebView，动态add到Activity
        * 持有activity弱引用
        * activity停止时remove掉，再destroy掉WebView
 
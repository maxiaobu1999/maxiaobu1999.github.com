##Android 理论总结
*   [service与thread区别](http://60.205.190.18/zerg/public/Android/Activity.html)
    * service
        * 在本或其他进程执行操作，没有界面，保持app运行
        * 可以与其他组件进行交互，接收广播
    * Thread
        * 程序执行的最小单元，cpu调度的基本单位
        * thread运行独立于activity，没有thread引用时无法操作
        
*   [dvm的进程、Linux进程，应用的进程是否位同一概念](http://60.205.190.18/zerg/public/Android/Activity.html)
    * Linux中zygote进程fork一个出新的Dalvik虚拟机，虚拟机上面运行应用
*   [Android编译流程](http://60.205.190.18/zerg/public/Android/Activity.html)
    * Java编译器对工程本身的java代码进行编译，这些java代码有三个来源：app的源代码，由资源文件生成的R文件(aapt工具)，以及有aidl文件生成的java接口文件(aidl工具)。产出为.class文件。
    * .class文件和依赖的三方库文件通过dex工具生成Delvik虚拟机可执行的.dex文件，可能有一个或多个，包含了所有的class信息，包括项目自身的class和依赖的class。产出为.dex文件。
    * apkbuilder工具将.dex文件和编译后的资源文件生成未经签名对齐的apk文件。这里编译后的资源文件包括两部分，一是由aapt编译产生的编译后的资源文件，二是依赖的三方库里的资源文件。产出为未经签名的.apk文件。
    * 分别由Jarsigner和zipalign对apk文件进行签名和对齐，生成最终的apk文件。
    * 总结为：编译-->DEX-->打包-->签名和对齐
    ![编译流程](http://upload-images.jianshu.io/upload_images/2839011-28f3fb0ca3af7d9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
*   [AIDL](http://60.205.190.18/zerg/public/Android/Activity.html)
    *   使用binder进行跨进程通信实现起来比较费劲，所以就搞了AIDL这种语言由系统去生成相关java文件。
        在生成的文件里呢用int型code给我们定义的方法设置ID，通过这个id服务端可分出客户端调用的是哪个方法。
        又声明了一个叫stub的内部类，也就是service 中我们实现并作为IBind返回那个对象，这个Stub就继承了Binder。
        在客户端中通过asInterface()可以冲stub获到到一个代理对象，Binder会通过这个代理找到对应的server中的对应方法，将参数传给server再把结果返回给客户端
        额外需要注意的地方就是server中维护了一个binder的线程池，所以即使server的方法是耗时的也不需要用异步的方式去实现。但是当客户端发起请求的时候当前的线程会被挂起，知道服务端把结果返回来，所以不能在ui线程中发起调用
    * 接口定义语言，利用它定义Clint与service都认可的接口，将对象分解成操作系统可以识别的对象在进程间传递
    * 原理
        * Stub存根 继承Binder实现我编写方法的接口  5 把server实现的方法执行再把结果返回去
        * asInterface(Binder) 1获取远程服务的代理Proxy
        * onTransact()  4接收传过来的Proxy 拿到code方法编号
        * transact()  3把proxy通过系统底层发到Stub去
        * Proxy代理 
        * Funcation()  2调用service方法
    ![aidl原理](https://raw.githubusercontent.com/MaoLyx/Pager/master/android/image/AIDL%E5%8E%9F%E7%90%86%E5%9B%BE.png)
    * 创建 .aidl 文件,gen目录下生成.java文件
    
            intentface IAidl{
                //计算两个数字的和并返回
                int add（int num1,int num2
                }
    * 编写服务端 实现.Stub抽象类,并重写 onBind() 以返回 Stub 类的实现
            
            public class RemoteService extends Service {
            
                @Override
                public void onCreate() {
                    super.onCreate();
                }
            
                @Override
                public IBinder onBind(Intent intent) {
                    // Return the interface
                    return mBinder;
                }
            
                private final IRemoteService.Stub mBinder = new IRemoteService.Stub() {
                    public int add(int num1,int num2){
                        return num1 + num2;
                    }
                };
            }
    
    * 编写Clint端 创建 .aidl 文件,gen目录下生成.java文件，实现ServiceConnection
    *客户端只是使用该aidl接口，不需要实现它的Stub类，获取服务端得aidl对象后mService = AIDLService.Stub.asInterface(service);，就可以在客户端使用它了，对mService对象方法的调用不是在客户端执行，而是在服务端执行。
            
            AIDLService mService;  
            private ServiceConnection mConnection = new ServiceConnection() {  
                public void onServiceConnected(ComponentName className, IBinder service) {  
                    Log("connect service");  
                    mService = AIDLService.Stub.asInterface(service);  
                    try {  
                        mService.registerTestCall(mCallback);  
                    } catch (RemoteException e) {  
              
                    }  
                }  
              
              
                public void onServiceDisconnected(ComponentName className) {  
                    Log("disconnect service");  
                    mService = null;  
                }  
            };  

*   [fragment交互](http://60.205.190.18/zerg/public/Android/Activity.html)
    * fragment A掉B getActivity() 获取aty对象 ，aty持有B对象，用aty作为中间层实现
    * 把需要调用的方法抽象成接口
    * 广播 EventBus
*   [JAVA范型](http://60.205.190.18/zerg/public/Android/Activity.html)
    * 泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。
    * 使用通配符为了传入的类型有一个指定的范围，三种通配符形式：
        *  无限制通配符 < ?>
        * extends 关键字声明了类型的上界，表示参数化的类型可能是所指定的类型，或者是此类型的子类
        * super 关键字声明了类型的下界，表示参数化的类型可能是指定的类型，或者是此类型的父类
    * 类型擦除 当编译器对带有泛型的java代码进行编译时，它会去执行类型检查和类型推断，然后生成普通的不带泛型的字节码，这种普通的字节码可以被一般的 Java 虚拟机接收并执行

*   [JSBridge](http://60.205.190.18/zerg/public/Android/Activity.html)
    * 为什么要用JSBridge
        * Android4.2以下,addJavascriptInterface方式有安全漏掉
        * iOS7以下,JS无法调用Native
    * 如何自定义JSBridge
        * js 调用 native
            * addJavascriptInterface
            * WebViewClient 的shouldOverrideUrlLoading
            * WebChromeClient 的弹出框onJsPrompt
        * native调js
            * webView.loadUrl("javascript:insertHTML('" + html + "')");
        * jsBridge 回调
            * 回调 回调要触发的方法以接口的形式用集合保存，
                shouldOverrideUrlLoading触发native方法遍历集合调用回调方法

*   [LeakCanary原理](http://gudong.name/2017/05/15/leakcanary-theory.html)
    * 只要在 activity onDestory 时，把 Activity 对象绑定在 WeakReference 中，
    然后手动执行一次 GC，然后观察 ReferenceQueue 中是不是包含对应的 Activity 对象，
    如果不包含，说明 Activity 被强引用，也就是发生了内存泄漏。
*   [NDK](http://www.imooc.com/video/8021)
    ![流程](https://www.google.com/search?q=%E8%A1%A8%E6%83%85%E5%8C%85&tbm=isch&tbs=simg:CAQSlQEJ7sMNzRQ97FAaiQELEKjU2AQaAggKDAsQsIynCBpiCmAIAxIoyh3kE7YItwifCMcdyB2gCOETnQjJP-M94T2xNMc29T_1wM4U9jTe_1PRowrseXN6h-vi7R5fIAbElJRs0uiPZakO8qRaEPk1yWEjRh7sS7peemSW5ZJaWCAGx9IAQMCxCOrv4IGgoKCAgBEgQ5kaR4DA&sa=X&ved=0ahUKEwjp4I-clYjWAhWEabwKHSO3A6cQwg4IJCgA)
    * 编写java类代码，声明native方法-》编译成.class字节码文件-》
    javah生成头文件——》编写jni实现代码库文件——》编写mk文件-》
   编译成链接库文件 ndk。build生成so——》system.loadLibrary（）加载so文件
    * ndk是什么
        * 一组工具集，允许你在android中使用c/C++,管理native-activity，
        访问设备硬件，说白了基于jni用于编译生成so文件的工具
    * 交叉编译
        * 在一个平台上生成另一个平台上可执行的代码
    * jni
        * Java Native Interface标准是java的一部分，用于java其他语言写的
        代码进行交互
    * 链接库
        * 静态链接库 所有依赖文件打包成一个文件  .a结尾
        * 动态链接库 依赖文件不打包            SO文件
    * make文件 配置文件

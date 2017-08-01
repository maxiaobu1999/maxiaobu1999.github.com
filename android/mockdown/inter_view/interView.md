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

##插件化开发总结
*   [动态加载apk](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 2014 任玉刚 dynamic-load-apk
    * 加载code
        * DexClassLoader可以加载jar/apk/dex，可以从SD卡中加载未安装的apk
        * PathClassLoader只能加载系统中已经安装过的apk
        * 通过binder传递参数
    * 加载资源
        * 反射出AssetManager，加载资源
    * 插件中activity继承ProxyActivity，因为没有context同步proxyActivity生命周期
*   [热更新](https://maxiaobu1999.github.io/html5/heima/README.html)
    * dexElements数组保存dex，classLoader遍历数组寻找类，生成新dex放前面，后面同名类不会被用到
 
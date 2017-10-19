##性能优化总结
*   [图片优化](https://developer.android.com/topic/performance/graphics/index.html)
    * [高效加载大图片](https://developer.android.com/topic/performance/graphics/load-bitmap.html)
        * 通过BitmapFactory.Options，读取位图尺寸类型信息
        * 加载缩小版的位图进内存
    * [图片缓存](https://developer.android.com/topic/performance/graphics/cache-bitmap.html)
        * [lru缓存淘汰算法](http://www.cnblogs.com/-OYK/archive/2012/12/05/2803317.html)
            * 最近最少使用 维护一个缓存对象列表，其中对象列表的排列方式是按照访问顺序实现的，即一直没访问的对象，将放在队尾，即将被淘汰。而最近访问的对象将放在队头，最后被淘汰。
            * 新数据放在表头
            * 每当缓存命中(即缓存数据被访问)，数据移到表头
            * 链表满了，将尾部数据删除
        * 内存
            * 通过LruCache类进行处理
            * 不要通过软引用&弱引用进行处理,因为现在内存回收都是[并发](https://developer.android.com/topic/performance/graphics/manage-memory.html)
        * 磁盘
            * 通过DiskLruCache类进行处理
            * 都是用LinkHashMap保存数据，原理与LruCache基本相同
        * [处理配置变换](https://developer.android.com/topic/performance/graphics/cache-bitmap.html)
            * 缓存放fragment里面，配置变化后从fragment里找cache类
    * [图片内存管理](https://developer.android.com/topic/performance/graphics/manage-memory.html)
        * [3.0以下：](http://www.jianshu.com/p/eadb0ef271b0)
            * 像素信息(pixel data)存在native堆中，bitmap对象存在Dalvik，bitmap对象可以
            被垃圾回收机制回收，但是像素信息不会被同步回收，所以使用后必须调用recycle()
        * 3.0以上：
            * 像素信息(pixel data)与bitmap对象都存在Dalvik堆中，所以能够同步回收
            * 通过inBitmap字段复用bitmap的内存空间
                * 用完的bitmap用软引用保存起来
                * 把保存的bitmap赋值给inBitmap
                * 注：在Android 4.4（API level 19）之前，只有新旧两个bitmap的尺寸一样才能复用内存空间。Android 4.4开始只要旧bitmap的尺寸大于等于新的bitmap就可以复用了。
    * 图片压缩
       * 上传图片
          *  [luban](https://github.com/Curzibn/Luban)

       * 本地资源( [PNG优化](http://www.jianshu.com/p/cc17d18c3447) )
                    * appt工具会对png图片进行优化处理
                    * 但是[TinyPng等](http://www.jianshu.com/p/e72054351c58)工具可以更好的优化png
                    * 多次处理可能会导致png更大，所以需要[关闭appt对png的优化](http://www.jianshu.com/p/cc17d18c3447)

            * 无损压缩&[有损压缩]()

           |  | 格式 | 表现 | 加载后占用的内存 |
           | ------| ------ | ------ | ------|
           | 无损压缩 | png | 保留所有细节 |       一样|
           | 有损压缩 | jpg&jpeg | 删除了部分细节 |一样|

           * png&jpg
                * jpg全名jpeg
                * jpg缺点：不适用保存颜色单一的图片，解压耗时等等
                * jpg优点：相对png小点但有限，对图片大小有高要求可采用webp，依然不用jpg

     * 其他的图片格式
        * svg
        * webp
        * Web—jpg
    * [四大图片加载哪家强](http://www.jianshu.com/p/ada9b90fa9e6)
        * ImageLoader 不维护了弃
        * picasso
            * 小
            * 依赖okHttp，如果不用OkHttp处理网络请求弃
            * 缓存图片的原始尺寸
        * glide
            * 默认使用HttpURLConnection下载图片，最好配置成okHttp
            * 图片磁盘缓存多种尺寸（包括原始尺寸），
            * 默认的 Bitmap 格式是 RGB_565
            * 对listView进行了优化，不需要处理图片错位
        * Fresco
            * 使用Native堆存放图片，不占用app内存
            * 可实现渐进式图片加载
            *
            * 太大，用了大2m
    * [Glide源码]
        * [生命周期管理](http://www.jianshu.com/p/317b2d6bde1b)
            * 根据传入的context，创建无界面fragment，(Fragment用ChildFragmentManager)
            * 通过lifecycle接口监听创建的fragment生命周期，实现生命周期绑定
        * [线程池](https://www.zhihu.com/question/37804956)
            * 下载线程池(FifoPriorityThreadPoolExecutor)
                * 线程数固定，大小为jvm可用cup
                * 线程优先级是THREAD_PRIORITY_BACKGROUND
            * 磁盘线程池 类一样  数量1
        * [缓存机制](http://www.10tiao.com/html/227/201705/2650239697/1.html)
            * 内存
                * 继承LruCache 缓存命中从缓存中移除
                    * 加个方法trimMemory(int level)  根据内存情况(后台)调整缓存大小
                * 正在使用的bitmap用弱引用保存
            * 磁盘
                * 默认保存路径CacheDir
            * 同样基于LRU淘汰算法
    * 计算bitmap内存大小
        * 宽(px)* 高(px)*每像素大小(ARGB888四字节，RGB565两字节)*缩放比(mdpi)
        * 缩放比
            * mdpi下50x50px图片  在hdpi会被拉伸为75x75px
        * [位图配置](https://developer.android.com/reference/android/graphics/Bitmap.Config.html)
            * ARGB888  4字节  有alpha通道
            * RGB565   2字节  莫有alpha通道
            * 其他配置参数用不上
            * [alpha通道：取值0～1，0透明，1不透明，外在表现为透明度](http://www.cnblogs.com/suogasus/p/5311264.html)

    * 滑动优化
        * onScrollListener 暂停图片加载

*   [渲染优化](https://developer.android.com/topic/performance/rendering/index.html)
    * 过度绘制
        * 一个像素点上绘制多次
        * 开发者选项：gpu过度绘制
            * 无/白色：绘制1次
            * 蓝色：绘制2次（理想状态）
            * 绿色：绘制3次
            * 浅红：绘制4次（要优化了）
            * 深红：绘制5次或5次以上。（必须要优化了）
        * 解决
            * 去掉多余背景
            * 减少ui层级
            * [少用透明度（透明度会多绘制一次）离屏缓存后渲染](http://android.jobbole.com/81944/)
    * [cpu呈现模式分析（迷之柱状图）](https://developer.android.com/topic/performance/rendering/profile-gpu.html)
        * 触摸事件Input Handling
        * 动画Animation
        * 测量布局Measurement/Layout
        * 绘制Drawing
        * cpu传递给gpu的时间Sync/Upload
    * [两种绘制模型](https://www.zhihu.com/question/25811504/answer/142136864)
        * 软件绘制
            * cpu主导，把要绘制的内容写进bitmap，然后把这个位图像素内容渲染法哦屏幕上
        * 硬件加速
            * gup主导，把canvas的绘制方法转成绘制指令，存在DisplayList里交给GPU，
            GPU通过OpenGl完成绘制
    * [视图分层（离屏缓存）](http://android.jobbole.com/81944/)
        * 单独启用一块地方绘制view，并把绘制结果保存下来，只要绘制内容没有改变，重回的时候
        就可以复用之前的绘制结果，不需要从新计算
        * 平移缩放旋转渐变不需要调用invalidate()，所以离屏缓存+硬件加速可以提高动画流畅度
        * View 级别的硬件加速开关
*   [启动优化](http://www.voidcn.com/article/p-txcdhyvg-tk.html)
*   [APK优化](https://developer.android.com/topic/performance/reduce-apk-size.html#multiple-apks)
    * 去除不必要的依赖库+混淆
    * png压缩 使用svg 使用drawable编辑图片，动态下载
    * zip对齐
*   [反射](http://blog.csdn.net/javazejian/article/details/70768369#constructor类及其用法)
    * 类在编译后生成对象之前，jvm会生成Class类型的Class对象，这个Class对象保存了
    这个类的类型信息（构造，字段信息，方法），jvm根据Class对象生成类的对象。通过
    这个class对象我也可以创建对象，或者根据对象找到对应的class对象，这个过程
    就是反射
    * 得到Class对象的方法
        * xxx.getClass();
        * Class.forName("XXX")
        * xxx.class
*   [泛型](http://blog.csdn.net/javazejian/article/details/70768369#constructor类及其用法)
    * 把操作数据类型指定为一个参数
    * [通配符 ？ 通配泛型 表示任何类型 ](http://www.jianshu.com/p/95f349258afb)
    * [T,KV,T1T2  表示具体类型](http://www.jianshu.com/p/95f349258afb)
    * [上界 《T super String》 下界《T extend String》](http://www.infoq.com/cn/articles/cf-java-generics)
    * [泛型类、泛型接口、泛型方法](https://segmentfault.com/a/1190000002646193)[Class《T》](http://www.infoq.com/cn/articles/cf-java-generics)
    * [类型擦除](http://www.infoq.com/cn/articles/cf-java-generics)
* [注解](http://josh-persistence.iteye.com/blog/2226493)
    * 以类似注释的形式，将数据与程序元素（类、方法、成员变量等）关联起来
    * 元注解  负责注解其他注解的注解
    *
![binder机制图](http://frodoking.github.io/img/android/okhttp_request_process.png)
##性能优化总结
*   [anr](https://maxiaobu1999.github.io/html5/heima/README.html)
    * ActivityManager和windowManager系统服务监视 aty 5秒 broadcast10秒 service15秒 系统显示应用未响应对话框
    * 主线程进行io操作  主线程存在耗时操作
    * 解决
        * 使用Thread或HandleThread时设定优先级
        * 使用Asynctask处理耗时操作  
        * 使用handler处理工作线程耗时操作
        * 在生命周期回调方法中避免好使代码
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
        * 内部使用LinkHashMap实现
        * trimToSize()把用的最久的缓存对象清除，并添加新的缓存对象
        * safeSizeOf()计算大小并从缓存中去掉
        * put()  把新对象加进去
    * 计算inSampleSize 
        * BitmapFactory.Options()算出缩放比例
    * 三级缓存    
        * 网络 本地 内存 
*   [UI卡顿](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 60fps->16ms
        * 一秒60帧 每16ms更新一次页面
    * overdraw
        * 同一帧绘制多次
![binder机制图](http://frodoking.github.io/img/android/okhttp_request_process.png)
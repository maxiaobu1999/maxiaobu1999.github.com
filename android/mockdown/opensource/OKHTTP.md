##OkHttp总结
*   [OkHttp使用](https://maxiaobu1999.github.io/html5/heima/README.html)

        * OkHttpClient mOkHttpClient= new OkHttpClient();
        * Request.Builder request = new Request.Builder();
        * Response response = client.newCall(request).execute();

*   [OkHttp源码分析](https://maxiaobu1999.github.io/html5/heima/README.html)
    * request对象交给okhttp处理 拦截器链分层处理网络请求 
    得到response分发给ui线程
    
    

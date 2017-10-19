##OkHttp总结
*   [OkHttp使用](https://maxiaobu1999.github.io/html5/heima/README.html)

        * OkHttpClient mOkHttpClient= new OkHttpClient();
        * Request.Builder request = new Request.Builder();
        * Response response = client.newCall(request).execute();

*   [OkHttp源码分析](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 责任链模式：多个拦截器组成上家对下家形式的链条，一个被处理的对象在链条中传递。每一个拦截器可以选择直接处理，或者直接交给下家处理，也可以生
        成新的对象交给下家，有点流水线的意思
    * request对象交给okhttp处理 拦截器链分层处理网络请求 
    得到response分发给ui线程
    
    

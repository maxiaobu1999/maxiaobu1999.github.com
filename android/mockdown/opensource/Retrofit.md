##Retrofit总结
*   [Retrofit使用](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 在Retrofit中通过一个接口作为http请求的api接口

            @POST("mme.do")
            Observable<BeanMe> getBeanMe(@Query("memid") String memid);
    * 创建一个Retrofit实例

            Retrofit retrofit = new Retrofit.Builder()
                  .client(builder.build())
                  .baseUrl(baseUrl)
                  .addConverterFactory(GsonConverterFactory.create())
                  .addCallAdapterFactory(RxJavaCallAdapterFactory.create())
                  .build()
                  .create(RetrofitService.class);    
    * 调用api

             Observable<BeanMe> beanMineObservable = App.getRetrofitUtil().getRetrofit()
                            .getBeanMe(请求参数);
    
*   [Retrofit源码分析](https://maxiaobu1999.github.io/html5/heima/README.html)
    * Retrofit是Http网络请求处理的上层封装，即生成Http Request，丢给下面的Http处理引擎OkHttp，收到回复的数据后回调到上层
    * Request：通过注解方式定义接口，声明了请求所需的各类参数，采用动态代理的方式根据这个接口实例化一个对象，生成httpCall以适配器方式交给okHttp
    
# 使用 OpenFeign 组件的应用在 ARMS 中数据不完整怎么办？ {#concept_112094_zh .concept}

若您的 OpenFeign 应用接入 ARMS 应用监控后，出现数据不完整、看不到下游应用的数据等情况，可能原因是：

-   OpenFeign 组件默认开启了Hystrix，Hystrix 使用了 RxJava 异步框架。而 ARMS 不支持异步框架。
-   OpenFeign 组件默认使用 JDK 的 HttpURLConnection 请求类。因 ARMS 插桩机制限制，该类经常因加载时间过早等原因导致拦截失败，应使用 OkHttp 请求类。

您可以通过关闭 Hystrix 并配置 OkHttp 请求类来解决此类问题。操作步骤如下：

1.  在 pom 文件中添加以下依赖。

    ```
    <!-- OKHttp 对 Feign 支持 -->
    <dependency>
        <groupId>io.github.openfeign</groupId>
        <artifactId>feign-okhttp</artifactId>
    </dependency>
    
    ```

2.  在 SpringCloud 配置文件中添加以下配置。

    ```
    feign.okhttp.enabled: true
    feign.hystrix.enabled: false 
    
    ```

3.  配置 OkHttp。

    ```
    @Configuration
    @ConditionalOnClass(Feign.class)
    @AutoConfigureBefore(FeignAutoConfiguration.class)
    public class FeignClientOkHttpConfiguration {
    
        @Bean
        public OkHttpClient okHttpClient() {
            return new OkHttpClient.Builder()
                    // 连接超时
                    .connectTimeout(20, TimeUnit.SECONDS)
                    // 响应超时
                    .readTimeout(20, TimeUnit.SECONDS)
                    // 写超时
                    .writeTimeout(20, TimeUnit.SECONDS)
                    // 是否自动重连
                    .retryOnConnectionFailure(true)
                    // 连接池
                    .connectionPool(new ConnectionPool())
                    .build();
        }
    
    ```



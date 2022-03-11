# Retrofit 싱글턴 만들기
```kotlin
object RetrofitBuilder {
    private val clientBuilder = OkHttpClient.Builder()
    private val loggingInterceptor = HttpLoggingInterceptor()

    private const val url: String = URL // 보안작업은 알아서. property 파일로 만들고 빼서 써도됨
    var api: API

    init {
        loggingInterceptor.level = HttpLoggingInterceptor.Level.BODY
        clientBuilder.addInterceptor(loggingInterceptor)

        val retrofit = Retrofit.Builder()
            .baseUrl(url)
            .addConverterFactory(GsonConverterFactory.create())
            .client(clientBuilder.build())
            .build()
        api = retrofit.create(API::class.java)
    }
}
```

# API Call 할때 Authorization 추가하기
```kotlin
    @Headers("Content-Type: application/json; charset=utf-8")
    @PUT("api/test")
    fun testCall(
        @Body body: Body,
        @Header("Authorization") auth: String
    ): Call<TestDTO>
```
- auth : "Bearer $token" 형식으로 넣어줘야함. Bearer랑 token 사이 띄워쓰기 필수

Retrofit 객체 만들때 Intercepter 설정 해줘도 되긴 한데 그냥 api call 할일이 많이 없으면 할때마다 token 넣어주는것도 괜찮은듯


# 혹시몰라 추가하는 Intercepter에서 토큰 설정하기
```kotlin
object RetrofitBuilder {
    private val clientBuilder = OkHttpClient.Builder()
    private val loggingInterceptor = HttpLoggingInterceptor()

    private const val url: String = URL
    var api: API
    var token: String = ""

    init {
        loggingInterceptor.level = HttpLoggingInterceptor.Level.BODY
        clientBuilder.addInterceptor(loggingInterceptor)
        clientBuilder.addInterceptor(AddInterceptor())

        val retrofit = Retrofit.Builder()
            .baseUrl(url)
            .addConverterFactory(GsonConverterFactory.create())
            .client(provideOkHttpClient(AddInterceptor()))
            .build()
        api = retrofit.create(API::class.java)
    }

    private fun provideOkHttpClient(interceptor: AddInterceptor): OkHttpClient
    = OkHttpClient.Builder().run {
        addInterceptor(interceptor)
        build()
    }

    class AddInterceptor: Interceptor {
        override fun intercept(chain: Interceptor.Chain): Response = with(chain) {
            val newRequest = request().newBuilder()
                .addHeader("token", TOKEN)
                .build()
            proceed(newRequest)
        }
    }
}
```
위에꺼랑 적당히 섞으면 loggingInterceptor랑 같이 붙일수도 있음..

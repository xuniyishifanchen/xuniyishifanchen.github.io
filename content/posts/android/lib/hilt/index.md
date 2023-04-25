---
title: "Android lib - Hilt"
date: 2023-04-17T01:06:41+08:00
draft: false

tags: 
  - android
  - lib

categories:
  - android lib
---

## 1 About [Hilt](https://developer.android.google.cn/training/dependency-injection/hilt-android)
>基于dagger2由google针对android平台开发的一个依赖注入库。

## 2 基本使用方法
### 2.1 在项目根级gradle添加class 插件
```groovy
plugins {
  ...
  id 'com.google.dagger.hilt.android' version '2.44' apply false
}
```
### 2.2 在module 中添加依赖
```groovy
...
plugins {
  id 'kotlin-kapt'
  id 'com.google.dagger.hilt.android'
}

dependencies {
  implementation "com.google.dagger:hilt-android:2.44"
  kapt "com.google.dagger:hilt-compiler:2.44"
}

// Allow references to generated code
kapt {
  correctErrorTypes true
}
```

### 2.3 创建Application并添加`@HiltAndroidApp`注解,并在`AndroidManifest.xml`注册
```kotlin
@HiltAndroidApp
class HiltApp : Application()
```

### 2.4 在依赖的android组件上添加`@AndroidEntryPoint`注解
```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity() {

}
```

### 2.5 使用`@Provider`注入实例
```kotlin
@InstallIn(SingletonComponent::class)
@Module
class ApplicationModule {

    @Provides
    fun provideBaseUrl() = BuildConfig.BASE_URL

    @Provides
    @Singleton
    fun provideOkHttpClient() = if (BuildConfig.DEBUG) {
        val loggingInterceptor = HttpLoggingInterceptor()
        loggingInterceptor.setLevel(HttpLoggingInterceptor.Level.BODY)
        OkHttpClient.Builder()
            .addInterceptor(loggingInterceptor)
            .build()
    } else OkHttpClient
        .Builder()
        .build()

    @Provides
    @Singleton
    fun provideRetrofit(
        okHttpClient: OkHttpClient,
        BASE_URL: String
    ): Retrofit =
        Retrofit.Builder()
            .addConverterFactory(MoshiConverterFactory.create())
            .baseUrl(BASE_URL)
            .client(okHttpClient)
            .build()

    @Provides
    @Singleton
    fun provideApiService(retrofit: Retrofit): ApiService = retrofit.create(ApiService::class.java)

    @Provides
    @Singleton
    fun provideApiHelper(apiHelper: ApiHelperImpl): ApiHelper = apiHelper
}
```

### 2.6 使用`@bind`注入接口实例
```kotlin
interface AnalyticsService {
  fun analyticsMethods()
}

// Constructor-injected, because Hilt needs to know how to
// provide instances of AnalyticsServiceImpl, too.
class AnalyticsServiceImpl @Inject constructor(
  ...
) : AnalyticsService { ... }

@Module
@InstallIn(ActivityComponent::class)
abstract class AnalyticsModule {

  @Binds
  abstract fun bindAnalyticsService(
    analyticsServiceImpl: AnalyticsServiceImpl
  ): AnalyticsService
}
```

### 2.7 同一类型的不同实现
```kotlin
@Qualifier
@Retention(AnnotationRetention.RUNTIME)
annotation class LocalData

@Qualifier
@Retention(AnnotationRetention.RUNTIME)
annotation class RemoteData

@InstallIn(SingletonComponent::class)
@Module
object DataModule{

    @LocalData
    @Provides
    fun provideLocalData() = "LocalData"

    @RemoteData
    @Provides
    fun provideRemoteData() = "RemoteData"
}
```

### 2.8 构造函数注入
```kotlin
class User @Inject constructor()
```

### 2.9 使用`@Inject`注入User对象
```kotlin
    @Inject
    lateinit var user: User

    @Inject
    lateinit var user2: User
```

## 3 和`viewModel` 搭配使用
>提供 ViewModel，方法是为其添加 @HiltViewModel 注解，并在 ViewModel 对象的构造函数中使用 @Inject 注解
```kotlin
@HiltViewModel
class ExampleViewModel @Inject constructor(
  private val savedStateHandle: SavedStateHandle,
  private val repository: ExampleRepository
) : ViewModel() {
  ...
}
```

```kotlin
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() {
  private val exampleViewModel: ExampleViewModel by viewModels()
  ...
}
```

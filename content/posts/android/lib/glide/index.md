---
title: "Android lib - Glide"
date: 2023-04-17T00:35:53+08:00
draft: false

featuredImage: glide_logo.png

tags: 
  - android
  - lib

categories:
  - android lib
---
## 1 About [Glide](https://muyangmin.github.io/glide-docs-cn)
>一个图片异步加载库Glide是一个快速高效的Android图片加载库，注重于平滑的滚动。Glide提供了易用的API，高性能、可扩展的图片解码管道（decode pipeline），以及自动的资源池技术。
[Github](https://github.com/bumptech/glide)

## 2 使用方法
### 添加依赖
```groovy
repositories {
  google()
  mavenCentral()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.15.1'
}
```
### 添加使用权限
* INTERNET: 用于加载网络图片
* ACCESS_NETWORK_STATE：可以监听用户的连接状态并在用户重新连接到网络时重启之前失败的请求
* READ_EXTERNAL_STORAGE：读取本地图片
* WRITE_EXTERNAL_STORAGE：将缓存保存到外部存储器
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="your.package.name">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
</manifest>
```

### 基本用法
#### 加载
```java
Glide.with(fragment)
    .load(myUrl)
    .into(imageView);
```

#### 取消加载
```java
Glide.with(fragment).clear(imageView);
```

#### 定制需求
>Glide 提供了许多可应用于单一请求的选项，包括变换、过渡、缓存选项等.
>默认选项可以直接应用于请求上：
```java
Glide.with(fragment)
  .load(myUrl)
  .placeholder(placeholder)
  .fitCenter()
  .into(imageView);
```
>选项也可以通过 RequestOptions 类来在多个请求之间共享：

```java
RequestOptions sharedOptions = 
    new RequestOptions()
      .placeholder(placeholder)
      .fitCenter();

Glide.with(fragment)
  .load(myUrl)
  .apply(sharedOptions)
  .into(imageView1);

Glide.with(fragment)
  .load(myUrl)
  .apply(sharedOptions)
  .into(imageView2);
```

#### 在 ListView 和 RecyclerView 中的使用
```java
@Override
public void onBindViewHolder(ViewHolder holder, int position) {
    String url = urls.get(position);
    Glide.with(fragment)
        .load(url)
        .into(holder.imageView);
}
```

#### 非 View 目标
```java
Glide.with(context
  .load(url)
  .into(new CustomTarget<Drawable>() {
    @Override
    public void onResourceReady(Drawable resource, Transition<Drawable> transition) {
      // Do something with the Drawable here.
    }

    @Override
    public void onLoadCleared(@Nullable Drawable placeholder) {
      // Remove the Drawable provided in onResourceReady from any Views and ensure 
      // no references to it remain.
    }
  });
```

#### 后台线程
```
utureTarget<Bitmap> futureTarget =
  Glide.with(context)
    .asBitmap()
    .load(url)
    .submit(width, height);

Bitmap bitmap = futureTarget.get();

// Do something with the Bitmap and then when you're done with it:
Glide.with(context).clear(futureTarget);
```
## 3. 混淆规则
```
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public class * extends com.bumptech.glide.module.AppGlideModule
-keep public enum com.bumptech.glide.load.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}
```

{{< admonition >}}
如果你的 target API 低于 Android API 27，请添加：
```pro
-dontwarn com.bumptech.glide.load.resource.bitmap.VideoDecoder
```
如果你使用 DexGuard 你可能还需要添加：
```
# for DexGuard only
-keepresourcexmlelements manifest/application/meta-data@value=GlideModule
```
{{< /admonition >}}
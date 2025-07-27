# 轻松上手Shizuku API

## 下载

- [网盘（访问密码：5372）](https://url93.ctfile.com/f/48492093-1517941276-a7d73c?p=5372)

- [Github](https://github.com/RikkaApps/Shizuku/releases)

## 启动 Shizuku

无线调试 和 root 设备的启动方式见官方文档。

- adb 命令

> PS: 如果无法使用 adb 命令，自行 Google 将 adb 工具添加至环境变量。
> Linux系统 一般在当前用户目录下的 `\Android\Sdk\platform-tools`。
> Win系统 一般在当前用户目录下的 `\AppData\Local\Android\Sdk\platform-tools`

![adb_start_shizuku.png](adb_start_shizuku.png)

![start_success.png](start_success.png)

- [无线调试](https://shizuku.rikka.app/zh-hans/guide/setup/)

- [Root 设备](https://shizuku.rikka.app/zh-hans/guide/setup/)

## 项目引入 Shizuku 依赖 {id="shizuku_1"}

```kotlin
// 必须
implementation("dev.rikka.shizuku:api:13.1.5")

// 如果是 sui 模式（针对 root 设备）非必须
implementation("dev.rikka.shizuku:provider:13.1.5")

// 必须，否则部分 api 调用时异常
implementation("org.lsposed.hiddenapibypass:hiddenapibypass:6.1")

// 部分隐藏 api 接口
compileOnly(project(":demo-hidden-api-stub"))
```

## 配置 Provider

在 `AndroidManifest.xml` 中的 `application` 标签内增加以下 `provider`

```xml
<provider
        android:name="rikka.shizuku.ShizukuProvider"
        android:authorities="${applicationId}.shizuku"
        android:multiprocess="false"
        android:enabled="true"
        android:exported="true"
        android:permission="android.permission.INTERACT_ACROSS_USERS_FULL"/>
```

[视频教程](https://www.bilibili.com/video/BV11mNzzHEXr/)
# project07-CryptoApp

## Preview

![project07_cover.jpg](project07_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project07_crypto_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1522341247-b6a3dd?p=5372)

- strings

```XML
<resources>
    <string name="str_splash_title">Crypto Just\nGot Easier</string>
    <string name="str_splash_label">Experience Seamless\trading and secure\nasset managment</string>
    <string name="str_get_started">Get Started</string>
    <string name="str_login">Login</string>
    <string name="str_welcome_user">Hello %s</string>
    <string name="str_main_profile">Main Profile</string>
    <string name="str_stock_futures">Stock Futures</string>
    <string name="str_most_popular">Most Popular</string>
    <string name="str_see_all">See all</string>
</resources>
```

- navigation

```kotlin
// projecct kts
kotlin("plugin.serialization") version "2.1.0"

// app kts
kotlin("plugin.serialization")
implementation("androidx.navigation:navigation-compose:2.8.9")
implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.8.0")
```

- koin

```kotlin
implementation("io.insert-koin:koin-android:4.1.0-Beta5")
implementation("io.insert-koin:koin-androidx-compose:4.1.0-Beta5")
implementation("io.insert-koin:koin-androidx-compose-navigation:4.1.0-Beta5")
```

- SparkLineLayout

```kotlin
// settings.gradle.kts
maven { url = uri("https://jitpack.io") }

// app kts
implementation("com.github.majorkik:SparkLineLayout:1.0.1")
```
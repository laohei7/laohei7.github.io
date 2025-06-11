# project06-MusicPlayerApp

## Preview

![project06_cover.jpg](project06_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project06_music_player_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1514873389-824a7c?p=5372)

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

- coil

```kotlin
implementation("io.coil-kt.coil3:coil-compose:3.1.0")
```

- media3

```kotlin
implementation("com.google.android.exoplayer:exoplayer:2.19.0")
```

- audiowaveform

```kotlin
// settings.gradle.kts
maven { url = uri("https://jitpack.io") }

//  app build.gradle.kts
implementation("com.github.lincollincol:compose-audiowaveform:1.1.1")
```

## Model

### Song

```kotlin
data class Song(
    val id: Long,
    val title: String,
    val artist: String,
    val data: String,
    val albumId: Long
)
```
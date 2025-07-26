# project09-QuizApp

## Preview

![project02_cover.jpg](project09_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project09-quiz_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1543886711-3fd2d1?p=5372)

- dependencies

```toml
[versions]
koinCompose = "4.1.0"
kotlin = "2.2.0"
kotlinx-serialization = "1.9.0"
androidx-navigation = "2.9.0-beta03"

[libraries]
koin-compose-viewmodel = { module = "io.insert-koin:koin-compose-viewmodel", version.ref = "koinCompose" }
koin-compose = { module = "io.insert-koin:koin-compose", version.ref = "koinCompose" }
koin-core = { module = "io.insert-koin:koin-core", version.ref = "koinCompose" }
koin-compose-viewmodel-navigation = { module = "io.insert-koin:koin-compose-viewmodel-navigation", version.ref = "koinCompose" }
androidx-navigation-compose = { module = "org.jetbrains.androidx.navigation:navigation-compose", version.ref = "androidx-navigation" }
kotlinx-serialization-json = { module = "org.jetbrains.kotlinx:kotlinx-serialization-json", version.ref = "kotlinx-serialization" }

[plugins]
kotlinx-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }

[bundles]
common-koin = [
    "koin-compose", "koin-compose-viewmodel", "koin-compose-viewmodel-navigation", "koin-core"
]
```

```kotlin
// project build.gradle.kts
plugins {
    alias(libs.plugins.kotlinx.serialization) apply false
}


// composeApp build.gradle.kts
plugins {
    alias(libs.plugins.kotlinx.serialization)
}

sourceSets {
    implementation(libs.androidx.navigation.compose)
    implementation(libs.kotlinx.serialization.json)
    implementation(libs.bundles.common.koin)
}
```

## Model

### QuestionModel

```kotlin
data class QuestionModel(
    val id:Int,
    val question:String?,
    val options:List<String?>,
    val correctAnswer: String?,
    val score:Int,
    val pic: DrawableResource,
    val selectAnswer: String?
)
```

### UserModel

```kotlin
data class UserModel(
    val id: Int,
    val name: String,
    val pic: DrawableResource,
    val score: Int
)
```
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

val questionList = listOf(
    QuestionModel(
        1,
        "Which planet is the largest planet in the solar system?",
        "Earth",
        "Mars",
        "Neptune",
        "Jupiter",
        "d",
        5,
        Res.drawable.q_1,
        null
    ),
    QuestionModel(
        2,
        "Which country is the largest country in the world by land area?",
        "Russia",
        "Canada",
        "United States",
        "China",
        "a",
        5,
        Res.drawable.q_2,
        null
    ),
    QuestionModel(
        3,
        "Which of the following substances is used as an anti-cancer medication?",
        "Cheese",
        "Lemon juice",
        "Cannabis",
        "Paspalum",
        "c",
        5,
        Res.drawable.q_3,
        null
    ),
    QuestionModel(
        4,
        "Which moon has an atmosphere?",
        "Luna",
        "Phobos",
        "Venus' moon",
        "None of the above",
        "d",
        5,
        Res.drawable.q_4,
        null
    ),
    QuestionModel(
        5,
        "Which symbol represents the element with atomic number 6?",
        "O",
        "H",
        "C",
        "N",
        "c",
        5,
        Res.drawable.q_5,
        null
    ),
    QuestionModel(
        6,
        "Who is credited with inventing theater as we know it?",
        "Shakespeare",
        "Arthur Miller",
        "Ashkouri",
        "Ancient Greeks",
        "d",
        5,
        Res.drawable.q_6,
        null
    ),
    QuestionModel(
        7,
        "Which ocean is the largest?",
        "Pacific",
        "Atlantic",
        "Indian",
        "Arctic",
        "a",
        5,
        Res.drawable.q_7,
        null
    ),
    QuestionModel(
        8,
        "Which religions are most practiced?",
        "Islam, Christianity, Judaism",
        "Buddhism, Hinduism, Sikhism",
        "Zoroastrianism, Brahmanism",
        "Taoism, Shintoism",
        "a",
        5,
        Res.drawable.q_8,
        null
    ),
    QuestionModel(
        9,
        "Which continent has the most independent countries?",
        "Asia",
        "Europe",
        "Africa",
        "Americas",
        "c",
        5,
        Res.drawable.q_9,
        null
    ),
    QuestionModel(
        10,
        "Which ocean has the greatest average depth?",
        "Pacific",
        "Atlantic",
        "Indian",
        "Southern",
        "d",
        5,
        Res.drawable.q_10,
        null
    )
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

val userList = listOf(
    UserModel(1,"User 1",Res.drawable.person1,300),
    UserModel(2,"User 2",Res.drawable.person2,200),
    UserModel(3,"User 3",Res.drawable.person2,100),
)
```
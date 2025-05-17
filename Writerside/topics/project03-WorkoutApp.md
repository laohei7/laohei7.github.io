# project03-WorkoutApp

## Preview

![](../images/project03_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1505611354-0a4ecc?p=5372)

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
// app kts
implementation("io.insert-koin:koin-android:4.1.0-Beta5")
implementation("io.insert-koin:koin-androidx-compose:4.1.0-Beta5")
implementation("io.insert-koin:koin-androidx-compose-navigation:4.1.0-Beta5")
```

## Model

### Lession

```kotlin
data class Lession(
    val title:String,
    val duration:String,
    val link:String,
    val pic:Int
)
```

### Workout

```Kotlin
data class Workout(
    val title:String,
    val description:String,
    val pic:Int,
    val kcal:Int,
    val durationAll:String,
    val lessions:List<Lession>
)
```

## Data

```Kotlin
object WorkoutDataProvider {

    fun getData(): List<Workout> = listOf(
        Workout("Running", "Desc...", "pic_1", 160, "9 min", getLessions1()),
        Workout("Stretching", "Desc...", "pic_2", 230, "85 min", getLessions2()),
        Workout("Yoga", "Desc...", "pic_3", 180, "65 min", getLessions3())
    )

    fun getLessions1() = listOf(
        Lession("Lesson 1", "03:46", "HBPMvFkpNgE", R.drawable.pic_1_1),
        Lession("Lesson 2", "03:41", "K6I24WgiiPw", R.drawable.pic_1_2),
        Lession("Lesson 3", "01:57", "Zc08v4YYOeA", R.drawable.pic_1_3),
        Lession("Lesson 3", "01:57", "Zc08v4YYOeA", R.drawable.pic_1_3),
        Lession("Lesson 3", "01:57", "Zc08v4YYOeA", R.drawable.pic_1_3),
        Lession("Lesson 3", "01:57", "Zc08v4YYOeA", R.drawable.pic_1_3),
        Lession("Lesson 3", "01:57", "Zc08v4YYOeA", R.drawable.pic_1_3),
        Lession("Lesson 3", "01:57", "Zc08v4YYOeA", R.drawable.pic_1_3)
    )
    fun getLessions2() = listOf(
        Lession("Lesson 1", "20:23", "L3eImBAXT7I", R.drawable.pic_3_1),
        Lession("Lesson 2", "18:27", "47ExgzO7FlU", R.drawable.pic_3_2),
        Lession("Lesson 3", "32:25", "OmLx8tmaQ-4", R.drawable.pic_3_3),
        Lession("Lesson 4", "07:52", "w86EalEoFRY", R.drawable.pic_3_4)
    )
    fun getLessions3() = listOf(
        Lession("Lesson 1", "23:00", "v7AYKMP6rOE", R.drawable.pic_3_1),
        Lession("Lesson 2", "27:00", "Eml2xnoLpYE", R.drawable.pic_3_2),
        Lession("Lesson 3", "25:00", "v7SN-d4qXx0", R.drawable.pic_3_3),
        Lession("Lesson 4", "21:00", "LqXZ628YNj4", R.drawable.pic_3_4)
    )

}
```
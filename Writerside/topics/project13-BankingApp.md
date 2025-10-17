# project13-BankingApp

## Preview

![project02_cover.jpg](project13_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project13-banking_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-8453442257-fed475?p=5372)


- dependencies

```toml
[versions]
koinCompose = "4.1.0"
androidx-navigation = "2.9.0-beta03"
constraintlayoutComposeMultiplatform = "0.6.1"
coilCompose = "3.3.0"
kotlinx-serialization = "1.9.0"

[libraries]
constraintlayout-compose-multiplatform = { module = "tech.annexflow.compose:constraintlayout-compose-multiplatform", version.ref = "constraintlayoutComposeMultiplatform" }
coil-compose = { module = "io.coil-kt.coil3:coil-compose", version.ref = "coilCompose" }
koin-compose-viewmodel = { module = "io.insert-koin:koin-compose-viewmodel", version.ref = "koinCompose" }
koin-compose = { module = "io.insert-koin:koin-compose", version.ref = "koinCompose" }
koin-core = { module = "io.insert-koin:koin-core", version.ref = "koinCompose" }
koin-compose-viewmodel-navigation = { module = "io.insert-koin:koin-compose-viewmodel-navigation", version.ref = "koinCompose" }
androidx-navigation-compose = { module = "org.jetbrains.androidx.navigation:navigation-compose", version.ref = "androidx-navigation" }
kotlinx-serialization-json = { module = "org.jetbrains.kotlinx:kotlinx-serialization-json", version.ref = "kotlinx-serialization" }

[plugins]
kotlinx-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }

[bundles]
koin = [
    "koin-compose",
    "koin-compose-viewmodel",
    "koin-compose-viewmodel-navigation",
    "koin-core"
]
```

```kotlin
// project/build.gradle.kts
plugins {
    // ...
    alias(libs.plugins.kotlinx.serialization) apply false
}

// composeApp/build.gradle.kts
plugins {
    // ...
    alias(libs.plugins.kotlinx.serialization)
}

sourceSets {
    androidMain.dependencies {
        // ...
    }
    commonMain.dependencies {
        // ...
        implementation(compose.materialIconsExtended)
        implementation(libs.coil.compose)
        implementation(libs.androidx.navigation.compose)
        implementation(libs.bundles.koin)
        implementation(libs.kotlinx.serialization.json)
        implementation(libs.constraintlayout.compose.multiplatform)
    }
    commonTest.dependencies {
        // ...
    }
    jvmMain.dependencies {
        // ...
    }
}
```

## Theme

- Color

```Kotlin
import androidx.compose.ui.graphics.Color

val PurplePrimary = Color(0xFF7B61FF)
val PurpleSecondary = Color(0xFFB388FF)
val OrangeAccent = Color(0xFFFF8A65)

val BackgroundLight = Color(0xFFF4F2FF)
val SurfaceLight = Color(0xFFFFFFFF)
val TextDark = Color(0xFF1E1E1E)

val ErrorLight = Color(0xFFFF6B6B)
val ErrorDark = Color(0xFFFF8A80)

val PrimaryDark = Color(0xFFB388FF)
val SecondaryDark = Color(0xFF7B61FF)
val BackgroundDark = Color(0xFF121212)
val SurfaceDark = Color(0xFF1E1E1E)
val TextLight = Color(0xFFF4F2FF)
```

- Shape

```Kotlin
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.Shapes
import androidx.compose.ui.unit.dp

val BankAppShapes = Shapes(
    small = RoundedCornerShape(8.dp),
    medium = RoundedCornerShape(16.dp),
    large = RoundedCornerShape(28.dp)
)
```

- Theme

```Kotlin
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Shapes
import androidx.compose.material3.darkColorScheme
import androidx.compose.material3.lightColorScheme
import androidx.compose.runtime.Composable
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.unit.dp

private val LightColorScheme = lightColorScheme(
    primary = PurplePrimary,
    onPrimary = Color.White,
    secondary = PurpleSecondary,
    onSecondary = Color.White,
    tertiary = OrangeAccent,
    background = BackgroundLight,
    surface = SurfaceLight,
    onBackground = TextDark,
    onSurface = TextDark,
    error = ErrorLight
)

private val DarkColorScheme = darkColorScheme(
    primary = PrimaryDark,
    onPrimary = Color.Black,
    secondary = SecondaryDark,
    onSecondary = Color.Black,
    tertiary = OrangeAccent,
    background = BackgroundDark,
    surface = SurfaceDark,
    onBackground = TextLight,
    onSurface = TextLight,
    error = ErrorDark
)

@Composable
fun BankAppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val colorScheme = if (darkTheme) DarkColorScheme else LightColorScheme

    MaterialTheme(
        colorScheme = colorScheme,
        shapes = Shapes(
            small = RoundedCornerShape(8.dp),
            medium = RoundedCornerShape(16.dp),
            large = RoundedCornerShape(28.dp)
        ),
        content = content
    )
}
```

## Model

- Profile

```Kotlin
data class Profile(
    val profileName: String,
    val profileImage: String,
    val totalBalance: String,
    val income: String,
    val banner: String,
    val outcome: String,
    val friend: List<Friend>,
    val transaction: List<Transaction>
)
```

- Friend

```Kotlin
data class Friend(
    val imageUrl: String,
    val name: String
)
```

- Transaction

```Kotlin
data class Transaction(
    val imageUrl: String,
    val name: String,
    val date: String,
    val amount: String
)
```

## Data

```Kotlin
val JacksonProfile = Profile(
    profileName = "Pitter Jackson",
    profileImage = "drawable/profile.png",
    totalbalance = "$ 451,654.54",
    income = "$ 1,549.46",
    banner = "drawable/banner.png",
    outcome = "$ 1,134.54",
    friend = listOf(
        Friend(
            imageUrl = "drawable/friend_4.png",
            name = "Sara"
        ),
        Friend(
            imageUrl = "drawable/friend_2.png",
            name = "Kevin"
        ),
        Friend(
            imageUrl = "drawable/friend_3.png",
            name = "Emmy"
        ),
        Friend(
            imageUrl = "drawable/friend_1.png",
            name = "Jack"
        ),
        Friend(
            imageUrl = "drawable/friend_5.png",
            name = "Maddie"
        )
    ),
    transaction = listOf(
        Transaction(
            imageUrl = "drawable/transaction_1.png",
            name = "Resturant",
            date = "17 jun 2024 19:15",
            amount = "-573.12"
        ),
        Transaction(
            imageUrl = "drawable/transaction_2.png",
            name = "David William",
            date = "18 jun 2024 10:15",
            amount = "+253.21"
        ),
        Transaction(
            imageUrl = "drawable/transaction_3.png",
            name = "McDonald",
            date = "18 jun 2024 22:15",
            amount = "-120.12"
        )
    )
)
```

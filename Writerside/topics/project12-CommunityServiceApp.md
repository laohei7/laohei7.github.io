# project12-CommunityServiceApp

## Preview

![project02_cover.jpg](project12_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project12-community_service_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-8444997019-11897e?p=5372)

- dependencies

```toml
[versions]
koinCompose = "4.1.0"
androidx-navigation = "2.9.0-beta03"
coilCompose = "3.3.0"
kotlinx-serialization = "1.9.0"

[libraries]
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

// 主题主色 (Primary / CTA)
val PurplePrimary = Color(0xFF7B4DFF)   // "Book Now" 按钮紫色
val PurpleLight = Color(0xFFE7DDFF)     // 按钮浅紫色背景
val PurpleDark = Color(0xFF5A32CC)      // 深紫色

// 状态/提醒色
val RedDiscount = Color(0xFFE53935)     // 折扣角标红
val GreenSuccess = Color(0xFF43A047)    // 成功提示绿
val YellowWarning = Color(0xFFFFC107)   // 警告黄
val BlueInfo = Color(0xFF1E88E5)        // 信息蓝

// 卡片背景 (柔和浅色)
val GreenCard = Color(0xFFDFF5D8)       // Home Cleaning 背景绿
val PinkCard = Color(0xFFFFE1F1)        // Bathroom Cleaning 粉色
val YellowCard = Color(0xFFFFF0CC)      // Sofa Carpet Cleaning 浅黄
val BlueCard = Color(0xFFDDEFFF)        // Kitchen Cleaning 浅蓝
val PurpleCard = Color(0xFFEFE0FF)      // 其他备用紫卡片背景

// 背景与中性色
val BackgroundLight = Color(0xFFF9F7F1) // 整体米白背景
val SurfaceWhite = Color(0xFFFFFFFF)    // 卡片 / 容器白
val DividerGray = Color(0xFFDDDDDD)     // 分割线
val IconGray = Color(0xFF757575)        // 图标灰
val TextPrimary = Color(0xFF212121)     // 主文字
val TextSecondary = Color(0xFF616161)   // 副文字
```

- Shape

```Kotlin
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.Shapes
import androidx.compose.ui.unit.dp

val AppShapes = Shapes(
    small = RoundedCornerShape(8.dp),
    medium = RoundedCornerShape(16.dp),
    large = RoundedCornerShape(24.dp)
)

class BurstShape(
    private val spikes: Int = 12,
    private val innerRatio: Float = 0.7f,   // 内圈半径比例
    private val cornerRadius: Float = 22f   // 圆角半径（像素）
) : Shape {
    override fun createOutline(
        size: Size,
        layoutDirection: LayoutDirection,
        density: Density
    ): Outline {
        val path = Path()
        val radius = min(size.width, size.height) / 2f
        val center = Offset(size.width / 2f, size.height / 2f)
        val step = (2 * PI / spikes).toFloat()

        // 所有点
        val points = mutableListOf<Offset>()
        var angle = 0f
        for (i in 0 until spikes * 2) {
            val r = if (i % 2 == 0) radius else radius * innerRatio
            val x = center.x + r * cos(angle)
            val y = center.y + r * sin(angle)
            points.add(Offset(x, y))
            angle += step / 2
        }

        // 圆角连接
        for (i in points.indices) {
            val prev = points[(i - 1 + points.size) % points.size]
            val current = points[i]
            val next = points[(i + 1) % points.size]

            // 方向向量
            val v1 = Offset(current.x - prev.x, current.y - prev.y)
            val v2 = Offset(current.x - next.x, current.y - next.y)

            // 单位化
            val v1n = v1 / v1.getDistance()
            val v2n = v2 / v2.getDistance()

            // 在当前点两边退让 cornerRadius
            val p1 = current - v1n * cornerRadius
            val p2 = current - v2n * cornerRadius

            if (i == 0) path.moveTo(p1.x, p1.y) else path.lineTo(p1.x, p1.y)
            path.quadraticTo(current.x, current.y, p2.x, p2.y)
        }

        path.close()
        return Outline.Generic(path)
    }
}
```

- Theme

```Kotlin
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.darkColorScheme
import androidx.compose.material3.lightColorScheme
import androidx.compose.runtime.Composable
import androidx.compose.ui.graphics.Color


private val LightColors = lightColorScheme(
    primary = PurplePrimary,
    onPrimary = Color.White,
    primaryContainer = PurpleLight,
    onPrimaryContainer = PurpleDark,

    secondary = BlueInfo,
    onSecondary = Color.White,
    secondaryContainer = BlueCard,
    onSecondaryContainer = BlueInfo,

    background = BackgroundLight,
    onBackground = TextPrimary,

    surface = SurfaceWhite,
    onSurface = TextPrimary,

    error = RedDiscount,
    onError = Color.White
)

private val DarkColors = darkColorScheme(
    primary = PurpleLight,
    onPrimary = PurpleDark,
    secondary = BlueCard,
    onSecondary = Color.Black,
    background = Color(0xFF121212),
    onBackground = Color.White,
    surface = Color(0xFF1E1E1E),
    onSurface = Color.White,
    error = Color(0xFFEF9A9A),
    onError = Color.Black
)

@Composable
fun AppTheme(
    darkTheme: Boolean = false,
    content: @Composable () -> Unit
) {
    val colors = if (darkTheme) DarkColors else LightColors

    MaterialTheme(
        colorScheme = colors,
        shapes = AppShapes,
        content = content
    )
}
```

## Model

- UserProfile

```Kotlin
data class UserProfile(
    val pic: String,
    val name: String,
    val job: String? = null,
    val phone: String
)

val UserJamali = UserProfile(
    pic = "drawable/profile.jpg",
    name = "Mohsen Jamali",
    phone = "00123456789"
)

val UserAnderson = UserProfile(
    pic = "drawable/cleaner_profile.jpg",
    name = "Tina Anderson",
    job = "Cleaner",
    phone = "00123456789"
)
```

- Banner

```Kotlin
data class Banner(
    val pic: String,
    val title: String,
    val subtitle: String
)

val BannerList = listOf(
    Banner(
        pic = "drawable/banner.png",
        title = "Quick Home\nRepair Service",
        subtitle = "24/7 Support"
    )
)
```

- Category

```Kotlin
data class Category(
    val id: Int,
    val title: String
)

val CategoryList = listOf(
    Category(0, "All"),
    Category(1, "Cleaning"),
    Category(2, "Plumber"),
    Category(3, "Mechanic"),
    Category(4, "Electric"),
)
```

- Service

```Kotlin
data class Service(
    val profile: UserProfile,
    val title: String,
    val subtitle: String,
    val description: String,
    val pic: String,
    val price: Double,
    val oldPrice: Double,
    val off: Double,
    val classicPrice: Double,
    val classicOldPrice: Double,
    val premiumPrice: Double,
    val premiumOldPrice: Double,
    val platinumPrice: Double,
    val platinumOldPrice: Double,
    val category: Category,
)

val ServiceList = listOf(
    Service(
        profile = UserAnderson,
        category = CategoryList[1],
        title = "Home Deep Cleaning",
        subtitle = "Get 20% off Our Best Services Today",
        description = "Our expert service man provides a thorough and detailed cleaning of your entire home. From dusting and vacuuming to scrubbing and sanitizing, every corner is taken care of with precision. This service is ideal if you want to refresh your living space, remove hidden dirt, and create a healthier environment for you and your family. We use safe and effective cleaning methods to make sure your furniture, floors, and surfaces are spotless.",
        pic = "drawable/cleaner1.png",
        price = 1499.0,
        oldPrice = 1499.0,
        off = 20.0,
        classicPrice = 1499.0,
        classicOldPrice = 1999.0,
        premiumPrice = 1999.0,
        premiumOldPrice = 2100.0,
        platinumPrice = 24990.0,
        platinumOldPrice = 2599.0
    ),
    Service(
        profile = UserAnderson,
        category = CategoryList[1],
        title = "Bathroom Cleaning",
        subtitle = "Sparking bathrooms.\nenjoy 20$ today",
        description = "Our bathroom cleaning service ensures spotless tiles, sanitized sinks, and shining mirrors. We focus on removing tough stains, soap scum, and bacteria to leave your bathroom fresh and hygienic. Perfect if you want a hotel-like shine in your own home.",
        pic = "drawable/cleaner2.png",
        price = 1299.0,
        oldPrice = 1699.0,
        off = 24.0,
        classicPrice = 1299.0,
        classicOldPrice = 1699.0,
        premiumPrice = 1799.0,
        premiumOldPrice = 2100.0,
        platinumPrice = 2299.0,
        platinumOldPrice = 2599.0
    ),
    Service(
        profile = UserAnderson,
        category = CategoryList[1],
        title = "Sofa Carpet Clean",
        subtitle = "Fresh sofas, carpets get 15% off",
        description = "We specialize in deep cleaning sofas, carpets, and upholstery to remove dust, stains, and allergens. Using eco-friendly solutions, we restore the original freshness of your furniture and create a healthier home atmosphere.",
        pic = "drawable/cleaner3.png",
        price = 1199.0,
        oldPrice = 1499.0,
        off = 15.0,
        classicPrice = 1199.0,
        classicOldPrice = 1499.0,
        premiumPrice = 1599.0,
        premiumOldPrice = 1899.0,
        platinumPrice = 20990.0,
        platinumOldPrice = 2399.0
    ),
    Service(
        profile = UserAnderson,
        category = CategoryList[1],
        title = "Kitchen Cleaning",
        subtitle = "Get 20% off Our Best Services Today",
        description = "Our kitchen cleaning service removes grease, grime, and food stains from counters, cabinets, and appliances. From stovetops to sinks, we leave your kitchen sparkling clean and safe for cooking.",
        pic = "drawable/cleaner4.png",
        price = 1399.0,
        oldPrice = 1799.0,
        off = 22.0,
        classicPrice = 1399.0,
        classicOldPrice = 1799.0,
        premiumPrice = 1999.0,
        premiumOldPrice = 2099.0,
        platinumPrice = 2299.0,
        platinumOldPrice = 2599.0
    )
)
```
# project02-RealEstateApp

## Preview

![project02_cover.jpg](project02_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project02_real_estate_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1503409906-f6b536?p=5372)

- strings

```XML
<resources>
    <string name="app_name">real_estate_app</string>
    <string name="str_intro_title">PERFECT CHOICE\nFOR YOUR FUTURE</string>
    <string name="str_intro_desc">Our properties the masterpiece for every client with lasting value</string>
    <string name="str_intro_hint">Already have an account?</string>
    <string name="str_login">Sign in</string>
    <string name="str_start">Start</string>
    <string name="str_locations">Locations</string>
    <string name="str_search_hint">Search…</string>
    <string name="str_recommend">Recommend Property</string>
    <string name="str_nearby">Nearby Property</string>
    <string name="str_see_all">See all</string>
    <string name="str_ask_tips">Ask for Virtual Tour</string>
    <string name="str_request">Request Appointment</string>
    <string-array name="array_locations">
        <item>LosAngles, USA</item>
        <item>NewYork, USA</item>
    </string-array>
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

## 实体类

### PropertyDomain

```Kotlin
data class PropertyDomain(
    val title: String,
    val type: String,
    val address: String,
    val description: String,
    @DrawableRes val pic: Int,
    val price: Int,
    val bed: Int,
    val bath: Int,
    val size: Int,
    val garage: Boolean,
    val score: Double
)
```

### 数据

```Kotlin
val propertyDomains = listOf(
    PropertyDomain(
        title = "Apartment",
        type = "Royal Apartment",
        address = "LosAngles LA",
        description = """
            This 2 bed /1 bath home boasts an enormous, open-living plan, accented by striking architectural features and high-end finishes. Feel inspired by open sight lines that embrace the outdoors, crowned by stunning coffered ceilings.
        """.trimIndent(),
        pic = R.drawable.house_1,
        price = 1500,
        bed = 2,
        bath = 3,
        size = 350,
        garage = true,
        score = 4.5
    ),
    PropertyDomain(
        title = "House",
        type = "House with Great View",
        address = "NewYork NY",
        description = """
            This 2 bed /1 bath home boasts an enormous, open-living plan, accented by striking architectural features and high-end finishes. Feel inspired by open sight lines that embrace the outdoors, crowned by stunning coffered ceilings.
        """.trimIndent(),
        pic = R.drawable.house_2,
        price = 800,
        bed = 1,
        bath = 2,
        size = 500,
        garage = false,
        score = 4.9
    ),
    PropertyDomain(
        title = "Villa",
        type = "Royal Villa",
        address = "LosAngles La",
        description = """
            This 2 bed /1 bath home boasts an enormous, open-living plan, accented by striking architectural features and high-end finishes. Feel inspired by open sight lines that embrace the outdoors, crowned by stunning coffered ceilings.
        """.trimIndent(),
        pic = R.drawable.house_3,
        price = 999,
        bed = 2,
        bath = 1,
        size = 400,
        garage = true,
        score = 4.7
    ),
    PropertyDomain(
        title = "House",
        type = "Beauty House",
        address = "NewYork NY",
        description = """
            This 2 bed /1 bath home boasts an enormous, open-living plan, accented by striking architectural features and high-end finishes. Feel inspired by open sight lines that embrace the outdoors, crowned by stunning coffered ceilings.
        """.trimIndent(),
        pic = R.drawable.house_4,
        price = 1750,
        bed = 3,
        bath = 2,
        size = 1100,
        garage = true,
        score = 4.3
    ),
)
```
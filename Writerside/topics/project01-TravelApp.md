# project01-TravelApp

## Preview

![project01_travel_app_cover.jpg](project01_travel_app_cover.jpg)

## Settings Project

- navigation

```kotlin
// projecct kts
kotlin("plugin.serialization") version "2.1.0"

// app kts
kotlin("plugin.serialization")
implementation("androidx.navigation:navigation-compose:2.8.9")
implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.8.0")
```

- room

```kotlin
// project kts
id("com.google.devtools.ksp") version "2.1.10-1.0.31" apply false
id("androidx.room") version "2.7.1" apply false

// app kts
id("com.google.devtools.ksp")
id("androidx.room")

implementation("androidx.room:room-runtime:2.7.1")
implementation("androidx.room:room-ktx:2.7.1")
implementation("androidx.room:room-compiler:2.7.1")

// app kts android { ... }
room {
    schemaDirectory("$projectDir/schemas")
}
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
implementation("io.coil-kt.coil3:coil-network-okhttp:3.1.0")
```

- rating bar

```kotlin
// project kts 
maven { url = uri("https://jitpack.io") }

// app kts
implementation("com.github.a914-gowtham:compose-ratingbar:1.3.12")
```

## Create Room Database

### Entities

- BannerEntity

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "Banner")
data class BannerEntity(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val url: String
)
```

- CategoryEntity

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "Category")
data class CategoryEntity(
    @PrimaryKey
    val id: Int,
    val imagePath: String,
    val name: String
)
```

- ItemEntity

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "Item")
data class ItemEntity(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val address: String,
    val bed: Int,
    val dateTour: String,
    val description: String,
    val distance: String,
    val duration: String,
    val pic: String,
    val price: Int,
    val score: Double,
    val timeTour: String,
    val title: String,
    val tourGuideName: String,
    val tourGuidePhone: String,
    val tourGuidePic: String
)
```

- LocationEntity

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "Location")
data class LocationEntity(
    @PrimaryKey
    val id: Int,
    val loc: String
)
```

- PopularEntity

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "Popular")
data class PopularEntity(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val address: String,
    val bed: Int,
    val dateTour: String,
    val description: String,
    val distance: String,
    val duration: String,
    val pic: String,
    val price: Int,
    val score: Double,
    val timeTour: String,
    val title: String,
    val tourGuideName: String,
    val tourGuidePhone: String,
    val tourGuidePic: String
)
```

### Dao

- BannerDao

```kotlin
import androidx.room.*

@Dao
interface BannerDao {

    @Query("SELECT * FROM Banner")
    suspend fun getAllBanners(): List<BannerEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertBanner(banner: BannerEntity)

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertBanners(banners: List<BannerEntity>)

    @Delete
    suspend fun deleteBanner(banner: BannerEntity)

    @Update
    suspend fun updateBanner(banner: BannerEntity)
}
```

- CategoryDao

```kotlin
import androidx.room.*

@Dao
interface CategoryDao {

    @Query("SELECT * FROM Category")
    suspend fun getAllCategories(): List<CategoryEntity>

    @Query("SELECT * FROM Category WHERE id = :id")
    suspend fun getCategoryById(id: Int): CategoryEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertCategory(category: CategoryEntity)

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertCategories(categories: List<CategoryEntity>)

    @Delete
    suspend fun deleteCategory(category: CategoryEntity)

    @Update
    suspend fun updateCategory(category: CategoryEntity)
}
```

- ItemDao

```kotlin
import androidx.room.*

@Dao
interface ItemDao {

    @Query("SELECT * FROM Item")
    suspend fun getAllItems(): List<ItemEntity>

    @Query("SELECT * FROM Item WHERE id = :id")
    suspend fun getItemById(id: Int): ItemEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertItem(item: ItemEntity)

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertItems(items: List<ItemEntity>)

    @Delete
    suspend fun deleteItem(item: ItemEntity)

    @Update
    suspend fun updateItem(item: ItemEntity)
}
```

- LocationDao

```kotlin
import androidx.room.*

@Dao
interface LocationDao {

    @Query("SELECT * FROM Location")
    suspend fun getAllLocations(): List<LocationEntity>

    @Query("SELECT * FROM Location WHERE id = :id")
    suspend fun getLocationById(id: Int): LocationEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertLocation(location: LocationEntity)

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertLocations(locations: List<LocationEntity>)

    @Delete
    suspend fun deleteLocation(location: LocationEntity)

    @Update
    suspend fun updateLocation(location: LocationEntity)
}
```

- PopularDao

```kotlin
import androidx.room.*

@Dao
interface PopularDao {

    @Query("SELECT * FROM Popular")
    suspend fun getAllPopulars(): List<PopularEntity>

    @Query("SELECT * FROM Popular WHERE id = :id")
    suspend fun getPopularById(id: Int): PopularEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertPopular(popular: PopularEntity)

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertPopulars(populars: List<PopularEntity>)

    @Delete
    suspend fun deletePopular(popular: PopularEntity)

    @Update
    suspend fun updatePopular(popular: PopularEntity)
}
```

### database

```kotlin
import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase

@Database(entities = [
    BannerEntity::class,
    CategoryEntity::class,
    ItemEntity::class,
    LocationEntity::class,
    PopularEntity::class
], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun bannerDao(): BannerDao
    abstract fun categoryDao(): CategoryDao
    abstract fun itemDao(): ItemDao
    abstract fun locationDao(): LocationDao
    abstract fun popularDao(): PopularDao

    companion object {
        @Volatile
        private var INSTANCE: AppDatabase? = null

        private const val DATABASE_NAME = "database.db"

        fun getInstance(context: Context): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                INSTANCE ?: buildDatabase(context).also { INSTANCE = it }
            }
        }

        private fun buildDatabase(context: Context): AppDatabase {
            return Room.databaseBuilder(
                context.applicationContext,
                AppDatabase::class.java,
                DATABASE_NAME
            ).createFromAsset(DATABASE_NAME)
                .build()
        }
    }
}
```

## Utils

- ActivityExt

```kotlin
import android.app.Activity
import android.os.Build
import android.view.View
import androidx.core.view.ViewCompat
import androidx.core.view.WindowCompat
import androidx.core.view.WindowInsetsCompat
import androidx.core.view.WindowInsetsControllerCompat

fun Activity.hideSystemUI() {
    val windowInsetsController =
        WindowCompat.getInsetsController(window, window.decorView)
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
        windowInsetsController
            .apply {
                hide(WindowInsetsCompat.Type.systemBars())
                systemBarsBehavior =
                    WindowInsetsControllerCompat.BEHAVIOR_SHOW_TRANSIENT_BARS_BY_SWIPE
            }
    } else {
        @Suppress("DEPRECATION")
        window.decorView.systemUiVisibility = (
                View.SYSTEM_UI_FLAG_FULLSCREEN or
                        View.SYSTEM_UI_FLAG_HIDE_NAVIGATION or
                        View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY
                )
    }
}

fun Activity.showSystemUI() {
    val windowInsetsController =
        WindowCompat.getInsetsController(window, window.decorView)
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
        windowInsetsController
            .apply {
                show(WindowInsetsCompat.Type.systemBars())
            }
    } else {
        @Suppress("DEPRECATION")
        window.decorView.systemUiVisibility = View.SYSTEM_UI_FLAG_VISIBLE
    }
}
```

## Note

- **网络图片加载不成功，自行使用素材包里的对应图片。**
- **预填充数据修改，使用 dbeaver 等打开 database.db 文件进行编辑**
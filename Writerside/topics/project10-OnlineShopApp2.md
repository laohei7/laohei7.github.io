# project10-OnlineShopApp2

## Preview

![project10_cover.jpg](project10_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project10-online_shop_app_2)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-8409477510-714617?p=5372)

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
implementation("io.insert-koin:koin-android:4.0.3")
implementation("io.insert-koin:koin-androidx-compose:4.0.3")
implementation("io.insert-koin:koin-androidx-compose-navigation:4.0.3")
```

- coil

```kotlin
implementation("io.coil-kt.coil3:coil-compose:3.1.0")
implementation("io.coil-kt.coil3:coil-network-okhttp:3.1.0")
```

- constraint layout

```kotlin
implementation("androidx.constraintlayout:constraintlayout-compose:1.0.1")
```

## Create Room Database

### Entities

- BannerEntity

```kotlin
@Entity(tableName = "Banner")
data class BannerEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    @ColumnInfo(name = "url") val url: String
)
```

- CategoryEntity

```kotlin
@Entity(tableName = "Category")
data class CategoryEntity(
    @PrimaryKey val id: Int,
    @ColumnInfo(name = "title") val title: String
)
```

- ItemEntity

```kotlin
@Entity(tableName = "Items")
data class ItemEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val title: String,
    val description: String,
    val price: Double,
    val oldPrice: Double,
    val rating: Double,
    val review: Int,
    val offPercent: String,
    val picUrls: String,   // 存储 JSON 字符串
    val sizes: String,     // 存储 JSON 字符串
    val colors: String     // 存储 JSON 字符串
)
```

### Dao

- BannerDao

```kotlin
@Dao
interface BannerDao {
    @Query("SELECT * FROM Banner")
    suspend fun getAll(): List<BannerEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(banners: List<BannerEntity>)
}
```

- CategoryDao

```kotlin
@Dao
interface CategoryDao {
    @Query("SELECT * FROM Category")
    suspend fun getAll(): List<CategoryEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(categories: List<CategoryEntity>)
}
```

- ItemDao

```kotlin
@Dao
interface ItemDao {
    @Query("SELECT * FROM Items")
    suspend fun getAll(): List<ItemEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(items: List<ItemEntity>)
}
```

### TypeConverter

```kotlin
class Converters {
    @TypeConverter
    fun fromStringList(list: List<String>): String {
        return Json.encodeToString(list)
    }

    @TypeConverter
    fun toStringList(jsonString: String): List<String> {
        return Json.decodeFromString(jsonString)
    }
}
```

### database

```kotlin
@Database(entities = [
    BannerEntity::class,
    CategoryEntity::class,
    ItemEntity::class,
], version = 1)
@TypeConverters(Converters::class)
abstract class AppDatabase : RoomDatabase() {
    abstract fun bannerDao(): BannerDao
    abstract fun categoryDao(): CategoryDao
    abstract fun itemDao(): ItemDao

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


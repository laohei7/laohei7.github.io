# Project04-OnlineShopApp

## Preview

![project03_cover.jpg](project04_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project04_online_shop_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1510479283-d948cb?p=5372)

- strings

```XML
<resources>
    <string name="str_welcome_back">Welcome Back</string>
    <string name="str_user">Tina Anderson</string>
    <string name="str_intro">Skincare made\nsimple,\nbeautiful,\nand effective</string>
    <string name="str_start">Let's Get Started</string>
    <string name="str_subtotal">Subtotal</string>
    <string name="str_free_delivery">Free Delivery</string>
    <string name="str_total_tax">Total Tax</string>
    <string name="str_total">Total</string>
    <string name="str_check_out">Check Out</string>
    <string name="str_product">Product</string>
    <string name="str_popular_product">Popular Product</string>
    <string name="str_see_all">See All</string>
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

- palette

```kotlin
implementation("androidx.palette:palette:1.0.0")
```

## Create Room Database

### Entity

- BannerEntity

```kotlin
@Entity(tableName = "banner")
data class BannerEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val url: String
)
```

- CategoryEntity

```kotlin
@Entity(tableName = "category")
data class CategoryEntity(
    @PrimaryKey val id: Int,
    val title: String
)
```

- ItemEntity

```kotlin
@Entity(tableName = "item")
data class ItemEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val title: String,
    val description: String,
    val price: Int,
    val rating: Double,
    val showRecommended: Boolean,
    val categoryId: Int
)
```

- PicUrlEntity

```kotlin
@Entity(tableName = "pic_url")
data class PicUrlEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val itemId: Int,
    val url: String
)
```

### Relation

- ItemWithPics

```kotlin
data class ItemWithPics(
    @Embedded val item: ItemEntity,
    @Relation(
        parentColumn = "id",
        entityColumn = "itemId"
    )
    val pics: List<PicUrlEntity>
)
```

- ItemWithCategory

```kotlin
data class ItemWithCategory(
    @Embedded val item: ItemEntity,
    @Relation(
        parentColumn = "categoryId",
        entityColumn = "id"
    )
    val category: CategoryEntity
)
```

- ItemFull

```kotlin
data class ItemFull(
    @Embedded val item: ItemEntity,
    @Relation(
        parentColumn = "id",
        entityColumn = "itemId"
    )
    val pics: List<PicUrlEntity>,

    @Relation(
        parentColumn = "categoryId",
        entityColumn = "id"
    )
    val category: CategoryEntity
)
```

### Dao

- BannerDao

```kotlin
@Dao
interface BannerDao {
    @Insert
    suspend fun insertAll(banners: List<BannerEntity>)

    @Query("SELECT * FROM banner")
    suspend fun getAll(): List<BannerEntity>
}
```

- CategoryDao

```kotlin
@Dao
interface CategoryDao {
    @Insert
    suspend fun insertAll(categories: List<CategoryEntity>)

    @Query("SELECT * FROM category")
    suspend fun getAll(): List<CategoryEntity>
}
```

- ItemDao

```kotlin
@Dao
interface ItemDao {
    @Insert
    suspend fun insertAll(items: List<ItemEntity>)

    @Transaction
    @Query("SELECT * FROM item")
    suspend fun getItemsWithPics(): List<ItemWithPics>

    @Transaction
    @Query("SELECT * FROM item WHERE showRecommended = 1")
    suspend fun getRecommendedItems(): List<ItemWithPics>

    @Transaction
    @Query("SELECT * FROM item WHERE categoryId = :categoryId")
    suspend fun getItemsByCategory(categoryId: Int): List<ItemWithPics>

    @Transaction
    @Query("SELECT * FROM item")
    suspend fun getFullItemData(): List<ItemFull>
}
```

- PicUrlDao

```kotlin
@Dao
interface PicUrlDao {
    @Insert
    suspend fun insertAll(picUrls: List<PicUrlEntity>)

    @Query("SELECT * FROM pic_url WHERE itemId = :itemId")
    suspend fun getPicsByItemId(itemId: Int): List<PicUrlEntity>
}
```

### database

```kotlin
@Database(entities = [
    BannerEntity::class,
    CategoryEntity::class,
    ItemEntity::class,
    PicUrlEntity::class
], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun bannerDao(): BannerDao
    abstract fun categoryDao(): CategoryDao
    abstract fun itemDao(): ItemDao
    abstract fun picUrlDao(): PicUrlDao

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

### BitmapUtil

```kotlin
fun Bitmap.toNonHardwareBitmap(): Bitmap {
    return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O && this.config == Bitmap.Config.HARDWARE) {
        this.copy(Bitmap.Config.ARGB_8888, true)
    } else {
        this
    }
}
```
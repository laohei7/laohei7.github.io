# project08-FoodApp

## Preview

![project07_cover.jpg](project08_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project08_food_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1523670844-76fa80?p=5372)

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

## Create Room Database

### Entity

- CategoryEntity

```kotlin
@Entity(tableName = "category")
data class CategoryEntity(
    @PrimaryKey val id: Int,
    @ColumnInfo(name = "image_path") val imagePath: String,
    @ColumnInfo(name = "name") val name: String
)
```

- FoodEntity

```kotlin
@Entity(
    tableName = "food",
    foreignKeys = [
        ForeignKey(
            entity = CategoryEntity::class,
            parentColumns = ["id"],
            childColumns = ["category_id"],
            onDelete = ForeignKey.CASCADE
        )
    ],
    indices = [Index("category_id")]
)
data class FoodEntity(
    @PrimaryKey val id: Int,
    @ColumnInfo(name = "title") val title: String,
    @ColumnInfo(name = "description") val description: String,
    @ColumnInfo(name = "image_path") val imagePath: String,
    @ColumnInfo(name = "price") val price: Double,
    @ColumnInfo(name = "calorie") val calorie: Int,
    @ColumnInfo(name = "star") val star: Double,
    @ColumnInfo(name = "best_food") val bestFood: Boolean,
    @ColumnInfo(name = "time_id") val timeId: Int,
    @ColumnInfo(name = "time_value") val timeValue: Int,
    @ColumnInfo(name = "price_id") val priceId: Int,
    @ColumnInfo(name = "location_id") val locationId: Int,
    @ColumnInfo(name = "category_id") val categoryId: Int
)
```

### Dao

- CategoryDao

```kotlin
@Dao
interface CategoryDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(categories: List<CategoryEntity>)

    @Query("SELECT * FROM category")
    suspend fun getAll(): List<CategoryEntity>
}
```

- FoodDao

```kotlin
@Dao
interface FoodDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(foods: List<FoodEntity>)

    @Query("SELECT * FROM food WHERE category_id = :categoryId")
    suspend fun getFoodsByCategory(categoryId: Int): List<FoodEntity>

    @Query("SELECT * FROM food WHERE best_food = 1")
    suspend fun getBestFoods(): List<FoodEntity>
}
```


### database

```kotlin
@Database(entities = [
    CategoryEntity::class,
    FoodEntity::class,
], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun categoryDao(): CategoryDao
    abstract fun foodDao(): FoodDao

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



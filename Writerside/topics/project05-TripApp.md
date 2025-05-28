# project05-TripApp

## Preview

![project03_cover.jpg](project05_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project05_trip_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-1509129577-7e4f53?p=5372)

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

## Create Database

### Entity

- TripEntity

```kotlin
@Entity(tableName = "Trips")
data class TripEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    @ColumnInfo(name = "companyLogo") val companyLogo: String,
    @ColumnInfo(name = "companyName") val companyName: String,
    @ColumnInfo(name = "arriveTime") val arriveTime: String,
    @ColumnInfo(name = "date") val date: String,
    @ColumnInfo(name = "from") val from: String,
    @ColumnInfo(name = "fromShort") val fromShort: String,
    @ColumnInfo(name = "price") val price: Double,
    @ColumnInfo(name = "time") val time: String,
    @ColumnInfo(name = "to") val to: String,
    @ColumnInfo(name = "toShort") val toShort: String,
    @ColumnInfo(name = "score") val score: Double,
    @ColumnInfo(name = "numberSeat") val numberSeat: Int?
)
```

- RecommendedPlaceEntity

```kotlin
@Entity(tableName = "RecommendedPlace")
data class RecommendedPlaceEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    @ColumnInfo(name = "title") val title: String,
    @ColumnInfo(name = "picUrl") val picUrl: String
)
```

- LocationEntity

```kotlin
@Entity(tableName = "Locations")
data class LocationEntity(
    @PrimaryKey val id: Int,
    val name: String
)
```

### Dao

- TripDao

```kotlin
@Dao
interface TripDao {
    @Query("SELECT * FROM Trips")
    suspend fun getAllTrips(): List<TripEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(trips: List<TripEntity>)
}
```

- RecommendedPlaceDao

```kotlin
@Dao
interface RecommendedPlaceDao {
    @Query("SELECT * FROM RecommendedPlace")
    suspend fun getAllPlaces(): List<RecommendedPlaceEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(places: List<RecommendedPlaceEntity>)
}
```

- LocationDao

```kotlin
@Dao
interface LocationDao {
    @Query("SELECT * FROM Locations")
    suspend fun getAllLocations(): List<LocationEntity>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(locations: List<LocationEntity>)
}
```

### database

```kotlin
@Database(entities = [
    TripEntity::class,
    RecommendedPlaceEntity::class,
    LocationEntity::class,
], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun tripDao(): TripDao
    abstract fun recommendedPlaceDao(): RecommendedPlaceDao
    abstract fun locationDao(): LocationDao

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
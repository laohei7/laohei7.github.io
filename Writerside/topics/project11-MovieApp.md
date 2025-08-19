# project11-MovieApp

## Preview

![project10_cover.jpg](project11_cover.jpg)

## Settings Project

- resources

[Github](https://github.com/laohei7/compose_ui/tree/main/project11-movie_app)

[压缩包（访问密码：5372）](https://url93.ctfile.com/f/48492093-8418122632-b93df9?p=5372)

- navigation & serialization

```kotlin
// projecct kts
kotlin("plugin.serialization") version "2.1.0"

// app kts
kotlin("plugin.serialization")
implementation("androidx.navigation:navigation-compose:2.8.9")
implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.8.0")
```

- coil

```kotlin
implementation("io.coil-kt.coil3:coil-compose:3.1.0")
implementation("io.coil-kt.coil3:coil-network-okhttp:3.1.0")
```

- koin

```kotlin
implementation("io.insert-koin:koin-android:4.0.3")
implementation("io.insert-koin:koin-androidx-compose:4.0.3")
implementation("io.insert-koin:koin-androidx-compose-navigation:4.0.3")
```

- constraint layout

```kotlin
implementation("androidx.constraintlayout:constraintlayout-compose:1.0.1")
```

- common_ui_kit

```kotlin
// settings.gradle.kts
maven { url = uri("https://jitpack.io") }

// app kts
implementation("com.github.laohei7:common_ui_kit:1.0.0")
```

- ktor client

```kotlin
implementation("io.ktor:ktor-client-core:3.2.3")
implementation("io.ktor:ktor-client-okhttp:3.2.3")
implementation("io.ktor:ktor-client-content-negotiation:3.2.3")
implementation("io.ktor:ktor-serialization-kotlinx-json:3.2.3")
```

## Api

- Movie List

```text
https://moviesapi.ir/api/v1/movies?page={page}
```

- Movie Detail

```Text
https://moviesapi.ir/api/v1/movies/{movie_id}
```

- Genres

```Text
https://moviesapi.ir/api/v1/genres
```

## Model

- MovieResponse

```kotlin
import kotlinx.serialization.SerialName
import kotlinx.serialization.Serializable

@Serializable
data class MovieResponse(
    val data: List<Movie> = emptyList(),
    val metadata: Metadata? = null
)

@Serializable
data class Movie(
    val id: Int = 0,
    val title: String = "",
    val poster: String? = null,
    val year: String? = null,
    val country: String? = null,
    @SerialName("imdb_rating")
    val imdbRating: String? = null,
    val genres: List<String> = emptyList(),
    val images: List<String> = emptyList()
)

@Serializable
data class Metadata(
    @SerialName("current_page")
    val currentPage: String? = null,
    @SerialName("per_page")
    val perPage: Int = 0,
    @SerialName("page_count")
    val pageCount: Int = 0,
    @SerialName("total_count")
    val totalCount: Int = 0
)
```

- MovieDetail

```kotlin
import kotlinx.serialization.SerialName
import kotlinx.serialization.Serializable

@Serializable
data class MovieDetail(
    val id: Int = 0,
    val title: String = "",
    val poster: String? = null,
    val year: String? = null,
    val rated: String? = null,
    val released: String? = null,
    val runtime: String? = null,
    val director: String? = null,
    val writer: String? = null,
    val actors: String? = null,
    val plot: String? = null,
    val country: String? = null,
    val awards: String? = null,
    val metascore: String? = null,

    @SerialName("imdb_rating")
    val imdbRating: String? = null,

    @SerialName("imdb_votes")
    val imdbVotes: String? = null,

    @SerialName("imdb_id")
    val imdbId: String? = null,

    val type: String? = null,
    val genres: List<String> = emptyList(),
    val images: List<String> = emptyList()
)
```

- Genre

```kotlin
import kotlinx.serialization.Serializable

@Serializable
data class Genre(
    val id: Int = 0,
    val name: String = ""
)
```


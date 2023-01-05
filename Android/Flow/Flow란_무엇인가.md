## Flow

- 여러 값을 순차적으로 내보낼 수 있는 유형
- 비동기식으로 계산할 수 있는 데이터 스트림
- 코루틴 상에서 리액티브 프로그래밍을 지원하기 위한 구성요소

### 데이터 스트림

데이터 스트림의 구성요소는 생성자, 중객자, 소비자이다.

- Producer(생성자) : 스트림에 추가되는 데이터 생산
- Intermediary(중개자 - 선택사항) : 스트림에 내보내는 각각의 값이나 스트림 자체를 수정할 수 있음
- Customer(소비자) : 스트림의 값을 사용


### 리액티브 프로그래밍

- 데이터가 변경될 때 이벤트를 발생시켜서 데이터를 계속 전달하도록 하는 프로그래밍 방식

### Flow 만들기(Producer)

- flow 빌더 API를 사용해서 Flow를 만들 수 있습니다.
- emit 함수를 사용해서 새 값을 수동으로 데이터 스트림에 내보낼 수 있는 새 흐름을 만듭니다.
- flow{} 블록 내부에서 emit()을 통해 데이터를 생성합니다.

```kotlin
class NewsRemoteDataSource(
    private val newsApi: NewsApi,
    private val refreshIntervalMs: Long = 5000
) {
    val latestNews: Flow<List<ArticleHeadline>> = flow {
        while(true) {
            val latestNews = newsApi.fetchLatestNews()
            emit(latestNews) // Emits the result of the request to the flow
            delay(refreshIntervalMs) // Suspends the coroutine for some time
        }
    }
}

// Interface that provides a way to make network requests with suspend functions
interface NewsApi {
    suspend fun fetchLatestNews(): List<ArticleHeadline>
}
```

### 스트림 수정(Intermediary)

- 중간 연산자를 사용하여 값을 소비하지 않고도 데이터 스트림을 수정할 수 있습니다.
- ex) map(데이터 변형), filter(데이터 필터링), onEach(모든 데이터마다 연산 수행)

```kotlin
class NewsRepository(
    private val newsRemoteDataSource: NewsRemoteDataSource,
    private val userData: UserData
) {
    /**
     * Returns the favorite latest news applying transformations on the flow.
     * These operations are lazy and don't trigger the flow. They just transform
     * the current value emitted by the flow at that point in time.
     */
    val favoriteLatestNews: Flow<List<ArticleHeadline>> =
        newsRemoteDataSource.latestNews
            // Intermediate operation to filter the list of favorite topics
            .map { news -> news.filter { userData.isFavoriteTopic(it) } }
            // Intermediate operation to save the latest news in the cache
            .onEach { news -> saveInCache(news) }
}
```

### Flow 수집(Customer)

- 터미널 연산자를 사용해 값 수신 대기를 시작하는 flow를 트리거합니다.
- collect를 사용해서 전달된 데이터를 사용합니다.
- collect는 정지 함수 → 코루틴 내에서 실행해야 합니다.

```kotlin
class LatestNewsViewModel(
    private val newsRepository: NewsRepository
) : ViewModel() {

    init {
        viewModelScope.launch {
            // Trigger the flow and consume its elements using collect
            newsRepository.favoriteLatestNews.collect { favoriteNews ->
                // Update View with the latest favorite news
            }
        }
    }
}
```

### 예외 포착

- 예기치 않은 예외를 처리하려면 catch 중간 연산자를 사용합니다.
- catch는 항목을 흐름에 emit할 수도 있습니다.

```kotlin
class LatestNewsViewModel(
    private val newsRepository: NewsRepository
) : ViewModel() {

    init {
        viewModelScope.launch {
            newsRepository.favoriteLatestNews
                // Intermediate catch operator. If an exception is thrown,
                // catch and update the UI
                .catch { exception -> notifyError(exception) }
                .collect { favoriteNews ->
                    // Update View with the latest favorite news
                }
        }
    }
}
```

### Jetpack 라이브러리의 Flow

- Flow는 실시간 데이터 업데이트 및 무제한 데이터 스트림에 적합합니다.
- Flow with Room을 사용하여 데이터베이스 변경 알림을 받을 수 있습니다.
    - DAO를 사용하는 경우 실시간 업데이트를 받으려면 Flow 유형을 반환합니다.

```kotlin
@Dao
abstract class ExampleDao {
    @Query("SELECT * FROM Example")
    abstract fun getExamples(): Flow<List<Example>>
}
```

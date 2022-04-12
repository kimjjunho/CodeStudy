# Room

- 스마트폰 내장 DB에 **데이터를 저장하기 위해 사용하는 라이브러리**이다.
- SQLite라는 데이터베이스 엔진은 사용하기 어렵다는 단점이 있다. Room은 이러한 SQLite의 문제들을 자동으로 처리할 수 있도록 도와준다.
- Room은 완전히 새로운 개념이 아니라, SQLite를 활용해서 객체 매핑을 해주는 역할을 한다.

### 구조

![img](https://blog.kakaocdn.net/dn/cu1a4e/btq1KAzkc4e/SGWx0e7YJkeycMQerJbLh1/img.png)

- Room Database, Data Access Objects, Entities 이렇게 3개가 Room의 구성요소이다.
- Rest of The App는 앱의 나머지 부분을 의미
- 간단한 정보는 sharedpreferences를 사용하면 좋다
- 대량의 데이터는 Realm을 사용하면 좋다

### Entity

- '개체'인 Entity는 관련이 있는 속성들이 모여 **하나의 정보 단위**를 이룬 것이다.
- 예를 들어 사람의 이름, 나이, 번호라는 속성이 모여서 하나의 정보 단위를 이루면 이것을 Entity라고 한다.
- 객체는 개체를 포함한 더 큰 개념이다.

```kotlin
@Entity
data class User (
    var name: String,
    var age: String,
    var phone: String
){
    @PrimaryKey(autoGenerate = true) var id: Int = 0
}
```

- data class에 @Entity를 생성해야 한다.(데이터베이스 테이블을 만든다고 생각)
- @Entity(tableName="이름") 이런식으로 테이블 이름을 정해줄 수 있다
- @PrimaryKey는 키 값이기 때문에 유일한 값이어야 한다. 직접 지정 가능. autoGenerate를 true로 주면 자동으로 값을 생성

### DAO

- Data Access Object의 줄임말

- 데이터에 접근할 수 있는 메서드를 저의해놓은 인터페이스

  ```kotlin
  @Dao
  interface UserDao {
      @Insert
      fun insert(user: User)
   
      @Update
      fun update(user: User)
   
      @Delete
      fun delete(user: User)
  }
  ```

- @Insert : 테이블에 데이터 삽입

- @Update : 테이블의 데이터 수정

- @Delete를 붙이면 테이블의 데이터 삭제

  ```kotlin
  @Dao
  interface UserDao {
      @Query("SELECT * FROM User") // 테이블의 모든 값을 가져와라
      fun getAll(): List<User>
   
      @Query("DELETE FROM User WHERE name = :name") // 'name'에 해당하는 유저를 삭제해라
      fun deleteUserByName(name: String)
  }
  ```

  - Query 어노테이션을 붙이고 그 안에 어떤 동작을 할 건지 sql문법으로 작성을 해야함

### Room Database

```kotlin
@Database(entities = [User::class], version = 1)
abstract class UserDatabase: RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

- 데이터베이스를 생성하고 관리하는 데이터베이스 객체 만들기 위해서 위와 같은 추상 클래스를 만들어줘야 한다. 우선 RoomDatabase클래스를 상속받고, @Database 어노테이션으로 데이터베이스임을 표시한다.
- 어노테이션 괄호 안에 entities에는 위에서 만든 entity를 넣어주면 된다.
- version은 앱을 업데이트하다가 entitiy의 구조를 변경해야 하는 일이 생겼을 때 이전 구조와 현재 구조를 구분해주는 역할을 한다
- 만약 구조가 바뀌었는데 버진이 같다면 에러가 뜨며 디버깅이 되지 않는다
- 처음 데이터베이스를 생성하는 상황이라면 그냥 1을 넣어주면 된다

```kotlin
@Database(entities = arrayOf(User::class, Student::class), version = 1)
abstract class UserDatabase: RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

- 만약 하나의 데이터 베이스가 여러 개의 entity를 가져야 한다면 arrayOf() 안에 콤마로 구분해서 entity를 넣어주면 된다.

- 데이터베이스 객체를 인스턴스 할 때 싱글톤으로 구현하기를 권장하고 있다.

  *싱글톤 : **객체의 인스턴스가 오직 1개만 생성**되는 패턴을 의미한다. 

```kotlin
@Database(entities = [User::class], version = 1)
abstract class UserDatabase: RoomDatabase() {
    abstract fun userDao(): UserDao
 
    companion object {
        private var instance: UserDatabase? = null
 
        @Synchronized
        fun getInstance(context: Context): UserDatabase? {
            if (instance == null) {
                synchronized(UserDatabase::class){
                    instance = Room.databaseBuilder(
                        context.applicationContext,
                        UserDatabase::class.java,
                        "user-database"
                    ).build()
                }
            }
            return instance
        }
    }
}
```

- companion object로 객체를 선언해서 사용하면 된다. 싱글톤으로 구현하지 않을 거라면 저 코드 부분을 호출할 부분에서 사용하면 된다.
- 객체를 생성할 때 databaseBuilder라는 static 메서드를 사용하는데, context와, database 클래스 그리고 데이터 베이스를 저장할 때 사용할 데이터베이스의 이름을 정해서 넘겨주면 된다(다른 데이터베이스랑 이름이 겹치면 꼬여버리니 주의해야 한다)

### 데이터 베이스 사용

"Cannot access database on the main thread since it may potentially lock the UI for a long period of time"에러가 뜨기도 하는데 이는 비동기 처리를 통해 해결할 수 있다.

**1번째 선택지**

-> allowMainThreadQueries()를 사용해 강제로 실행시킨다

-> 이 경우 나중에 문제가 생길 수 있다. Room을 한 번 사용해보는 학습단계에서는 써도 무방하다.

**2번째 선택지**

-> 비동기 실행을 하면 된다.(아래 사이트 글에서는 코루틴을 사용)





https://todaycode.tistory.com/39
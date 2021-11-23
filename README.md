# databinding
## 데이터 바인딩이란?
```kotlin
// findViewById 쓸때는 이렇게 했었고
textView.text = "안녕" 
 
// 뷰 바인딩 쓸때는 이렇게 했다.
binding.textView.text = "안녕"
```
여태까지 우리는 텍스트 뷰에 문장을 넣기 위해 코드상에서 값을 집어넣는 작업을 해주었다.
```kotlin
<TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@{user.name}" />

```
근데 이걸 xml에 코드를 집어넣어서 해결하는 방법이 있다.

이렇게 하면 자연스레 액티비티에는 로직만을 위한 코드만 남게 되고 뷰와 관련된 작업은 레이아웃 파일에 정의된다.

**데이터와 뷰를 연결하는 작업을 레이아웃에서 처리**하는 기술,

이것을 우리는 데이터 바인딩이라고 부른다.

## 데이터 바인딩 vs 뷰 바인딩

<img src="https://user-images.githubusercontent.com/48902047/142985022-78607cc8-f8ef-4efc-a57a-18c1cf385d81.png"></img>
그림에서 알 수 있듯이, 데이터 바인딩은 뷰 바인딩의 역할도 할 수 있을뿐더러

추가로 동적 UI 콘텐츠 선언, 양방향 데이터 결합도 지원한다.

<img src="https://user-images.githubusercontent.com/48902047/142985113-01f61b8c-84e9-4c77-9c11-c32dd74a2bd0.png"></img>

땡! 데이터 바인딩이 기능은 다양하지만

뷰 바인딩이 상대적으로 간단하며 퍼포먼스 효율이 좋고 용량이 절약된다는 장점이 있다.

실제로 구글 공식문서에서는 단순히 findViewById를 대체하기 위한 거라면, 뷰 바인딩을 사용할 것을 권장하고 있다.

그러니 상황에 맞게 바인딩을 골라 사용하면 된다.

## 사용법

### gradle 추가

```kotlin
// 안드로이드 스튜디오 4.0 이상
android {
    ...
    buildFeatures {
        dataBinding = true
    }
}
```
### 액티비티
```kotlin
class MainActivity : AppCompatActivity() {
 
    private lateinit var binding: ActivityMainBinding
 
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
 
        binding.user = UserProfile("민규", "임")
    }
}
```
데이터 바인딩도 뷰 바인딩과 크게 다를 게 없다.

firstName과 lastName을 저장하는 UserProfile 클래스를 만들어 이름을 저장해주었다.
```kotlin
data class UserProfile(
    var firstName: String = ""
    , var lastName: String = ""
)
```

### 레이아웃
데이터 바인딩을 사용하기 위해서 우리가 기존에 알던 레이아웃 구조를 좀 바꾸어야 한다.

<layout> 태그가 가장 바깥쪽에 위치해있으며 그 안에 <data> 태그와 우리가 사용하던 레이아웃이 들어가는 형태이다.

```kotlin
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
            name="user"
            type="com.example.selfstudy_kotlin.UserProfile" />
    </data>
 
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity">
 
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.firstName}" />
 
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.lastName}" />
    </LinearLayout>
</layout>
```

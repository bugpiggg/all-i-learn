# Data class

- data class 는 컴파일러가 `toString(), hashCode(), equals(), copy()` 메소드를 자동으로 만들어주는 클래스이다 

#### 특징
데이터클래스의 룰
- 다른 클래스가 Data class를 상속 받을 수 없음
- 1개 이상의 프로퍼티를 가져야 함
- abstract, open, sealed, inner 를 붙일 수 없음
- 모든 생성자의 파라미터는 val or var
- 파라미터가 없는 생성자를 활용해야 싶다면, 기본값을 설정해야 함

데이터멤버들의 룰
- `toString(), hashCode(), equals()` 함수들을 data class 내부에서 명시적으로 선언하거나, super class 에서 final 로 해당 함수들을 선언했다면 -> 컴파일러가 자동으로 안 만들어 줌
- `componentN(), copy()` 함수들은 명시적 선언이 허용되지 않음

그 외
- 주생성자에 선언된 파라미터에 대해서만 위 함수들이 생성됨
  - copy() 에서도 age는 복사되지 않음
  ```kotlin
  data class Person(val name: String) {
    var age: Int = 0
  }
  ```


#### Reference
- https://kotlinlang.org/docs/data-classes.html#properties-declared-in-the-class-body

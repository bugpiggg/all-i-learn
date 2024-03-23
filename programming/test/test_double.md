# Test Double

테스트더블이란?  
= 테스팅에서 실제 객체를 대신해서 사용하는 모든 방법을 일컫는 것이다.  
= 실제 객체가 아닌 단순한 객체를 이용하여 테스트 하는 경우 그 객체를 정의하는 방법  

필요한 이유는?  
= 특정 레이어에 대해서만 테스트를 하고 싶지만, 다른 레이어의 동작도 요구되는 상황이라면, 위 방법을 통해 의존성을 끊어 독립적인 테스트를 만들 수 있게 된다


#### 1. Dummy
- 실제 객체는 전달되지만, 사용되지 않는 객체이다. 단순히 인스턴스화 될 수 있는 수준이면 충분하다
- e.g.) 함수 파라미터로는 전달되지만 내부에서 사용되지 않는 경우
- 테스트에 UserDao updateUserHistory() 함수호출이 필요하지만, 호출만 필요한 경우라면 아래와 같이 정의한 dummy 를 활용할 수 있다
  ```kotlin
  interface UserDao {
    fun updateUserHistory(userUid: Int);
  }

  class UserDaoDummy: UserDao {
    override fun updateUserHistory(userUid: Int) {
      // dummy
    }
  }
  ```

#### 2. Fake
- 실제로 동작하는 구현을 가지고 있지만, 프로덕션에는 사용할 수 없는 객체이다
- 동작하게금 간략하게 객체를 구현해놓는 방법이라고 생각하면 된다
- 아래와 같이 가짜 구현을 하는 경우이다 
  ```kotlin
  class FakeUserDao: UserDao {
    private users = mutalbeMapOf<Int,User>()

    override fun getUser(userUid: Int): User? {
      return users[userUid]
    }
    override fun saveUser(user: User): Int {
      if (users[user.uid] == null) {
        users[user.uid] = user
      } 

      return users[user.uid]!!.uid
    }
  }
  ```


#### 3. Stub
- Dummy 데이터가 실제로 동작하도록 만들어둔 객체이다
- 미리 정의된 데이터를 보유하고, 테스트 호출시에 응답하도록 만들어둔다
- 특정 상태를 가정해 하드코딩 해놓은 상태라고 보면 된다

#### 4. Spy
- Stub 의 역할을 하면서 추가적인 정보를 기록하는 방법(상태를 검증하기 위해 사용함)
- 예를 들어, 메서드가 잘 호출 되었는지, 몇 번이나 호출되었는지

#### 5. Mock
- 행위를 검증하기 위해, 가짜 객체를 만들고 테스트하는 방법
- 호출에 대한 기대를 명세하고 내용에 따라 동작하도록 프로그래밍 된 객체

#### Test Double 선택시에

- 어떤 테스트가 필요한지 결정하고, 가능한 최소한의 테스트를 선택해야 함
- 다음과 같은 순서대로 테스트의 범위가 넓어지고 생성이 어려워짐
  - Dummy -> Stub -> Fake -> Spy -> Mock

#### 아직 mock과 stub 의 차이를 모르겠다?

- 스택오버플로우 한 유저가 아래와 같이 정리했다.
  > A stub is a simple fake object. It just makes sure test runs smoothly.  
  > A mock is a smarter stub. You verify your test passes through it.


- 다른 유저는 아래와 같이 정리하기도 했다.
  > Stub - override methods to return hard-coded values, also referred to as state-based.  
  Example: Your test class depends on a method Calculate() taking 5 minutes to complete. Rather than wait for 5 minutes you can replace its real implementation with stub that returns hard-coded values; taking only a small fraction of the time.

  > Mock - very similar to Stub but interaction-based rather than state-based. This means you don't expect from Mock to return some value, but to assume that specific order of method calls are made.  
  Example: You're testing a user registration class. After calling Save, it should call SendConfirmationEmail.

- 구현관점에서 정리하면,
  - Stub 은 상태 검증을 위해 사용하기 때문에, 하드코딩된 값을 리턴하는 부분만 구현하면 됨
  - Mock 은 행위 검증을 위해 사용하기 때문에, 특정 함수들의 호출이 기대됨. 그렇기에 재구현이나 부분구현이 아닌 실제 구현을 활용할 수도 있음? 

## Reference

- https://brunch.co.kr/@tilltue/55
- https://codinghack.tistory.com/92
- https://www.crocus.co.kr/1555
- https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub/3459431#3459431
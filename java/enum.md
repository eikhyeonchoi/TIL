# enum
```
열거타입
클래스이기 때문에 멤버변수 함수를 가질 수 있다
enum 키워드로 생성된 클래스들은 모두 java.lang.Enum을 상속한다

- static method
EnumType[] values()
원소들을 명시한 순새대로 변환해줌

EnumType valueOf(String arg);
문자열로 요소들을 찾을 수 있다
매칭되는 요소가 없다면 IllegalArgumentException 발생

- non-static method
String name()
enum 상수 반환

int ordinal()
상수가 열거된 순서를 반환한다 0부터 시작

int compareTo(E)
자기 자신(this)의 ordinal 값과 파라미터로 받아온 객체의 ordinal의 차이를 반환한다
```

# EnumSet
```
enum 타입을 요소로 하는 Set 구현체

allOf(Class<E> elementType)
모든 enum 상수를 요소로 가지는 EnumSet 생성

noneOf(Class<E> elementType)
빈 요소를 가지는 EnumSet 생성
```
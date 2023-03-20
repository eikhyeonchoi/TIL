# Mockito
```
단위 테스트를 하기 위한 라이브러리
```

## Test Double?
```
테스트시 실제 객체의 역할을 대신 해주는 객체를 의미
```

## Stub
```
로직은 없고 단지 원하는 값을 반환하는 Test Double로 실제로 동작하는 것 "처럼" 만들어 놓은 객체
[인터페이스(기본 클래스)을 구현하며 미리 결과를 준비 -> 테스트 대상인 메서드 등 호출 -> 준비된 결과와 같은 상태가 되었는지 검증]
이런 과정을 거치는 구조 따라서 Stub를 활용한 테스트 방식을 "상태기반 테스트 방식" 이라고 한다.
```

## Mock
```
Mock은 테스트하고자 하는 코드와 맞물려 동작하는 객체들을 대신하여 동작하기 위해 만들어지는 껍데기 객체이다.
```

***

```
@Mock: 가짜 객체를 만들어 반환해주는 어노테이션
@Spy: Stub하지 않은 메소드들은 원본 메소드 그대로 사용하는 어노테이션
@InjectMocks: @Mock 또는 @Spy로 생성된 가짜 객체를 자동으로 주입시켜주는 어노테이션

주의
@Mock객체는 무조건 stubing 해야한다
@Spy는 실제 객체이다
```

```java
// 단위 테스트 예제
@ExtendWith(MockitoExtension.class)
@Transactional
public class TddMockTest {
    @Spy
    @InjectMocks
    UserServiceImpl userService;

    @Mock
    UserRepo userRepo;
    @Mock
    GradeRepo gradeRepo;
    @Mock
    PointRepo pointRepo;

    @Test
    @DisplayName("회원가입 테스트")
    void 회원가입() {
        // make stub
        User mockUser = User.builder().id(22L).email("email").nickname("nickname").pwd("pwd").build();
        Grade mockGrade = Grade.builder().id(1L).pivot(0).name("firstGrade").build();

        // given & stubing
        given(gradeRepo.getFirstGrade()).willReturn(mockGrade);
        given(userRepo.save(any())).willReturn(mockUser);

        // when
        Long index = userService.create(UserDto.Create.builder().build());

        // then
        then(gradeRepo).should(only()).getFirstGrade();
        then(userRepo).should(only()).save(any());
        then(pointRepo).should(only()).save(any());

        assertThat(index).isEqualTo(22L);
    }
}
```
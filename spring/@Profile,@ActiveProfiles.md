# @Profile
```
스프링빈에게 Profile을 구분하여 빈을 로드할 수 있다
@ConditionalOnProperty는 .yml or .properties 값에 따라 빈을 로드한다면
@Profile은 애플리케이션이 동작 시 적용되는 profile에 따라 빈을 로드할 수 있다
```

# @ActiveProfiles
```
테스트 수행 시 어떤 Profile을 사용할 것인지 정해주는 애노테이션
예를들어 로컬에서 테스트하는데 테스트db를 연결하고 싶을경우 .yml or .properties의 profile을 test로 직접 수정하는게 아닌
테스트시 @ActiveProfiles를 test로 설정해주면 test Profile을 사용한다
```
# 에러와 그 이유
```
JPA 관련 Hibernate 에러: object references an unsaved transient instance - save the transient instance before flushing
외래키가 걸려있는 테이블의 object가 없을 경우 발생
```

```
JPA 관련 Hibernate 에러: no session
LAZY 로딩이 불가하다는 뜻 = 트랜잭션이 없다
```
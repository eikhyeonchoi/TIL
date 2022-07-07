# 에러와 그 이유
```
JPA 관련 Hibernate 에러: object references an unsaved transient instance - save the transient instance before flushing
외래키가 걸려있는 테이블의 object가 없을 경우 발생
```

```
JPA 관련 Hibernate 에러: no session
LAZY 로딩이 불가하다는 뜻 = 트랜잭션이 없다
```

```
references an unknown entity

다중 DB 사용 시 다른 데이터베이스에 있는 테이블과 Join하는 경우
return builder
    .dataSource(dataSource())
    .packages("package.path", "package.path", "package.path")
    .persistenceUnit("entityManager")
    .properties(properties)
    .build();

packages에 없는 패키지는 탐색하지 않아서 
@OneToOne or @ManyToOne on ... references an unknown entity ... 에러가 발생한다
패키지를 추가해야줘야함
```

```
JPA 관련 에러
ids for this class must be manually assigned before calling save()
Auto Increment PK일 경우 entity에 GeneratedValue가 설정되어있지 않은 경우 발생
```
# JPA(QueryDSL) query 경험 모음

## Subquery #1
```java
// 멤버의 마지막 접속로그(히스토리)를 가져와야할 때?
// 조건: 멤버-히스토리는 1:N이다
@Override
public Optional<Member> findByIdWithLastHistory(Long id) {
    Member findMember = factory.select(member)
            .from(member)
            .leftJoin(history)
            .on(member.id.eq(history.member.id)
                    .and(history.id.eq(
                                select(history.id.max()).from(history).where(history.member.id.eq(member.id))
                            )
                    )
            )
            .where(member.id.eq(id))
            .fetchOne();

    return Optional.ofNullable(findMember);
}
// 의문: 걍 limit 1쓰면 되는거 아님? -> limit이 안먹힘;
```

## RightJoin & CollectionJoin #2
```java
// member(1) - history(n) 관계일 때 right join은 의미가 있을까 ??

List<Member> fetch = factory.selectFrom(member)
        .rightJoin(history).on(member.id.eq(history.member.id)).fetchJoin() // 1
        // .rightJoin(member.histories, history).fetchJoin().fetch();       // 2
        .fetch();

// 1과 2의 쿼리는 동일함
// OneToMany의 대해 fetchJoin을 했기에 기본적으로 데이터가 원하는 결과보다 뻥튀기 되고 
// 페이징을 같이해버리면 메모리에서 페이징을 해버리기 때문에 절대 하면 안된다
// 1과 2 모두 의미가 없다 -> 쿼리가 의미가 없다
// 어차피 member를 통해 history에 접근하려면 mappedBy 설정된 history에 접근할텐데
// 접근할 때 default_batch_fetch_size 에 설정된 값만큼 in 쿼리가 나가서 데이터를 가져옴

// 결론
// @XToOne  -> 모두 다 fetch로 걸어서 일단 한번에 가져옴(dto 리턴이 아니라면)
// @XToMany -> default_batch_fetch_size or @BatchSize 설정을 통해 in쿼리 호출을 시도해야함

// 테스트의 관한 잡지식
// test시 @BeforeEach를 통해 미리 데이터를 셋업하는경우
// 영속성 컨텍스트에 데이터가 이미 있다면 테스트 도중 원하는 쿼리가 나가지 않을 수 있음
// 따라서 @BeforeEach를 함수에 EntityManager flush, clear를 하는것을 추천
```
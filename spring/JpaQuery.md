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
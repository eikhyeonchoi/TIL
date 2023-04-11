# GroupBy
```java
// Group By 참조 테스트 코드
// 밑에 주석친 건 
// 예를들어 select에 사용한 별칭으로 order by 하는 경우가 있을 때
// .orderBy(dddd) 이렇게 하면된다
public class TempRepoImpl implements TempRepoCustom {
    private JPAQueryFactory queryFactory;

    public TempRepoImpl(EntityManager entityManager) {
        this.queryFactory = new JPAQueryFactory(entityManager);
    }

    @Override
    public List<TempDto.GroupByDateDto> findAllGroupByDate() {
        DateTemplate<LocalDateTime> formatDate =
                Expressions.dateTemplate(LocalDateTime.class,
                        "DATE_FORMAT({0}, {1})", temp.testDate, "%Y-%m-%d");

        // StringPath dddd = Expressions.stringPath("temp.testDate");
        // .groupBy(dddd)

        return queryFactory
                .select(
                        Projections.constructor(
                                TempDto.GroupByDateDto.class,
                                temp.amount.sum(),
                                temp.testDate
                        )
                ).from(temp)
                .groupBy(formatDate)
                .fetch();
    }
}
```
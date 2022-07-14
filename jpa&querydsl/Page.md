# About Page

```java
@Override
public Page<Element> getAllWithPage(Pageable pageable) {
    List<Element> fetch = factory.selectFrom(element).fetch();
    return new PageImpl<>(fetch, pageable, fetch.size());
}

@Override
public Page<Element> getAllWithPage2(Pageable pageable) {
    List<Element> data = factory.select(element)
            .from(element)
            .offset(pageable.getOffset())
            .limit(pageable.getPageSize())
            .fetch();

    JPAQuery<Element> countQuery = factory.select(element).from(element);

    // fetchCount => deprecated 
    // fetchCount() => fetch().size();
    return PageableExecutionUtils.getPage(data, pageable, () -> countQuery.fetch().size());
}

/**
PageableExecutionUtils.getPage는 PageImpl과 같은 역할을 하지만 한가지 기가막힌 점은 마지막 인자로 함수를 전달하는데 내부 작동에 의해서 토탈카운트가 페이지사이즈 보다 적거나,  
위의 코드의 경우 카운트 쿼리 마지막에 fetchCount()가 여기서 관리되고 있다. PageableExecutionUtils.getPage을 사용하면 조금 더 성능 최적화가 된다.  
쿼리하나가 아쉬울 때는 PageImpl 보다는 PageableExecutionUtils.getPage를 사용할 것
*/


// where 조건에 넣어서 사용하면 좋다 "BooleanExpression"
private BooleanExpression ageGoe(Integer ageGoe) {
    return ageGoe != null ? element.column.goe(ageGoe) : null;
}

*fetchCount, fetchResult는 둘다 querydsl 내부에서 count용 쿼리를 만들어서 실행해야 하는데,  
이때 작성한 select 쿼리를 기반으로 count 쿼리를 만들어냅니다. 그런데 이 기능이 select 구문을 단순히 count 처리하는 것으로 바꾸는 정도여서,  
단순한 쿼리에서는 잘 동작하는데, 복잡한 쿼리에서는 잘 동작하지 않습니다. - 김영한*
```
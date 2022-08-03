# JPA

```java
// transantional
// 만약 이런 코드가 한 트랜잭션에 있다면?
Integer boardId = insertBoard(boardDto);
Board board = getBoardById(boardId);

// board의 기본정보는 불러올 수 있지만 join 데이터는 가져오지 못함
// 따라서 update transaction에 propagation Propagation.REQUIRES_NEW 옵션을 줘야함
@Transactional(propagation = Propagation.REQUIRES_NEW, rollbackFor = Exception.class)
public Integer insertBoard(BoardDto boardDto) {
    // code ...
}

@Transactional(readoly=true)
public Board getBoard(Integer boardId) {
    // code ...
}
```
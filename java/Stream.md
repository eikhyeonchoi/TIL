# Tip of Stream
```
List<Object> 에서 객체로 index가 아닌 객체로 get() 사용하기
(index를 모르고 특정 객체의 특정값만 아는 경우 ex. 아이디(PK)값만 아는 경우)

Integer id = 1;
List<Object> list = ...;
Optional<Object> first = list.stream().filter(elem->elem.getSid()==id).findFirst();
if (first.isPresent()) {
    Object object = list.get(list.indexOf(first.get()));

    // write your code ...
}
```
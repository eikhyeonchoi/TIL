# About Join
```java
// cross join이 발생하는 경우 ?

select(
    a.b.name
).from(a)
.join(b).on(b.id.eq(a.b.id));

/**
어떻게 설명해야할지 모르겠지만
a,b 를 조인한 상태에서
a를 통해 b를 참조한다면 (위에서 a.b.name)
암묵적으로 cross join이 발생함

a.b.name -> b.name
이렇게 해줘야 cross join이 붙지 않음
*/
```
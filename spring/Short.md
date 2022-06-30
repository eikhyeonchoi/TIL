# 깨알팁
```
dto로 request를 받을 때 dto에 @AllArgsConstructor 선언해야한다
```

```
jpa repository에서 dto리턴 시 페치조인은 불가하다
```

```
gradle에 spring security가 있을 시 기본 cors 설정외에 cors 설정이 추가된다
```

```
dto들은 inner class로 관리하는게 깔끔하다
```

```
Spring Security 기본 설정 제외하기
MainApplication에서
+ annotation @SpringBootApplication(exclude = {SecurityAutoConfiguration.class, UserDetailsServiceAutoConfiguration.class})
```

```
RestController에서 response를 내릴 때
JSONObject는 serialize가 잘 안됨 HashMap 쓸 것
```
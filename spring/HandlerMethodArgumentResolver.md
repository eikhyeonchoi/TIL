# HandlerMethodArgumentResolver
## 개요
```
Strategy interface for resolving method parameters into argument values in the context of a given request.
API 통신을 위한 Controller에 들어오는 파리미터를 가공하거나 설정을 해주기 위해 사용함
공통 파라미터를 처리할 때 유용
```

## 동작순서
```
1. Client Request
2. Dispatcher Servlet 요청 처리
3. Client Request에 대한 Handler Mapping
    3-1. RequestMapping에 대한 매칭
    3-2. Interceptor 처리
    3-3. Argument Resolver 처리
    3-4. Message Convertor 처리
4. Controller Method Invoke

+ Advanced
Interceptor에서 받은 정보를 HttpServletRequest에 저장한 후 
Argument Resolver에서 받아서 사용해도 좋을 듯
```

## 구현 예시
- 어노테이션 기반으로 공통 파라미터 처리하기
```java
// 어노테이션 작성
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
public @interface CommonParamAnno {
    
}
```

```java
// 공통으로 받을 request dto 작성
@Getter
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class CommonParam {
    private String token;
}
```

```java
// Resolver 작성 및 처리
@Slf4j
@Component
public class CommonParamResolver implements HandlerMethodArgumentResolver {
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.hasParameterAnnotation(CommonParamAnno.class);
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        // write your logic ...
        // 만약 jwt를 위한 Interceptor를 사용한다면
        // resolver에서 다시한번 decrypt 하지 않아도 될 듯 하다
        // return하면 

        return new CommonParam("test_token");
    }
}
```

```java
// configuration에 등록
@Configuration
@RequiredArgsConstructor
public class Config implements WebMvcConfigurer {
    private final CommonParamResolver commonParamResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(commonParamResolver);
    }
}
```

```java
// 실제 사용 예
@RestController
@RequestMapping("/hello")
@Slf4j
public class HelloController {
    @GetMapping
    public ResponseEntity get(@CommonParamAnno CommonParam cp) {
        System.out.println("cp.getToken() = " + cp.getToken());
        return ResponseEntity.ok(200);
    }
}
```


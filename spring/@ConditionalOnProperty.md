# @ConditionalOnProperty
```
Property 조건에 따라 작동하는 Bean(Spring Container에서 관리여부를 property에 따라 다르게함)
Property란 yml, properties에 정의해서 사용하는 값들을 의미
-> 애플리케이션을 개발할 때 구성 속성의 존재와 갑셍 따라 조건부로 일부 Bean을 생성해야 할 때 사용

아래 예제는 build.gradle의 값 -> applcation.properties -> application으로 가져오는 예제이다
```

```java
@ConditionalOnProperty(value = "conditional.test.property", havingValue = "true")
@Component
public class Bbean {
}

@ConditionalOnProperty(value = "conditional.test.property", havingValue = "false")
@Component
public class Abean {
}
```

```
# application.properties
conditional.test.property=${ext.conditionalTestProperty}
```

```
# build.gradle
processResources {
	filesMatching('**/**.properties') {
		expand(project.properties)
	}
}

ext {
	conditionalTestProperty = true
}
```
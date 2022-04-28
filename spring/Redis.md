# Redis
spring boot redis 연동<br/>
@Autowired를 사용해 redisTemplate로 redis 접근 가능

```java
@Configuration
public class RedisConfig {
    @Value("${spring.redis.host}")
    private String redisHost;

    @Value("${spring.redis.port}")
    private Integer redisPort;

    @Value("${spring.redis.password}")
    private String redisPassword;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        RedisStandaloneConfiguration conn = new RedisStandaloneConfiguration();
        conn.setHostName(this.redisHost);
        conn.setPort(this.redisPort);
        conn.setPassword(this.redisPassword);
        return new LettuceConnectionFactory(conn);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory());
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new StringRedisSerializer());
        return template;
    }
}
```

<br/>
<br/>

예시
```java
@Slf4j
@RestController
@RequestMapping("/api/v1")
@RequiredArgsConstructor
public class TestController {
    private final RedisTemplate<String, Object> redisTemplate;

    @GetMapping("/test")
    public TuneResponse test(@RequestParam HashMap<String, String> map) {
        String key = map.get("key");
        String value = map.get("value");

        if (key != null && value != null) {
            ValueOperations<String, Object> vop = redisTemplate.opsForValue();
            vop.set(key, value);
        }

        return new TuneResponse();
    }
}
```
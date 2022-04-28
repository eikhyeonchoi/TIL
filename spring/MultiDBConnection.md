# Multi DB Connection
spring boot에서 여러 DB 연결
LocalContainerEntityManagerFactoryBean 내에 설정하는 package는 엔티티 모델이 위치하는 곳을 지정해야 함<br/>
EnableJpaRepositories 에 지정하는 basePackage는 Repository파일이 있는 경로를 지정해야 함<br/>
Main 이고 따로 필요한 Sub DB는 이름만 다르게 설정하면 됨

```java
@Configuration
@EnableJpaRepositories(
        entityManagerFactoryRef = "mainEntityManagerFactory",
        transactionManagerRef = "mainTransactionManager",
        basePackages = "exam.barbaz.repository"
)
@RequiredArgsConstructor
public class MainDatasourceConfig {
    private final Environment env;
    private final JpaProperties jpaProperties;
    private final HibernateProperties hibernateProperties;

    @Primary
    @Bean
    public DataSource mainDatasource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(env.getProperty("spring.datasource.driver-class-name"));
        dataSource.setUrl(env.getProperty("spring.datasource.url"));
        dataSource.setUsername(env.getProperty("spring.datasource.username"));
        dataSource.setPassword(env.getProperty("spring.datasource.password"));
        return dataSource;
    }

    @Primary
    @Bean
    public LocalContainerEntityManagerFactoryBean mainEntityManagerFactory(EntityManagerFactoryBuilder builder) {
        Map<String, Object> properties = hibernateProperties.determineHibernateProperties(jpaProperties.getProperties(), new HibernateSettings());
        return builder
                .dataSource(mainDatasource())
                .packages("exam.barbaz.entity")
                // persistenceUnit은 추후 EntityManager에서 사용할 고유이름
                .persistenceUnit("mainEntityManager")
                .properties(properties)
                .build();
    }

    @Primary
    @Bean
    public PlatformTransactionManager mainTransactionManager(@Qualifier(value = "mainEntityManagerFactory") EntityManagerFactory factory) {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(factory);
        return transactionManager;
    }
}
```
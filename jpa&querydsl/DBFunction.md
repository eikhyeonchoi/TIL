# About DBFunction

```java
/**
QueryDsl에서 Mysql Function 사용하기

spring.jpa.database-platform=net.addtune.partner.config.MysqlFunctionConfig
properties에 입력해야줘야 함
*/
public class MysqlFunctionConfig extends MySQL57Dialect {
    public MysqlFunctionConfig() {
        super();
        this.registerFunction("GROUP_CONCAT", new StandardSQLFunction("GROUP_CONCAT", new StringType()));
    }
}

// ...

/**
example. 
ExpressionUtils.template(String.class, "GROUP_CONCAT({0})", syscodeManager.sid)
*/
```
# log4j2
```
성능 좋은 log4j2로 로깅해보자
```

## 1. dependency 추가
```
implementation 'org.springframework.boot:spring-boot-starter-log4j2'
implementation 'com.fasterxml.jackson.dataformat:jackson-dataformat-yaml'

// spring boot starter에는 기본 logger가 있기에 제외시킨다
configurations {
	all {
		exclude group: 'org.springframework.boot', module: 'spring-boot-starter-logging'
	}
}
```

## 2. Config 설정
```
// 로그파일을 지정해준다
logging.config=classpath:log4j2.yml
```

## 3. log 파일 작성
```
# resources/log4j2.yml
# 작성 예시
Configuration:
  name: Default # just name
  status: info # log level: error|warn|info|debug|trace(로그 레벨 순서 내림차 순)

  Properties: # property 명시
    Property:
      name: log-path
      value: "logs"

  Appenders: # 로깅 전략 명시
    Console:
      name: Console_Appender # just name
      target: SYSTEM_OUT # 표준입출력임을 명시
      PatternLayout:
        pattern: "%style{%d{yyyy-MM-dd HH:mm:ss.SSS}}{cyan} %highlight{[%-5p]}{FATAL=bg_red, ERROR=red, INFO=green, DEBUG=blue, TRACE=bg_yellow} [%C] %style{[%t]}{yellow}- %m%n"
    File:
      name: File_Appender
      fileName: ${log-path}/logfile.log # just name
      append: false # 파일을 추가로 이어서 쓸건지 false면 새 파일이 생성됨
      PatternLayout:
        pattern: "%style{%d{yyyy-MM-dd HH:mm:ss.SSS}}{cyan} %highlight{[%-5p]}{FATAL=bg_red, ERROR=red, INFO=green, DEBUG=blue, TRACE=bg_yellow} [%C] %style{[%t]}{yellow}- %m%n"
    RollingFile:
      - name: RollingFile_Appender # just name
        fileName: ${log-path}/rollingfile.log # 저장할 파일 이름
        filePattern: "${log-path}/archive/rollingfile.log_%d{yyyy-MM-dd}-%i.gz" # 저장할 파일 경로
        PatternLayout:
          pattern: "%style{%d{yyyy-MM-dd HH:mm:ss.SSS}}{cyan} %highlight{[%-5p]}{FATAL=bg_red, ERROR=red, INFO=green, DEBUG=blue, TRACE=bg_yellow} [%C] %style{[%t]}{yellow}- %m%n"
        Policies: # file rollingUp의 기준을 지정
          TimeBasedTriggeringPolicy: # time에 따른 trigger
            Interval: 1
            modulate: true
        #          SizeBasedTriggeringPolicy: # size에 따른 trigger
        #            size: "10 MB"
        DefaultRollOverStrategy: # rolling file을 over(삭제)하는 기준을 지정함
          max: 10 # 동시간대에 최대 10개까지 rollingFiles가 생성될 수 있도록 지정
          Delete:
            basePath: "${log-path}/archive" # 아카이브화할 로그 파일 위치를 지정함
            maxDepth: "1"
            IfLastModified: # 수정된지 14일이 지난 파일은 삭제
              age: "P14D"
            IfAccumulatedFileCount: # 압축파일을 저장하는 디렉토리에 존재할 수 있는 파일 개수(초과시 오래된 파일부터 삭제함)
              exceeds: 140
  Loggers:
    Root:
      level: info
      AppenderRef:
        - ref: Console_Appender
    Logger: # 로거 적용
      - name: com.oursurvey # 패키지 경로 적어줌 하위패키지 모두 로깅됨
        additivity: false # false로 설정시 로그가 중복되어 찍히는 것을 방지할 수 있다
        level: debug
        AppenderRef:
          - ref: Console_Appender
          - ref: File_Appender
```

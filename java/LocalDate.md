# Tip of LocalDate

```java
LocalDate date = new LocalDate.now();
LocalDate date2 = new LocalDate.of(2022, 12, 27);

// 월의 첫날
date.withDayOfMonth(1);

// 월의 말일
date.withDayOfMonth(date.lengthOfMonth());

// 비교
date.isBefore(date2);
date.isAfter(date2);

// 차이 계산
Period period = Period.between(date, date2);
period.getYears();
period.getMonths();
period.getDays(); 

// 차이 계산2
ChronoUnit.DAYS.between(startDate, endDate); 
// ChronoUnit.Years
// ChronoUnit.MONTHS
// ChronoUnit.WEEKS
// ChronoUnit.DAYS
// ChronoUnit.HOURS
// ChronoUnit.SECONDS
// ChronoUnit.MILLIS
// ChronoUnit.NANOS

// 포맷
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
String nowString = now.format(date);   
```
# 날짜와 시간
## Calendar와 Date
 Date는 날짜와 시간을 다룰 목적으로 JDK1.0부터 제공되어온 클래스다.   
 하지만 기능이 부족해 Calendar라는 새로운 클래스를 JDK1.1부터 제공했고 JDK1.8부터 java.time패키지로 기존의 단점들을 개선한 새로운 클래스들이 추가했다.
 
 #### Calendar와 GregorianCalendar
  Calendar는 추상클래스이기 때문에 직접 객체를 생성할 수 없고, 메서드를 통해서 완전히 구현된 클래스의 인스턴스를 얻어야 한다.
  ```java
 Calendar cal = Calendar.getInstance();
 ```
 > getInstance()메서드는 추상클래스를 구현한 클래스의 인스턴스를 반환한다.
    
Calendar를 상속받아 완전히 구현한 클래스로는 GregorianCalendar와 BuddhistCalendar가 있는데   
getInstance()는 시스템의 국가와 지역설정을 확인해서 태국인 경우에는 BuddhistCalendar의 인스턴스를 반환하고, 그 외에는 GregorianCalendar의 인스턴스를 반환한다.
> 인스턴스를 직접 생성해서 사용하지 않고 이처럼 메서드를 통해서 인스턴스를 반환받게 하는 이유는    
> 최소한의 변경으로 프로그램이 동작할 수 있도록 하기 위한 것이다.

```java
public Static void main(String[] args) {
  Calendar cal = new GregorianCalendar()  ...
// 이와 같이 특정 인스턴스를 생성하도록 프로그램이 작성되어 있다면,
// 다른 종류의 인스턴스를 필요로 하는 경우에 MyApplication을 변경해야한다.
Calendar cal = Calendar.getInstance();  
//하지만 이와 같이 메서드를 통해 인스턴스를 얻어오면 MyApplication을 변경하지 않아도 된다.
```

#### Date와 Calendar간의 변환
 ```java
 //1. Calendar를 Date로 변환
 Calendar cal = Calendar.getInstance();
 Data d = new Date(cal.getTImeInMillis());
 
 //2. Date를 Calenddar로 변환
 Date d = new Date();
 Calendar cal = Calendar.getInstance();
 cal.setTime(d);
 ```
 
 #### Calendar 메서드
 ```java
		Calendar cal = Calendar.getInstance();
		
System.out.println(cal.get(Calendar.YEAR));                       //이 해의 년도
System.out.println(cal.get(Calendar.MONTH));                      //월(0~11)
System.out.println(cal.get(Calendar.WEEK_OF_YEAR));               //이 해의 몇 째 주
System.out.println(cal.get(Calendar.WEEK_OF_MONTH));              //이 달의 몇 째 주
System.out.println(cal.get(Calendar.DATE));                       //이 달의 몇 일
System.out.println(cal.get(Calendar.DAY_OF_MONTH));               //이 달의 몇 일
System.out.println(cal.get(Calendar.DAY_OF_YEAR));                //이 해의 몇 일
System.out.println(cal.get(Calendar.DAY_OF_WEEK));                //요일(1~7) 1:일요일
System.out.println(cal.get(Calendar.DAY_OF_WEEK_IN_MONTH));       //이 달의 몇 번째 요일
System.out.println(cal.get(Calendar.AM_PM));                      //오전_오후(0_1)
System.out.println(cal.get(Calendar.HOUR));                       //시간(0~11)
System.out.println(cal.get(Calendar.HOUR_OF_DAY));                //시간(0~23)
System.out.println(cal.get(Calendar.MINUTE));                     //분(0~59)
System.out.println(cal.get(Calendar.SECOND));                     //초(0~59)
System.out.println(cal.get(Calendar.MILLISECOND));                //1000분의 1초(0~999)
System.out.println(cal.get(Calendar.ZONE_OFFSET)/(60*60*1000));   //TimeZone(-12~12)
System.out.println(cal.getActualMaximum(Calendar.DATE));          //이 달의 마지막 날

Calendar cal1 = Calendar.getInstance();
cal1.set(2015, 7, 15); // 날짜를 2015년 8월 15일로 설정한다.

Calendar cal2 = Calendar.getInstance();
cal2.set(Calendar.HOUR_OF_DAY, 10);     
cal2.set(Calendar.MINUTE, 20);
cal2.set(Calendar.SECOND, 30);      // 10시 20분 30초로 시간 설정

long difference = (cal2.getTimeMillis() - cal1.getTimeMillis())/1000;      //두 날짜간의 차이(초).

final int[] TIME_UNIT = {3600, 60, 1};
final String[] TIME_UNIT_NAME = {"시간", "분", "초"};
String tmp = "";                            // "x시간 y분 z초"로 계산되어 저장된다.

for(int i = 0; i<TIME_UNIT.length; i++) {
  tmp += difference/TIME_UNIT[i] + TIME_UNIT_NAME[i];
  difference %= TIME_UNIT[i]; }
  
  cal1.add(Calendar.DATE, 1);       //일에 1 더한다.
  cal1.add(Calendar.DATE, -2);      //일에 -2를 더한다.(2 뺀다)
  cal.roll(Calendar.DATE, 30);      //일에 30을 더하지만 다른 필드에 영향을 끼치지 않는다.(월은 그대로)
  cal.roll(Calendar.MONTH, 1);      //일이 말일일 경우 예외로 일 필드에 영향을 끼친다.
```

# 형식화 클래스
 이 클래스는 java.text패키지에 포함되어 있으며 숫자, 날짜, 텍스트 데이터를 일정한 형식에 맞게     
 표현할 수 있는 방법을 객체지향적으로 설계하여 표준화하였다.     
 형식화 클래스는 형식화에 사용될 패턴을 정의하는데, 데이터를 정의된 패턴에 맞춰 형식화할 수 있을 뿐만 아니라      
 역으로 형식화된 데이터에서 원래의 데이터를 얻어낼 수도 있다.   
> 즉, 형식화된 데이터의 패턴만 정의해주면 복잡한 문자열에서도 쉽게 원하는 값을 얻어낼 수 있다.

## DecimalFormat
> DecimalFormat을 이요하면 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현할 수 있으며,   
> 반대로 일정한 형식의 텍스트 데이터를 숫자로 쉽게 변환하는 것도 가능하다.
```java
DecimalFormat df = new DecimalFormat("##,###.##");
DecimalFormat df2 = new DecimalFormat("#.###E0");

try {
  Number num = df.parse("1,234,567.89");
  
  double d = num.doubleValue();
  } catch(Exception e) {...}
  ```
  > parse메서드를 이용하면 기호와 문자가 포함된 문자열을 숫자로 변환할 수 있다.     
  > parse(String source)는 DecimalFormat의 조상인 NumberFormat에 정의된 메서드이다.     
 
 ## SimpleDateFormat
 날짜 데이터를 원하는 형태로 다양하게 출력하는 메서드이다.
 > 원하는 출력형식의 패턴을 작성하여 SimpleDateFormat인스턴스를 생성한 다음,    
 > 출력하고자 하는 Date인스턴스를 가지고 format(Date d)를 호출하면 출력형식에 맞게 변환된 문자열을 얻게 된다.    
 > 여러가지 기호가 있으므로 검색해보자.
```java
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");

String result = df.format(today);
```
## ChoiceFormat
 ChoiceFormat은 특정 범위에 속하는 값을 문자열로 변환해준다.     
```java
double[] limits = {60, 70, 80, 90};
//String pattern = "60#D|70#c|80#B|90#A";	//limit#value의 형태로 사용된다.
//String pattern = "59<D|69<C|79<B|89<A";	//#은 경계값을 범위에 포함시키지만, <는 포함시키지 않는다.
String[] grades = {"D", "C", "B", "A"};

int[] scores = {100, 95, 88, 70, 52, 60, 70};

ChoiceFormat form = new ChoiceFormat(limits, grades};
//ChooiceFormat form = new ChoiceFormat(pattern);	//패턴을 이용
for(int i =0; i<scores.length; i++) {
	System.out.println(scores[i] + ":" +form.format(scores[i]));
```
> 두 개의 배열이 사용되었는데 하나limits는 범위의 경계값을 저장하는데 사용되었고,   
> 또 하나grades는 범위에 포함된 값을 치환할 문자열을 저장하는데 사용되었다.     
> 경계값은 double형으로 반드시 모두 오름차순으로 정렬되어 있어야 하며,   
> 치환될 문자열의 개수는 경계값에 의해 정의된 범위의 개수와 일치해야한다. 그렇지않으면 IllegalArgumentException이 발생한다.

## MessageFormat
 MessageFormat은 데이터를 정해진 양식에 맞게 출력할 수 있도록 도와준다.   
 데이터가 들어갈 자리를 마련해 놓은 양식을 미리 작성하고    
 프로그램을 이용해서 다수의 데이터를 같은 양식으로 출력할 때 사용하면 좋다.
 ```java
 String msg = "Name: {0} \nTel: {1} \nAge: {2} \nBirthday: {3}";
 
 Object[] arguments = {"이자바", "02-123-1234", "27", "07-09"};
 
 String result = MessageFormat.format(msg, arguments);		//{index}에 맞는 위치로 들어간다.
 //홀따옴표'가 escape문자로 사용되기 때문에 문자열 msg에서 홀따옴표를 사용하기 위해서는 두 번 연속으로 사용해야한다.
 ```
 
 #java.time패키지
 java.time - 날짜와 시간을 다루는데 필요한 핵심 클래스들을 제공.
 java.time.chrono - 표준ISO이 아닌 달력 시스템을 위한 클래스들을 제공.
 java.time.format - 날짜와 시간을 파싱하고, 형식화하기 위한 클래스들을 제공.
 java.time.temporal - 날짜와 시간의 필드field와 단위unit를 위한 클래스들을 제공.
 java.time.zone - 시간대time-zone와 관련된 클래스들을 제공.
 > 위 클래스들은 String클래스와 같이 불변이다.   
 > 멀티 쓰레드 환경에서는 동시에 여러 쓰레드가 같은 객체에 접근할 수 있기 때문에,   
 > 변경 가능한 객체는 데이터가 잘못될 가능성이 있으며, 이를 쓰레드에 안전하지 않다고 한다.

## java.time 패키지의 핵심 클래스
날짜와 시간을 하나로 표현하는 Calendar클래스와 달리,    
java.time패키지에서는 날짜와 시간을 별도의 클래스로 분리해 놓았다.    
> 시간은 LocalTime클래스, 날짜는 LocalDate클래스를, 모두 필요할 때는 LocalDateTime클래스를 이용한다.     
> 여기에 시간대까지 다뤄야 한다면 ZonedDateTime클래스를 사용한다.

#### Priod와 Duration
> period는 두 날짜간의 차이를 표현하고, Duration은 시간의 차이를 표현한다.

#### 객체 생성하기 - now(), of()
```java
LocalDate date = LocalDate.now();	//2022-02-18
LocalTime time = LocalTime.now();	//21:54:01.875
LocalDateTime dateTime = LocalDateTime.now();	//2022-02-18T21:54:01.875
ZonedDateTime dateTimeInKr = ZonedDateTime.now();	//2022-02-18T21:54:01.875+09:00[Asia/Seoul]

LocalDate date = LocalDate.of(2022, 02, 18);	//2022년 11월 23일
LocalTime time = LocalTime.of(23, 59, 59);	//23시 59분 59초

LocalDateTime dateTime = LocalDateTime.of(date, time);
ZonedDateTime zDateTime = ZonedDateTime.of(dateTime, ZonedId.of("Asia/Seoul");
```

#### Temporal과 TemporalAmount
날짜와 시간을 표현하기 위한 클래스들은 모두 Temporal, TemporalAccessor, TemporalAdjuster 인터페이스를 구현했고,     
Duration과 Period는 TemporalAmount인터페이스를 구현했다.

#### TemporalUnit과 TemporalField
 날짜와 시간의 단위를 정의해 놓은 것이 TemporalUnit인터페이스이고, 이 인터페이스를 구현한 것이 열거형 ChronoUnit이다.    
 TemporalField는 년, 월, 일 등 날짜와 시간의 필드를 정의해 놓은 것으로 열거형 ChronoField가 이 인터페이스를 구현했다.
 ```java
 LocalTime now = LocalTime.now();	//현재시간
 int minute = now.getMinute(); =now.get(ChronoField.MINUTE_OF_HOUR); //분만 뽑아낸다. 두 문장 동일.
 
 LocalDate now = LocalDate.now();
 LcalDate tomorrow = today.plusDays(1); = today.plus(1, ChronoUnit.DAYS);	//오늘에 1일을 더한다.
 ```
 
 ## LocalDate와 LocalTime
 객체를 생성하는 방법은 현재의 날짜와 시간을 LocalDate와 LocalTime으로 각각 반환하는 now()와   
 지정된 날짜와 시간으로 LocalDate와 LocalTime객체를 생성하는 of()가 있다.   
 둘 다 static메서드이다.
 ```java
 LocalDate birthday = LocalDate.of(1993, 09, 20);	//1993년 9월 20일
 LocalTime birthTime = LocalTime.of(04, 30, 00);	//4시 30분 00초
 
 static LocalDate of(int year, Month month, int dayOfMoneth)
 static LocalDate of(int year, int month, int dayOfMoneth)
 
 static LocalTime of(int hour, int min, int sec, int nanoOfSecond)
 static LocalTime of(int hour, int min, int sec)
 static LocalTime of(int hour, int min)
 
 LocalDate birthDate = LocalDate.ofYearDay(1999, 365);	//1999년의 365번째 날, 1999년 12월 31일
 LocalTime birthTime = LocalTime.ofSecondDay(86399);	//23시 59분 59초
 
 LocalDate birthDate = LocalDate.parse("1999-12-31");	//1999년 12월 31일
 LocalTime birthTime = LocalTime.parse("23:59:59");	//23시 59분 59초
```
> parse()를 이용하면 문자열을 날짜와 시간으로 변환할 수 있다.

#### 특정 필드의 값 가져오기 - get(), getXXX()
```java

LocalDate date = LocalDate.now();
date.getYear()				//년도2022
date.getMonthValue()			//월 2
date.getMonth()				//월FEBRUARY	getMonth().getValue() = 12
date.getDayOfMonth()			//일19
date.getDayOfYear()			//같은 해의 1월 1일부터 몇 번째 일 50
date.getDayOfWeek()			//요일 SATURDAY  getDayOfWeek().getValue()=6
date.lengthOfMonth()			//같은 달의 총 일수 28
date.lengthOfYear()			//같은 해의 총 일수365
date.isLeapYear()			//윤년여부 확인 false

LocalTime time = LocalTime.now();
time.getHour()				//시 0
time.getMinute()			//분 3
time.getSecond()			//초 59
time.getNano()				//나노초 0
```
> Calendar와 달리 월의 범위가 1~12이고, 요일은 월요일이 1 일요일이 7 이다.

#### 필드의 값 변경하기 - with(), plus(), minus()
 날짜와 시간에서 특정 필드 값을 변경하려면, 다음과 같이 with로 시작하는 메서드를 사용하면 된다.
 ```java
 LocalDate withYear(int year)
 LocalDate withMonth(int month)
 LocalDate withDayOfMonth(int dayOfMonth)
 LocalDate withDayOfYear(int dayOfYear)
 
 LocalTime withHour(int hour)
 LocalTime withMinute(int minute)
 LocalTime withSecond(int second)
 LocalTime withNano(int nanoOfSecond)
 
 date = date.withYear(2000)	//년도를 2000년으로 변경
 //필드를 변경하는 메서드들은 항상 새로운 객체를 반환하므로
 // 이와 같이 대입 연산자를 같이 사용해야한다.
 
 LocalTime time = LocalTime.of(12, 34, 56);	//12시 34분 56초
 time = time.truncatedTo(ChronoUnit.HOURS);	//시hour보다 작은 단위를 0으로     12시 00분 00초
 ```
 > LocalDate에는 truncatedTo()가 없는데 이는 년, 월, 일이 0이 될 수 없기 때문이다.
 
 #### 날짜와 시간의 비교 - isAfter(), isBefore(), isEqual()
 ```java
  int result = date1.compareTo(date2);		//같으면 0, date1이 이전이면 -1, 이 후면 1
  ```
  > LocalTime과 LocalDate 모두 compareTo()가 오버라이딩되어있다.

```java
LocalDate date1 = LocalDate.of(1999, 12, 31);
JapaneseDate date2 = JapaneseDate.of(1999, 12, 31);

date1.equals(date2)	//false YEAR_OF_ERA 가 일본은 다름
date1.isEquals(date2)	//true

boolean isAfter(ChronoLocalDate other)
boolean isBefore(ChronoLocalDate other)
boolean isEqual(ChronoLocalDate other)		//LocalDate에만 있음.
```

#### Instant
>Instant는 에포크 타임(EPOCH TIME, 1970-01-01 00:00:00 UTC)부터 경과한 시간을 나노초 단위로 표현한다.    

```java
Instant now = Instant.now();
Instant now2 = Instant.ofEpochSecond(now.getEpochSecond());
Instant now3 = Instant.ofEpochSecond(now.getEpochSecond(), now.getNano());
```
> Instant를 생성할 땐 위와 같이 now()와 ofEpochSecond()를 사용한다.
```java
long epochSec = now.getEpochSecond();
int nano = now.getNano();
```
> 필드에 저장된 값을 가져올 때는 이와 같이 한다.     

```java
static Date from(Instant instant)		//Instant에서 Date로
Instant toInstant()				//Date에서 Instant로
```
> Date와 Instant의 변환 방법이다.

## LocalDateTime과 ZonedDateTime
> LocalDate와 LocalTime을 합친것이 LocalDateTime이고, 이에 시간대time zone을 추가한 것이 ZonedDateTime 이다.

```java
LocalDate date = LocalDate.of(2022, 02, 21);
LocalTime time = LocalTime.of(12,34,56);

LocalDateTime dt = LocalDateTime.of(date, time);
LocalDateTime dt2 = date.atTime(time);
LocalDateTime dt3 = time.atDate(date);
LocalDateTime dt4 = date.atTime(12, 34, 56);
LocalDateTime dt5 = time.atDate(LocalDate.of(2022, 02, 21));
LocalDateTime dt6 = date.atStartOfDay();	//dt6 = date.atTime(0,0,0);
```
> LocalDate와 LocalTime을 합쳐서 LocalDateTime을 만드는 방법이다.

#### LocalDateTime의 변환
```java
LocalDateTime dt = LocalDateTime.of(2022, 02, 21, 12, 34, 56);
LocalDate date = dt.toLocalDate();					//LocalDateTime- > LocalDate
LocalTime time = dt.toLocalTime();					//LocalDateTime -> LocalTime
```

#### LocalDateTime으로 ZonedDateTime 만들기
> LocalDateTime에 시간대time-zone을 추가하면, ZonedDateTime이 된다.   
> LocalDateTime에 atZone()으로 시간대 정보를 추가하면, ZonedDateTime을 얻을 수 있다.
```java
ZoneId zid = ZoneId.of("Asia/Seoul");
ZonedDateTime zdt = dateTIme.atZone(zid);		// 2022-02-21T12:34:56.451+09:00[Asia/Seoul]

ZonedDateTime zdt = LocalDate.now().atStartOfDay(zid);	// 2022-02-21T00:00+09:00[Asia/Seoul]

ZoneId = nyId = ZoneId.of("America/New_York");
ZonedDateTIme nyTime = ZonedDateTime.now().withZoneSameInstant(nyId);
```

#### ZoneOffset
> UTC에서 얼마나 떨어져 있는지를 ZoneOffset으로 표현한다.
```java
ZoneOffset krOffset = ZonedDateTime.now().getOffset();
int krOffsetInsec = krOffset.gt(ChronoField.OFFSET_SECONDS);	//32400초
```

#### OffsetDateTime
> ZonedDateTime은 ZoneId로 구역을 표시하는데, ZoneId가 아닌 ZoneOffset을 사용하는 것이 OffsetDateTime이다.     
> ZoneId는 일광절약시간처럼 시간대와 관련된 규칙들을 포함하고 있는데, ZoneOffset은 시간대를 시간의 차이로만 구분한다.

```java
ZonedDateTime zdt = ZonedDateTime.of(date, time, zid);
OffsetDateTime odt = OffsetDateTime.of(date, time, krOffset);

OffsetDateTime odt = zdt.toOffsetDateTime();			//ZonedDateTime -> OffsetDateTime
```

#### ZonedDateTime의 변환
```java
LocalDate date = LocalDate.of(2020, 02, 21);
LocalTime time = LocalTime.of(12, 34, 56);
		
LocalDateTime dt = LocalDateTime.of(date, time);				//2020-02-21T12:34:56
		
ZoneId zid = ZoneId.of("Asia/Seoul");						//"Asia/Seoul"
ZonedDateTime zdt = dt.atZone(zid);						//2020-02-21T12:34:56+09:00[Asia/Seoul]
//String strZid = zdt.getZone().getId();					//Asia/Seoul
		
ZonedDateTime seoulTime = ZonedDateTime.now();					//2022-02-21T21:14:13.592525800+09:00[Asia/Seoul]
ZoneId nyId = ZoneId.of("America/New_York");
ZonedDateTime nyTime = ZonedDateTime.now().withZoneSameInstant(nyId);		//2022-02-21T07:14:13.594521600-05:00[America/New_York]
		
OffsetDateTime odt = zdt.toOffsetDateTime();					//2020-02-21T12:34:56+09:00
```
## TemporalAdjusters

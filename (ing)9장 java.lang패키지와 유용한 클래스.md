#### java.lang패키지는 자바프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다.
#### 그렇기 때문에 import문 없이 사용할 수 있다.

# Object 클래스
```java
protected Object clone()
```
> 객체 자산의 복사본을 반환한다.


```java
public boolean equals(Object obj)
```
> 객체 자신과 객체 obj가 같은 객체인지 알려준다.(같으면 true)


```java
protected void finalize()
```
> 객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출된다
> 이 때 수행되어야하는 코드가 있을 때 오버라이딩한다.(거의 사용안함)


```java
public class getClass()
```
> 객체 자신의 클래스 정보를 담고 있는 Class인스턴스를 반환한다.


```java
public int hashCode()
```
> 객체 자신의 해시코드를 반환한다.

```java
public String toString ()
```
> 객체 자신의 정보를 문자열로 반환한다.

```java
public void notify ()
```
> 객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.

```java
public notifyAll()
```
> 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.

```java
public void wait()
public void wait(long timeout)
public void wait(long timeout, int nanos)
```
> 다른 쓰레드가 nofity()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간(timeout, nanos)동안 기다리게 한다.
--------------------------------
## equals(Object obj)
```java
public boolean equals(Object obj) {
  return (this == obj);
```
> 매개변수로 객체의 참조변수를 받아서 비교하여 그 결과를 boolean값으로 알려준다.   
> 두 객체의 같고 다름을 참조변수의 값(주소)으로 판단한다.   
> 그렇기 때문에 서로 다른 두 객체를 equals메서드로 비교하면 항상 false를 결과로 얻게 된다.   

```java
public boolean equals(Object obj){
  if(obj != null && obj instanceof Person){
    return id == ((person)obj).id;    //obj가 Object 타입이므로 형변환을 했다.
   } else 
      return false;     // 타입이 다르다면 값을 비교할 필요도 없다.
 }
```
> equals 메서드가 Person인스턴스의 주소값이 아닌 멤버변수 id의 값을 비교하도록 하기위해 equals메서드를 오버라이딩했다.

## hashCode()
> 데이터관리기법 중 하나인 해싱기법에 사용되는 해시함수를 구현한 것이다.   
> 다량의 데이터를 저장하고 검색하는데 유용하다.   
> 찾고자하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드를 반환한다.   
> 64 bit JVM에서는 8byte 주소값으로 해시코드(4byte)를 만들기 때문에 해시코드가 중복될 수 있어 적절히 오버라이딩해야 한다.   
> String 클래스는 문자열의 내용이 같으면, 동일한 해시코드를 반환하도록 오버라이딩되어 있다.   

```java
System.identityHashCode(Object x) 
```
> 반면에 System.identityHashCode(Object x) 는 Object클래스의 hashCode메서드처럼    
> 객체의 주소값으로 해시코드를 생성하기 때문에 모든 객체에 대해 항상 다른 해시코드값을 반환한다.     
> 호출 결과는 실행할 때 마다 바뀔 수 있다.    

## toString()
```java
public String toString(){
  return getClass().getName() + "@" + Integer.toHexString(hashCode());
```
> 클래스를 작성할 때 toString()을 오버라이딩하지 않는다면, 위와 같은 내용이 그대로 사용된다.

```java
public String toString(){
  return "kind : " + kind + "number : " + number;
```
> Object클래스에서 toString()의 접근 제어자가 public이므로   
> 이를 오버라이딩하는 클래스에서 toString()의 접근 제어자를 public으로 할 수 밖에 없다.   

## clone()
> 자신을 복제하여 새로운 인스턴스를 생성하는 메서드이다.   
> Object클래스에 정의된 clone()은 단순히 인스턴스변수의 값만 복사하기 때문에    
> 참조 타입의 인스턴스 변수가 있는 클래슨는 완전한 인스턴스 복제가 이루어지지 않는다.   
> 예를 들어 배열의 경우, 복제된 인스턴스도 같은 배열의 주소를 갖게 되므로   
> 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미치게 된다.    
> 이런 경우 clone메서드를 오버라이딩해서 새로운 배열을 생성하고 배열의 내용을 복사한다.    


```java
class Point implements Cloneable {      // Cloneable인터페이스를 구현한 클래스에서만 clone()을 호출할 수 있다.
  public Object clone(){
    Object obj = null;
    try{                          // clone()은 반드시 예외처리를 해주어야 한다.
      obj = super.clone();        // 조상클래스의 clone()을 호출.
    } catch(CloneNotSupportedException e) { }
     return obj;
  }
}

Point original = new Point(3, 5);
Point copy = (Point)original.clone();
```
> clone()을 사용하려면, 먼저 복제할 클래스가 Cloneable인터페이스를 구현해야하고,
> clone()을 오버라이딩하면서 접근 제어자를 protected에서 public으로 변경한다.
> 그래야만 상속관계가 없는 다른 클래스에서 clone()을 호출할 수 있다.

```java
public class Object {
  protected native Object clone() throws CloneNotSupportedException;
}
```
## 공변 반환타입
> 오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용한다.

```java
  public Point clone(){
    Object obj = null;
    try{                          
      obj = super.clone();        
    } catch(CloneNotSupportedException e) { }
     return (Point)obj;
  }

Point copy = original.clone();
```
> 이처럼 공변 반환타입을 사용하면, 조상의 타입이 아닌   
> 실제로 반환되는 자손 객체의 타입으로 반환할 수 있어서 번거로운 형변환이 줄어든다.

### 얕은 복사와 깊은 복사
> 얕은 복사shallow copy : 원본과 복제본이 같은 객체를 공유하는 복제.   
> 원본을 변경하면 복사본도 영향을 받는다.      
> 깊은 복사deep copy : 원본이 참조하고 있는 객체까지 복제한다.    

```java
public Circle shallowCopy(){
  Object obj = null;
  try {
    obj = super.clone();
  } catch(CloneNotSupportedException e) { }
  return (Circle)obj;
}

public Circle deepCopy(){
  object obj = null;
  try{
    obj = super.clone();
  } catch (CloneNotSupportedException e) { }
  
   Circle c = (Circle)obj;
   c.p = new Point(this.p.x, this.p.y);
  
   return c;
```

## getclass()
> 자신이 속한 클래스의 Class객체를 반환하는 메서드이다.    
> Class객체는 이름이 'Class'인 클래스의 객체이다.    

```java
public final class Class implements ... { }
```
### Class 객체
> Class객체는 클래스의 모든 정보를 담고 있으며, 클래스 당 1개만 존재한다.    
> 클래스 파일이 '클래스 로더Class Loader'에 의해서 메모리에 올라갈 때, 자동으로 생성된다.   
> 클래스 로더는 실행 시에 필요한 클래스를 동적으로 메모리에 로드, 변환하는 역할을 한다.   
> 먼저 기존에 생성된 클래스의 객체가 메모리에 존재하는지 확인하고,   
> 있으면 객체의 참조를 반환하고 없으면 클래스 패스Classpath에 지정된 경로를 따라서 클래스 파일을 찾는다.     
> 못 찾으면 ClassNotFoundException이 발생하고, 찾으면 해당 클래스 파일을 읽어서 Class객체로 변환한다.     
> 그러니까 파일 형태로 저장되어 있는 클래스를 읽어서 Class클래스에 정의된 형식으로 변환하는 것이다.    
> 즉, 클래스 파일을 읽어서 사용하기 편한 형태로 저장해 놓은 것이 클래스 객체이다.    

#### class 객체를 얻는 방법
```java
Class cObj = new Card().getClass();   //생성된 객체로 부터 얻는 방법.
Class cObj = Card.class;              //클래스 리터럴(*.class)로 부터 얻는 방법.
Class cObj = Class.forName("Card");   //클래스 이름으로 부터 얻는 방법.
```
> forName()은 특정 클래스 파일, 예를 들어 데이터베이스 드라이버를 메모리에 올릴 때 주로 사용한다.       
> Class객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등, 클래스에 대한 모든 정보를 얻을 수 있어    
> Class객체를 통해서 객체를 생성하고 메서드를 호출하는 등 보다 동적인 코드를 작성할 수 있다. 

```java
Card c = new Card();                        //new연산자를 이용해서 객체 생성.
Card c = Card.class.newInstance();          //Class객체를 이용해서 객체 생성.
// newInstance()는 InstantitaionException이 발생할 수 있으므로 예외처리가 필요하다.
```
----------------------------------------------

# String클래스

## 변경 불가능한 클래스 immutable class
```java
public final class String implements java.io.Serializable, Comparable {
    private char[] value;
```
> 한번 생성된 String인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다.    
> '+'연산자를 이용해서 무자열을 결합하는 경우     
>  인스턴스내의 문자열이 바뀌는 것이 아니라 새로운 문자열이 담긴 String인스턴스가 생성되는 것이다.    

## 문자열의 비교
```java
String str1 = "abc";
String str2 = new String("abc");
```

> String클래스의 생성자를 이용한 경우에는 new 연산자에 의해 메모리할당이 이루어져 새로운 String 인스턴스가 생성된다.    
> 그러나 문자열 리터럴은 이미 존재하는 것을 재사용하는 것이다.     

## 문자열 리터럴
> String리터럴들은 컴파일시에 클래스파일에 저장된다.   
> 클래스 파일에는 소스파일에 포함된 모든 리터럴의 목록이 있다.   
> 해당 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때,    
>  리터럴의 목록에 있는 리터럴들이 JVM내에 있는 상수저장소constant pool에 저장된다.     

## 빈 문자열empty string
> char형 변수는 길이가 0일 수 없지만, char형 배열인 String은 가능하다.

## String클래스의 생성자와 메서드.

```java
String (String s)

String s = new String("Hello");
```
> 주어진 문자열(s)을 갖는 String인스턴스를 생성한다.

```java
String(char[] value)

char[] c = {'H','e','l','l',''o'};
String s = new String(c);
```
> 주어진 문자열(value)을 갖는 String인스턴스를 생성한다.

```java
String(StringBuffer buf)

StringBuffer sb = new StringBuffer("Hello");
String s = new String(sb);
```
> StringBuffer인스턴스가 갖고 있는 문자열과 같은 내용의 String인스턴스를 생성한다.

```java
char charAt(int index)

String s = "Hello";
char c = s.charAt(1);   // c = 'e'
```
> 지정된 위치(index)에 있는 문자를 알려준다.(0부터 시작)

```java
int compareTo(String str)

int i = "aaa".compareTo("aaa");   //0
int i = "aaa".compareTo("bbb");   //-1
int i = "bbb".compareTo("aaa");   //1
```
> 문자열str과 사전순서로 비교한다.   
> 같으면 0, 사전순으로 이전이면 음수, 이후면 양수를 반환한다.

```java
String concat(String str)

String s = "Hello";
String s2 = s.concat(" World");    //s2 = "Hello World"
```
> 문자열str을 뒤에 덧붙인다.

```java
boolean contains (CharSequence s)

String s = "abcdefg";
boolean b = s.contains("bc");   // b = true
```
> 지정된 문자열s이 포함되었는지 검사한다.

```java
boolean endsWith(String Suffix)

String file = "Hello.txt";
boolean b = file.endsWith("txt");    // b = true
```
> 지정된 문자열suffix로 끝나는지 검사한다.

```java
boolean equals(Object obj)

String s = "Hello";
boolean b = s.equals("Hello");    //b = true
boolean b2 = s.equals("hello");   //b2 = false
```
> 매개변수로 받은 문자열obj과 String인스턴스의 문자열을 비교한다.   
> obj가 String이 아니거나 문자열이 다르면 false를 반환한다.

```java
boolean equalsIgnoreCase(String str)

String s = "Hello";
boolean b = s.equalsIgnoreCase("HELLO");    //b = true;
boolean b2 = s.equalsIgnoreCase("hello");   //b2 = true;
```
> 문자열과 String인스턴스의 문자열을 대소문자 구분없이 비교한다.

```java
int indexOf(char ch)

String s = "Hello";
int idx1 = s.indexOf('o');    //idx1 = 4
int idx2 = s.indexOf('k');    //idx2 = -1
```
> 주어진 문자ch가 문자열에 존재하는지 확인하여 위치index를 알려준다.   
> 못 찾으면 -1을 반환한다.(index는 0부터 시작)

```java
int indexOf(char ch, int pos)

String s = "Hello";
int idx1 = s.indexOf('e', 0);   //idx1 = 1
int idx2 = s.indexOf('e', 2);   //idx2 = -1
```
> 주어진 문자ch가 문자열에 존재하는지 지정된 위치pos부터 확인하여 위치index를 알려준다.     
> 못 찾으면 -1을 반환한다.(index는 0부터 시작)    

```java
int index(String str)

String s = "ABCDEDG";
int idx = s.index("CD");    //idx = 2;
```
> 주어진 문자열이 존재하는지 확인하여 그 위치index를 알려준다.     
> 못 찾으면 -1을 반환한다. (index는 0부터 시작)   

```java
String intern()

String s = new String("abc");
String s2 = new String("abc");
boolean b = (s == s2);                        // b = false
boolean b2 = s.equals(s2);                    // b2 = true 
boolean b3 = (s.intern() == s2.intern());     // b3 = true
```
> 문자열을 상수풀constant pool에 등록한다.      
> 이미 상수풀에 같은 내용의 문자열이 있을 경우 그 문자열의 주소값을 반환한다.

```java
int lastIndexOf (char ch)

String s = "java.lang.Object";
int idx1 = s.lastIndexOf('.');    //idx1 = 9
int idx2 = s.indexOf('.');        //idx2 = 4
```
> 지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 위치(index)를 반환한다.
> 못 찾으면 -1을 반환한다.

```java
int length()

String s = "Hello";
int length = s.length();    //length = 5;
```
> 문자열의 길이를 알려준다.

```java
String replace (char old, char new)

String s = "Hello";
String s1 = s.replace('H', 'C')   //s1 = "Cello"
```
> 문자열 중의 문자old를 새로운 문자nw로 바꾼 문자열을 반환한다.

```java
String replace(CharSequence old, CharSequence nw)

String s = "Hellollo";
String s1 = s.replace("ll", "LL");    //s1 = "HeLLoLLo"
```
> 문자열 중의 문자열old을 새로운 문자열nw로 모두 바꾼 문자열을 반환한다.

```java
String replaceAll(String regex, String replacement)

String ab = "AABBAABB";
String r = ab.replaceAll("BB", "bb")    //r = "AAbbAAbb"
```
> 문자열 중에서 지정된 문자열regex과 일치하는 것을 새로운 문자열replacement로 모두 변경한다.

```java
String replaceFirst(Strng regex, String replcement)

String ab = "AABBAABB";
String r = ab.replaceFirst("BB", "bb")    // r = "AAbbAABB"
```
> 문자열 중에서 지정된 문자열regex과 일치하는 것 중, 첫 번째 것만 새로운 문자열replacement로 변경한다.

```java
String [] split(String regex)

String animals = "dog, cat, bear";
String [] arr = animals.split(",");   //arr[0] = "dog", arr[1] = "cat", arr[2] = "bear"
```
> 문자열을 지정된 분리자regex로 나누어 문자열 배열에 담아 반환한다.

```java
String [] split (String regex, int limit) 

String animals = "dog, cat, bear";
String [] arr = animals.split(",", 2);    //arr[0] = "dog", arr[1] = "cat,bear"
```
> 문자열을 지정된 분리자regex로 나누어 문자열배열에 담아 반환한다.   
> 단, 문자열 전체를 지정된 수limit로 자른다.   

```java
boolean startsWith (String prefix)

String s = "java.lang.Object";
boolean b = s.startsWith("java");     // b = true
boolean b2 = s.startsWith("lang");    // b2 = false
```
> 주어진 문자열prefix로 시작하는지 검사한다.

```java
String substring (int begin)
String substring (int begin, int end)

String s = "java.lang.Object";
String c = s.substring(10);     // c = "Object"
String p = s.substring(5, 9);   // p = "lang"
```
> 주어진 시작위치begin부터 끝 위치end 범위에 포함된 문자열을 얻는다.    
> 시작위치의 문자는 포함되지만 끝 위치의 문자는 포함되지 않는다.    

```java
String toLowerCase()

String s = "Hello";
String s1 = s.toLowerCase();    //s1 = "hello"
```
>String인스턴스에 저장되어있는 모든 문자열을 소문자로 변환하여 반환한다.

```java
String toString();

String s = "Hello";
String s1 = s.toString();   // s1 = "Hello"
```
> String인스턴스에 저장되어 있는 문자열을 반환한다.

```java
String toUpperCase()

String s = "Hello";
String s1 = s.toUpperCase();    // s1 = "HELLO"
```
> String인스턴스에 저장되어있는 모든 문자열을 대문자로 변환하여 반환한다.

```java
String trim()

String s = "    Hello World   ";
String s1 = s.trim();   // s1 = Hello World
```
> 문자열의 왼쪽 끝과 오른쪽 끝에 있는 공백을 없앤 결과를 반환한다.    
> 문자열 중간에 있는 공백은 제거되지 않는다.    

```java
static String valueOf(boolean b)
static String valueOf(char c)
static String valueOf(int i)
static String valueOf(long l)
static String valueOf(float f)
static String valueOf(double d)
static String valueOf(Object o)

String b = String.valueOf(true);            // b = "true"
String c = String.valueOf('a');             // c = "a"
String i = String.valueOf(100);             // i = "100"
String l = String.valueOf(100L);            // l = "100"
String f = String.valueOf(10f);             // f = "10.0"
String d = String.valueOf(10.0);            // d = "10.0"
java.util.Date dd = new java.util.Date();   // date = "날짜"
String Date = String.valueOf(dd);
```
> 지정된 값을 문자열로 변환하여 반환한다.   
> 참조변수의 경우, toString()을 호출한 결과를 반환한다.

## join()과 StringJoiner
```java
String animals = "dog, cat, bear";
String[] arr = animals.split(",");
String str = String.join("-", arr);    //arr = "dog-cat-bear"
```
> Join은 여러 문자열 사이에 구분자를 넣어서 결합한다.

```java
StringJoiner sj = new StringJoiner(",", "[", "]");
String[] strArr = { "aaa", "bbb", "ccc"};

for(int i = 0; i<strArr.length; i++)
	sj.add(strArr[i]);
  
  System.out.println(sj.toString());    //[aaa,bbb,ccc]
```
> java.util.StringJoiner클래스를 사용해서 문자열을 결합할 수 있다.

## 유니코드의 보충문자
> 유니코드는 원래 2byte, 즉 16비트 문자체계인데, 모자라서 20비트로 확장하게 되었다.   
> 그래서 하나의 문자를 char타입으로 다루지 못하고, int타입으로 다룰 수 밖에 없다.   
> 이 확장에 의해 새로 추가된 문자들을 보충문자라고 한다.

## 문자 인코딩 변환
> getBytes(String charsetName)를 사용하면, 문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.

```java
byte[] utf8_str = "가".getBytes("UTF-8");      //문자열을 UTF_8로 변환.
String str = new String(utf8_str, "UTF-8");    //byte배열을 문자열로 변환.
```
> 서로 다른 문자 인코딩을 사용하는 컴퓨터 간에 데이터를 주고받을 때는 적절한 문자 인코딩이 필요하다.

## String.format()
> 형식화된 문자열을 만들어내는 방법이다.

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5);     //3 더하기 5는 8입니다.
```

## 기본형 값을 String으로 변환

```java
int i = 100;
String str1 = i + "" ;
String str2 = String.valueOf(i);
```
> 성능은 valueOf가 더 좋으므로 성능향상이 필요할 때는 valueOf를 사용하자.

## String을 기본형 값으로 변환


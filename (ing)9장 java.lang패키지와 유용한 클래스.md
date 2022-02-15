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

```java
int i = Integer.parseInt("100");	//"100"을 100으로 변환.
int i2 = Integer.valueOf("100");	//"100"을 100으로 변환.
int i3 = Integer.parseInt(" 123  ",trim());	// 문자열 양 끝의 공백을 제거 후 변환.
```
> 문자열 양 끝에 공백이 있을 수 있어 trim()을 습관처럼 사용하는 것이 좋다.

## StringBuffer클래스와 StringBuilder클래스
> String클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 StringBuffer클래스는 변경이 가능하다.    
> 내부적으로 문자열 편집을 위한 버퍼buffer를 가지고 있으며 StringBuffer인스턴스를 생성할 때 그 크기를 지정할 수 있다.    
> StringBuffer인스턴스가 생성될 때, char형 배열이 생성되며 이를 인스턴스변수 value가 참조하게 된다.    

#### StringBuffer의 생성자
```java
public StringBuffer (int length) {
	value = new char[length];
	shared = false;
}
public StringBuffer() {			//버퍼의 크기를 지정하지 않으면 16이 된다.
	this(16);
}
public StringBuffer(String str) {
	this(str.length() + 16);	//지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성한다.
	append(str);
}
```

#### StringBuffer 크기 변경
```java
char newValue[] = new char[newCapacity];

System.arraycopy(value, 0, newvalue, 0, count);
value = new value;
```

#### StringBuffer의 변경
```java
StringBuffer sb = new StringBuffer("ABC");
sb.append("123");
sb.append("456").append("ZZ");
```
> append()는 반환타입이 StringBuffer이여서 연속적으로 append()를 호출하는 것이 가능하다.

#### StringBuffer의 비교
```java
StringBuffer sb = new StringBuffer("123");
StringBuffer sb2 = new StringBuffer("123");

System.out.println(sb == sb2);		//false
System.out.println(sb.equals(sb2));	//false

String s = sb.toString();
String s = sb2.toString();

System.out.println(sb.equals(sb2));	//true
```
> String클래스에서는 equals메서드를 오버라이딩해서 문자열의 내용을 비교하도록 구현되어 있지만,    
> StringBuffer클래스는 equals메서드를 오버라이딩하지 않아서 StringBuffer클래스의 equals메서드를 사용해도   
> 등가비교연산자( == )로 비요한 것과 같은 결과를 얻는다.     
> 반면에 toString()은 오버라이딩되어 있어서 StringBuffer인스턴스에 toString()을 호출하면, 담고있는 문자열을 String으로 반환한다.     
> 그래서 StringBuffer인스턴스에 담긴 문자열을 비교하기 위해서는 toString을 호출해서 String클래스를 얻은 다음,    
> 여기에 equals메서드를 사용해서 비교해야한다.

#### StringBuffer클래스의 생성자와 메서드

```java
StringBuffer()

StringBuffer sb = new StringBuffer();
```
> 16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다.


```java
StringBuffer(int length)

StringBuffer sb = new StringBuffer(10);
```
> 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer인스턴스를 생성한다.


```java
StringBuffer(String str)

StringBuffer sb = new StringBuffer("Hi");	//sb = "Hi"
```
> 지정된 문자열 값str을 갖는 StringBuffer 인스턴스를 생성한다.


```java
StringBuffer append(boolean b)
StringBuffer append(char c)
StringBuffer append(char[] str)
StringBuffer append(double d)
StringBuffer append(float f)
StringBuffer append(int i)
StringBuffer append(long l)
StringBuffer append(Object obj)
StringBuffer append(String str)

StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = sb.append(true);			//sb = "abctrued10.0ABC123"
sb.append('d').append(10.0f);				//sb2 = "abctrued10.0ABC123"
StringBuffer sb3 = sb.append("ABC").append(123);	//sb3 = "abctrued10.0ABC123"  모두 같은 객체 참조
```
> 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다.


```java
int capacity()

StringBuffer sb = new StringBuffer(100);
sb.append("abcd");
int bufferSize = sb.capacity();		//bufferSize = 100
int stringSize = sb.length();		//stringSize = 4
```
> StringBuffer인스턴스의 버퍼크기를 알려준다. length()는 버퍼에 담긴 문자열의 길이를 알려준다.


```java
char charAt(int index)

String buffer sb = new StringBuffer("abc");
char c = sb.charAt(2);				// c = 'c' 
```
> 지정된 위치index에 있는 문자를 반환한다.

```java
StringBuffer delete(int start, int end)

StringBuffer sb = new StringBuffer("0123456");		//sb = "0126"
StringBuffer sb2 = sb.delete(3, 6);			//sb2 = "0126"
```
> 시작위치start부터 끝 위치end 사이에 있는 문자를 제거한다. 단, 끝 위치는 제외.

```java
StringBuffer deleteCharAt(int index)

StringBuffer sb = new StringBuffer("0123456");
sb.deleteCharAt(3);					//sb = "012456"
```
> 지정된 위치index의 문자를 제거한다.

```java
StringBuffer insert(int pos, boolean b)
StringBuffer insert(int pos, char c)
StringBuffer insert(int pos, char[] str)
StringBuffer insert(int pos, double d)
StringBuffer insert(int pos, float f)
StringBuffer insert(int pos, int i)
StringBuffer insert(int pos, long l)
StringBuffer insert(int pos, Object obj)
StringBuffer insert(int pos, String str)

StringBuffer sb = new StringBuffer("0123456");
sb.insert(4, '.');					//sb = "0123.456")
```
> 두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치 pos에 추가한다. pos는 0부터 시작.

```java
int length()

StringBuffer sb = new StringBuffer("0123456");
int length = sb.length();					//length = 7
```
> StringBuffer인스턴스에 저장되어 있는 문자열의 길이를 반환한다.

```java
StringBuffer replace(int start, int end, String str)

StringBuffer sb = new StringBuffer("0123456"
sb.replace(3, 6, "AB");						//sb = "012AB6"
```
> 지정된 범위 start~end의 문자들을 주어진 문자열로 바꾼다.    
> end위치의 문자는 범위에 포함되지 않는다.


```java
StringBuffer reverse()

StringBuffer sb = new StringBuffer("0123456");
sb.reverse();							//sb = "6543210"
```
> StringBuffer인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열한다.


```java
void setCharAt(int index, char ch)

StringBuffer sb = new StringBuffer("0123456");
sb.setCharAt(5, 'o');						//sb = "01234o6"
```
> 지정된 위치의 문자를 주어진 문자ch로 바꾼다.


```java
void setLength(int newLength)

StringBuffer sb = new StringBuffer("0123456");
sb.setLength(5);						//sb = "01234"

stringBuffer sb2 = new StringBuffer("0123456");
sb2.setLength(10);						//sb2 = "0123456    "
Stirng str = sb2.toString().trim();				//str = "0123456"
```
> 지정된 길이로 문자열의 길이를 변경한다.     
> 길이를 늘리는 경우에 나머지 빈 공간을 널문자 '\n0000'로 채운다.     


```java
String toString()

StringBuffer sb = new StringBuffer("0123456");
String str = sb.toString();					//str = "0123456"
```
> stringBuffer인스턴스의 문자열을 String으로 반환.


```java
String substring(int start)
String substring(int start, int end)

StringBuffer sb = new StringBuffer("0123456");
String str = sb.substring(3);					//str = "3456"
String str2 = sb.substring(3, 5);				//str2 = "34"
```
> 지정된 범위 내의 문자열을 String으로 뽑아서 반환한다.    
> 시작위치start만 지정하면 시작위치부터 문자열 끝까지 반환한다.    

#### StringBuilder란?
> StringBuffer는 멀티쓰레드에 안전(thread safe)하도록 동기화되어있다.   
> 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능을 떨어뜨린다.     
> StringBuilder는 StringBuffer와 완전히 똑같은 기능으로 작성되어 있어서,     
> 소스코드에서 StringBuffer타입의 참조변수를 선언한 부분과 StringBuffer의 생성자만 바꾸면 된다.    

# Math클래스
> Math클래스의 생성자는 접근 제어자가 private이기 때문에 다른 클래스에서 Math인스턴스를 생성할 수 없다.     
> 클래스 내에 인스턴스변수가 하나도 없어서 인스턴스를 생성할 필요가 없기 때문이다.   
> Math클래스의 메서드는 모두 static이며, 자연로그 E와 원주율 3.14- 만 상수로 정의해 놓았다.

#### 올림, 버림, 반올림

```java
double a = 90.7552;
double b = a*100;		//b = 9075.52
double c = Math.round(b);	//c = 9076
double d = c/100;		//d = 90.76
```
> round()메서드는 항상 소수점 첫째자리에서 반올림을 해서 정수값long을 결과로 돌려준다.

```java
double d = Math.ceil(1.1);	//올림   d = 2.0
double d = Math.floor(1,5);	//버림   d = 1.0
double d = Math.round(1.1);	//반올림 d = 1
double d = Math.round(1.5);	//반올림, 반환값이 int d = 2
double d = Math.rint(1.5);	//반올림, 반환값이 double d, 가운데 값은 가까운 짝수 정수를 반환 = 2.000000
double d = Math.round(-1.5);	//반올림 d = -1
double d = Math.rint(-1.5);	//반올림, 반환값이 double d, 가운데 값은 가까운 짝수 정수를 반환 = -2.000000
double d = Math.ceil(-1.5);	//올림   d = -1.0
double d = Math.floor(-1.5);	//버림   d = -2.0
```

#### 예외를 발생시키는 메서드
> 메서드 이름에 Exact가 포함된 메서드들은 정수형간의 연산에서 발생할 수 있는 오버플로우를 감지하기 위한 것이다.
```java
int addExact (int x, int y)		//x + y
int subtractExact( int x, int y)	//x - y
int multipleExact ( int x, int y)	//x * y
int incrementExact (int a)		//a++
int decrementExact (int a)		//a--
int negateExact (int a)			//-a
int toIntExact (long value)		//(int)value  int로의 형변환
```
> 이 메서드들은 오버플로우가 발생하면, 예외ArithmeticException를 발생시킨다.

#### StrictMath클래스
> 성능을 다소 포기하고, 어떤 OS에서 실행되어도 항상 같은 결과를 얻도록 Math클래스를 새로 작성한 클래스이다.

#### Math클래스의 메서드
```java
static double abs (double a)
static float abs (float f)
static int abs (int f)
static long abs (long f)

int i = Math.abs(-10);			// i = 10
double d = Math.abs(-10.0);		// d = 10.0
```
> 주어진 값의 절대값을 반환한다.

```java
static double ceil(double a)

double d = Math.ceil(10.1);		// d = 11.0
double d2 = Math.ceil(-10.1);		// d2 = -10.0
double d3 = Math.ceil(10.000015);	// d3 = 11.0
```
> 주어진 값을 올림하여 반환한다.

```java
static double floor(double a)

double d = Math.floor(10.8);		// d = 10.0
double d2 = Math.floor(-10.8);		// d2 = -11.0
```
> 주어진 값을 버림하여 반환한다.

```java
static double max(double a, double b)
static float max(float a, float b)
static int max(int a, int b)
static long max(long a, long b)

double d = Math.max(9.5, 9.500001);	// d = 9.500001
int i = Math.max(0, -1);		// i = 0
```
> 주어진 두 값을 비교하여 큰 쪽을 반환한다.

```java
static double min(double a, double b)
static float min(float a, float b)
static int min(int a, int b)
static long min(long a, long b)

double d = Math.min(9.5, 9.500001);	// d = 9.5
int i = Math.min(0, -1);		// i = -1
```
> 주어진 두 값을 비교하여 작은 쪽을 반환한다.

```java
static double random()

double d = Math.random();		// 0.0 <= d < 1.0
int i = (int)(Math.random()*10)+1;	// 0<= i < 11
```
> 0.0~1.0범위의 임의의 double값을 반환한다.

```java
static double rint(double a)

double d = Math.rint(1.2);		// d = 1.0
double d2 - Math.rint(2.6);		// d2 = 3.0
double d3 = Math.rint(3.5);		// d3 = 4.0
double d4 = Math.rint(4.5);		// d4 = 4.0
```
> 주어진 double값과 가장 가까운 정수값을 double형으로 반환한다.    
> 단, 두 정수의 정가운데 있는 값(.5)은 짝수를 반환.

```java
static long round(double a)
static long round(float a)

long l = Math.round(1.2);		// l = 1
long l2 = Math.round(2.6);		// l2 = 3
long l3 = Math.round(3.5);		// l3 = 4
long l4 = Math.round(4.5);		// l4 = 5
double d = 90.7552;
double d2 = Math.round(d*100)/100.0	// d2 = 90.76
```
> 소수점 첫째자리에서 반올림한 정수값long을 반환한다.   
> 매개변수의 값이 음수인 경우, round()와 rint()의 결과가 다르다는 것에 주의하자.

---------------------------------------------

# 래퍼wrapper 클래스
> 기본형 값을 객체로 다룰 수 있게 해주는 클래스
```java
Boolean b = new boolean(true);
Character c = new Character('a');
Integer i = new Integer(100);
Integer i2 = new Integer("100");
```
> char형과 int형을 제외한 나머지는 자료형 이름의 첫 글자를 대문자로 한 것이 각 래퍼 클래스의 이름이다.     
> 래퍼 클래스의 생성자는 매개변수로 문자열이나 각 자료형의 값들을 인자로 받는다.    

```java
Integer i = new Integer(100);
Integer i2 = new Integer(100);

i == i2			//false
i.equals(i2)		//true
i.compareTo(i2)		//0
i.toString()		//100

Integer.MAX_VALUE	//2147483647
Integer.MIN_VALUE	//-2147483648
Integer.SIZE		//32
Integer.BYTES		//4
Integer.TYPE		//int
```
> 래퍼 클래스들은 모두 equals()가 오버라이딩되어 있어서 주소값이 아닌 객체가 가지고 있는 값을 비교한다.     
> 오토박싱이 된다고 해도 Integer객체에 비교연산자를 사용할 수 없고, 대신 compareTo()를 제공한다.     

#### Number 클래스
> Number클래스는 추상클래스로 내부적으로 숫자를 멤버변수로 갖는 래퍼 클래스들의 조상이다.     
> 그 외에도 BigInteger와 BigDemical 등이 있다.     
> BigInteger는 long으로도 다룰 수 없는 큰 범위의 정수를,      
> BigDemical은 double로도 다룰 수 없는 큰 범위의 부동 소수점수를 처지하기 위한 것으로 연산자의 역할을 대신하는 다양한 메서드를 제공한다.   

#### 문자열을 숫자로 변환하기
```java
int i = new Integer("100").intValue();		//floatValue(), longValue()...
int i2 = Integer.parseInt("100");		//주로 이 방법을 많이 사용, 반환값이 기본형
Integer i3 = Integer.valueOf("100");		//반환값이 래퍼 클래스 타입
int i4 = Integer.parseInt("100", 2);		//100(2) = 4
```
> 오토박싱 기능으로 반환값이 기본형일 때와 래퍼 클래스일 때의 차이는 없어졌다.    
> 다만 성능은 valueOf()가 조금 더 느리다.    
> 다른 진법의 숫자일 때도 변환이 가능하다.    

#### 오토박싱 & 언박싱
> 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것을 '오토박싱'이라고 하고,    
> 반대로 변환하는 것을 '언박싱'이라고 한다.   
```java
int i = 5;
Integer iobj = new Integer(7);
int sum = i + iobj;

ArrayList<Integer> list = new ArrayList<Integer>();		//Vector클래스에서도 가능하다.
list.add(10);			//오토박싱. 10 -> new Integer(10)
int value = list.get(0);	//언박싱. new Integer(10) -> 10
```

---------------------------------

# 유용한 클래스

## java.util.Objects 클래스
> Object클래스의 보조 클래스로 모든 메서드가 static이다.
```java
static <T> T requireNonNull(T obj)
static <T> T requireNonNull(T obj, String message)
static <T> T requireNonNull(T obj. String message, Supplier<String> messageSupplier)

void setName(String name)
	this.name = Object.requireNonNull(name, "name must not be null.");	
```
> null이면 NullPointerException, 아니면 this.name = name을 실행한다.

```java
static int compare (Object a, Object b, Comparator c)

static boolean equals(Object a, Object b) {
	return (a==b) || (a != null && e.equals(b));	}	
	
	if (Objects.equals(a,b) ...
	
comparator c = String.CASE_INSENSITIVE_ORDER;
System.out.println(compare("aa", "bb", c);	//-1
System.out.println(compare("bb", "aa", c);	// 1
System.out.println(compare("ab", "AB", c);	// 0


```
> a, b 모두 null인 경우에도 true를 반환한다.     
> null인지 검사할 필요가 없다.     

```java
static boolean deepequals (Object a, Object b)

String[][] str2D = new String[][]{{"aaa","bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Objects.equals(str2D, str2D2));	//false
System.out.println(Objects.deepequals(str2D, str2D2));	//true
```
> 이 메서드는 객체를 재귀적으로 비교하기 때문에 다차원 배열의 비교도 가능하다.
> equals()로 비교하려면 반복문을 사용해야하는데 deepequals()를 쓰면 간단히 끝난다.

```javaa
static String toString(Object o)
static String toString(Object o, String nullDefault)

String s = "notnull";
System.out.println(Objects.toString(s, "널"));		// s = notnull  	s가 null일 경우 널 출력.
```
> 내부적으로 Null 검사를 하고 Null일시 nullDefault를 반환한다.

```java
static int hashCode(Object o)
static int hash(Object... values)

System.out.println(Objects.hashcode(null));		//null일 경우 0을 반환한다.
```
## java.util.Random 클래스

```java
double randNum = Math.random();
double randNum = new Random().nextDouble();	//두 문장 동일

//1~6 사이의 정수를 얻는 방법
int num = (int)(math.random() *6) + 1;
int num = new Random().nextInt(6) + 1;
```
> 두 메서드의 차이는 종자값seed를 지정할 수 있다는 것이다.     
> 종자값이 같은 Random인스턴스들은 항상 같은 난수를 같은 순서대로 반환한다.

#### Random클래스의 생성자와 메서드
```java
public Random() {
	this(System.currentTimeMillis());	//Random(long seed)를 호출한다.
```
> 생성자 Random()은 종자값을 System.currentTimeMillis()로 하기 때문에 실행할 때마다 얻는 난수가 달라진다.
## 정규식 - java.util.regex패키지
> 정규식이란 텍스트 데이터 중에서 원하는 조건(패턴)과 일치하는 문자열을 찾아내기 위해 사용하는 것으로     
> 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 말한다.     
> 정규식을 이용하면 많은 양의 텍스트 파일 중에서 원하는 데이터를 손쉽게 뽑아낼 수도 있고    
> 입력된 데이터가 형식에 맞는지 체크할 수도 있다.

```java
String[] data = {"bat", "baby", "bonus", "cA", "ca", "co", "c.", "c0",
							"car", "combat", "count", "data", "disc"};
Pattern p = Pattern.compile("c[a-z]*");		//정규식을 매개변수로 Pattern클래스의 static메서드인 
						//Pattern compile(String regex)을 호출하여 Pattern인스턴스를 얻는다.
for(int i =0; i<data.length; i++) {			
	Matcher m = p.matcher(data[i]);		//정규식으로 비교할 대상을 매개변수로 Pattern클래스의 
						//Matcher matcher를 호출해서 Matcher인스턴스를 얻는다.
	if(m.matches())				//Matcher인스턴스에 boolean matches()를 호출해서 정규식에 부합하는지 확인한다.
		System.out.print(data[i]+ ",");		//ca,co,car,combat,count,
		}
```
> Pattern은 정규식을 정의하는데 사용되고 Matcher는 정규식(패턴)을 데이터와 비교하는 역할을 한다.       


| 정규식 패턴 | 설명 | 정규식 패턴            | 설명   |
|------------|------|-----------------------|-------|
| c[a-z]*     | c로 시작하는 영단어 | c[a-z] |c로 시작하는 두 자리 영단어 |
| c[a-zA-z]   | c로 시작하는 두 자리 영단어, 대소문자 구분 안 함 | c[a-zA-Z0-9] , c\\w | c로 시작하고 숫자와 영어로 조합된 두 글자 |
| .*          | 모든 문자열 | c. | c로 시작하는 두 자리 문자열 |
| c.\* | c로 시작하는 모든 문자열(기호포함)| c\\d, c[0-9] | c와 숫자로 구성된 두 자리 문자열 |
| c.\*t | c로 시작하고 t로 끝나는 모든 문자열 | [b\|c].\*, [bc].\*, [^b-c].\* | b 또는 c로 시작하는 문자열, ^이 붙으면 시작하지 않는 문자열.|
|.\*a.\* | a를 포함하는 모든 문자열. \*: 0 또는 그 이상의 문자 | .\*a.+ | a를 포함하는 모든 문자열, + : 1또는 그 이상의 문자. a로 끝나는 것은 미포함. |
| [b\|c].{2} | b 또는 c로 시작하는 세자리 문자열.|0\\\\d{1,2} | 0으로 시작하는 최소 2자리 최대 3자리 숫자(0포함) |
| \\\\d{3,4} | 최소 3자리 최대 4자리 숫자 | \\\\d{4} | 4자리 숫자 |

```java
String source = "HP:011-1111-1111, HOME:02-999-9999";
String pattern = "(0\\d{1,2})-(\\d{3,4})-(\\d{4})";

Pattern p = Pattern.compile(pattern);
Matcher m = p.matcher(source);
while(m.find())  {			//일치하는 패턴이 없으면 false를 반환한다.
System.out.println(m.group());		//011-1111-1111, 02-999-9999  매칭된 문자열을 그대로 반환한다.
System.out.println(m.group(1));		// 	0\\d{1,2}	011, 02
System.out.println(m.group(2));		// 	\\d{3,4}	1111, 999
System.out.println(m.group(3));	}	// 	\\d{4}		1111, 9999
```

```java
String source = "A broken hand works, but not a broken heart.";
String pattern = "broken";
StringBuffer sb = new StringBuffer();
	
Patten p = pattern.compile(pattern);
Matcher m = p.matcher(source);

int i = 0;

while(m.find()) {
	System.out.println(++i + "번째 매칭" + m.start() + "~" + m.end());	//1번째 매칭: 2~8, 	2번째 매칭: 31~37
	m.appendReplacement (sb, "drunken");	}		//broken을 drunken으로 치환하여 sb에 저장한다.	
m.appendTail(sb);
System.out.println(i);			//2
System.out.println(sb.toString());	//A drunken hand works, but not a drunken heart.
```
> m.appendReplacement (sb, "drunken"); 이 호출되면 source의 시작부터 broken을 찾은 위치까지의 내용에 drunken을 더해서 저장한다.        
> m.find()는 첫 번재로 발견된 위치의 끝에서부터 다시 검색을 시작하여 두 번재 broken을 찾는다.         
> m.appendTail(sb); 이 호출되면 마지막으로 치환된 이후의 부분을 sb에 덧붙인다.      

## java.util.Scanner 클래스
> 화면, 파일, 무자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 준다.     

```java
Scanner (String souce)
Scanner (File source)
Scanner (InputStream souce)
Scanner (Readable source)
Scanner (ReadableByteChannel source)
Scanner (path source)
//이와 같은 생성자를 지원하기 때문에 다양한 입력소스로부터 데이터를 읽을 수 있다.       

Scanner useDelimiter (Pattern pattern)
Scanner useDelimiter (String pattern)
```
> 정규식 표현을 이용한 라인단위의 검색을 지원하며 구분자delimiter에도     
> 정규식 표현을 사용할 수 있어서 복잡한 형태의 구분자도 처리가 가능하다.       

## java.util.StringTokenizer클래스
> 긴 문자열을 지정된 구분자를 기준으로 토큰이라는 여러 개의 문자열로 잘라내는데 사용된다.    
> 구분자로 단 하나의 문자 밖에 사용하지 못하기 때문에 보다 복잡한 형태의 구분자로    
> 문자열을 나누어야 할 때는 정규식을 사용해야 한다.

#### StringTokenizer의 생성자와 메서드
```java
StringTokenizer(String str, String delim)
```
> 문자열str에 지정된 구분자delim로 나누는 StringTokenizer를 생성한다. 구분자는 토큰으로 간주되지 않음
```java
StringTokenizer(String str, String delim, boolean returnDelims)
```
> returnDelimes의 값을 true로 하면 구분자도 토큰으로 간주된다.

```java
int countTokens()		//전체 토큰의 수를 반환한다.
boolean hasMoreTokens()		//토큰이 남아있는지 알려준다.
String nextToken()		//다음 토큰을 반환한다.
```
*시간날 때 마다 풀어보기.
```java
package practice;

import java.util.StringTokenizer;

public class Main {

	public static void main(String[] args) {
		String input = "삼십만삼천백십오";
		System.out.println(han(input));
	}

	public static long han(String input) {
		long result = 0;
		long tmpResult = 0;
		long num = 0;
		
		final String NUM = "영일이삼사오육칠팔구";
		final String UNIT = "십백천만억조";
		final long[] UNIT_NUM = {10, 100, 1000, 10000, (long)1e8, (long)1e12};
		
		StringTokenizer st = new StringTokenizer(input, UNIT, true);
		
		while(st.hasMoreTokens()) {
			String token = st.nextToken();
			int check = NUM.indexOf(token);
			
			if(check == -1) {
				if("만억조".indexOf(token)==-1) {
					tmpResult += (num!=0 ? num: 1) * UNIT_NUM[UNIT.indexOf(token)];
				}else {
					tmpResult += num;
					result += (tmpResult != 0? tmpResult : 1) * UNIT_NUM[UNIT.indexOf(token)];
					tmpResult =0;
				}
				num = 0;
			} else {
				num = check;
			}
			
		}
	return result + tmpResult + num;
}
}

```
```java
String result = data.split(",");
StringTokenizer st = new StringTokenizer(data, ",");
```
> split()은 빈 문자열도 토큰으로 인식하는 반면, StringTokenizer는 빈 문자열은 토큰으로 인식하지 않는다.      
> split()은 데이터를 토큰으로 잘라낸 결과를 배열에 담아 반환하고    
> StringTokenizer는 데이터를 토큰으로 잘라서 바로 반환하여 성능이 더 뛰어나다.

## java.math.BigInteger 클래스
> BigInteger는 내부적으로 int배열을 사용하여 long 타입보다 훨씬 큰 값을 다룬다. 대신 성능은 떨어진다.
```java
final int signum;	//부호. 1, 0, -1 셋 중 하나
final int[] mag		//값 magnitude 
```
> 코드에서 알 수 있듯이, BigInteger는 String처럼 불변immutable 이다.    
> 부호를 따로 저장하고 배열에는 값 자체만 저장한다. 2의 보수.

#### BigInteger의 생성
> 문자열로 숫자를 표현하는 것이 일반적이다.     
> 정수형 리터럴로는 표형할 수 있는 값의 한계가 있기 때문이다.

```java
BigInteger val = new BigInteger("12345678901234567890");	//문자열로 생성
BigInteger val = new BigInteger("FFFF", 16);			//n진수radic의 문자열로 생성
BigInteger val = new BigInteger(1234567890L);			//숫자로 생성
```

#### 다른 타입으로의 변환
```java
String toString()		//문자열로 변환
String toString(int radix)	//지정된 진법(radix)의 문자열로 변환
byte[] toByteArray()		//byte배열로 변환

int intValue()
long longValue()
float floatValue()
double doubleValue()

byte byteValueExact()
int intValueExact()
long longValueExact()
```
> Exact가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.

#### BigInteger의 연산
```java
BigInteger add(BigInteger val)		//덧셈 this+val
BigInteger subtract(BigInteger val)	//뺄셈 this-val
BigInteger multiply(BigInteger val)	//곱셈 this*val
BigInteger divide(BigInteger val)	//나눗셈 this/val
BigInteger remainder(BigInteger val)	//나머지 this%val
```
> BigInteger는 불변이므로, 반환타입이 BigInteger란 얘기는 새로운 인스턴스가 반환된다는 뜻이다.

#### 비트 연산 메서드
> 큰 숫자를 다루기 위한 클래스이므로, 성능을 향상싴키기 위해 비트단위로 연산을 수행하는 메서드들을 가지고 있다.

```java
int bitCount()			//2진수로 표현했을 때, 1의 개수(음수는 0의 개수)를 반환
int bitLenghth()		//2진수로 표현했을 때, 값을 표현하는데 필요한 bit의 수
boolean testBit(int n)		//우측에서 n+1번째 비트가 1이면 true
BigInteger setBit(int n)	//우측에서 n+!번째 비트를 1로 변경
BigInteger clearBit(int n)	//우측에서 n+!번째 비트를 0으로 변경
BigInteger flipBit(int n)	//우측에서 n+!번째 비트를 전환(0 <-> 1)
```
```java
BigInteger bi = new BigInteger("4");
if(bi.remainder(new BigInteger("2")).equals(BigInteger.ZERO)) {...}

BigInteger bi = new BigInteger("4");
if(!bi.testBit(0)) { ... }		//비트연산으로 처리하는 것이 더 간단하다.
```
> 정수가 짝수인지 확인하는 조건식이다.   
> 

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
> 다량의 데이터를 저장하고 검색하는데 유용하다
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

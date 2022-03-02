# 지네릭스Generics
## 지네릭스란?
지네릭스는 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입체크(compile-time type check)를 해주는 기능이다.     
객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.     


지네릭스의 장점
> 타입 안정성을 제공한다.      
> 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.    

## 지네릭스의 선언
지네릭스 타입은 클래스와 메서드에 선언할 수 있다.
```java
class Box<T>{		//지네릭 타입 T를 선언
	T item;
	
	void setItem(T item) {
		this.item = item;
	}
	T getItem() { return item; }
}
```
> 클래스 이름 옆에 <T>를 붙이고 Object를 모두 T로 바꿧다.
 Box<T>에서 T를 타입변수type variable이라고 하며, Type의 첫 글자에서 따온 것이다.      
 타입 변수는 다른 것을 사용해도 되며, ArrayList<E>는 요소Element에서 E를 따왓다.    
 타입 변수가 여러 개인 경우 Map<K, V>와 같이 컴마,를 구분자로 나열하면 된다.     
 
 ```java
Box<String> b = new Box<String>();	//타입 T대신, 실제 타입을 지정
b.setItem(new Object());		    //에러, String타입 외에는 지정불가.
b.setItem("ABC");
String item = b.getItem();		    //형변환이 필요없음.

//String을 지정해줌으로 아래와 같아진다.
class Box{
    String item;
    
    void setItem(String item) { this.item = item;}
    String getItem()    { return item; }
```
지네릭 클래스가 된 Box클래스의 객체를 생성할 때는 다음과 같이 참조변수와 생성자에 타입T대신 사용될 실제 타입을 지정해주어야 한다.   

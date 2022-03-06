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
> 클래스 이름 옆에 < T >를 붙이고 Object를 모두 T로 바꿧다.
 Box< T >에서 T를 타입변수type variable이라고 하며, Type의 첫 글자에서 따온 것이다.      
 타입 변수는 다른 것을 사용해도 되며, ArrayList< E >는 요소Element에서 E를 따왓다.    
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

#### 지네릭스 용어
* class Box< T > { }
> Box< T > 지네릭 클래스. 'T의 Box' 또는 'T Box'라고 읽는다.     
> T 타입 변수 또는 타입 매개변수.(T는 타입문자)      
> Box 원시타입(raw type)
* Box< String > b = new Box< String >();
> 매개변수에 타입을 지정하는 것을 지네릭 타입 호출 이라고 한다.     
> 지정된 타입String을 매개변수화된 타입 이라고 한다.     

	
Box< String >과 Box< Integer >는 지네릭 클래스 Box< T >에 서로 다른 타입을 대입하여 호출한 것일 뿐,     
이 둘이 별개의 클래스를 의미하는 것은 아니다. add(3,5)와 add(2,4)가 같은 메서드를 호출하는 것과 같은 것이다.			

#### 지네릭스의 제한
지네릭 클래스 Box의 객체를 생성할 때, 객체별로 다른 타입을 지정하는 것은 적절하다.
```java
  Box<Apple> appleBox = new Box<Apple>();
  Box<Grape> grapeBox = new Box<Grape>();
  ```
그러나 모든 객체에 대해 동일하게 동작해야하는 static멤버에 타입 변수 T를 사용할 수 없다.    
	T는 인스턴스변수로 간주되기 때문이다.(static멤버는 인스턴스 변수를 참조할 수 없다)
```java
class Box<T>
  static T item;				//에러
  static int compare(T t1, T t2) { ... }	//에러
	```
static멤버는 타입 변수에 지정된 타입, 즉 대입된 타입의 종류에 관계없이 동일한 것이어야 하기 때문이다.    
지네릭 타입의 배열을 생성하는 것은 허용되지 않는다. 
```java
class Box<T>	{
	T[] itemArr;				//ok. T타입의 배열을 위한 참조변수
	...
	T[] tmpArr = new T[item.length];
	...
	return tmpArr
}
```
지네릭 배열을 생성할 수 없는 것은 new 연산자 때문인데, 이 연산자는 컴파일 시점에 타입 T가 뭔지 정확히 알아야 한다.    
그런데 위의 코드에 정의된 Box<T>클래스를 컴파일하는 시점에서는 T가 어떤 타입이 될지 전혀 알 수 없다.    
instanceof연산자도 new연산자와 같은 이유로 T를 피연산자로 사용할 수 없다.       

# 지네릭 클래스의 객체 생성과 사용

```java
class Box<T>	{
	ArrayList<T> list = new ArrayList<T>();
	
	void add(T item)				{list.add(item);}
	T get(int i)						{return lsit.get(i);}
	ArrayList<T> getList()	{return list;}
	int size()							{return list.size()}
	public String toString	{return list.toString();}
}
	
//Box< T >의 객체를 생성할 때는 참조변수와 생성자에 대입된 타입(매개변수화된 타입)이 일치해야 한다.
Box<Apple> appleBox = new Box<Apple>();
Box<Apple> appleBox = new Box<Grape>();						//에러
	
//두 타입이 상속관계에 있어도 마찬가지다. Apple이 Fruit의 자손이라고 가정.
Box<Fruit> appleBox = new Box<Apple>();						//에러
	
//단, 두 지네릭 클래스의 타입이 상속관계에 있고, 대입된 타입이 같은 것은 괜찮다.
//FruitBox는 Box의 자손이라고 가정.
Box<Apple> appleBox = new FruitBox<Apple>();
	
//추정이 가능한 경우 타입을 생략할 수 있다.
//참조변수의 타입으로부터 Box가 Apple타입의 객체만 저장한다는 것을 알 수 있기 때문에,
//생성자에 반복해서 타입을 지정해주지 않다고 되는 것이다. 아래 두 문장은 동일하다.
Box<Apple> appleBox = new Box<Apple>();
Box<Apple> appleBox = new Box<>();
	
//생성된 Box<T>의 객체에 'void add(T item)'으로 객체를 추가할 때, 대입된 타입과 다른 타입의 객체는 추가할 수 없다.
Box<Apple> appleBox = new Box<Apple>();
appleBox.add(new Apple());
appleBox.add(new Grape());		//에러
	
//그러나 타입T가 Fruit인 경우 'void add(Fruit item)'가 되므로 Fruit의 자손들은 이 메서드의 매개변수가 될 수 있다.
Box<Fruit> fruitBox = new Box<Fruit>();
fruitBox.add(new Fruit());
fruitBox.add(new Apple());
```
	
```java
class Fruit {public String toString() { return "Fruit";}}
class Apple extends Fruit {public String toString() { return "Apple";}}
class Grape extends Fruit {public String toString() { return "Grape";}}
class Toy {public String toString()	{ return "Toy";}}

public class FruitBoxEx1 {
  public static void main(String[] args) {
    Box<Fruit> fruitBox = new Box<Fruit>();
    Box<Apple> appleBox = new Box<Apple>();
    Box<Toy> toyBox = new Box<Toy>();
//  Box<Grape> grapeBox = new Box<Apple>();		//에러, 타입 불일치
		
    fruitBox.add(new Fruit());
    fruitBox.add(new Apple());					//void add(Fruit item)
		
    appleBox.add(new Apple());
    appleBox.add(new Apple());
//  appleBox.add(new Toy());					//에러, Box<Apple>에는 Apple만 담을 수 있음
		
		toyBox.add(new Toy());
//  toyBox.add(new Apple());					//에러, Box<Toy>에는 Toy만 담을 수 있음
		
		System.out.println(fruitBox);				//[Fruit, Apple]
		System.out.println(appleBox);				//[Apple, Apple]
		System.out.println(toyBox);					//[Toy]
	}
}
class Box<T>{
	ArrayList<T> list = new ArrayList<T>();
	void add(T item) { list.add(item);}
	T get(int i)	 { return list.get(i);}
	int size()		 { return list.size();}
	public String toString() { return list.toString();}
}
```       
  
## 제한된 지네릭 클래스     
  지네릭 타입에 extends를 사용하면, 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.
  
```java
class FruitBox<T extends Fruit> {					//Fruit의 자손만 타입으로 지정가능
  ArrayList<T> list = new ArrayList<T>();
	...
}
	
FruitBox<Apple> appleBox = new FruitBox<Apple>();
FruitBox<Toy>	toyBox = new FruitBox<Toy>();				//에러. Toy는 Fruit의 자손이 아님
	
//add()의 매개변수의 타입 T도 Fruit과 그 자손 타입이 될 수 있으므로, 여러 과일을 담을 수 있는 상자가 가능하게 된다.
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
fruitBox.add(new Apple());	//Apple이 Fruit의 자손
fruitBox.add(new Grape());	//Grape가 Fruit의 자손
//타입 매개변수 T에 Object를 대입하면 모든 종류의 객체를 저장할 수 있게 된다.
```
  
```java
//만일 클래스가 아니라 인터페이스를 구현해야 한다는 제약이 필요하면,
//implement가 아니라 extends를 사용한다.
interface Eatable {}
class FruitBox<T extends Eatable> {...}
//클래스 Fruit의 자손이면서 Eatable 인터페이스도 구현해야한다면 & 기호로 연결한다.
class FruitBox<T extends Fruit & Eatable> {...}
//FruitBox에는 Fruit의 자손이면서 Eatable을 구현한 클래스만 타입 매개변수 T에 대입될 수 있다.
```
  
```java
class Fruit implements Eatable {
  public String toString() {return "Fruit";}
}
class Apple extends Fruit {public String toString() { return "Apple";}}
class Grape extends Fruit {public String toString() { return "Grape";}}
class Toy				   {public String toString() { return "Toy";}}

interface Eatable{}

public class FruitBoxEx2 {

	public static void main(String[] args) {
		FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
		FruitBox<Apple> appleBox = new FruitBox<Apple>();
		FruitBox<Grape> grapeBox = new FruitBox<Grape>();
//  FruitBox<Grape> grapeBox = new FruitBox<Apple>();	//에러, 타입 불일치
//  FruitBox<Toy>	  toyBox   = new FruitBox<Toy>();		//에러

		fruitBox.add(new Fruit());
		fruitBox.add(new Apple());
		fruitBox.add(new Grape());
		appleBox.add(new Apple());
//  appleBox.add(new Grape());			//에러, Grape는 Apple의 자손이 아님
		grapeBox.add(new Grape());
		
		System.out.println(fruitBox);		//[Fruit, Apple, Grape]
		System.out.println(appleBox);		//[Apple]
		System.out.println(grapeBox);		//[Grape]
	}
}
class FruitBox<T extends Fruit & Eatable> extends Box<T>{}
class Box<T>{
	ArrayList<T> list = new ArrayList<T>();
	void add(T item)	{list.add(item);	}
	T get(int i)		{return list.get(i);}
	int size()			{return list.size();}
	public String toString() {return list.toString();}
}
```
## 와일드 카드
```java
class Juicer {
	static Juice makeJuice(FruitBox<Fruit> box) {			//<Fruit>으로 지정
		String tmp = " ";
		for(Fruit f : box.getList())	tmp += f + " ";
		return new Juice(tmp);
	}
}
//Juicer클래스는 지네릭 클래스가 아닌데다, 지네릭 클래스라고 해도 static메서드에는 타입 매개변수 T를 매개변수에 사용할 수 없으므로
//아예 지네릭스를 적용하지 않던가, 위와 같이 타입 매개변수 대신, 특정 타입을 지정해줘야 한다.

FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> appleBox = new appleBox<Apple>();
...
System.out.println(Juicer.makeJuice(fruitBox));		//FruitBox<Fruit>
System.out.println(Juicer.makeJuice(appleBox));		//에러, FruitBox<Apple>
// 지네릭 타입을 FruitBox<Fruit>로 고정해 놓으면, FruitBox<Apple>타입의 객체 makeJuice()의 매개변수가 될 수 없으므로,
// 다음과 같이 여러가지 타입의 매개변수를 갖는 makeJuice()를 만들 수 밖에 없다.
	
static Juice makeJuice (FruitBox<Fruit> box) {
	String tmp = " ";
	for(Fruit f : box.getList()) tmp += f + " ";
	  return new Juice(tmp);
}
	
static Juice makeJuice(FruitBox<Apple> box) {
	String tmp = " ";
	for(Fruit f : box.getList()) tmp += f + " ";
	return new Juice(tmp);
 }
	
//그러나 위와 같이 오버로딩하면, 컴파일 에러가 발생한다. 지네릭 타입이 다른 것만으로는 오버로딩이 성립하지 않기 때문이다.
//지네릭 타입은 컴파일러가 컴파일할 때만 사용하고 제거해버린다. 그래서 위의 두 메서드는 오버로딩이 아니라
//메서드 중복 정의 이다.
```
	
이럴때 사용하기 위해 고안된 것이 와일드 카드이다.      
와일드 카드는 기호 '?'로 표현하는데, 와일드 카드는 어떠한 타입도 될 수 있다.      
'?'만으로는 Object타입과 다를 게 없으므로, extends와 super로 상한과 하한을 제한할 수 있다.     
* < ? extends T > 와일드 카드의 상한 제한. T와 그 자손들만 가능.         
* < ? super T > 와일드 카드의 하한 제한. T와 그 조상들만 가능.         
* < ? > 제한 없음. 모든 타입이 가능. < ? extends Object >와 동일.       


와일드 카드를 사용해서 makeJuice()의 매개변수 타입을 FruitBox< Fruit >에서        
FruitBox < ? extends Fruit >으로 바꾸면 다음과 같이 된다.
```java
static Juice (FruitBox<? extends Fruit> box {
  String tmp = "";
  for(Fruit f : box.getList()) tmp += f + " ";
  return new Juice(tmp);
}
```
이제 이 메서드의 매개변수로 FruitBox< Fruit >뿐 아니라, FruitBox< Apple >와 FruitBox< Grape >도 가능하게 된다.     

```java
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> appleBox = new FruitBox<Apple>();
	...
System.out.println(Juicer.makeJuice(fruitBox));
System.out.println(Juicer.makeJuice(appleBox));
```

매개변수의 타입을 FruitBox< ? extends Object >로 하면, 모든 종류의 FruitBox가 이 메서드의 매개변수로 가능해진다.     
대신 box의 요소가 Fruit의 자손이라는 보장이 없으므로 아래의 for문에서 box에 저장된 요소를 Fruit타입의 참조변수로 못받는다.

```java
static Juice makeJuice(FruitBox <? extends Object> box {
	String tmp = " ";
	for(Fruit f : box.getList())
		tmp += f + " ";			//에러. Fruit이 아닐 수 있음
	return new Juice(tmp);
```

그러나 실제로 테스트하면 문제없이 컴파일되는데 그 이유는 지네릭 클래스 FruitBox를 제한했기 때문이다.

```java
class FruitBox <T extends Fruit> extends Box<T> {}
```

컴파일러는 위 문장으로부터 모든 FruitBox의 요소들이 Fruit의 자손이라는 것을 알고 있으므로 문제 삼지 않는 것이다.

```java
import java.util.ArrayList;
class Fruit { public String toString() { return "Fruit";}}
class Apple extends Fruit { public String toString() { return "Apple";}}
class Grape extends Fruit { public String toString() { return "Grape";}}
class Juice {
	String name;
	
	Juice(String name) { this.name = name + "Juice";}
	public String toString() { return name;}
}
class Juicer{
	static Juice makeJuice(FruitBox<? extends Fruit> box) {
		String tmp = " ";
		
		for(Fruit f : box.getList())
			tmp += f + " ";
		return new Juice(tmp);
	}
}
public class FruitBoxEx3 {

	public static void main(String[] args) {
		FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
		FruitBox<Apple> appleBox = new FruitBox<Apple>();
		
		fruitBox.add(new Apple());
		fruitBox.add(new Grape());
		appleBox.add(new Apple());
		appleBox.add(new Apple());
		
		System.out.println(Juicer.makeJuice(fruitBox));
		System.out.println(Juicer.makeJuice(appleBox));
	}
}
class FruitBox <T extends Fruit> extends Box<T>{}
class Box<T>{
	ArrayList<T> list = new ArrayList<T>();
	void add(T item) { list.add(item);}
	T get(int i) { return list.get(i);}
	ArrayList<T> getList() { return list;}
	int size() { return list.size();}
	public String toString() { return list.toString();}
}
```

```java
import java.util.*;

class Fruit {
	String name;
	int weight;
	
	Fruit (String name, int weight){
		this.name = name;
		this.weight = weight;
	}
	public String toString() {return name + "(" + weight + ")";}
}
class Apple extends Fruit {
	Apple(String name, int weight){
		super(name, weight);
	}
}
class Grape extends Fruit {
	Grape(String name, int weight){
		super(name, weight);
	}
}
class AppleComp implements Comparator<Apple>{
	public int compare(Apple t1, Apple t2) {
		return t2.weight - t1.weight;
	}
}
class GrapeComp implements Comparator<Grape>{
	public int compare(Grape t1, Grape t2) {
		return t2.weight - t1.weight;
	}
}
class FruitComp implements Comparator<Fruit>{
	public int compare(Fruit t1, Fruit t2) {
		return t1.weight - t2.weight;
	}
}
public class FruitBoxEx4 {

	public static void main(String[] args) {
		FruitBox<Apple> appleBox = new FruitBox<Apple>();
		FruitBox<Grape> grapeBox = new FruitBox<Grape>();
		
		appleBox.add(new Apple("GreenApple", 300));
		appleBox.add(new Apple("GreenApple", 100));
		appleBox.add(new Apple("GreenApple", 200));
		
		grapeBox.add(new Grape("GreenGrape", 400));
		grapeBox.add(new Grape("GreenGrape", 300));
		grapeBox.add(new Grape("GreenGrape", 200));
		
		Collections.sort(appleBox.getList(), new AppleComp());
		Collections.sort(grapeBox.getList(), new GrapeComp());
		System.out.println(appleBox);			//[GreenApple(300), GreenApple(200), GreenApple(100)]
		System.out.println(grapeBox);			//[GreenGrape(400), GreenGrape(300), GreenGrape(200)]
		Collections.sort(appleBox.getList(), new FruitComp());
		Collections.sort(grapeBox.getList(), new FruitComp());
		System.out.println(appleBox);			//[GreenApple(100), GreenApple(200), GreenApple(300)]
		System.out.println(grapeBox);			//[GreenGrape(200), GreenGrape(300), GreenGrape(400)]
	}
}
class FruitBox<T extends Fruit> extends Box<T>{}
class Box<T>{
	ArrayList<T> list = new ArrayList<T>();
	
	void add(T item) {
		list.add(item);
	}
	T get(int i) {
		return list.get(i);
	}
	ArrayList<T> getList(){ return list;}
	int size() {
		return list.size();
	}
	public String toString() {
		return list.toString();
	}
}
```

이 예제는 Collections.sort()를 이용해서 appleBox와 grapeBox에 담긴 과일을 무게별로 정렬한다.     
이 메서드의 선언부는 다음과 같다

```java
static <T> void sort(List<T> list, Comparator<? super T> c)
```

static 옆에 있는 < T >는 메서드에 선언된 지네릭 타입이다. 이런 메서드를 지네릭 메서드라고 한다.    
첫 매개변수는 정렬 대상이고, 두 번째 매개변수는 정렬할 방법이 정의된 Comparator이다.     
Comparator의 지네릭 타입에 하한이 걸려있는 와일드 카드가 사용되었다.

```java
static <T> void sort(List<T> list, Comparator<T> c)
//와일드 카드를 사용하지 않았다고 가정

//매개변수 T에 Apple 대입
static void sort(List<Apple> list, Comparator<Apple> c)
//이것은 List<Apple>을 정렬하기 위해서는 Comparator<Apple>이 필요하다는 것을 의미한다.

//Comparator<Apple>을 구현한 AppleComp클래스를 정의
class AppleComp implements Comparator<Apple>{
	public int compare (Apple t1, Apple t2) {
		return t2.weight - t1.weight;
	}
}
//위의 코드는 문제가 없어보이나 Apple대신 Grape가 대입된다면 에러가 발생한다.     
//그래서 위 예제에서 같은 코드를 두 번 작성하였다.
```
AppleComp와 GrapeComp는 타입만 다를 뿐 완전히 같은 코드다.    
코드의 중복도 문제지만 새로운 Fruit의 자손이 생길 때마다 위와 같은 코드를 반복해서 만들어야 한다는 것이 더 문제이다.     
이 문제를 해결하기 위해 타입 매개변수에 하한 제한의 와일드 카드를 적용해야 한다.

```java
static void sort(List<Apple> list, Comparator<? super Apple> c_)
```
매개변수의 타입이 Comparator< ? super Apple >이라는 의미는 Comparator의 타입 매개변수로 Apple과 그 조상이 가능하다는 뜻이다.     
즉, Comparator< Apple >, Comparator< Fruit >, Comparator< Object > 중의 하나가 두 번재 매개변수로 올 수 있다는 뜻이다.

- Comparator< ? super Apple > : Comparator< Apple >, Comparator< Fruit >, Comparator < Object >
- Comparator< ? super Grape > : Comparator< Grape >, Comparator< Fruit >, Comparator < Object >

그래서 아래와 같이 FruitComp를 만들면, List< Apple >과 List< Grape >를 모두 정렬할 수 있다.     
비교의 대상이 되는 weight는 Apple과 Grape의 조상인 Fruit에 정의되어 있기 때문에 가능한 것이기도 하다.

```java
class FruitComp implements Comparator<Fruit> {
	public int compare(Fruit t1, Fruit t2) {
		return t1.weight - t2.weight;
	}
}
//List<Apple>과 List<Grape>를 모두 Comparator<Fruit>로 정렬
Collections.sort(appleBox.getList(), new FruitBox());
Collections.sort(grapeBox.getList(), new FruitBox());
```
이러한 장점 때문에 Comparator에는 항상 < ? super T >가 습관적으로 따라 붙는다.

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

#### 지네릭스 용어
* class Box<T> { }
> Box<T> 지네릭 클래스. 'T의 Box' 또는 'T Box'라고 읽는다.     
> T 타입 변수 또는 타입 매개변수.(T는 타입문자)      
> Box 원시타입(raw type)
* Box<String> b = new Box<String>();
> 매개변수에 타입을 지정하는 것을 지네릭 타입 호출 이라고 한다.     
> 지정된 타입String을 매개변수화된 타입 이라고 한다.     

	
Box<String>과 Box<Integer>는 지네릭 클래스 Box<T>에 서로 다른 타입을 대입하여 호출한 것일 뿐,     
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
	
	//Box<T>의 객체를 생성할 때는 참조변수와 생성자에 대입된 타입(매개변수화된 타입)이 일치해야 한다.
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
	fruitBox.add(new Applew());
```
	
```java
	class Fruit 			  {public String toString() { return "Fruit";}}
class Apple extends Fruit {public String toString() { return "Apple";}}
class Grape extends Fruit {public String toString() { return "Grape";}}
class Toy				  {public String toString()	{ return "Toy";}}

public class FruitBoxEx1 {
	public static void main(String[] args) {
		Box<Fruit> fruitBox = new Box<Fruit>();
		Box<Apple> appleBox = new Box<Apple>();
		Box<Toy> toyBox = new Box<Toy>();
//		Box<Grape> grapeBox = new Box<Apple>();		//에러, 타입 불일치
		
		fruitBox.add(new Fruit());
		fruitBox.add(new Apple());					//void add(Fruit item)
		
		appleBox.add(new Apple());
		appleBox.add(new Apple());
//		appleBox.add(new Toy());					//에러, Box<Apple>에는 Apple만 담을 수 있음
		
		toyBox.add(new Toy());
//		toyBox.add(new Apple());					//에러, Box<Toy>에는 Toy만 담을 수 있음
		
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
	class Fruit1 implements Eatable {
	public String toString() {return "Fruit";}
}
class Apple1 extends Fruit1 {public String toString() { return "Apple";}}
class Grape1 extends Fruit1 {public String toString() { return "Grape";}}
class Toy1				   {public String toString() { return "Toy";}}

interface Eatable{}

public class FruitBoxEx2 {

	public static void main(String[] args) {
		FruitBox1<Fruit1> fruitBox = new FruitBox1<Fruit1>();
		FruitBox1<Apple1> appleBox = new FruitBox1<Apple1>();
		FruitBox1<Grape1> grapeBox = new FruitBox1<Grape1>();
//		FruitBox1<Grape1> grapeBox = new FruitBox1<Apple1>();	//에러, 타입 불일치
//		FruitBox1<Toy1>	  toyBox   = new FruitBox1<Toy1>();		//에러

		fruitBox.add(new Fruit1());
		fruitBox.add(new Apple1());
		fruitBox.add(new Grape1());
		appleBox.add(new Apple1());
//		appleBox.add(new Grape1());			//에러, Grape는 Apple의 자손이 아님
		grapeBox.add(new Grape1());
		
		System.out.println(fruitBox);		//[Fruit, Apple, Grape]
		System.out.println(appleBox);		//[Apple]
		System.out.println(grapeBox);		//[Grape]
	}
}
class FruitBox1<T extends Fruit1 & Eatable> extends Box1<T>{}
class Box1<T>{
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
	//Juice클래스는 지네릭 클래스가 아닌데다, 지네릭 클래스라고 해도 static메서드에는 타입 매개변수 T를 매개변수에 사용할 수 없으므로
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
	> <? extends T> 와일드 카드의 상한 제한. T와 그 자손들만 가능.      
  > <? super T> 와일드 카드의 하한 제한. T와 그 조상들만 가능.     
	> <?> 제한 없음. 모든 타입이 가능. <? extends Object>와 동일.

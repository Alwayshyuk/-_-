# 람다식 Lambda expression
람다식의 도입으로 인해 자바는 객체지향언어인 동시에 함수형 언어가 되었다.     
이 덕분에 자바는 함수형 언어의 장점들을 누릴 수 있게 되었다.

## 람다식이란?
람다식은 메서드를 하나의 식expression으로 표현한 것이다.      
람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다.     
메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 익명함수anonymous function이라고도 한다.      

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random()*5+1));

int method(){
	return (int)(Math.random()*5)+1;
```
위 람다식이 하는 일을 메서드로 표현하면 그 아래와 같다.    
메서드는 클래스에 포함되어야 하므로 클래스도 새로 만들어야 하고, 객체도 생성해야한 메서드를 호출할 수 있다.    
그러나 람다식은 그러한 과정없이 오직 람다식만으로도 이 메서드의 역할을 대신할 수 있다.     
게다가 람다식은 메서드의 매개변수로 전달되어지는 것이 가능하고, 메서드의 결과로 반환될 수도 있다.    
람다식으로 인해 메서드를 변수처럼 다루는 것이 가능해진 것이다.     

## 람다식 작성하기
람다식은 메서드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통{} 사이에 ->를 추가한다.

> 반환타입 매서드이름 (매개변수 선언) { 문장들 }        
> (매개변수 선언) -> { 문장들 }

예를 들어 두 값 중에서 큰 값을 반환하는 메서드 max를 람다식으로 변환하면, 아래와 같다.

```java
int max(int a, int b) { return a>b? a:b; }

(int a, int b) -> { return a>b? a:b; }
```
반환값이 있는 메서드의 경우, return문 대신 식으로 대신할 수 있다. 식의 연산결과가 자동으로 반환값이 된다.     
이때는 문장이 아닌 식이므로 끝에 ;을 붙이지 않는다.

```java
(int a, int b) -> { return a>b? a:b }

(int a, int b) -> a>b? a:b
```
람다식에 선언된 매개변수의 타입은 추론이 가능한 경우 생략할 수 있는데, 대부분의 경우 생략가능하다.     
람다식에 반환타입이 없는 이유도 항상 추론이 가능하기 때문이다.

```java
(int a, int b) -> a>b? a:b

(a, b) -> a>b? a: b
```


아래와 같이 선언된 매개변수가 하나뿐인 경우에는 괄호()를 생략할 수 있다.      
단, 매개변수의 타입이 있으면 괄호()를 생략할 수 있다.     

```java
(a) -> a*a
(int a) -> a*a

a-> a*a //OK
int a -> a*a //에러
```

마찬가지로 괄호{} 안의 문장이 하나일 때는 괄호{}를 생략할 수 있다.    
이 떄 문장의 끝에 ;을 붙이지 않아야 한다는 것에 주의해야 한다.

```java
(String name, int i) -> { System.out.println(name+ "=" + i}; }

(String name, int i) -> System.out.println(name + "=" + i)
```

그러나 괄호{}안의 문장이 return문일 경우 괄호{}를 생략할 수 없다.

```java
(a, b) -> return a>b a:b	//에러
(a, b) -> { return a>b a:b;} //OK
```

아래는 메서드를 람다식으로 변환한 예시이다.

```java
int max(int a, int b) { return a>b?a:b; }
(a,b) -> a>b?a:b

void printVar(String name, int i) { System.out.println(name+"="+i); }
(name, i) -> System.out.println(name + "=" + i)

int square(int x) { return x*x; }
x -> x*x

int roll(){ return (int)(Math.random()*6); }
() -> (int)(Math.random()*6)

int Sum(int[] arr) {
	int sum = 0;
	for(int i : arr)
		sum += i;
	return sum
}
(int[] arr) -> {
	int sum = 0;
	for(int i : arr)
		sum +=i;
	return sum;
}
```

## 함수형 인터페이스 Functional Interface
하나의 메서드가 선언된 인터페이스를 정의해서 람다식을 다루는 것은 기존 자바의 규칙들을 어기지 않으면서도 자연스럽다.      
그래서 인터페이스를 통해 람다식을 다루기로 결정되었으며, 람다식을 다루기 위한 인터페이스를 함수형 인터페이스라고 부르기로 했다.     
단, 함수형 인터페이스에는 오직 하나의 추상 메서드만 정의되어 있어야 한다는 제약이 있다.      
그래야 람다식과 인터페이스의 메서드가 1:1로 연결될 수 있기 때문이다. 반면에 static메서드와 default메서드의 개수에는 제약이 없다.      

> @FunctionalInterface를 붙이면 컴파일러가 함수형 인터페이스를 올바르게 정의하였는지 확인해준다.     

기존에는 아래와 같이 인터페이스의 메서드 하나를 구현하는데도 복잡하게 해야 했으나, 그 아래와 같이 간단해졌다.

```java
List<String> list = Arrays.asList("abc", "aaa", "bbb", "ddd", "aaa");

Collections.sort(list, new Comparator<String>()) {
	public int compare(String s1, String s2){
		return s2.compareTo(s1);
	}
}

Collections.sort(list, (s1, s2) -> s2.compareTo(s1));
```

#### 함수형 인터페이스 타입의 매개변수와 반환타입
함수형 인터페이스 MyFunction이 아래와 같이 정의되어 있을 때, 

```java
@FunctionalInterface
interface MyFunction {
	void myMethod();	//추상 메서드
}
```

메서드의 매개변수가 MyFunction타입이면, 이 메서드를 호출할 때 람다식을 참조하는 참조변수를 매개변수로 지정해야 한다.

```java
void aMethod(MyFunction f) {
	f.myMethod();
}
	...
MyFunction f = () -> System.out.println("myMethod()");
aMethod(f);
```

또는 참조변수 없이 아래와 같이 직접 람다식을 매개변수로 지정하는 것도 가능하다.

```java
aMethod(()->System.out.println("myMethod()"));
```

그리고 메서드의 반환타입이 함수형 인터페이스타입이라면,      
이 함수형 인터페이스의 추상메서드와 동등한 람다식을 가리키는 참조변수를 반환하거나 람다식을 직접 반환할 수 있따.

```java
MyFunction myMethod() {
	MyFunction f = () -> {};
	return f;	//이 줄과 윗 줄을 한 줄로 줄이면, return ()->{};
```

람다식을 참조변수로 다룰 수 있다는 것은 메서드를 통해 람다식을 주고받을 수 있다는 것을 의미한다.      
즉, 변수처럼 메서드를 주고받는 것이 가능해진 것이다.     
사실상 메서드가 아니라 객체를 주고받는 것이라 근본적으로 달라진 것은 아무것도 없지만,      
람다식 덕분에 예전보다 코드가 더 간결하고 이해하기 쉬워졌다.

```java
@FunctionalInterface
interface MyFunction {
	void run();	//public abstract void run();
}

public class LambdaEx1 {
	static void execute(MyFunction f) {
		f.run();	//매개변수 타입이 MyFunction인 메서드
	}
	static MyFunction getMyFunction() {
		MyFunction f = () -> System.out.println("f3.run()");
		return f;	//반환 타입이 MyFunction인 메서드
	}
	public static void main(String[] args) {
		//람다식으로 MyFunction의 run()을 구현
		MyFunction f1 = () ->System.out.println("f1.run()");
		
		MyFunction f2 = new MyFunction() {	//익명 클래스로 run구현
			public void run() {	//public을 반드시 붙여야함
				System.out.println("f2.run()");
			}
		};
		MyFunction f3 = getMyFunction();
		
		f1.run();
		f2.run();
		f3.run();
		
		execute(f1);
		execute(() -> System.out.println("run()"));
	}
}
```

#### 람다식의 타입과 형변환
함수형 인터페이스로 람다식을 참조할 수 있는 것일 뿐, 람다식의 타입이 함수형 인터페이스의 타입과 일치하는 것은 아니다.    
람다식은 익명 객체이고 익명 객체는 타입이 없다.    
정확히는 타입은 있지만 컴파일러가 임의로 이름을 정하기 때문에 알 수 없는 것이다.      
그래서 대입 연산자의 양변의 타입을 일치시키기 위해 아래와 같이 형변환이 필요하다.

> MyFunction은 interface MyFunction{ void method(); }와 같이 정의되었다고 가정한다.

```java
MyFunction f = (MyFunction) () -> {};	//양변의 타입이 다르므로 형변환 필요.
```

람다식은 MyFunction인터페이스를 직접 구현하지 않았지만,      
이 인터페이스를 구현한 클래스의 객체와 완전히 동일하기 때문에 위와 같은 형변환을 허용한다.    
그리고 이 형변환은 생략가능하다.     
람다식은 이름이 없을 뿐 분명한 객체인데도, Object타입으로 형변환 할 수 없다. 오직 함수형 인터페이스로만 형변환이 가능하다.       
굳이 Object타입으로 형변환하려면, 먼저 함수형 인터페이스로 변환해야 한다.

```java
Object obj = (Object)(MyFunction) (() -> {});
String str = ((Object)(MyFunction) (() -> {})).toString();
```

```java
@FunctionalInterface
interface MyFunction {
	void myMethod();	//public abstract void myMethod();
}
public class LambdaEx2 {

	public static void main(String[] args) {
		MyFunction f = () -> {};	//MyFuntion f = (MyFuntion) (()->{});
		Object obj = (MyFunction) (() -> {});	//Object타입으로의 형변환이 생략됨
		String str = ((Object)(MyFunction) () -> {}).toString();
		
		System.out.println(f);
		System.out.println(obj);
		System.out.println(str);
		
//		System.out.println(()->{});//에러. 람다식은 Object타입으로 형변환 안됨
		System.out.println((MyFunction)()->{});
//		System.out.println((MyFunction)(()->{}).toString());//에러.
		System.out.println(((Object)(MyFunction)(()->{})).toString());
	}
}
tmp.LambdaEx2$$Lambda$1/0x0000000800c00bf8@5674cd4d
tmp.LambdaEx2$$Lambda$2/0x0000000800c01000@63961c42
tmp.LambdaEx2$$Lambda$3/0x0000000800c01218@85ede7b
tmp.LambdaEx2$$Lambda$4/0x0000000800c01430@1be6f5c3
tmp.LambdaEx2$$Lambda$5/0x0000000800c01648@38af3868

```
실행결과를 보면, 컴파일러가 람다식의 타입을 어떤 형식으로 만들어내는지 알 수 있다.    
일반적인 익명 객체라면, 객체의 타입이 '외부클래스이름$번호'와 같은 형식으로 타입이 결정되었을 텐데,     
람다식의 타입은 '외부클래스이름$$Lambda$번호'와 같은 형식으로 되어있다.

#### 외부 변수를 참조하는 람다식
람다식도 익명 객체, 즉 익명 클래스의 인스턴스이므로 람다식에서 외부에 선언된 변수에 접근하는 규칙은 앞서 배운 익명클래스와 같다.     

```java
@FunctionalInterface
interface MyFunction {
	void myMethod();
}
class Outer {
	int val = 10;	//Outer.this.val
	
	class Inner {
		int val = 20; //this.val
		
		void method(int i) { //void method(final int i)
			int val = 30;	//final int val = 30;
//			i = 10;	//에러. 상수의 값은 변경 불가
			
			MyFunction f = () -> {
				System.out.println("i: " + i);	//100
				System.out.println("val: " + val);	//30
				System.out.println("this.val: " + this.val);	//20
				System.out.println("Outer.this.val: " + Outer.this.val);	//10
			};
			f.myMethod();
		}
	}
}
public class LambdaEx3 {

	public static void main(String[] args) {
		Outer outer = new Outer();
		Outer.Inner inner = outer.new Inner();
		inner.method(100);
	}
}
```
이 예제는 람다식 내에서 외부에 선언된 변수에 접근하는 방법을 보여준다.     
람다식 내에서 참조하는 지역변수는 final이 붙지 않았어도 상수로 간주된다.     
람다식 내에서 지역변수 i와 val을 참조하고 있으므로 람다식 내에서나 다른 어느 곳에서도 이 변수들의 값을 변경하는 일은 허용되지 않는다.      
반면에 Inner클래스와 Outer클래스의 인스턴스 변수인 this.val과 Outer.this.val은 상수로 간주되지 않으므로 값을 변경해도 된다.     
그리고 외부 지역변수와 같은 이름의 람다식  매개변수는 허용되지 않는다.     

## java.util.function 패키지
일반적으로 자주 쓰이는 형식의 메서드를 함수형 인터페이스로 미리 정의해놓은 패키지이다.     
매번 새로운 함수형 인터페이스를 정의하지 말고, 가능하면 이 패키지의 인터페이스를 활용하는 것이 좋다.     
그래야 함수형 인터페이스에 정의된 메서드 이름도 통일되고, 재사용성이나 유지보수 측면에서도 좋다.    

```java
함수형 인터페이스: java.lang.Runnable
메서드: void run()
설명: 매개변수도 없고, 반환값도 없음.

함수형 인터페이스: Supplier<T>
메서드: T get()
설명: 매개변수는 없고, 반환값T만 있음.

함수형 인터페이스: Consumer<T>
메서드: void accept<T t>
설명: 매개변수t만 있고, 반환값은 없음

함수형 인터페이스: Function<T, R>
메서드: R apply(T t)
설명: 일반적인 함수. 하나의 매개변수t를 받아서 결과R를 반환

함수형 인터페이스: Predicate<T>
메서드: boolean test(T t)
설명: 조건식을 표현하는데 사용됨. 매개변수t는 하나, 반환타입은 boolean
```

매개변수와 반환값의 유무에 따라 4개의 함수형 인터페이스가 정의되어 있고, Function의 변형으로 Predicate가 있는데,     
반환값이 boolean이라는 것만 제외하면 Function과 동일하다. Predicate는 조건식을 함수로 표현하는데 사용된다.   

> 타입 문자T는 Type을, R은 Return Type을 의미한다.

#### 조건식의 표현에 사용되는 Predicate
Predicate는 Function의 변형으로, 반환타입이 boolean이라는 것만 다르다.      
Predicate는 조건식을 람다식으로 표현하는데 사용된다.    

> 수학에서 결과로 true 또는 false를 반환하는 함수를 Predicate라고 한다.

```java
Predicate<String> isEmptyStr = s -> s.length() == 0;
String s = "";

if(isEmptyStr.test(s))	//if(s.length == 0)
	System.out.println("This is an empty String.");
```

#### 매개변수가 두 개인 함수형 인터페이스
매개변수의 개수가 2개인 함수형 인터페이스는 이름 앞에 접두사 Bi가 붙는다.

> 매개변수의 타입으로 보통 T를 사용하므로, 알파벳에서 T의 다음 문자인 U, V, W를 매개변수의 타입으로 사용한다.   

```java
함수형 인터페이스: BiConsumer<T, U>
메서드: void accept(T t, U u)
설명: 두개의 매개변수만 있고, 반환값이 없음

함수형 인터페이스: BiPredicate<T, U>
메서드: booelan accept(T t, U u)
설명: 조건식을 표현하는데 사용됨. 매개변수는 둘, 반환값은 boolean

함수형 인터페이스: BiFunction<T, U, R>
메서드: R apply (T t, U u)
설명: 두 개의 매개변수를 받아서 하나의 결과를 반환
```

> Supplier는 반환값만 있는데 메서드는 두 개의 값을 반환할 수 없으므로 BiSupplier가 없다.

두 개 이상의 매개변수를 갖는 함수형 인터페이스가 필요하다면 직접 만들어서 써야한다.       
만일 3개의 매개변수를 갖는 함수형 인터페이스를 선언한다면 아래와 같을 것이다.     

```java
@FunctionalInterface
interface TriFunction<T, U, V, R> {
	R apply(T t, U u, V v);
}
```

#### UnaryOperator와 BinaryOperator
Function의 또 다른 변형으로 UnaryOperator와 BinaryOperator가 있는데,      
매개변수의 타입과 반환타입의 타입이 모두 일치한다는 점만 제외하고는 Function과 같다.     

```java
함수형 인터페이스: UnaryOperator<T>
메서드: T apply(T t)
설명: Function의 자손, Function과 달리 매개변수와 결과의 타입이 같다.

함수형 인터페이스: BinaryOperator<T>
메서드: T apply(T t, T t)
설명: BiFunction의 자손, BiFunction과 달리 매개변수와 결과의 타입이 같다.
```

#### 컬렉션 프레임웍과 함수형 인터페이스
컬렉션 프레임웍의 인터페이스에 다수의 디폴트 메서드가 추가되었는데,     
그 중의 일부는 함수형 인터페이스를 사용한다. 아래는 그 메서드들의 목록이다.     

```java
인터페이스: Collection
메서드: boolean removeIf(Predicate<E> filter)
설명: 조건에 맞는 요소를 삭제

인터페이스: List
메서드: void replaceAll(UnaryOperator<E> operator)
설명: 모든 요소를 변환하여 대체

인터페이스: Iterable
메서드: void forEach(Consumer<T> action)
설명: 모든 요소에 작업 action을 수행

인터페이스: Map
메서드: V compute(K key, BiFunction<K,V,V> f)
설명: 지정된 키의 값에 작업 f를 수행\
메서드: V computeIfAbsent(K key, Function<K,V> f)
설명: 키가 없으면, 작업 f 수행 후 추가
메서드: V computeIfPresent(K key, BiFunction<K,V,V> f)
설명: 지정된 키가 있을 때, 작업 f를 수행
메서드: V merge(K key, V value, BiFunction<V,V,V> f)
설명: 모든 요소에 병합작업 f를 수행
메서드: void forEach(BiConsumer<K,V> action)
설명: 모든 요소에 작업 action을 수행
메서드: void replaceAll(BiFunction<K,V,V> f)
설명: 모든 요소에 치환작업 f를 수행
```

Map인터페이스에 있는 compute로 시작하는 메서드들은 맵의 value를 변환하는 일을 하고     
merge()는 Map을 병합하는 일을 한다.

```java
public class LambdaEx4 {

	public static void main(String[] args) {
		ArrayList<Integer> list = new ArrayList<>();
		
		for(int i = 0; i<10; i++)
			list.add(i);
		
		//list의 모든 요소를 출력
		list.forEach(i->System.out.print(i + ","));
		System.out.println();
		
		//list에서 2 또는 3의 배수를 제거
		list.removeIf(x -> x % 2==0 || x % 3 ==0);
		System.out.println(list);
		
		list.replaceAll(i -> i*10); 	//list의 각 요소에 10을 곱한다.
		System.out.println(list);
		
		Map<String, String> map = new HashMap<>();
		
		map.put("1", "1");
		map.put("2", "2");
		map.put("3", "3");
		map.put("4", "4");
		
		//map의 모든 요소를 {k,v}의 형식으로 출력한다.
		map.forEach((k, v) -> System.out.print("{"+k+","+v+"}, "));
		System.out.println();
	}
}
```

```java
public class LambdeEx5 {

	public static void main(String[] args) {
		Supplier<Integer> s = () -> (int)(Math.random()*100)+1;
		Consumer<Integer> c = i -> System.out.print(i+",");
		Predicate<Integer> p = i -> i%2 == 0;
		Function<Integer, Integer> f = i -> i/10*10;
		List<Integer> list = new ArrayList<>();
		makeRandomList(s, list);
		System.out.println(list);
		printEvenNum(p, c, list);
		List<Integer> newList = doSomething(f, list);
		System.out.println(newList);
	}
	static <T> List<T> doSomething(Function<T, T> f, List<T> list){
		List<T> newList = new ArrayList<T>(list.size());
		
		for(T i : list)
			newList.add(f.apply(i));

		return newList;
	}
	static <T> void printEvenNum (Predicate<T> p, Consumer<T> c, List<T> list) {
		System.out.print("[");
		
		for(T i : list)
			if(p.test(i))
				c.accept(i);
		
		System.out.println("]");
	}
	static <T> void makeRandomList(Supplier<T> s, List<T> list) {
		for(int i = 0; i<10; i++)
			list.add(s.get());
	}
}
```

#### 기본형을 사용하는 함수형 인터페이스
위의 함수형 인터페이스는 매개변수와 반환값의 타입이 모두 지네릭 타입이었는데,     
기본형 타입의 값을 처리할 때도 래퍼wrapper클래스를 사용해왔다.    
기본형 대신 래퍼클래스를 사용하는 것은 비효율적이므로 기본형을 사용하는 함수형 인터페이스들이 제공된다.

```java
함수형 인터페이스: DoubleToIntFunction
메서드: int applyAsInt(double d)
설명: A To B Function은 입력이 A타입, 출력이 B타입

함수형 인터페이스: ToIntFunction<T>
메서드: int applyAsInt(T value)
설명: To B Function은 출력이 B타입이다. 입력은 지네릭 타입

함수형 인터페이스: IntFunction<R>
메서드: R apply(T t, U u)
설명: A Function은 입력이 A타입이고 출력은 지네릭 타입

함수형 인터페이스: ObjIntConsumer<T>
메서드: void accept(T t, U u)
설명: Obj A Function은 입력이 T, A타입이고 출력은 없다.
```

이전 예제를 기본형을 사용하는 함수형 인터페이스로 변경하면 다음과 같다.

```java
public class LambdaEx6 {

	public static void main(String[] args) {
		IntSupplier s = () -> (int)(Math.random()*100)+1;
		IntConsumer c = i -> System.out.print(i+",");
		IntPredicate p = i -> i % 2 == 0;
		IntUnaryOperator op = i -> i/10*10;
		
		int[] arr = new int[10];
		
		makeRandomList(s, arr);
		System.out.println(Arrays.toString(arr));
		printEvenNum(p, c, arr);
		int[] newArr = doSomething(op, arr);
		System.out.println(Arrays.toString(newArr));
	}
	static void makeRandomList(IntSupplier s, int[] arr) {
		for(int i = 0; i<arr.length; i++)
			arr[i] = s.getAsInt();
	}
	static void printEvenNum(IntPredicate p, IntConsumer c, int[] arr) {
		System.out.print("[");
		for(int i : arr)
			if(p.test(i))
				c.accept(i);
		
		System.out.println("]");
	}
	static int[] doSomething(IntUnaryOperator op, int[] arr) {
		int[] newArr = new int[arr.length];
		
		for(int i = 0; i<newArr.length; i++)
			newArr[i] = op.applyAsInt(arr[i]);
		
		return newArr;
	}
}
```
위 예제에서 만일 IntUnaryOperator대신 Function을 사용하면 에러가 발생한다.     

```java
Funtion f = (a) -> a*2;	//에러. a의 타입을 알 수 없으므로 연산 불가.
```
매개변수와 반환 값의 타입을 추정할 수 없기 때문이다. 그래서 타입을 지정해 주어야 한다.   

```java
Function<Integer, Integer> f = (a) -> a*2;
```
또는 Function대신 IntFunction을 사용할 수도 있지만,      
IntUnaryOperator가 Function이나 IntFunction보다 오토박싱&언박싱의 횟수가 줄어들어 더 성능이 좋다.

```java
IntFunction<Integer> f = (a) -> a*2;
```

IntFunction, ToIntFunction, IntToLongFunction은 있어도 IntToIntFunction은 없는데,     
그 이유는 IntUnaryOperator가 그 역할을 하기 때문이다.     
매개변수의 타입과 반환타입이 일치할 때는 Function대신 UnaryOperator를 사용하는 것이 좋다.    

## Function의 합성과 Predicate의 결합
java.util.function패키지의 함수형 인터페이스에는 추상메서드 외에도 디폴트 메서드와 static메서드가 정의되어 있다.     
우리는 Function과 Predicate에 정의된 메서드에 대해서만 살펴볼 것인데,    
그 이유는 다른 함수형 인터페이스의 메서드도 유사하기 때문이다.     

> 원래 Function인터페이스는 반드시 두 개의 타입을 지정해 줘야하기 때문에,     
> 두 타입이 같아도 Function<T>라고 쓸 수 없다. Function<T, T>라고 써야 한다.     

```java
//Function
default <V> Function<T,V> andThen (Function<? super R, ? extends V> after)
default <V> Function<V,R> compose (Function<? super V, ? extends T> before)
static <T> Function<T,T> identity()

//Predicate
default Predicate<T> and(Predicate <? super T> other)
default Predicate<T> or(Predicate <? super T> other)
default Predicate<T> negate()
static <T> Predicate<T> isEqual(Object targetRef)
```

#### Function의 합성
수학에서 두 함수를 합성해서 하나의 새로운 함수를 만들어낼 수 있다는 것처럼,    
두 람다식을 합성해서 새로운 람다식을 만들 수 있다. 두 함수의 합성은 어느 함수를 먼저 적용하느냐에 따라 달라진다.     
함수 f,g가 있을 때, f.andThen(g)는 함수 f를 먼저 적용하고, f.compose(g)는 반대로 g를 먼저 적용한다.    
예를 들어, 문자열을 숫자로 변환하는 함수 f와 숫자를 2진 문자열로 변환하는 함수 g를 andThen()으로 합성하여 새로운 함수 h를 만들어낼 수 있다.     

```java
Function<String,Integer> f = (s) -> Integer.parseInt(s, 16);
Function<Integr,String> g = (i) -> Integer.BinaryString(i);
Function<String,String> h = f.andThen(g);

System.out.println(h.apply("FF");	//"FF" - 255 - "11111111"
```

함수 h의 지네릭 타입이 <String,String> 이므로 String을 입력받아서 String을 결과로 반환한다.    
예를 들어 함수 h에 문자열 "FF"를 입력하면, 결과로 "11111111"을 얻는다.     


아래는 compose()를 이용해서 두 함수를 반대의 순서로 합성한다.

```java
Function<Integer,String> g = (i) -> Integer.toBinaryString(i);
Function<String,Integer> f = (s) -> Integeer.parseInt(s, 16);
Function<Integer,Integer> h = f.compose(g);

System.out.println(h.apply(2));		//2 - "10" - 16
```
이전과 달리 함수 h의 지네릭 타입이 <Integer,Integer>이다. 함수 h에 숫자 2를 입력하면, 결과로 16을 얻는다.    
그리고 identity()는 함수를 적용하기 이전과 이후가 동일한 항등 함수가 필요할 때 사용한다.     
이 함수를 람다식으로 표현하면 x->x 이다. 아래의 두 문장은 동일하다.     

> 항등 함수는 함수에 x를 대입하면 결과가 x인 함수를 말한다. f(x) = x

```java
Function<String, String> f = x -> x;
//Function <String, String> f = Function.identity();	//위의 문장과 동일

System.out.println(f.apply("AAA"));	//AAA가 그대로 출력됨.
```
항등 함수는 잘 사용되지 않는 편이며, 나중에 배울 map()으로 변환작업할 때, 변환없이 그대로 처리하고자할 때 사용된다.     

#### Predicate의 결합
여러 조건식을 논리 연산자신 &&, ||, ! 으로 연결해서 하나의 식을 구성할 수 있는 것처럼,     
여러 Predicate를 and(), or(), negate()로 연결해서 하나의 새로운 Predicate로 결합할 수 있다.    

```java
Predicate<Integer> p = i -> i < 100;
Predicate<Integer> q = i -> i < 200;
Predicate<Integer> r = i -> i % 2 == 0;
Predicate<Integer> notP = p.negate();	//i >= 100

//100 <= i && (i < 200 || i % 2 == 0)
Predicate<Integer> all = notP.and(q.or(r));
System.out.println(all.test(150));	//true
```
이처럼 and(), or(), negate()로 여러 조건식을 하나로 합칠 수 있다. 물론 아래와 같이 람다식을 직접 넣어도 된다.    

```java
Predicate<Integer> all = notP.and(i -> i < 200).or(i -> i % 2 == 0);
```

> Predicate의 끝에 negate()를 붙이면 조건식 전체가 부정이 된다.    

그리고 static메서드인 isEqual()은 두 대상을 비교하는 Predicate를 만들 때 사용한다.    
먼저, isEqual()의 매개변수로 비교대상을 하나 지정하고, 또 다른 비교대상은 test()의 매개변수로 지정한다.    

```java
Predicate<String> p = Predicate.isEqual(str1);
boolean result = p.test(str2);	//str1과 str2가 같은 지 결과를 반환

boolean result = Predicate.isEqual(str1).test(str2);
```
위의 두 무장을 합치면 그 아래와 같다.    

```java
public class LambdaEx7 {

	public static void main(String[] args) {
		Function<String,Integer> f = (s) -> Integer.parseInt(s, 16);
		Function<Integer,String> g = (i) -> Integer.toBinaryString(i);
		
		Function<String,String> h = f.andThen(g);
		Function<Integer,Integer> h2 = f.compose(g);
		
		System.out.println(h.apply("FF"));	//11111111
		System.out.println(h2.apply(2));	//16
		
		Function<String,String> f2 = x->x;
		System.out.println(f2.apply("AAA"));	//AAA
		
		Predicate<Integer> p = i -> i<100;
		Predicate<Integer> q = i -> i<200;
		Predicate<Integer> r = i -> i%2 == 0;
		Predicate<Integer> notP = p.negate();
		
		Predicate<Integer> all = notP.and(q.or(r));
		System.out.println(all.test(150));	//true
		
		String str1 = "abc";
		String str2 = "abc";
		
		//str1과 str2가 같은 지 비교한 결과를 반환
		Predicate<String> p2 = Predicate.isEqual(str1);
		boolean result = p2.test(str2);	
		System.out.println(result);	//true
	}
}
```

## 메서드 참조
람다식이 하나의 메서드만 호출하는 경우에는 메서드 참조라는 방법으로 람다식을 더 간략히 할 수 있다.      
예를 들어 문자열을 정수로 변환하는 람다식은 아래와 같이 작성할 수 있다.     

```java
Function<String,String> f = (String s) -> Integer.parseInt(s);
```

보통은 이렇게 람다식을 작성하는데, 이 람다식을 메서드로 표현하면 아래와 같다.    

```java
Integer wrapper(String s) { return Integer.parseInt(s); }
```

여기서 메서드를 벗겨내고 Integer.parseInt()를 직접 호출하면 아래와 같다.

```java
Function<String,Integer> f = Integer::parseInt;	//메서드 참조
```

위 메서드 참조에서 람다식의 일부가 생략되었지만, 컴파일러는 생략된 부분을 우변의 parseInt메서드의 선언부로부터,       
또는 좌변의 Function인터페이스에 지정된 지네릭 타입으로부터 쉽게 알아낼 수 있다.     

```java
BiFunction<String,String,Boolean> f = (s1, s2) -> s1.equlas(s2);
```

참조변수 f의 타입을 보면 람다식이 두 개의 String타입의 매개변수를 받는 다는 것을 알 수 있으므로,     
림다식의 매개변수들은 없어도 된다. 위 람다식에서 매개변수들을 제거해서 메서드 참조로 변경하면 아래와 같다.     

```java
BiFunction<String,String,Boolean> f = String::equals;
```
매개변수 s1과 s2를 생략해버리고 나면 equals만 남는데, 두 개의 String을 받아서 Boolean을 반환하는 equals라는 이름의 메서드는      
다른 클래스에도 존재할 수 있기 때문에 equals앞에 클래스 이름은 반드시 필요하다.     
메서드 참조를 사용할 수 있는 경우가 한 가지 더 있는데, 이미 생성된 객체의 메서드를 람다식에서 사용한 경우에는     
클래스 이름 대신 그 객체의 참조변수를 적어줘야 한다.     

```java
MyClass obj = new MyClass();
Function<String,Boolean> f = (x) -> obj.equals(x);	//람다식
Function<String,Boolean> f = obj::equals;	//메서드 참조
```


```java
종류: static 메서드 참조			람다: (x) -> ClassName.method(x)	메서드 참조: ClassName::method
종류: 인스턴스메서드 참조		람다: (obj,x) -> obj.method(x)		메서드 참조: ClassName::method
종류: 특정 객체 인스턴스메서드 참조	람다: (x) -> obj.method(x)		메서드 참조: obj::method
```

> 하나의 객체만 호출하는 람다식은 클래스이름::메서드이름 또는 참조변수::메서드이름 으로 바꿀 수 있다.

#### 생성자의 메서드 참조
생성자를 호출하는람다식도 메서드 참조로 변환할 수 있다.    

```java
Supplier<MyClass> s = () -> new MyClass();	//람다식
Supplier<MyClass> s = MyClass::new;		//메서드 참조
```

매개변수가 있는 생성자라면, 매개변수의 개수에 따라 알맞은 함수형 인터페이스를 사용하면 된다.    
필요하다면 함수형 인터페이스를 새로 정의해야 한다.

```java
Function<Integer,MyClass> f = (i) -> new MyClass(i);	//람다식
Function<Integer,MyClass> f = MyClass::new;	//메서드 참조

BiFunction<Integer,String,MyClass> bf = (i,s) -> new MyClass(i,s);
BiFunction<Integer,String,MyClass> bf = MyClass::new;

배열을 생성할 때는 아래와 같이 하면 된다.
Function<Integer,int[]> f = x -> new int[x];	//람다식
Function<Integer,int[]> f = int[]::new;	//메서드 참조
```

메서드 참조는 람다식을 마치 static변수처럼 다룰 수 있게 해준다. 그리고 메서드 참조는 코드를 간략히 하는데 유용해서 맣이 사용된다.

# 스트림 stream

## 스트림이란?
지금까지 많은 수의 데이터를 다룰 때, 컬렉션이나 배열에 데이터를 담고 원하는 결과를 얻기 위해     
for문과 Iterator를 이용해서 코드를 작성해왔다. 그러나 이 방식으로 작성된 코드는 너무 길고 알아보기 어렵고 재사용성이 떨어진다.     
또 다른 문제는 데이터 소스마다 다른 방식으로 다뤄야한다는 것이다.    
Collection이나 Iterator와 같은 인터페이스를 이용해서 컬렉션을 다루는 방식을 표준화하기는 했지만,     
각 컬렉션 클래스에는 같은 기능의 메서드들이 중복해서 정의되어 있다.     
예를 들어 List를 정렬할 때는 Collections.sort()를 사용해야하고, 배열을 정렬할 떄는 Arrays.sort()를 사용해야 한다.     


이러한 문제점들을 해결하기 위해서 만든 것이 스트림이다.      
스트림은 데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의해 놓았다.     
데이터 소스를 추상화하였다는 것은,      
데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 되었다는 것과 코드의 재사용성이 높아진다는 것을 의미한다.      
스트림을 이용하면, 배열이나 컬렉션뿐만 아니라 파일에 저장된 데이터도 모두 같은 방식으로 다룰 수 있다.    
예를 들어, 문자열 배열과 같은 내용의 문자열을 저장하는 List가 있을 때,     

```java
String[] strArr = { "aaa", "ddd", "ccc" };
List<String> strList = Arrays.asList(strArr);
```

이 두 데이터 소스를 기반으로 하는 스트림은 다음과 같이 생성한다.

```java
stream<String> strStream1 = strList.stream();	//스트림을 생성
stream<String> strStream2 = Arrays.stream(strArr);	//스트림을 생성
```
이 두 스트림으로 데이터 소스의 데이터를 읽어서 정렬하고 화면에 출력하는 방법은 다음과 같다.    
데이터 소스가 정렬되는 것은 아니라는 것을 유의하여야 한다.    

```java
strStream1.sorted().forEach(System.out::println);
strStream2.sorted().forEach(System.out::println);
```

두 스트림의 데이터 소스는 서로 다르지만, 정렬하고 출력하는 방법은 완전히 동이라다.     
예전에는 아래와 같이 코드를 작성해야 했다.

```java
Arrays.sort(strArr);
Collections.sort(strList);

for(String str : strArr)
	System.out.println(str);

for(String str : strList)
	System.out.println(str);
```

스트림을 사용한 코드가 간결하고 이해하기 쉬우며 재사용성도 높다는 것을 알 수 있다.     

#### 스트림은 데이터 소스를 변경하지 않는다.
그리고 스트림은 데이터 소스로 부터 데이터를 읽기만할 뿐, 데이터 소스를 변경하지 않는다는 차이가 있다.    
필요하다면 정렬된 결과를 컬렉션이나 배열에 담아서 반환할 수도 있다.

```java
//정렬된 결과를 새로운 List에 담아서 반환한다.
List<String> sortedList = strStream2.sorted().collect(Collections.toList());
```

#### 스트림은 일회용이다.
스트림은 Iterator처럼 일회용이다.     
Iterator로 컬렉션의 요소를 모두 읽고 나면 다시 사용할 수 없는 것처럼,     
스트림도 한번 사용하면 닫혀서 다시 사용할 수 없다.    
필요하다면 스트림을 다시 생성해야 한다.    

```java
strStream1.sorted().forEach(System.out::println);
int numOfStr = strStream1.count();	//에러. 스트림이 이미 닫혔음.
```

#### 스트림은 작업을 내부 반복으로 처리한다.
스트림을 이용한 작업이 간결할 수 있는 비결중의 하나가 바로 내부반복 이다.     
내부반복이라는 것은 반복문을 메서드의 내부에 숨길 수 있다는 것을 의미한다.     
forEach()는 스트림에 정의된 메서드 중의 하나로 매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다.     

```java
for(String str : strList)
	System.out.println(str);

stream.forEach(System.out::println);
```

> 메서드 참조 System.out::println을 람다식으로 표현하면 (str) -> System.out.println(str)과 같다.

즉, forEach()는 메서드 안으로 for문을 넣은 것이다. 수행할 작업은 매개변수로 받는다.   

```java
void forEach(Consumer<? super T> action) {
	Objects.requireNonNull(action);		//매개변수의 널 체크
	
	for(T t : src)	//내부 반복
		action.accept(t);
}
```

#### 스트림의 연산
스트림이 제공하는 다양한 연산을 이용해서 복잡한 작업들을 간단히 처리할 수 있다.    
마치 데이터베이스에 SELECT문으로 질의(쿼리 query)하는 것과 같은 느낌이다.     

> 스트림에 정의된 메서드 중에서 데이터 소스를 다루는 작업을 수행하는 것을 연산이라고 한다.    

스트림이 제공하는 연산은 중간 연산과 최종 연산으로 분류할 수 있는데,     
중간 연산은 연산결과를 스트림으로 반환하기 때문에 중간 연산을 연속해서 연결할 수 있다.    
반면에 최종 연산은 스트림의 요소를 소모하면서 연산을 수행하므로 단 한번만 연산이 가능하다.    

> 중간 연산: 연산 결과가 스트림인 연산. 스트림에 연속해서 중간 연산할 수 있음.     
> 최종 연산: 연산 결과가 스트림이 아닌 연산. 스트림의 요소를 소모하므로 단 한번만 가능.

```java
stream.distinct().limit(5).sorted().forEach(System.out::println)

forEach()를 제외하고 모두 중간 연산.
```

모든 중간 연산의 결과는 스트림이지만, 연산 전의 스트림과 같은 것은 아니다.     
위 문장과 달리 모든 스트림 연산을 나누어 쓰면 아래와 같다. 각 연산의 반환타입을 주의하여야 한다.

```java
String[] str = {"dd", "aaa", "CC", "cc", "b"};
Stream<String> stream = Stream.of(strArr);	//문자열 배열이 소스인 스트림
Stream<String> filteredStream = stream.filter();	//걸러내기(중간 연산)
Stream<String> distinctedStream = stream.distinct();	//중복제거(중간 연산)
Stream<String> sortedStream = stream.sort();	//정렬(중간 연산)
Stream<String> limitedStream = stream.limit(5);	//스트림 자르기(중간 연산)
int total = stream.count();	///요소 개수 세기(최종 연산)
```

Stream에 정의된 연산을 정리하면 아래와 같다.



```java
//중간 연산
Stream<T> distinct()
중복을 제거

Stream<T> filter(Predicate<T> predicate)
조건에 안 맞는 요소 제외

Stream<T> limit(long maxSize)
스트림의 일부를 잘라낸다.

Stream<T> skip(long n)
스트림의 일부를 건너뛴다.

Stream<T> peek(Consumer<T> action)
스트림의 요소에 작업수행

Stream<T> sorted()
Stream<T> sorted(Comparator<T> comparator)
스트림의 요소를 정렬한다.

Stream<R> map(Function<T,R> mapper)
DoubleStream mapToDouble(ToDoubleFunction<T> mapper)
IntStream mapToInt(ToIntFunction<T> mapper)
LongStream mapToLong(ToLongFunction<T> mapper)

Stream<R> flatMap(Function<T,Stream<R>> mapper)
DoubleStream flatMapToDouble(Function<T,DoubleStream> m)
IntStream flatMapToInt(Function<T,IntStream> m)
LongStream flatMapToLong(Function<T,LongStream> m)
스트림의 요소를 변환한다.
```

```java
//최종 연산

void forEach(Consumer<? super T> action)
void forEachOrdered(Consumer<? super T> action)
각 요소에 지정된 작업 수행

long count()
스트림 요소의 개수 반환

Optional<T> max(Comparator<? super T> comparator)
Optional<T> min(Comparator<? super T> comparator)
스트림의 최대값/최소값을 반환

Optional<T> findAny()	//아무거나 하나
Optional<T> findFirst()	//첫 번째 요소
스트림의 요소 하나를 반환

boolean allMatch(Predicate<T> p)	//모두 만족하는지
boolean anyMatch(Predicate<T> p)	//하나라도 만족하는지
boolean noneMatch(Predicate<> p)	//모두 만족하지 않는지
주어진 조건을 모든 요소가 만족시키는지, 아닌지 확인

Object[] toArray()
A[] toArray(IntFunction<A[]> generator)
스트림의 모든 요소를 배열로 반환

Optional<T> reduce (BinaryOperator<T> accumulator)
T reduce (T identity, BinaryOperator<T> accumulator)
U reduce (U identity, BiFunction<U,T,U> accumulator, BinaryOperator<U> combiner)
스트림의 요소를 하나씩 줄여가면서(리듀싱) 계산한다.

R collect(Collector<T,A,R> collector)
R collect(Supplier<R> supplier, BiConsumer<R,T> accumulator, BiConsumer<R,R> combiner)
스트림의 요소를 수집한다. 주로 요소를 그룹화하거나 분할한 결과를 컬렉션에 담아 반환하는데 사용된다.
```

중간 연산은 map()과 flatmap(), 최종 연산은 reduce(), collect()가 핵심이다.     

> Optional은 일종의 래퍼 클래스로 내부에 하나의 객체를 저장할 수 있다.

#### 지연된 연산
스트림 연산은 최종 연산이 수정되기 전까지는 중간 연산이 수행되지 않는다.     
스트림에 대해 distinct()나 sort() 같은 중간 연산을 호출해도 즉각적인 연산이 수행되는 것은 아니라는 것이다.     
중간 연산을 호출하는 것은 단지 어떤 작업이 수행되어야하는지를 지정해주는 것일 뿐이다.     
최종 연산이 수행되어야 비로소 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 수행된다.     

#### Stream<Integer>와 IntStream
요소의 타입이 T인 스트림은 기본적으로 Stream<T>이지만,    
오토박싱&언박싱으로 인한 비효율을 줄이기 위해      
데이터 소스의 요소를 기본형으로 다루는 스트림, IntStream, LongStream, DoubleStream이 제공된다.      
일반적으로 Stream<Integer>대신 IntStream을 사용하는 것이 더 효율적이고,     
IntStream에는 int타입의 값으로 작업하는데 유용한 메서드들이 포함되어 있다.

#### 병렬 스트림
스트림으로 데이터를 다룰 때의 장점 중 하나가 바로 병렬 처리가 쉽다는 것이다.     
병렬 스트림은 내부적으로 fork&join프레임웍을 이용해서 자동적으로 연산을 병렬도 수행한다.    
우리는 그저 스트림에 parallel()이라는 메서드를 호출해서 병렬로 연산을 수행하도록 지시하면 될 뿐이다.     
반대로 병렬로 처리되지 않게 하려면 sequential()을 호출하면 된다.     
모든 스트림은 기본적으로 병렬 스트림이 아니므로 sequential()을 호출할 필요가 없다.     
이 메서드는 parallel()을 호출한 것을 취소할 때만 사용한다.     

> parallel()과 sequential()은 새로운 스트림을 생성하는 것이 아니라 스트림의 속성을 변경할 뿐이다.

```java
int sum = strStream.parallel()	//strStream을 병렬 스트릠으로 전환
			.mapToInt(s -> s.length())
			.sum();
```
> 병렬 처리가 항상 더 빠른 결과를 얻게 해주는 것은 아니다.

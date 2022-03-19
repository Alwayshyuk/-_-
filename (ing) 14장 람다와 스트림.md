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

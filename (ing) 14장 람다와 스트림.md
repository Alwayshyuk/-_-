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

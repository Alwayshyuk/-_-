### 컴파일 에러 - 컴파일 시에 발생하는 에러.
### 런타임 에러 - 실행 시에 발생하는 에러.
### 논리적 에러 - 실행은 되지만, 의도와 다르게 동작하는 것.

---------------------------------

### 에러 
* 메모리 부족(OutOfMemoryError)이나 스택오버플로우(StackOverflowError)와 같이 일단 발생하면 프로그램 코드에 의해 복구할 수 없는 심각한 오류.
### 예외 Exception 
* 프로그램 코드에 의해 수습될 수 있는 다소 미약한 오류.
 
----------------

> Exception클래스들 - 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외.   
> checked 예외 : 컴파일러가 예외처리를 확인한다.


> RuntimeException클래스들 - 프로그래머의 실수로 발생하는 예외.  
> unchecked예외 : RuntimeException과 그 자손에 해당하는 예외는 프로그래머의 실수로 발생하는 것이기 때문에 예외처리를 하지않아도 컴파일 된다.

-----------------------

# 예외처리 exception handling
 * 정의 - 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것.
 * 목적 - 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것.
 ```java
 try { 
	catch (Exception e)   { ... }
	catch (Exception e2) { ... }   }
  ```
 ------------------------------
 
 ```java
 printStrackTrace( )
 ```
 > 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.    

```java
getMessage( )
```
> 발생한 예외 클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

```java
try {
  System.out.println(0/0);
  } catch ( ArithmeticException qe) {
    ae.printStackTrace( );    //참조변수 ae를 통해, 생성된 ArithmeticException 인스턴스에 접근할 수 있다.
    System.out.println("예외메시지 : " + ae.getMessage( ) );
    }
```

> 호출스택에 대한 정보와 예외 메시지를 출력한다.

# 멀티 catch블럭
* JDK1.7부터 여러 catch블럭을 '|' 기호를 통해서 하나의 catch블럭으로 합칠 수 있게 되었다.
```java
try { ... }
 catch (Exception A | ExceptionB e) {
      e.printStackTrace();
      }
```   
      
> 만일 멀티 catch블럭의 '|' 기호로 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다.
> 두 예외 클래스가 조상과 자손의 관계에 있다면, 조상 클래스만 써주는 것과 같기 때문이다.   
    
-------------------------------
 # 예외 발생시키기
 * 키워드 throw를 사용해서 프로그래머가 고의로 예외를 발생시킬 수 있다.
 
 1. 먼저, 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든다
 2. 키워드 throw를 이용해서 예외를 발생시킨다.
 
 ```java
 Exception e = new Exception ("고의로 발생시켰음");
 throw e;
 throw new Exception("고의로 발생시켰음"); // 위 두 줄을 한 줄로 요약 가능하다.
 ```
 > Exception 인스턴스를 생성할 때, 생성자에 String을 넣어 주면, 이 String이 Exception 인스턴스에 메시지로 저장된다
 > 이 메시지는 getMessage()를 이용해서 얻을 수 있다.
 
 ----------------------
 # 메서드에 예외 선언하기
```java
void method( ) throws Exception1, Exception2, ...ExceptionN { ... }   
void method( ) throws Exception { ... }
```
> 만일 위와 같이 모든 예외의 최고조상인 Exception클래스를 메서드에 선언하면,    
> 이 메서드는 모든 종류의 예외가 발생할 가능성이 있다는 뜻이다.   
#### 예외의 개수가 아니라 상속관계까지 고려해야 한다.   
> JAVA API 문서를 통해 사용하고자 하는 메서드의 선언부와 'Throws'를 보고,   
> 이 메서드에서는 어떤 예외가 발생할 수 있으며 반드시 처리해주어야 하는 예외는 어떤 것들이 있는지 확인하는 것이 좋다.   




 


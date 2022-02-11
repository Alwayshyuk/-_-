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

--------------------------

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

----------------------------

# finally 블럭
> 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다.   
> try-catch문의 끝에 선택적으로 덧붙여 사용할 수 있으며,   
> try-catch-finally의 순서로 구성된다.

```java
	try {
		//예외가 발생할 가능성이 있는 문장들을 넣는다.
	} catch (Exception1 e1) {
		//예외처리를 위한 문장을 넣는다.
	} finally {
	 	//예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
		//finally 블럭은 try-catch문의 맨 마지막에 위치해야한다.
	}
```

> 예외가 발생한 경우에는 try-catch-finally순으로 실행되고,    
> 예외가 발생하지 않은 경우에는 try-finally순으로 실행된다.

--------------------------

# 자동 자원 반환 -try-with-resources문
> 어렵다...15장 후에 다시 보자.

-----------------------

# 사용자정의 예외 만들기
```java
class MyException extends Exception {
	MyException (String msg) {
		super(msg);
	}
}
```
> 보통 위와 같이 Exception클래스 또는 RuntimeException클래스로부터 상속받아 클래스를 만들지만,   
> 필요에 따라서 알맞은 예외 클래스를 선택할 수 있다.    
#### 가능하면 새로운 예외 클래스를 만들기보다 기존의 예외클래스를 활용하자.

```java
class MyException extends Exception {

	// 에러 코드 값을 저장하기 위한 필드를 추가 했다.
	private final int ERR_CODE;	//생성자를 통해 초기화한다.
	
	
	MyException (String msg, int errCode) {		//생성자
		super(msg);
		ERR_CODE = errCode;
	}
	
	MyException (String msg) {	//생성자
		this(msg, 100);		//ERR_CODE를 기본값(100)으로 초기화한다.
	}
	public int getErrCode() {	//에러 코드를 얻을 수 있는 메서드
		return ERR_CODE;	//이 메서드는 주로 getMessage()와 함께 사용된다.
	}
}
```

--------------------

# 예외 던지기
* 예외를 처리한 후에 인위적으로 다시 발생시키는 방법.   
> 하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용한다.   

```java
try{
	method1();
	} catch (Exception e) {
		System.out.println("main 메서드에서 예외가 처리되었습니다");
	}
}
static void method1() throws Exception{
	try {
		throw new Exception();
		} catch (Exception e) {
		System.out.println("method1 메서드에서 예외가 처리되었습니다")
		throw e;		//다시 예외를 발생시킨다.
		}
	}
}
method1 메서드에서 예외가 처리되었습니다
main 메서드에서 예외가 처리되었습니다
```
> 반환값이 있는 return문의 경우 try 블럭과 catch 블럭 모두에 return문이 있어야 한다.   
> 또는 catch블럭에서 예외 되던지기를 해서 호출한 메서드로 예외를 전달하면, return문이 없어도 된다.
> finally블럭 내에서도 return문을 사용할 수 있다. 이 경우 최종적으로 finally블럭 내의 return문의 값이 반환된다.

-------------------------------------

# 연결된 예외
> 한 예외가 다른 예외를 발생시킬 수 있다.   
> 예외 A가 예외 B를 발생시켰다면, A를 B의 '원인 예외'라고 한다.

```java
try {
	startInstaller();	//SpaceException 발생
	copyFiles();
	} catch (SpaceException e) {
	InstallException ie = new InstallException ("설치중 예외발생");	//예외 생성
	ie.initCause(e); 		// InstallException의 원인 예외를 SpaceException으로 지정
	throw ie;			//InstallException을 발생시킨다.
	} catch ( ...
```
#### Trowable initCause (Throwable cause) - 지정한 예외를 원인 예외로 등록.   
#### Throwable getCause( ) - 원인 예외를 반환.

## 원인 예외로 등록해서 다시 예외를 발생시키는 이유   
* 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해
* checked예외를 unchecked예외로 바꿀 수 있도록 하기 위해 

# 프로세스와 쓰레드
프로세스란 간단히 말해서 '실행 중인 프로그램'이다.      
프로그램을 실행하면 OS로부터 필요한 자원(메모리)을 할당받아 프로세스가 된다.        


프로세스는 프로그램을 수행하는 데 필요한 데이터와 메모리 등의 자원, 그리고 쓰레드로 구성되어 있으며     
프로세스의 자원을 이용해서 실제로 작업을 수행하는 것이 바로 쓰레드이다.     
그래서 모든 프로세스에는 최소한 하나 이상의 쓰레드가 존재하며, 둘 이상의 쓰레드를 가진 프로세스를 '멀티쓰레드 프로세스'라고 한다.


하나의 프로세스가 가질 수 있는 쓰레드의 개수는 제한되어 있지 않으나 쓰레드가 작업을 수행하는데    
개별적인 메모리 공간(호출스택)을 필요로 하기 때문에 프로세스의 메모리 한계에 따라 생성할 수 있는 쓰레드의 수가 결정된다.     


#### 멀티태스킹과 멀티쓰레딩
멀티태스킹은 다중작업을 의미하고 멀티쓰레딩은 하나의 프로세스 내에서 여러 쓰레드가 동시에 작업을 수행하는 것을 의미한다.     
CPU의 코어는 한 번에 하나의 작업만 수행하지만 처리해야하는 쓰레드의 수는 언제나 코어의 개수보다 많기 때문에      
각 코어가 짧은 시간 동안 여러 작업을 번갈아 가며 수행함으로써 여러 작업들이 동시에 수행하는 것처럼 보이게 한다.     
그래서 프로세스의 성능이 단순히 쓰레드의 개수에 비례하는 것은 아니다.


#### 멀티쓰레딩의 장단점
멀티쓰레딩의 장점
- CPU의 사용률을 향상시킨다.
- 자원을 보다 효율적으로 사용할 수 있다.
- 사용자에 대한 응답성이 향상된다.
- 작업이 분리되어 코드가 간결해진다.

메신저가 싱글쓰레드로 작성되었다면 파일을 다운로드 받는 동시에 채팅을 할 수 없을 것이다.    
또한, 싱글쓰레드로 여러 사용자에게 서비스를 제공하는 서버 프로그램을 작성한다면     
사용자의 요청 마다 새로운 프로세스를 생성해야하는데 이는 쓰레드를 생성하는 것에 비해      
많은 시간과 메모리 공간을 필요로 하기 때문에 많은 수의 사용자 요청을 서비스하기 어려울 것이다.


그러나 멀티쓰레딩에 장점만 있는 것은 아니다.    
멀티쓰레드 프로세스는 여러 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에     
동기화syncronization, 교착상태deadlock와 같은 문제들을 고려해서 프로그래밍해야 한다.

# 쓰레드의 구현과 실행

쓰레드를 구현하는 방법은 Thread클래스를 상속받는 방법과 Runnable 인터페이스를 구현하는 방법이 있다.     
Thread클래스를 상속받으면 다른 클래스를 상속받을 수 없기 때문에 Runnable인터페이스를 구현하는 방법이 일반적이다.        
Runnable인터페이스를 구현하는 방법이 재사용성이 높고 코드의 일관성을 유지할 수 있기 때문에 보다 객체지향적인 방법이다.     

```java
//Thread클래스를 상속
class MyThread extends Thread {
	public void run() { //작업내용  }	//Thread클래스의 run()을 오버라이딩
}
```

```java
//Runnable인터페이스를 구현
class MyThread implements Runnable {
	public void run() {  //작업내용  } //Runnable 인터페이스의 run()을 구현
}
```

Runnable인터페이스는 오로지 run()만 정의되어 있는 간단한 인터페이스이다.     
Runnable인터페이스를 구현하기 위해서는 추상메서드인 run()의 몸통{}을 만들어주면 된다.	      

```java
public interface Runnable {
	public abstract void run();
}
```

쓰레드를 구현한다는 것은, 위 두 방법 중 어떤 것을 선택하든 쓰레드를 통해 작업하고자 하는 내용으로 run()을 작성하는 것이다.    

```java
public class ThreadEx1 {

	public static void main(String[] args) {
		ThreadEx1_1 t1 = new ThreadEx1_1();
		
		Runnable r = new ThreadEx1_2();
		Thread t2 = new Thread(r);		//생성자 Thread(Runnable target)
		
		t1.start();
		t2.start();
	}
}
class ThreadEx1_1 extends Thread {
	public void run() {
		for(int i = 0; i<5; i++) {
			System.out.println(getName());	//조상인 Thread의 getName()을 호출
			//Thread-0	Thread-0	Thread-0	Thread-0	Thread-0
		}
	}
}
class ThreadEx1_2 implements Runnable{
	public void run() {
		for(int i = 0 ; i < 5; i++) {
			//Thread.currentThread() - 현재 실행중인 Thread를 반환한다.
			System.out.println(Thread.currentThread().getName());
			//Thread-1	Thread-1	Thread-1	Thread-1	Thread-1
		}
	}
}
```

Thread클래스를 상속받은 경우와 Runnable인터페이스를 구현한 경우의 인스턴스 생성방법이 다르다.     

```java
ThreadEx1_1 t1 = new ThreadEx1_1();		//Thread의 자손 클래스의 인스턴스를 생성

Runnable r = new ThreadEx1_2();		//Runnable을 구현한 클래스의 인스턴스를 생성
Thread t2 = new Thread(r);		//생성자 Thread(Runnable target)

Thread t2 = new Thread(new ThreadEx1_2());	//위 두 줄을 간단히.
```

Runnable 인터페이스를 구현한 경우, Runnalbe인터페이스를 구현한 클래스의 인스턴스를 생성한 다음,      
이 인스턴스를 Thread클래스의 생성자의 매개변수로 제공해야 한다.      


아래의 코드는 실제 Thread클래스의 소스코드를 이해하기 쉽게 수정한 것인데,     
인스턴스변수로 Runnable타입의 변수 r을 선언해 놓고 생성자를 통해서      
Runnable인터페이스를 구현한 인스턴스를 참조하도록 되어 있는 것을 확인할 수 있다.      
그리고 run()을 호출하면 참조변수 r을 통해서 Runnable인터페이스를 구현한 인스턴스의 run()이 호출되게 하여      
상속을 통해 run()을 오버라이딩하지 않고도 외부로부터 run()을 제공받을 수 있게 된다.    

```java
public class Thread {
	private Runnable r;	//Runnable을 구현한 클래스의 인스턴스를 참조하기 위한 변수
	
	public Thread(Runnable r) {
		this.r = r;
	}
	
	public void run() {
		if(r != null)
			r.run();		//Runnable인터페이스를 구현한 인스턴스의 run()을 호출
	}	
		...
}
```

Thread클래스를 상속받으면, 자손 클래스에서 조상인 Thread클래스의 메서드를 직접 호출할 수 있지만,    
Runnable을 구현하면 Thread클래스의 static메서드인 currentThread()를 호출하여 쓰레드에 대한 참조를 얻어와야만 호출이 가능하다.

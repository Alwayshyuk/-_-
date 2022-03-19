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
동기화synchronization, 교착상태deadlock와 같은 문제들을 고려해서 프로그래밍해야 한다.

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


> static Thread currentThread()	현재 실행중인 쓰레드의 참조를 반환한다.
> String getName() 쓰레드의 이름을 반환한다.

그래서 Thread를 상속받은 ThreadEx1_1에서 간단히 getName()을 호출하면 되지만,    
Runnable을 구현한 ThreadEx1_2에는 멤버가 run()밖에 없기 때문에 Thread클래스의 getName()을 호출하려면,     
Thread.currentThread().getName()와 같이 해야 한다.     


참고로 쓰레의 이름은 다음과 같은 생성자나 메서드를 통해 지정 또는 변경할 수 있다.

```java
Thread (Runnable target, String name)
Thread (String name)
void setName(String name)
```

예제13-1의 결과에서도 알 수 있듯이 쓰레드의 이름을 지정하지 않으면 Thread-번호의 형식으로 이름이 정해진다.     
그리고 System.out.println(Thread.currentTread().getName());는 아래의 코드를 한 줄로 줄여 쓴 것이다.

```java
Thread t = Thread.currentThread();
String name = t.getName();
System.out.println(name);
```

#### 쓰레드의 실행-start()
쓰레드를 생성했다고 해서 자동으로 실행되는 것은 아니다. start()를 호출해야만 쓰레드가 실행된다.    

```java
t1.start();	//쓰레드 t1을 실행시킨다.
t2.start();	//쓰레드 t2를 실행시킨다.
```

사실 start()가 호출되었다고 해서 바로 실행되는 것이 아니라, 일단 실행대기 상태에 있다가 자신의 차례가 되어야 실행된다.     
만약 실행대기중인 쓰레드가 하나도 없다면 곧바로 실행상태가 된다.     

> 쓰레드의 실행순서는 OS의 스케줄러가 작성한 스케쥴에 의해 결정된다.

한 번 실행이 종료된 쓰레드는 다시 실행할 수 없다. 즉, 하나의 쓰레드에 대해 start()가 한 번만 호출될 수 있다는 뜻이다.     
그래서 만일 쓰레드의 작업을 한 번 더 수행해야 한다면 아래의 오른쪽 코드와 같이 새로운 쓰레드를 생성한 다음 start()를 호출해야 한다.      
만일 하나의 쓰레드에 대해 start()를 두 번 이상 호출하면 실행시에 IllegalThreadStateException이 발생한다.

```java
ThreadEx1_1 t1 = new ThreadEx1_1();
t1.start();
t1.start();	//예외 발생

ThreadEx1_1 t1 = new ThreadEx1_1();
t1.start();
t1 = new ThreadEx1_1();	//다시 생성
t1.start();
```
# start()와 run()

main메서드에서 run()을 호출하는 것은 생성된 쓰레드를 실행시키는 것이 아니라 단순히 클래스에 선언된 메서드를 호출하는 것일 뿐이다.      
반면에 start()는 새로운 쓰레드가 작업을 실행하는데 필요한 호출스택call stack을 생성한 다음에 run()을 호출해서,    
생성된 호출스택에 run()이 첫 번째로 올라가게 한다.     
모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출스택을 필요로 하기 때문에,     
새로운 쓰레드를 생성하고 실행시킬 때마다 새로운 호출스택이 생성되고 쓰레드가 종료되면 작업에 사용된 호출스택은 소멸된다.    

1. main메서드에서 쓰레드의 start()를 호출한다.
2. start()는 새로운 쓰레드를 생성하고, 쓰레드가 작업하는데 사용될 호출스택을 생성한다.
3. 새로 생성된 호출스택에 run()이 호출되어, 쓰레드가 독립된 공간에서 작업을 수행한다.
4. 이제는 호출스택이 2개이므로 스케줄러가 정한 순서에 의해서 번갈아 가면서 실행된다.

쓰레드가 둘 이상일 때는 호출스택의 최상위에 있는 메서드일지라도 대기상태에 있을 수 있다.      
스케줄러는 실행대기중인 쓰레드들의 우선순위를 고려하여 실행순서와 실행시간을 결정하고,     
각 쓰레드들은 작성된 스케줄에 따라 자신의 순서가 되면 지정된 시간동안 작업을 수행한다.      
주어진 시간동안 작업을 마치지 못한 쓰레드는 다시 자신의 차례가 돌아올 때까지 대기상태로 있게 되며,     
작업을 마친 쓰레드, 즉 run()의 수행이 종료된 쓰레드는 호출스택이 모두 비워지면서 이 쓰레드가 사용하던 호출스택은 사라진다.     

#### main쓰레드
main메서드의 작업을 수행하는 것을 main쓰레드라고 한다.     
프로그램이 실행되기 위해서 기본적으로 하나의 쓰레드를 생성하고 그 쓰레드가 main메서드를 호출해서 작업이 수행되도록 하는 것이다.     
> 실행 중인 사용자 쓰레드가 하나도 없을 때 프로그램은 종료된다.      

쓰레드는 사용자 쓰레드user thread(non-daemon thread)와 데몬 쓰레드 daemon thread 두 종류가 있다.

```java
package tmp;

public class ThreadEx2 {

	public static void main(String[] args) {
		ThreadEx2_1 t1 = new ThreadEx2_1();
		t1.start();
		//java.lang.Exception
		//at tmp.ThreadEx2_1.throwException(ThreadEx2.java:17)
		//at tmp.ThreadEx2_1.run(ThreadEx2.java:12)
	}
}
class ThreadEx2_1 extends Thread	{
	public void run()	{
		throwException();
	}
	
	public void throwException() {
		try {
			throw new Exception();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

새로 생성한 쓰레드에서 고의로 예외를 발생시키고 printStackTrace()를 이용해서 예외가 아니라 발생한 당시의 호출스택을 출력하는 예제이다.      
호출스택의 첫 메서드가 main이 아니라 run메서드이다.     
한 쓰레드가 예외 발생으로 종료되어도 다른 쓰레드의 실행에는 영향을 미치지 않는다.     
main쓰레드가 호출스택이 없는 이유는 main쓰레드가 이미 종료되었기 때문이다.     

```java
public class ThreadEx3 {

	public static void main(String[] args) {
		ThreadEx3_1 t1 = new ThreadEx3_1();
		t1.run();
		//java.lang.Exception
		//at tmp.ThreadEx3_1.throwException(ThreadEx3.java:17)
		//at tmp.ThreadEx3_1.run(ThreadEx3.java:13)
		//at tmp.ThreadEx3.main(ThreadEx3.java:7)
	}
}
class ThreadEx3_1 extends Thread	{
	public void run() {
		throwException();
	}
	public void throwException() {
		try {
			throw new Exception();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
이전 예제와 달리 쓰레드가 새로 생성되지 않았다. 그저 ThreadEx3_1클래스의 run()이 호출되었을 뿐이다.     

# 싱글쓰레드와 멀티쓰레드
하나의 쓰레드로 두 작업을 처리하는 경우는 한 작업을 마친 후에 다른 작업을 시작하지만,     
두 개의 쓰레드로 작업을 하는 경우에는 짧은 시간동안 2개의 쓰레드가 번갈아 가면서 작업을 수행해 동시에 두 작업이 처리되는 것처럼 보인다.     
위의 경우 멀티쓰레드로 작업한 시간이 싱글쓰레드로 작업한 시간보다 더 걸리게 되는데,    
이는 쓰레드간의 작업 전환context switching에 시간이 걸리기 때문이다.     
작업 전환을 할 때는 현재 진행 중인 작업의 상태, 예를 들면 다음에 실행해야할 위치 등의 정보를 저장하고 읽어 오는 시간이 소요된다.     
참고로 쓰레드의 스위칭에 비해 프로세스의 스위칭이 더 많은 정보를 저장해야하므로 더 많은 시간이 소요된다.

```java
public class TreadEx4 {

	public static void main(String[] args) {
		long startTime = System.currentTimeMillis();
		
		for(int i =0; i<300; i++)
			System.out.printf("%s", new String("-"));
		
		System.out.println("\n소요시간 1: " + (System.currentTimeMillis() - startTime));
		//소요시간 1: 20
		
		for(int i = 0; i<300; i++)
			System.out.printf("%s", new String("|"));
		
		System.out.println("\n소요시간 2: "+ (System.currentTimeMillis() - startTime));
		//소요시간 2: 24
	}
}
```

'-'를 출력하는 작업과 '|'를 출력하는 작업을 하나의 쓰레드가 연속적으로 처리하는 시간을 측정하는 예제이다.      
수행시간을 측정하기 쉽게 "-"대신 new String("-")를 사용해서 수행속도를 늦췄다.     

```java
public class ThreadEx5 {
	static long startTime = 0;
	
	public static void main(String[] args) {
		
		ThreadEx5_1 t1 = new ThreadEx5_1();
		t1.start();
		startTime = System.currentTimeMillis();
		
		for(int i = 0; i<300; i++)
			System.out.printf("%s", new String("-"));
		
		System.out.print("소요시간 1: "+(System.currentTimeMillis() - startTime));
		//싱글코어 ---|||---|||---|||소요시간2: 26소요시간 1: 28
		//듀얼코어 -||------|||||||--|-|||| 소요시간2: 23 소요시간1: 25
	}
}
class ThreadEx5_1 extends Thread	{
	public void run()	{
		for(int i = 0; i<300; i++)
			System.out.printf("%s", new String("|"));
		
		System.out.print("소요시간2: " + (System.currentTimeMillis()-ThreadEx5.startTime));
	}
}
```

새로운 쓰레드를 하나 생성해서 두 개의 쓰레드가 작업을 하나씩 나누어서 수행한 예제이다.        
이전 예제와 달리 두 작업이 아주 짧은 시간동안 번갈아가면서 실행되었으며 거의 동시에 작업이 완료되었음을 알 수 있다.     
두 개의 쓰레드로 작업을 한 경우가 더 많은 시간을 소요했는데 그 이유는 두 가지다.    
하나는 두 쓰레드가 번갈아가면서 작업을 처리하기 때문에 쓰레드간의 작업전환시간이 소요되기 때문이고,     
나머지 하나는 한 쓰레드가 화면에 출력하고 있는 동안 다른 쓰레드는 출력이 끝나기를 기다려야하므로 이때 발생하는 대기시간 때문이다.


싱글 코어인 경우에는 멀티쓰레드라도 하나의 코어가 번갈아가면서 작업을 수행하는 것이므로 두 작업이 절대 겹치지 않는다.     
그러나 멀티 코어에서는 멀티쓰레드로 두 작업을 수행하면, 동시에 두 쓰레드가 수행될 수 있으므로 두 작업이 겹치는 부분이 발생한다.     
> 여러 쓰레드가 여러 작업을 동시에 진행하는 것을 병행concurrent라고 하고,      
> 하나의 작업을 여러 쓰레드가 나눠서 처리하는 것을 병렬parallel이라고 한다.     

위 예제의 결과는 실행할 때마다 다른 결과를 얻을 수 있는데       
그 이유는 실행 중인 예제프로그램(프로세스)이 OS의 프로세스 스케줄러의 영향을 받기 때문이다.       
JVM의 쓰레드 스케줄러에 의해서 어떤 쓰레드가 얼마동안 실행될 것인지 결정되는 것과 같이    
프로세스도 프로세스 스케줄러에 의해서 실행순서와 실행시간이 결정되기 때문에        
매 순간 상황에 따라 프로세스에게 할당되는 실행시간이 일정하지 않고 쓰레드에게 할당되는 시간도 일정하지 않게 된다.     
그래서 쓰레드가 이러한 불확실성을 가지고 있다는 것을 염두에 두어야 한다.     
자바가 OS(플랫폼)에 독립적이라고는 하지만 실제로는 OS종속적인 부분이 몇 가지 있는데 쓰레드도 그 중의 하나이다.     


두 쓰레드가 서로 다른 자원을 사용하는 작업의 경우에는 싱글쓰레드 프로세스보다 멀티 쓰레드 프로세스가 더 효율적이다.        
예를 들어 사용자로부터 데이터를 입력받는 작업, 네트워크로 파일을 주고받는 작업,       
프린터로 파일을 출력하는 작업과 같이 외부기기와의 입출력을 필요로 하는 경우가 이에 해당한다.       
만일 사용자로부터 입력받는 작업과 화면에 출력하는 작업을 하나의 쓰레드로 처리한다면      
사용자가 입력을 마칠 때까지 아무 일도 하지 못하고 기다리기만 해야한다.     
그러나 두 개의 쓰레드로 처리한다면 사용자의 입력을 기다리는 동안         
다른 쓰레드가 작업을 처리할 수 있기 때문에 보다 효율적인 CPU의 사용이 가능하다.     

```java
import javax.swing.JOptionPane;
public class ThreadEx6 {

	public static void main(String[] args) {
		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
		System.out.printf("입력하신 값은 %s 입니다.\n", input);
		
		for(int i = 10; i>0; i--) {
			System.out.println(i);
			
			try {
				Thread.sleep(1000);	//1초간 시간을 지연시킨다.
			} catch (Exception e) {}
		}
		//입력하신 값은 헤헿 입니다.	10	9	8	7	6	5	4	3	2	1
	}
}
```

이 예제는 하나의 쓰레드로 사용자의 입력을 받는 작업과 화면에 숫자를 출력하는 작업을 처리하기 때문에      
사용자가 입력을 마치기 전까지는 화면에 숫자가 출력되지 않다가 사용자가 입력을 마치고 나서야 화면에 숫자가 출력된다.     

```java
import javax.swing.JOptionPane;
public class ThreadEx7 {

	public static void main(String[] args) {
		ThreadEx7_1 t1 = new ThreadEx7_1();
		t1.start();
		
		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
		System.out.printf("입력하신 값은 %s 입니다.\n", input);
		//10	9	8	7	입력하신 값은 아무 값이나 입니다.	6	5	4	3	2	1
	}
}
class ThreadEx7_1 extends Thread{
	public void run()	{
		for(int i = 10; i>0; i--) {
			System.out.println(i);
			try {
				Thread.sleep(1000);
			} catch (Exception e) {}
		}
	}
}
```
이전 예제와는 달리 사용자로부터 입력받는 부분과 화면에 숫자를 출력하는 부분을 두 개의 쓰레드로 나누어 처리했기 때문에      
사용자가 입력을 마치지 않았어도 화면에 숫자가 출력되는 것을 알 수 있다.    

# 쓰레드의 우선순위
쓰레드는 우선순위priority라는 속성(멤버변수)을 가지고 있는데, 이 우선순위의 값에 따라 쓰레드가 얻는 실행시간이 달라진다.    
쓰레드가 수행하는 작업의 중요도에 따라 쓰레드의 우선순위를 서로 다르게 지정하여 특정 쓰레드가 더 많은 작업시간을 갖도록 할 수 있다.     
예를 들어 파일전송기능이 있는 메신저의 경우, 파일다운로드를 처리하는 쓰레드보다 채팅내용을 전송하는 쓰레드의 우선순위가 더 높아야       
사용자가 채팅하는데 불편함이 없을 것이다. 대신 파일다운로드 작업에 걸리는 시간은 더 길어질 것이다.       
이처럼 시각적인 부분이나 사용자에게 빠르게 반응해야하는 작업을 하는 쓰레드의 우선순위는 다른 작업을 수행하는 쓰레드에 비해 높아야 한다.    

#### 쓰레드의 우선순위 지정하기

```java
void setPriority(int newPriortity)	//쓰레드의 우선순위를 지정한 값으로 변경한다.
int getPriority()		//쓰레드의 우선순위를 반환한다.

public static final int MAX_PRIORITY = 10	//최대우선순위
public static final int MIN_PRIORITY = 1	//최소우선순위
public static final int NOR_PRIORITY = 5	//보통우선순위
```

쓰레드가 가질 수 있는 우선순위의 범위는 1~10이며 높을수록 우선순위가 높다.      
쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속받는다.    
main메서드를 수행하는 쓰레드는 우선순위가 5이므로 main메서드내에서 생성하는 쓰레드의 우선순위는 자동으로 5가 된다.

```java
public class ThreadEx8 {

	public static void main(String[] args) {
		ThreadEx8_1 t1 = new ThreadEx8_1();
		ThreadEx8_2 t2 = new ThreadEx8_2();
		
		t2.setPriority(10);
		
		System.out.println(t1.getPriority());	//5
		System.out.println(t2.getPriority());	//10
		
		t1.start();
		t2.start();
		//싱글코어-|--|||||||||||||------------------
		//멀티코어-|-|-|-|-|-|-|-|-|-|-|-|-|-
	}
}
class ThreadEx8_1 extends Thread	{
	public void run()	{
		for(int i = 0; i<300; i++) {
			System.out.print("-");
			for(int x = 0; x<10000000; x++);
		}
	}
}
class ThreadEx8_2 extends Thread	{
	public void run()	{
		for(int i = 0 ; i<300; i++) {
			System.out.print("|");
			for(int x = 0; x<10000000; x++);
		}
	}
}
```

t1과 t2 모두 main메서드에서 생성하였기 때문에 main메서드를 실행하는 쓰레드의 우선순위인 5를 상속받았다.       
그 다음에 t2.setPriority(10)으로 t2의 우선순위를 10으로 변경한 다음에 start()를 호출해서 쓰레드를 실행시켰다.     
이처럼 쓰레드를 실행하기 전에만 우선순위를 변경할 수 있다.     

# 쓰레드 그룹 thread group
쓰레드 그룹은 서로 관련된 쓰레드를 그룹으로 다루기 위한 것으로, 폴더를 생성해서 관련된 파일들을 함께 넣어서 관리하는 것처럼      
쓰레드 그룹을 생성해서 쓰레드를 그룹으로 묶어서 관리할 수 있다.     
또한 폴더 안에 폴더를 생성할 수 있듯이 쓰레드 그룹에 다른 쓰레드 그룹을 포함시킬 수 있다.      
쓰레드 그룹은 보안상의 이유로 도입된 개념으로, 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경할 수 있지만      
다른 쓰레드 그룹의 쓰레드를 변경할 수는 없다.     
ThreadGroup을 사용해서 생성할 수 있으며, 주요 생성자와 메서드는 다음과 같다.

```java
ThreadGroup(String name)
//지정된 이름의 새로운 쓰레드 그룹 생성
ThreadGroup(ThreadGroup parent, String name)
//지정된 쓰레드 그룹에 포함되는 새로운 쓰레드 그룹을 생성
int activeCount()
//쓰레드 그룹에 포함된 활성상태에 있는 쓰레드의 수를 반환
int activeGroupCount()
//쓰레드 그룹에 포함된 
있는 쓰레드 그룹의 수를 반환
void checkAccess()
//현재 실행중인 쓰레드가 쓰레드 그룹을 변경할 권한이 있는지 체크.
//만일 권한이 없다면 SecurityException을 발생시킨다.
void destroy()
//쓰레드 그룹이나 하위 쓰레드 그룹까지 모두 삭제한다.
//단, 쓰레드 그룹이나 하위 쓰레드 그룹이 비어있어야 한다.
int enumerate(Thread[] list)
int enumerate(Thread[] list, boolean recurse)
int enumerate(ThreadGroup[] list)
int enumerate(ThreadGroup[] list, boolean recurse)
//쓰레드 그룹에 속한 쓰레드 또는 하위 쓰레드 그룹의 목록을 지정된 배열에 담고 그 개수를 반환
//recurse를 true로 하면 쓰레드 그룹에 속한 하위 쓰레드 그룹에 쓰레드 또는 쓰레드 그룹까지 배열에 담는다.
int getMaxPriority()
//쓰레드 그룹의 최대 우선순위를 반환
String getName()
//쓰레드 그룹의 이름을 반환
ThreadGroup getParent()
//쓰레드 그룹의 상위 쓰레드그룹을 반환
void interrupt()
//쓰레드 그룹에 속한 모든 쓰레드를 interrupt
boolean isDaemon()
//쓰레드 그룹이 데몬 쓰레드그룹인지 확인
boolean isDestroyed()
//쓰레드 그룹이 삭제되었는지 확인
void list()
//쓰레드 그룹에 속한 쓰레드와 하위 쓰레드 그룹에 대한 정보를 출력
boolean parentOf(ThreadGroup g)
//지정된 쓰레드 그룹의 상위 쓰레드 그룹인지 확인
void setDaemon(boolean daemon)
//쓰레드 그룹을 데몬 쓰레드 그룹으로 설정/해제
void setMaxPriortity(int pri)
//쓰레드 그룹의 최대우선순위를 설정

//쓰레드를 쓰레드 그룹에 포함시키려면 Thread의 생성자를 이용해야 한다.
Thread(Thread group, String name)
Thread(Thread group, Runnable target)
Thread(Thread group, Runnable target, String name)
Thread(Thread group, Runnable target, String name, long stackSize)

//그 외의 Thread의 쓰레드 그룹과 관련된 메서드
ThreadGroup getThreadGroup()
//쓰레드 자신이 속한 쓰레드 그룹을 반환한다.
void uncaughtException(Thread t, Throwable e)
///쓰레드 그룹의 쓰레드가 처리되지않은 예외에 의해 실행이 종료되었을 때,    
//JVM에 의애 이 메서드가 자동적으로 호출된다.
```



모든 쓰레드는 반드시 쓰레드 그룹에 포함되어 있어야 하기 때문에, 위와 같이 쓰레드 그룹을 지정하는 생성자를 사용하지 않은 쓰레드는     
기본적으로 자신을 생성한 쓰레드와 같은 쓰레드 그룹에 속하게 된다.      
자바 어플리케이션이 실행되면, JVM은 main과 system이라는 쓰레드 그룹을 만들고     
JVM운영에 필요한 쓰레드들을 생성해서 이 쓰레드 그룹에 포함시킨다.     
우리가 생성하는 모든 쓰레드 그룹은 main쓰레드 그룹의 하위 쓰레드 그룹이 되며,      
쓰레드 그룹을 지정하지 않고 생성한 쓰레드는 자동적으로 main쓰레드 그룹에 속하게 된다.

```java
public class ThreadEx9 {

	public static void main(String[] args) {
		ThreadGroup main = Thread.currentThread().getThreadGroup();
		ThreadGroup grp1 = new ThreadGroup("Group1");
		ThreadGroup grp2 = new ThreadGroup("Group2");
		
		//ThreadGroup(ThreadGroup parent, String name)
		ThreadGroup subGrp1 = new ThreadGroup(grp1, "subGroup1");
		
		grp1.setMaxPriority(3);	//쓰레드 그룹grp1의 최대우선순위를 3으로 변경.
		
		Runnable r = new Runnable() {
			public void run() {
				try {
					Thread.sleep(1000);
				} catch (Exception e) {}
			}
		};
		//Thread(ThreadGroup tg, Runnable r, String name)
		new Thread(grp1, r, "th1").start();
		new Thread(subGrp1, r, "th2").start();
		new Thread(grp2, r, "th3").start();
		
		System.out.println(main.getName());			//main
		System.out.println(main.activeGroupCount());	//3
		System.out.println(main.activeCount());		//4
		main.list();
//		java.lang.ThreadGroup[name=main,maxpri=10]
//			    Thread[main,5,main]
//			    java.lang.ThreadGroup[name=Group1,maxpri=3]
//			        Thread[th1,3,Group1]
//			        java.lang.ThreadGroup[name=subGroup1,maxpri=3]
//			            Thread[th2,3,subGroup1]
//			    java.lang.ThreadGroup[name=Group2,maxpri=10]
//			        Thread[th3,5,Group2]
	}
}
```
쓰레드 그룹과 쓰레드를 생성하고 main.list()를 호출해서 main쓰레드 그룹의 정보를 출력하는 예제이다.     
쓰레드 그룹에 대한 정보를 출력하기도 전에 쓰레드가 종료될 수 있으므로, sleep()을 호출하여 1초간 멈추게 하였다.         
결과를 보면 쓰레드 그룹에 포함된 하위 쓰레드 그룹이나 쓰레드는 들여쓰기를 이용해서 구별하였다.     
새로 생성한 모든 쓰레드 그룹은 main쓰레드 그룹의 하위 쓰레드 그룹으로 포함되어 있다는 것과     
setMaxPriority()는 쓰레드가 쓰레드 그룹에 추가되기 이전에 호출되어야 하며,     
쓰레드 그룹grp1의 최대우선순위를 3으로 했기 때문에, 후에 여기 속하게 된 쓰레드 그룹과 쓰레드가 영향을 받았다는 것을 확인할 수 있다.     
그리고 참조변수 없이 쓰레드를 생성해서 바로 실행시켰는데 그렇다고 이 쓰레드가 가비지 컬렉터의 제거 대상이 되지는 않는다.     
이 쓰레드의 참조가 ThreadGroup에 저장되어 있기 때문이다.

# 데몬 쓰레드 daemon Thread

데몬 쓰레드는 다른 일반 쓰레드(비 데몬 쓰레드)의 작업을 돕는 보조적인 역할을 수행하는 쓰레드이다.     
데몬 쓰레드는 일반 쓰레드의 보조역할을 수행하므로 일반 쓰레드가 종료되면,     
데몬 쓰레드의 존재 의미가 없어져 데몬 쓰레드는 강제적으로 자동 종료된다.     
이를 제외하곤 데몬 쓰레드와 일반 쓰레드는 크게 다르지 않다.     
데몬 쓰레드의 예로는 가비지 컬렉터, 워드프로세서의 자동저장, 화면자동갱신 등이 있다.      


데몬 쓰레드는 무한루프와 조건문을 이용해서 실행 후 대기하고 있다가 특정 조건이 만족되면 작업을 수행하고 다시 대기하도록 작성한다.        
데몬 쓰레드는 일반 쓰레드의 작성방법과 실행방법이 같으며 다만 쓰레드를 생성한 다음 실행 전에 setDaemon(true)를 호출하면 된다.     
그리고 데몬 쓰레드가 생성한 쓰레드는 자동적으로 데몬 쓰레드가 된다.        

```java
boolean isDaemon()
//쓰레드가 데몬인지 확인한다.
void setDaemon(boolean on)
// 쓰레드를 데몬 쓰레드 또는 사용자 쓰레드로 변경한다.
//매개변수 on이 true면 데몬 쓰레드가 된다.
```

```java
public class ThreadEx10 implements Runnable{
	static boolean autoSave = false;

	public static void main(String[] args) {
		Thread t = new Thread(new ThreadEx10());
		t.setDaemon(true);	//이 부분이 없으면 종료되지 않는다.
		t.start();
		
		for(int i = 1; i<=10; i++) {
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {}
				System.out.println(i);
				
				if(i==5)
					autoSave = true;
		}
		System.out.println("프로그램을 종료합니다.");
	}
	public void run()	{
		while(true) {
			try {
				Thread.sleep(3*1000);
			} catch (InterruptedException e) {}
			//autoSave의 값이 true면 autoSave()를 호출한다.
			if(autoSave)
				autoSave();
		}
	}
	public void autoSave() {
		System.out.println("작업 파일이 자동 저장되었습니다.");
	}
}
```
3초마다 변수 autoSave의 값을 확인해서 그 값이 true면, autoSave()를 호출하는 일을 반복하도록 쓰레드를 작성하였다.    
만일 이 쓰레드를 데몬 쓰레드로 설정하지 않았다면, 이 프로그램을 강제종료하지 않는 한 종료되지 않을 것이다.    
setDaemon메서드는 반드시 start()를 호출하기 전에 실행되어야 한다. 그렇지 않으면 IllegalThreadStateException이 발생한다.     

```java
import java.util.*;

public class ThreadEx11{
	
	public static void main(String args[]) throws Exception{
		ThreadEx11_1 t1 = new ThreadEx11_1("Thread1");
		ThreadEx11_2 t2 = new ThreadEx11_2("Thread2");
		t1.start();
		t2.start();
		
//		[1] name : Signal Dispatcher, group: system, damone: true
//
//		[2] name : Notification Thread, group: system, damone: true
//
//		[3] name : Thread1, group: main, damone: false
//		java.base@17.0.1/java.lang.Thread.sleep(Native Method)
//		app//tmp.ThreadEx11_1.run(ThreadEx11.java:20)
//
//		[4] name : Reference Handler, group: system, damone: true
//		java.base@17.0.1/java.lang.ref.Reference.waitForReferencePendingList(Native Method)
//		java.base@17.0.1/java.lang.ref.Reference.processPendingReferences(Reference.java:253)
//		java.base@17.0.1/java.lang.ref.Reference$ReferenceHandler.run(Reference.java:215)
//
//		[5] name : Attach Listener, group: system, damone: true
//
//		[6] name : Thread2, group: main, damone: false
//		java.base@17.0.1/java.lang.Thread.dumpThreads(Native Method)
//		java.base@17.0.1/java.lang.Thread.getAllStackTraces(Thread.java:1662)
//		app//tmp.ThreadEx11_2.run(ThreadEx11.java:30)
//
//		[7] name : Finalizer, group: system, damone: true
//		java.base@17.0.1/java.lang.Object.wait(Native Method)
//		java.base@17.0.1/java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:155)
//		java.base@17.0.1/java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:176)
//		java.base@17.0.1/java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:172)
//
//		[8] name : Common-Cleaner, group: InnocuousThreadGroup, damone: true
//		java.base@17.0.1/java.lang.Object.wait(Native Method)
//		java.base@17.0.1/java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:155)
//		java.base@17.0.1/jdk.internal.ref.CleanerImpl.run(CleanerImpl.java:140)
//		java.base@17.0.1/java.lang.Thread.run(Thread.java:833)
//		java.base@17.0.1/jdk.internal.misc.InnocuousThread.run(InnocuousThread.java:162)
	}
}

class ThreadEx11_1 extends Thread	{
	ThreadEx11_1(String name){
		super(name);
	}
	public void run() {
		try {
			sleep(5*1000);	//5초 기다린다.
		} catch (InterruptedException e) {}
	}
}

class ThreadEx11_2 extends Thread	{
	ThreadEx11_2(String name){
		super(name);
	}
	public void run() {
		Map map = getAllStackTraces();
		Iterator it = map.keySet().iterator();
		
		int x = 0;
		
		while(it.hasNext()) {
			Object obj = it.next();
			Thread t = (Thread)obj;
			StackTraceElement[] ste = (StackTraceElement[])(map.get(obj));
			
			System.out.println("["+ ++x + "] name : "+ t.getName() + ", group: " + t.getThreadGroup().getName()+", damone: "+ t.isDaemon());
			
			for(int i = 0; i<ste.length; i++) {
				System.out.println(ste[i]);
			}
			System.out.println();
		}
	}
}
```
getAllStackTraces()을 이용하면 실행 중 또는 대기상태, 즉 작업이 완료되지 않은 모든 쓰레드의 호출스택을 출력할 수 있다.     
결과를 보며 getAllStackTrace()가 호출되었을 때, 새로 생성한 Thread1, Thread2를 포함해서      
모두 9개의 쓰레드가 실행 중 또는 대기상태에 있다는 것을 알 수 있다.    

프로그램을 실행하면 JVM은 가비지컬렉션, 이벤트처리, 그래픽처리와 같이 프로그램이 실행되는 데 필요한 보조작업을 수행하는      
데몬 쓰레드들을 자동적으로 생성해서 실행시킨다. 그리고 이들은 system쓰레드 그룹 또는 main쓰레드 그룹에 속한다.     
AWT나 Swing과 같이 GUI를 가진 프로그램을 실행하는 경우에는 이벤트와 그래픽처리를 위해 더 많은 수의 데몬 쓰레드가 생성된다.    
> main쓰레드는 이미 종료되었기 때문에 결과에 포함되지 않았다.     

# 쓰레드의 실행제어
쓰레드 프로그래밍이 어려운 이유는 동기화synchronization와 스케줄링scheduling 때문이다.      
효율적인 멀티쓰레드 프로그램을 만들기 위해서는 보다 정교한 스케줄링을 통해     
프로세스에게 주어진 자원과 시간을 여러 쓰레드가 낭비없이 잘 사용하도록 프로그래밍 해야 한다.     
쓰레드의 스케줄링을 잘하기 위해서는 쓰레드의 상태와 관련 메서드를 잘 알아야 하는데,     
먼저 쓰레드의 스케줄링과 관련된 메서드는 다음과 같다.     

```java
static void sleep(long millis)
static void sleep(long millis, int nanos)
//지정된 시간동안 쓰레드를 일시정지시킨다.
//지정한 시간이 지나면 자동적으로 다시 실행대기상태가 된다.

void join()
void join(int millis)
void join(int millis, int nanos)
//지정된 시간동안 쓰레드가 실행되도록 한다.
//지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 쓰레드로 다시 돌아와 실행을 계속한다.

void interrupt()
//sleep()이나 join()에 의해 일시정지상태인 쓰레드를 깨워서 실행대기상태로 만든다.
//해당 쓰레드에서는 interruptedException이 발생함으로써 일시정지상태를 벗어나게 한다.

void stop()
//쓰레드를 즉시 종료시킨다.

void suspend()
//쓰레드를 일시정지시킨다.
//resume()을 호출하면 다시 실행대기상태가 된다.

void resume()
//suspend()에 의해 일시정지상태에 있는 쓰레드를 실행대기상태로 만든다.

static void yield()
//실행 중에 자신에게 주어진 실행시간을 다른 쓰레드에게 양보yield하고 자신은 실행대기상태가 된다.
```
> resume(), stop(), suspend()는 쓰레드를 교착상태로 만들기 쉽기 때문에 deprecated되었다.

쓰레드의 상태.
- NEW : 쓰레드가 생성되고 아직 start()가 호출되지 않은 상태
- RUNNABLE : 실행 중 또는 실행 가능한 상태
- BLOCKED : 동기화 블럭에 의해서 일시정지된 상태(lock이 풀릴 때까지 기다리는 상태)
- WAITING, TIMED_WAITING : 쓰레드의 작업이 종료되지는 않았지만 실행가능하지 않은 일시정지 상태
- TIME_WAITING은 일시정지시간이 지정된 경우를 의미한다.
- TERMINATED : 쓰레드의 작업이 종료된 상태

> 쓰레드의 상태는 Thread의 getState()메서드를 호출해서 확인할 수 있다.

#### sleep(long millis) - 일정시간동안 쓰레드를 멈추게 한다.
sleep()은 지정된 시간동안 쓰레드를 멈추게 한다.
> static void sleep(long millis)        
> static void sleep(long millis, int nanos)        

밀리세컨드와 나노세컨드의 시간단위로 세밀하게 값을 지정할 수 있지만 어느 정도의 오차가 발생할 수 있다.    
sleep()에 의해 일시정지 상태가 된 쓰레드는 지정된 시간이 다 되거나 interrupt()가 호출되면(InterruptedException발생),     
잠에서 꺠어나 실행대기 상태가 된다.    
그래서 sleep()을 호출할 때는 항상 try-catch문으로 예외를 처리해줘야 한다.     
매번 예외처리를 해주는 것이 번거롭기 때문에 아래와 같이 메서드를 만들어서 사용하기도 한다.    

```java
void delay(long millis) {
	try{
		Thread.sleep(millis);
	}catch{InterruptedException e) {}
```

```java
public class ThreadEx12 {
	public static void main(String[] args) {
		ThreadEx12_1 th1 = new ThreadEx12_1();
		ThreadEx12_2 th2 = new ThreadEx12_2();
		th1.start();
		th2.start();
		
		try {
			th1.sleep(2000);
		} catch (InterruptedException e) {}
	
		System.out.println("<<main 종료>>");
	}
}
class ThreadEx12_1 extends Thread {
	public void run() {
		for(int i = 0; i<300; i++)
			System.out.print("-");
		System.out.println("<<th1 종료>>");
	}
}
class ThreadEx12_2 extends Thread {
	public void run() {
		for(int i = 0; i<300; i++)
			System.out.print("|");
		System.out.println("<<th2 종료>>");
	}
}
//||----|||---||||||------------<<th1 종료>>
//|||||||||<<th2 종료>>
//<<main 종료>>
```

th1과 th2에 대해 start()를 호출하자마자 th1.sleep(2000)을 호출하여 쓰레드 th1이 2초 동안 작업을 멈추고 일시정지 상태에 있었으므로     
th1이 가장 늦게 종료되어야 하는데 가장 먼저 종료되었다.      
그 이유는 sleep()이 항상 현재 실행 중인 쓰레드에 대해 작동하기 떄문에 th1.sleep(2000)과 같이 호출하였어도    
실제로 영향을 받는 것은 main메서드를 실행하는 main쓰레드이다.       
그래서 sleep()은 static으로 선언되어 있으며 참조변수를 이용해서 호출하기 보다는 Thread.sleep(2000);과 같이 해야한다.     

#### interrupt()와 interrupted() - 쓰레드의 작업을 취소한다.
interrupt()는 쓰레드에게 작업을 멈추라고 요청한다. 단지 멈추라고 요청하는 것일 뿐 쓰레드를 강제로 종료시키지는 못한다.     
interrupt()는 그저 쓰레드의 interrupted상태(인스턴스 변수)를 바꾸는 것일 뿐이다.     
interrupted()는 쓰레드에 대해 interrupt()가 호출되었는지 알려준다.      
interrupt()가 호출되었다면 true, 그렇지 않으면 false를 반환한다.

```java
Thread th = new Thread();
th.start();
	...
th.interrupt();	//쓰레드 th에 interrupt()를 호출한다.

class MyThread extends Thread {
	public void run()	{
		while(!interrupted())	{	//interrupted()의 결과가 false인 동안 반복
			...
		}
	}
}
```

interrupt()가 호출되면, interrupted()의 결과가 false에서 true로 바뀌어 while문을 벗어나게 된다.     
isInterrupted()도 쓰레드의 interrupt()가 호출되었는지 확인하는데 사용할 수 있지만,     
interrupt()와 달리 isInterrupted()는 쓰레드의 interrupted상태를 false로 초기화하지 않는다.

```java
void interrupt()
//쓰레드의 interrupted상태를 false에서 true로 변경
boolean isInterrupted()
//쓰레드의 interrupted상태를 반환
static boolean interrupted()
//현재 쓰레드의 interrupted상태를 반환 후, false로 변경.
```

쓰레드가 sleep(), wait(), join()에 의해 일시정지 상태에 있을 때, 해당 쓰레드에 대해 interrupt()를 호출하면,      
sleep(), wait(), join()에서 InterruptedException이 발생하고 쓰레드는 실행대기 상태로 바뀐다.     
즉, 멈춰있던 쓰레드를 깨워서 실행가능한 상태로 만드는 것이다.    

```java
import javax.swing.JOptionPane;

public class ThreadEx13 {

	public static void main(String[] args) throws Exception {
		ThreadEx13_1 t1 = new ThreadEx13_1();
		t1.start();

		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요");
		System.out.println("입력하신 값은 " +input+"입니다.");
		t1.interrupt();	//interrupt()를 호출하면, interrupted가 true가 된다.
		System.out.println(t1.isInterrupted());
	}
}
class ThreadEx13_1 extends Thread	{
	public void run() {
		int i = 10;
		
		while(i !=0 && !isInterrupted()) {
			System.out.println(i--);
			for(long x = 0 ; x<2500000000L; x++);
		}
		System.out.println("카운트가 종료되었습니다.");
	}
}
```
사용자의 입력이 끝나자 interrupt()에 의해 카운트 다운이 중간에 멈췄다.

```java
import javax.swing.JOptionPane;

public class ThreadEx14 {

	public static void main(String[] args) throws Exception {
		ThreadEx14_1 t1 = new ThreadEx14_1();
		t1.start();

		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요");
		System.out.println("입력하신 값은 " +input+"입니다.");
		t1.interrupt();	//interrupt()를 호출하면, interrupted가 true가 된다.
		System.out.println(t1.isInterrupted());
	}
}
class ThreadEx14_1 extends Thread	{
	public void run() {
		int i = 10;
		
		while(i !=0 && !isInterrupted()) {
			System.out.println(i--);
			try {
				Thread.sleep(1000);
			} catch (Exception e) {}
		}
		System.out.println("카운트가 종료되었습니다.");
	}
}
```
이전 예제에서 시간지연을 위한 for문 대신 Thread.sleep(1000)으로 1초 동안 지연되도록 변경했을 뿐인데, 카운드가 종료되지 않았다.    
그 이유는 Thread.sleep(1000)에서 InterruptedException이 발생했기 때문이다.     
sleep()에 의해 쓰레드가 잠시 멈췄을 때 interrupt()를 호출하면 InterruptedException이 발생되고     
interrupted상태는 false로 자동 초기화된다.    
그럴 때는 아래와 같이 catch블럭에 interrupt()를 추가로 넣어줘서 쓰레드의 interrupted상태를 true로 다시 바꿔줘야 한다.

```java
try{
	Thread.sleep(1000);
}catch(InterruptedException e){}
//>>>>>>>>>>>>>>>
try{
	Thread.sleep(1000);
}catch(InterruptedException e){
	interrupt();
}
```
#### suspend(), resume(), stop()

suspend()는 stop()처럼 쓰레드를 멈추게 한다.    
suspend()에 의해 정지된 쓰레드는 resume()을 호출해야 다시 실행대기 상태가 된다.    
stop()은 실행 즉시 쓰레드가 종료된다.     
suspend()와 stop()은 교착상태를 일으키기 쉽게 작성되어 사용이 권장되지 않고 deprcated 되었다.    

```java
public class ThreadEx15 {

	public static void main(String[] args) {
		RunImplEx15 r = new RunImplEx15();
		Thread th1 = new Thread(r, "*");
		Thread th2 = new Thread(r, "**");
		Thread th3 = new Thread(r, "***");

		th1.start();
		th2.start();
		th3.start();
		
		try {
			System.out.println();
			Thread.sleep(2000);
			System.out.println();
			th1.suspend();	//쓰레드 th1을 잠시 정지시킨다.
			Thread.sleep(2000);
			System.out.println();
			th2.suspend();
			Thread.sleep(3000);
			System.out.println();
			th1.resume();	//쓰레드 th1을 다시 동작하도록 한다.
			Thread.sleep(3000);
			System.out.println();
			th1.stop();		//쓰레드를 강제종료시킨다.
			th2.stop();
			Thread.sleep(2000);
			System.out.println();
			th3.stop();
		} catch (InterruptedException e) {}
	}

}
class RunImplEx15 implements Runnable{
	public void run() {
		while(true) {
			System.out.println(Thread.currentThread().getName());
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {}
		}
	}
}
```
sleep(2000)은 쓰레드를 2초간 멈추게 하지만, 2초 후에 바로 실행상태가 아닌 실행대기상태가 된다.

```java
public class ThreadEx16 {

	public static void main(String[] args) {
		RunImplEx16 r1 = new RunImplEx16();
		RunImplEx16 r2 = new RunImplEx16();
		RunImplEx16 r3 = new RunImplEx16();
		
		Thread th1 = new Thread(r1, "*");
		Thread th2 = new Thread(r2, "**");
		Thread th3 = new Thread(r3, "***");
		
		th1.start();
		th2.start();
		th3.start();
		
		try {
			Thread.sleep(2000);
			r1.suspend();
			Thread.sleep(2000);
			r2.suspend();
			Thread.sleep(3000);
			r1.resume();
			Thread.sleep(3000);
			r1.stop();
			r2.stop();
			Thread.sleep(2000);
			r3.stop();
		} catch (InterruptedException e) {}
	}
}
class RunImplEx16 implements Runnable	{
	boolean suspended = false;
	boolean stopped = false;
	
	public void run() {
		while(!stopped) {
			if(!suspended) {
				System.out.println(Thread.currentThread().getName());
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e) {}
			}
		}
		System.out.println(Thread.currentThread().getName() + "- stopped");
	}
	public void suspend() { suspended = true;}
	public void resume() { suspended = true;}
	public void stop()	{ stopped = true;}
}
```
```java
public class ThreadEx18 {

	public static void main(String[] args) {
		ThreadEx18_1 th1 = new ThreadEx18_1("*");
		ThreadEx18_1 th2 = new ThreadEx18_1("**");
		ThreadEx18_1 th3 = new ThreadEx18_1("***");
		th1.start();
		th2.start();
		th3.start();

		try {
			Thread.sleep(2000);
			th1.suspend();
			Thread.sleep(2000);
			th2.suspend();
			Thread.sleep(3000);
			th1.resume();
			Thread.sleep(3000);
			th1.stop();
			th2.stop();
			Thread.sleep(2000);
			th3.stop();
		} catch (InterruptedException e) {}
	}
}
class ThreadEx18_1 implements Runnable {
	boolean suspended = false;
	boolean stopped = false;

	Thread th;

	ThreadEx18_1(String name) {
		th = new Thread(this, name); // Thread(Runnable r, String name)
	}

	public void run() {
		String name = th.getName();
		while (!stopped) {
			if (!suspended) {
				System.out.println(name);
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e) {
					System.out.println(name + "-interrupted");
				}
			} else
				Thread.yield();
		}
		System.out.println(name + "-stopped");
	}

	public void suspend() {
		suspended = true;
		th.interrupt();
		System.out.println(th.getName() + "-interrupt() by suspend()");
	}

	public void stop() {
		stopped = true;
		th.interrupt();
		System.out.println(th.getName() + "-interrupt() by stop()");
	}

	public void resume() {
		suspended = false;
	}

	public void start() {
		th.start();
	}
}
```
이 전의 에제에 yield()와 interrupt()를 추가해서 예제의 효율과 응답성을 향상시켰다.    

```java
while(!stopped) {
	if(!suspended) {
		...
		try{
			Thread.sleep(1000);
		}catch(InterruptedException e){}
	}
}
```
위 코드에서 만일 suspended값이 true라면, 즉 잠시 실행을 멈추게 한 상태라면, 쓰레드는 주어진 실행시간을     
그저 while문을 의미없이 돌며 낭비하게 될 것이다. 이를 바른 대기상태busy-waiting이라고 한다.    

```java
while(!stopped) {
	if(!suspended) {
		...
		try{
			Thread.sleep(1000);
		}catch(InterruptedException e){}
	}else{
		Thread.yiedl();
	}
}
```
그러나 위 코드는 같은 경우에 yield()를 호출해서 남은 실행시간을 while문에서 낭비하지 않고     
다른 쓰레드에게 양보yield하게 되므로 더 효율적이다. 
또한 suspend() 메서드와 stop()메서드에 th.interrupt()를 추가하였다.    
이는 stopped의 값이 true가 되었을 떄 interrupt()를 호출하여 sleep()에서 InterruptedException을 발생시키므로     
즉시 일시정지 상태에서 벗어나게하고 응답성을 향상시킨다.

#### join()-다른 쓰레드의 작업을 기다린다.
 쓰레드 자신이 하던 작업을 잠시 멈추고 다른 쓰레드가 지정된 시간동안 작업을 수행하도록 할 때 join()을 사용한다.
 
 ```java
 void join()
 void join(long millis)
 void join(long millis, int nanos)
 ```
 
 시간을 지정하지 않으면, 해당 쓰레드가 작업을 모두 마칠 때까지 기다리게 된다.    
 작업 중에 다른 쓰레드의 작업이 먼저 수행되어야할 필요가 있을 때 join()을 사용한다.
 
 ```java
 try{
 	th1.join();	//현재 실행중인 쓰레드가 쓰레드th1의 작업이 끝날때까지 기다린다.
 }catch(InterruptedException e) {}
 ```
 
 join()도 sleep()처럼 interrupt()에 의해 대기상태에서 벗어날 수 있으며, join()이 호출되는 부분을 try-catch문으로 감싸야 한다.    
 join()과 sleep()은 유사한 점이 많지만, 다른 점은 join()은 현재 쓰레드가 아닌 특정 쓰레드에 대해 동작하므로 static이 아니라는 것이다.
 
> join()은 자신의 작업 중간에 다른 쓰레드의 작업을 참여join시킨다는 의미로 이름 지어진 것이다.
 
 ```java
 public class ThreadEx19 {

	static long startTime = 0;
	
	public static void main(String[] args) {
		ThreadEx19_1 th1 = new ThreadEx19_1();
		ThreadEx19_2 th2 = new ThreadEx19_2();
		th1.start();
		th2.start();
		startTime = System.currentTimeMillis();
		
		try {
			th1.join();	//main쓰레드가 th1의 작업이 끝날때까지 기다린다.
			th2.join();	//main쓰레드가 th2의 작업이 끝날때까지 기다린다.
		} catch (InterruptedException e) {}
		
		System.out.println("소요시간: " + (System.currentTimeMillis() - startTime));
	}
}
class ThreadEx19_1 extends Thread {
	public void run() {
		for(int i = 0; i< 300; i++)
			System.out.print("-");
	}
}
class ThreadEx19_2 extends Thread {
	public void run() {
		for(int i = 0; i< 300; i++)
			System.out.print("|");
	}
}
```
join()을 사용하지 않았으면 main쓰레드는 바로 종료되었겠지만,      
join()으로 쓰레드th1과 th2의 작업을 마칠 때 까지 main쓰레드가 기다리도록 했다.              

```java
public class ThreadEx20 {

	public static void main(String[] args) {
		ThreadEx20_1 gc = new ThreadEx20_1();
		gc.setDaemon(true);
		gc.start();
		
		int requiredMemory = 0;
		
		for(int i = 0; i<20; i++) {
			requiredMemory = (int)(Math.random()*10)*20;
			
			//필요한 메모리가 사용할 수 있는 양보다 크거나 전체 메모리의 60%이상을
			//사용했을 경우 gc를 깨운다.
			if(gc.freeMemory()<requiredMemory
					|| gc.freeMemory() < gc.totalMemory()*0.4) {
				gc.interrupt(); 	//잠자고 있는 쓰레드 gc를 깨운다.
			}
			gc.usedMemory += requiredMemory;
			System.out.println("usedMemory: " + gc.usedMemory);
		}
	}
}

class ThreadEx20_1 extends Thread {
	final static int MAX_MEMORY = 1000;
	int usedMemory = 0;

	public void run() {
		while (true) {
			try {
				Thread.sleep(10 * 1000);
			} catch (InterruptedException e) {
				System.out.println("Awaken by interrupt().");
			}
			gc(); // garbage collection을 수행한다.
			System.out.println("Garbage Collected. Free Memory: " + freeMemory());
		}
	}
	public void gc() {
		usedMemory -= 300;
		if (usedMemory < 0)
			usedMemory = 0;
	}

	public int totalMemory() {
		return MAX_MEMORY;
	}

	public int freeMemory() {
		return MAX_MEMORY - usedMemory;
	}
}
```
이 예제는 JVM의 가비지 컬렉터를 흉내 내어 간단히 구현해 본 것이다.     
먼저 sleep()을 이용해서 10초 마다 한 번씩 가비지 컬렉션을 수행하는 쓰레드를 만든 다음, 쓰레드를 생성해서 데몬 쓰레드로 설정하였다.      


반복문을 사용해서 메모리의 양을 계속 감소시키도록 했고,     
매 반복마다 if문으로 메모리를 확인해서 남은 메모리가 전체 메모리의 40%미만일 경우에 interrupt()를 호출해서,     
즉시 가비지 컬렉터 쓰레드를 깨워서 gc()를 수행하도록 하였다.     


그러나 예제의 실행결과를 보면 MAX_MEMORY가 1000임에도 불구하고 usedMemory가 1000을 넘는 것을 알 수 있다.    
이는 쓰레드 gc가 interrupt()에 의해서 깨어났음에도 불구하고       
gc()가 수행되기 이전에 main쓰레드의 작업이 수행되어 메모리를 사용하기 때문이다.      
그래서 쓰레드 gc를 깨우는 것뿐만 아니라 join()을 이용해서 쓰레드 gc가 작업할 시간을 어느정도 주고    
main쓰레드가 기다리도록 해서, 사용할 수 있는 메모리가 확보된 다음에 작업을 계속 하는 것이 필요하다.

```java
if(gc.freeMemory()<requiredMemory...
	gc.interrupt();
}
//>>>>>>>>>>>>>>>>
if(gc.freeMemory()<requiredMemory...
	gc.interrupt();
	trt{
		gc.join(100);
	}catch(InterruptedException e){}
}
```
그래서 코드를 아래와 같이 join()을 호출해서 쓰레드 gc가 0.1초 동안 수행될 수 있도록 변경해야한다.     
가비지 컬렉터와 같은 데몬 쓰레드의 우선순위를 낮추기 보다는 sleep()을 이용해서 주기적으로 실행되도록 하다가      
필요할 때마다 interrupt()를 호출해서 즉시 가비지 컬렉션이 이루어지도록 하는 것이 좋다.   
그리고 필요하다면 join()도 함께 사용해야 한다.
# 쓰레드의 동기화
싱글쓰레드 프로세스의 경우 프로세스 내에서 단 하나의 쓰레드만 작업하기 때문에      
프로세스의 자원을 가지고 작업하는데 별문제가 없지만,    
멀티쓰레드 프로세스의 경우 여러 쓰레드가 같은 프로세스 내의 자원을 공유해서 작업하기 때문에     
서로의 작업에 영향을 주게 된다.    
이러한 일이 발생하는 것을 방지하기 위해서 한 쓰레드가 특정 작업을 마치기 전까지     
다른 쓰레드에 의해 방해받지 않도록 하는 것이 필요하다.    
그래서 도입된 개념이 임계 영역critical section과 잠금lock이다.    
공유 데이터를 사용하는 코드 영역을 임계 영역으로 지정해놓고,     
공유 데이터(객체)가 가지고 있는 lock을 획득한 단 하나의 쓰레드만 이 영역 내의 코드를 수행할 수 있게 한다.     
그리고 해당 쓰레드가 임계 영역 내의 모든 코드를 수행하고 벗어나서 lock을 반납해야만     
다른 쓰레드가 반납된 lock을 획득하여 임계 영역의 코드를 수행할 수 있게 된다.     

* 이처럼 한 쓰레드가 진행 중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것을 쓰레드의 동기화synchronization라고 한다.       

## synchronized를 이용한 동기화

synchronized 키워드를 이용한 동기화는 임계 영역을 설정하는데 사용된다.    

```java
//1. 메서드 전체를 임계 영역으로 지정
public synchronized void calcSum() { ... }

//2. 특정한 영역을 임계 영역으로 지정
synchronized(객체의 참조변수) { ... }
```

첫 방법은 메서드 앞에 synchronized를 붙이는 것인데, synchronized를 붙이면 메서드 전체가 임계 영역으로 설정된다.    
쓰레드는 synchronized메서드가 호출된 시점부터 해당 메서드가 포함된 객체의 lock을 얻어    
작업을 수행하다가 메서드가 종료되면 lock을 반환한다.    
두 번째 방법은 메서드 내의 코드 일부를 블럭{}으로 감싸고 블럭 앞에 synchronized(참조변수)를 붙이는 것인데,    
이때 참조변수는 락을 걸고자하는 객체를 참조하는 것이어야 한다.   
이 블럭을 synchronized블럭이라고 부르며, 이 블럭의 영역 안으로 들어가면서부터    
쓰레드는 지정된 객체의 lock을 얻게 되고, 이 블럭을 벗어나면 lock을 반납한다.     
두 방법 모두 lock의 획득과 반납이 자동으로 이루어지므로 임계 영역만 설정해주면 된다.      
모든 객체는 lock을 하나씩 가지고 있으며, 해당 객체의 lock을 가지고 있는 쓰레드만 임계 영역의 코드를 수행할 수 있다.    
그리고 다른 쓰레드들은 lock을 얻을 떄까지 기다리게 된다.     
임계 영역은 멀티쓰레드 프로그램의 성능을 좌우하기 때문에 가능하면 메서드 전체에 락을 거는 것보다    
synchronized블럭으로 임계 영역을 최소화해서 보다 효율적인 프로그램이 되도록 노력해야 한다.

```java
public class ThreadEx21 {

	public static void main(String[] args) {
		Runnable r = new RunnableEx21();
		new Thread(r).start();//ThreadGroup에 의해 참조되므로 gc대상이 아니다.
		new Thread(r).start();//ThreadGroup에 의해 참조되므로 gc대상이 아니다.
	}
}

class Account{
	private int balance = 1000;
	public int getBalance() {
		return this.balance;
	}
	public void withdraw(int money) {
		if(balance>=money) {
			try {Thread.sleep(1000);}catch(InterruptedException e) {}
			balance -= money;
		}
	}
}

class RunnableEx21 implements Runnable{
	Account acc = new Account();
	
	public void run() {
		while(acc.getBalance()>0) {
			//100, 200, 300 중의 한 값을 임의로 선택해서 출금withdraw
			int money = (int)(Math.random()*3+1) * 100;
			acc.withdraw(money);
			System.out.println("balance: "+acc.getBalance());
		}
	}
}
```

계좌account에서 잔고balance를 확인하고 임의의 금액을 출금withdraw하는 예제이다.     
코드는 잔고가 출금하려는 금액보다 큰 경우에만 출금하도록 되어 있으나 실행결과를 보면 잔고가 음수이다.    
한 쓰레드가 if문의 조건식을 통화하고 출금하기 바로 직전에 다른 쓰레드가 끼어들어서 출금을 먼저 했기 때문이다.    
그러므로 잔고를 확인하는 if문과 출금하는 문장은 하나의 임계 영역으로 묶여져야 한다.    


아래와 같이 withdraw메서드에 synchronized키워드를 붙이기만 하면 간단히 동기화가 된다.

```java
public synchronized void withdraw(int money) { ... }
```
한 쓰레드에 의해 먼저 withdraw()가 호출되면, 이 메서드가 종료되어 lock이 반납될때까지      
다른 쓰레드는 withdraw()를 호출하더라도 대기상태에 머물게 된다.     
메서드 앞에 synchronized를 붙이는 대신, synchronized블럭을 사용하면 다음과 같다.    

```java
public void withdraw() {
	synchronized(this) {
		if(balance>=money) {
			try{Thread.sleep(1000);}catch(InterruptedException e){}
			balance -= money;
		}
	}
}
```
이 경우에는 둘 중의 어느 쪽을 선택해도 같으니까 synchronized메서드로 하는 것이 낫다.    

```java
public class ThreadEx22 {

	public static void main(String[] args) {
		Runnable r = new RunnableEx21();
		new Thread(r).start();//ThreadGroup에 의해 참조되므로 gc대상이 아니다.
		new Thread(r).start();//ThreadGroup에 의해 참조되므로 gc대상이 아니다.
	}
}

class Account{
	private int balance = 1000;
	public int getBalance() {
		return this.balance;
	}
	public synchronized void withdraw(int money) {
		if(balance>=money) {
			try {Thread.sleep(1000);}catch(InterruptedException e) {}
			balance -= money;
		}
	}
}

class RunnableEx21 implements Runnable{
	Account acc = new Account();
	
	public void run() {
		while(acc.getBalance()>0) {
			//100, 200, 300 중의 한 값을 임의로 선택해서 출금withdraw
			int money = (int)(Math.random()*3+1) * 100;
			acc.withdraw(money);
			System.out.println("balance: "+acc.getBalance());
		}
	}
}
```
Account클래스의 인스턴스변수인 balance의 접근 제어자가 private이라는 것에 주의하여야 한다.    
만일 private이 아니면, 외부에서 직접 접근할 수 있기 때문에 아무리 동기화를 해도 이 값의 변경을 막을 수 없다.     
synchronized를 이용한 동기화는 지정된 영역의 코드를 한 번에 하나의 쓰레드가 수행하는 것을 보장하는 것일 뿐이기 때문이다.
## wait()와 notify()
synchronized로 동기화해서 공유 데이터를 보호하는 것 까지는 좋은데,   
특정 쓰레드가 객체의 락을 가진 상태로 오랜 시간을 보내지 않도록 하는 것도 중요하다.     
이러한 상황을 개선하기 위해 고안된 것이 바로 wait()과 notify()이다.     
동기화된 임계 영역의 코드를 수행하다가 작업을 더 이상 진행할 상황이 아니면,     
일단 wait()을 호출하여 쓰레드가 락을 반납하고 기다리게 한다.     
그러면 다른 쓰레드가 락을 얻어 해당 객체에 대한 작업을 수행할 수 있게 된다.     
나중에 작업을 진행할 수 있는 상황이 되면 notify()를 호출해서, 작업을 중단했던 쓰레드가 다시 락을 얻어 작업을 진행하게 된다.     


다만, 오래 기다린 쓰레드가 락을 얻는다는 보장은 없다.     
wait()이 호출되면, 실행 중이던 쓰레드는 해당 객체의 대기실에서 통지를 기다린다.    
notify()가 호출되면, 해당 객체의 대기실에 있던 모든 쓰레드 중에서 임의의 쓰레드만 통지를 받는다.     
notifyAll()은 기다리고 있는 모든 쓰레드에게 통보를 하지만, 그래도 Lock을 얻는 쓰레드는 하나뿐이다.    
나머지 쓰레드는 통보를 받긴 했지만, lock을 얻지 못하면 다시 lock을 기다리는 신세가 된다.     
wait()과 notify()는 특정 객체에 대한 것이므로 Object클래스에 정의되어있다.

```java
void wait()
void wait(long timeout)
void wait(long timeout, int nanos)
void notify()
void notifyAll()
```

wait()은 notify()나 notifyAll()이 호출될 때까지 기다리지만, 매개변수가 있는 wait()은 지정된 시간동안만 기다린다.     
즉, 지정된 시간이 지난 후에 자동적으로 notify()가 호출된다.     
그리고 waiting pool은 객체마다 존재하는 것이므로 notifyAll()이 호출된다고 모든 객체의 waiting pool에 있는 쓰레드가 깨워지는 것은 아니다.    
notifyAll()이 호출된 객체의 waiting pool에 대기 중인 쓰레드만 해당된다.

#### wait(), notify(), notifyAll()
> Object에 정의되어 있다.     
> 동기화 블록Synchronized블록 내에서만 사용할 수 있다.    
> 보다 효율적인 동기화를 가능하게 한다.

다음 예제는 식당에서 음식을 만들어서 테이블에 추가하는 요리사와 테이블의 음식을 소비하는 손님을 쓰레드로 구현했다.   

```java
public class ThreadWaitEx1 {

	public static void main(String[] args) throws Exception{
		Table table = new Table();	//여러 쓰레드가 공유하는 객체
		
		new Thread(new Cook(table), "COOK1").start();
		new Thread(new Customer(table, "donut"), "CUST1").start();
		new Thread(new Customer(table, "burger"), "CUST2").start();
		
		//0.1초 후에 강제 종료시킨다.
		Thread.sleep(100);	
		System.exit(0);
	}
}
class Customer implements Runnable	{
	private Table table;
	private String food;
	
	Customer (Table table, String food){
		this.table = table;
		this.food = food;
	}
	public void run() {
		while(true) {
			try {Thread.sleep(10);}catch(InterruptedException e) {}
			String name = Thread.currentThread().getName();
			
			if(eatFood())
				System.out.println(name + " ate a " + food);
			else
				System.out.println(name + " faild to eat. :( ");
		}
	}
	public boolean eatFood() {return table.remove(food);}
}
class Cook implements Runnable	{
	private Table table;
	
	Cook(Table table){this.table = table;}
	
	public void run() {
		while(true) {
			//임의의 요리를 하나 선택해서 table에 추가한다.
			int idx = (int)(Math.random()*table.dishNum());
			table.add(table.dishNames[idx]);
			
			try {Thread.sleep(1);}catch(InterruptedException e) {}
		}
	}
}
class Table{
	String[] dishNames = {"donut", "donut", "burger"};//donut이 더 자주 나온다.
	final int MAX_FOOD = 6;//테이블에 놓을 수 있는 최대 음식의 개수
	
	private ArrayList<String> dishes = new ArrayList<>();
	
	public void add(String dish) {
		//테이블에 음식이 가득찼으면, 테이블에 음식을 추가하지 않는다.
		if(dishes.size()>=MAX_FOOD)
			return;
		dishes.add(dish);
		System.out.println("Dishes: " + dishes.toString());
	}
	public boolean remove(String dishName) {
		//지정된 요리와 일치하는 요리를 테이블에서 제거한다.
		for(int i = 0; i<dishes.size(); i++)
			if(dishName.equals(dishes.get(i))) {
				dishes.remove(i);
				return true;		
			}
		
		return false;
	}
	public int dishNum() {return dishNames.length;}
}
```
반복해서 실행하면 2가지 종류의 예외를 볼 수 있다.    
요리사Cook 쓰레드가 테이블에 음식을 놓는 도중에,       
손님Customer 쓰레드가 음식을 가져가려했기 때문에 발생하는 예외ConcurrentModigicationException와      
손님 쓰레드가 테이블의 마지막 남은 음식을 가져가는 도중에 다른 손님 쓰레드가 먼저 음식을 낚아채버려서      
있지도 않은 음식을 테이블에서 제거하려했기 때문에 발생하는 예외IndexOutOfBoundsException 이다.     
이런 예외들이 발생하는 이유는 여러 쓰레드가 테이블을 공유하는데도 동기화를 하지 않았기 떄문이다.    
이제 이 예제에 동기화를 추가해서 예외가 발생하지 않도록 하자.

```java
public class ThreadWaitEx1 {

	public static void main(String[] args) throws Exception{
		Table table = new Table();	//여러 쓰레드가 공유하는 객체
		
		new Thread(new Cook(table), "COOK1").start();
		new Thread(new Customer(table, "donut"), "CUST1").start();
		new Thread(new Customer(table, "burger"), "CUST2").start();
		
		//0.1초 후에 강제 종료시킨다.
		Thread.sleep(5000);	
		System.exit(0);
	}
}
class Customer implements Runnable	{
	private Table table;
	private String food;
	
	Customer (Table table, String food){
		this.table = table;
		this.food = food;
	}
	public void run() {
		while(true) {
			try {Thread.sleep(10);}catch(InterruptedException e) {}
			String name = Thread.currentThread().getName();
			
			if(eatFood())
				System.out.println(name + " ate a " + food);
			else
				System.out.println(name + " faild to eat. :( ");
		}
	}
	public boolean eatFood() {return table.remove(food);}
}
class Cook implements Runnable	{
	private Table table;
	
	Cook(Table table){this.table = table;}
	
	public void run() {
		while(true) {
			//임의의 요리를 하나 선택해서 table에 추가한다.
			int idx = (int)(Math.random()*table.dishNum());
			table.add(table.dishNames[idx]);
			try {Thread.sleep(100);}catch(InterruptedException e) {}
		}
	}
}
class Table{
	String[] dishNames = {"donut", "donut", "burger"};//donut이 더 자주 나온다.
	final int MAX_FOOD = 6;//테이블에 놓을 수 있는 최대 음식의 개수
	
	private ArrayList<String> dishes = new ArrayList<>();
	
	public synchronized void add(String dish) {
		//테이블에 음식이 가득찼으면, 테이블에 음식을 추가하지 않는다.
		if(dishes.size()>=MAX_FOOD)
			return;
		dishes.add(dish);
		System.out.println("Dishes: " + dishes.toString());
	}
	public boolean remove(String dishName) {
		synchronized(this) {
			while(dishes.size() == 0) {
				String name = Thread.currentThread().getName();
				System.out.println(name + " is waiting. ");
				try {Thread.sleep(500);}catch(InterruptedException e) {}
			}
			for(int i = 0; i<dishes.size(); i++)
				if(dishName.equals(dishes.get(i))) {
					dishes.remove(i);
					return true;
				}
			}
			return false;
		}
	public int dishNum() {return dishNames.length;}
}
```
여러 쓰레드가 공유하는 개체인 테이블Table의 add()와 remove()를 동기화하였다.     
더 이상 전과 같은 예외는 발생하지 않지만, 요리사 쓰레드가 요리를 추가하지않아 원활히 진행되지도 않는다.    
그 이유는 손님 쓰레드가 테이블 객체의 lock을 쥐고 기다리기 때문이다.     
요리사 쓰레드가 음식을 새로 추가하려해도 테이블 객체의 lock을 얻을 수 없어서 불가능하다.     
이럴 때 사용하는 것이 wait() & notify() 이다.    
손님 쓰레드가 lock을 쥐고 기다리는 게 아니라, wait()으로 lock을 풀고 기다리다가 음식이 추가되면     
notify()로 통보를 받고 다시 lock을 얻어서 나머지 작업을 진행하게 할 수 있다.     
다음 예제는 위 예제에 wait()과 notify()를 추가한 것이다.    

```java
public class ThreadWaitEx1 {

	public static void main(String[] args) throws Exception{
		Table table = new Table();	//여러 쓰레드가 공유하는 객체
		
		new Thread(new Cook(table), "COOK1").start();
		new Thread(new Customer(table, "donut"), "CUST1").start();
		new Thread(new Customer(table, "burger"), "CUST2").start();
		
		Thread.sleep(2000);	
		System.exit(0);
	}
}
class Customer implements Runnable	{
	private Table table;
	private String food;
	
	Customer (Table table, String food){
		this.table = table;
		this.food = food;
	}
	public void run() {
		while(true) {
			try {Thread.sleep(100);}catch(InterruptedException e) {}
			String name = Thread.currentThread().getName();
			
			table.remove(food);
			System.out.println(name+ " ate a "+food);
		}
	}
}
class Cook implements Runnable	{
	private Table table;
	
	Cook(Table table){this.table = table;}
	
	public void run() {
		while(true) {
			//임의의 요리를 하나 선택해서 table에 추가한다.
			int idx = (int)(Math.random()*table.dishNum());
			table.add(table.dishNames[idx]);
			try {Thread.sleep(10);}catch(InterruptedException e) {}
		}
	}
}
class Table{
	String[] dishNames = {"donut", "donut", "burger"};//donut이 더 자주 나온다.
	final int MAX_FOOD = 6;//테이블에 놓을 수 있는 최대 음식의 개수
	
	private ArrayList<String> dishes = new ArrayList<>();
	
	public synchronized void add(String dish) {
		while(dishes.size()>=MAX_FOOD) {
			String name = Thread.currentThread().getName();
			System.out.println(name+" is waiting. ");
			try {
				wait();	//cook쓰레드를 기다리게 한다.
				Thread.sleep(500);
			} catch (InterruptedException e) {}
		}
		dishes.add(dish);
		notify();	//기다리고 있는 CUST를 깨우기 위함
		System.out.println("Dish: "+dishes.toString());
	}
	public void remove(String dishName) {
		synchronized(this) {
			String name = Thread.currentThread().getName();
			
			while(dishes.size() == 0) {
				System.out.println(name + " is waiting. ");
				try {
					wait();	//CUST를 기다리게 한다.
					Thread.sleep(500);
				}catch(InterruptedException e) {}
			}
			while(true) {
				for(int i = 0; i<dishes.size(); i++) {
					if(dishName.equals(dishes.get(i))) {
						dishes.remove(i);
						notify();	//잠자고 있는 COOK을 깨우기 위함
						return;
					}
				}
				try {
					System.out.println(name + " is waiting. ");
					wait();	//원하는 음식이 없는 CUST쓰레드를 기다리게 한다.
					Thread.sleep(500);
				}catch(InterruptedException e) {}
			}
		}
	}
	public int dishNum() {return dishNames.length;}
}
```
이전 예제에 wait()과 notify()를 추가하고 테이블에 음식이 없을 때뿐만 아니라 원하는 음식이 없을 때도 손님이 기다리도록 바꾸었다.     
이제 남은 문제는 waiting pool에 요리사와 손님이 함께 대기한다는 것이다.   
그래서 notify()가 호출되었을 때 누가 통지를 받을지 알 수 없다.    
손님 쓰레드가 통지를 받아 lock을 얻어도 원하는 음식이 없어 다시 waiting pool에 들어가 비효율을 만들 수 있다.

#### 기아 현상과 경쟁 상태
운이 나쁘면 요리사 쓰레드는 계속 통지를 받지 못하고 오랫동안 기다리게 되는데 이를 '기아현상'이라고 한다.   
이 현상을 막으려면 notify()대신 notifyAll()을 사용해야 한다.    
일단 모든 쓰레드에게 통지를 하면, 손님 쓰레드는 다시 waiting pool에 들어가더라도 요리사 쓰레드는      
결국 lock을 얻어서 작업을 진행할 수 있기 때문이다.

## Lock과 Condition을 이용한 동기화

동기화할 수 있는 방법은 synchronized블럭 외에도 java.util.concurrent.locks패키지가 제공하는 lock클래스들을 이용하는 방법이 있다.     
synchronized블럭으로 동기화하면 자동으로 lock이 잠기고 풀리고, 블럭 내에서 예외가 발생해도 lock은 자동으로 풀린다.    
그러나 같은 메서드내에서만 lock을 걸 수 있다는 제약이 있어 그럴 때 lock클래스를 사용한다.

> ReentrantLock : 재진입이 가능한 lock. 가장 일반적인 배타 lock        
> ReentrantReadWriteLock : 읽기에는 공유적이고, 쓰기에는 배타적인 lock     
> StamptedLock : ReentrantReadWriteLock에 낙관적인 lock의 기능을 추가    

ReentrantLock은 가장 일반적인 lock이다.     
reentrant(재진입할 수 있는)이라는 단어가 앞에 붙은 이유는 우리가 앞서 wait() & notify()를 배운 것처럼,      
특정 조건에서 lock을 풀고 나중에 다시 lock을 얻고 임계영역으로 들어와서 이후의 작업을 수행할 수 있기 떄문이다.     
ReentrantReadWriteLock은 읽기를 위한 lock과 쓰기를 위한 lock을 제공한다.     
ReentrantLock은 배타적인 lock이라서 무조건 lock이 있어야만 임계영역의 코드를 수행할 수 있지만,     
ReentrantReadWriteLock은 읽기 lock이 걸려있으면, 다른 쓰레드가 읽기 lock을 중복해서 걸고 읽기를 수행할 수 있다.     
읽기는 내용을 변경하지 않으므로 동기에 여러 쓰레드가 읽어도 문제가 되지 않는다.     
그러나 읽기 lock이 걸린 상태에서 쓰기 lock을 거는 것은 허용되지 않는다. 반대의 경우도 마찬가지다.     


StampedLock은 lock을 걸거나 해지할 떄 스탬프(long타입의 정수값)를 사용하며,     
읽기와 쓰기를 위한 lock외에 낙관적 읽기 lock(optimistic reading lock)이 추가된 것이다.      
읽기 lock이 걸려있으면, 쓰기 lock을 얻기 위해서는 읽기 lock이 풀릴 때까지 기다려야하는데 비해    
낙관적 읽기 lock은 쓰기 lock에 의해 바로 풀린다. 그래서 낙관적 읽기에 실패하면 읽기 lock을 얻어서 다시 읽어 와야 한다.    
* 무조건 읽기 lock을 걸지 않고, 쓰기와 읽기가 충돌할 때만 쓰기가 끝난 후에 읽기 lock을 거는 것이다.     
다음의 코드는 가장 일반적인 StampedLock을 이용한 낙관적 읽기의 예이다.    

```java
int getBalance(){
	long stamp = lock.tryOptimisticRead();	//낙관적 읽기 lock을 건다.
	
	int curBalance = this.balance;	//공유 데이터인 balance를 읽어온다.
	
	if(!lock.validate(stamp)){	//쓰기 lock에 의해 낙관적 읽기 lock이 풀렸는지 확인
		stamp = lock.readLock();	//lock이 풀렸으면, 읽기 lock을 얻으려고 기다린다.
		
		try{
			curBalance = this.balance;	// 공유 데이터를 다시 읽어온다.
		} finally {
			lock.unlockRead(stamp);	//읽기 lock을 푼다.
		}
	}
	return curBalance;	//낙관적 읽기 lock이 풀리지 않았으면 곧바로 읽어온 값을 반환
}
```

#### ReentrantLock의 생성자

> ReentrantLock()     
> ReentrantLock(boolean fair)     

생성자의 매개변수를 true로 주면, lock이 풀렸을 때 가장 오래 기다린 쓰레드가 lock을 획득할 수 있게 공정하게 처리한다.    
그러나 공정하게 처리하려면 어떤 쓰레드가 가장 오래 기다렸는지 확인하는 과정을 거쳐야하므로 성능은 떨어진다.     
대부분의 경우 굳이 공정하게 처리하지 않아도 문제가 되지 않으므로 공정함보다 성능을 선택한다.     

> void lock() : lock을 잠근다.      
> void unlock() : lock을 해지한다.     
> boolean isLocked() : lock이 잠겼는지 확인한다.     

자동으로 lock의 잠금과 해제가 관리되는 synchronized블럭과 달리,       
ReentrantLock과 같은 lock클래스들은 수동으로 lock을 잠그고 해제해야 된다.	      

```java
synchronized(lock) {  //임계 영역  }
//>>>>>>>>
lock.lock();
//임계 영역
lock.unlock();
```
임계 영역 내에서 예외가 발생하거나 return문으로 빠져 나가게 되면 lock이 풀리지 않을 수 있으므로      
unlock()은 try-finally문으로 감싸는 것이 일반적이다.      

```java
//참조변수 lock은 ReentrantLock객체를 참조한다고 가정하였다.
lock.lock();	//Reentrant lock = new ReentrantLock();
try {
	//임계 영역
} finally {
	lock.unlock();
}
```

이렇게 하면, try블럭 내에서 어떤 일이 발생해도 finally블럭에 있는 unlock()이 수행되어 lock이 풀리지 않는 일은 발생하지 않는다.     
대부분의 경우 lock() & unlock() 대신 synchronized블럭을 사용할 수 있으며, 그럴 떄는 synchronized블럭을 사용하는 것이 낫다.     
이 외에도 tryLock()이라는 메서드가 있는데, 이 메서드는 lock()과 달리,      
다른 쓰레드에 의해 lock이 걸려있으면 lock을 얻으려고 기다리지 않는다. 또는 지정된 시간만큼만 기다린다.

> boolean tryLock()      
> boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException

lock()은 lock을 얻을 때까지 쓰레드를 블락block시키므로 쓰레드의 응답성이 나빠질 수 있다.     
응답성이 중요한 경우, tryLock()을 이용해서 지정된 시간동안 lock을 얻지 못하면     
다시 작업을 시도할 것인지 포기할 것인지를 사용자가 결정할 수 있게 하는 것이 좋다.     
그리고 이 메서드는 InterruptedException을 발생시킬 수 있는데, 이는 지정된 시간동안 lock을 얻으려고 기다리는 중에     
interrupt()에 의해 작업이 취소될 수 있도록 코드를 작성할 수 있다는 뜻이다.     

#### ReentrantLock과 Condition
Condition은 앞서 wait()&notify() 예제에 요리사 쓰레드와 손님 쓰레드를 구분해서 통지하지 못한다는 단점을 해결하기 위한 것이다.     
wait()&notify()로 쓰레드의 종류를 구분하지 않고 공유 객체의 waiting pool에 같이 몰아넣는 대신,     
손님 쓰레드를 위한 Condition과 요리사 쓰레드를 위한 Condition을 만들어서 각각의 waiting pool에서 따로 기다리게 하면 문제는 해결된다.     
Condition은 이미 생성된 lock으로부터 newCondition()을 호출해서 생성한다.

```java
private ReentrantLock lock = new ReentrantLock();	//lock을 생성
//lock으로 condition을 생성
private Condition forCook = lock.newCondition();
private Condition forCust = lock.newCondition();
```
wait()&notify()대신 Condition의 await()&signal()을 사용하면 된다.

```java
//Object
void wait()
//Condition
void await()
void awaitUninterruptibly()
//Object
void wait(long timeout)
//Condition
boolean await(long time, TimeUnit unit)
long awaitNanos(long nanosTimeout)
boolean awaitUntil(Date deadline)
//Object
void notify()
//Condition
void signal()
//Object
void notifyAll()
//Condition
void signalAll()
```

아래의 코드는 이전 예제에서 테이블에 음식을 추가하는 add()인데, wait()&notify()대신 await()&signal()을 사용했다.

```java
public void add(String dish){
	lock.lock();
	
	try{
		while(dishes.size() >= MAX_FOOD) {
			String name = Thread.currentThread().getName();
			System.out.println(name + " is waiting. ");
			try {
				forCook.await();	//wait(); COOK쓰레드를 기다리게 한다.
			} catch(InterruptedException e) {}
		}
		
		dishes.add(dish);
		forCust.signal();	//notify(); 기다리고 있는 CUST를 깨우기 위함.
		System.out.println("Dishes: " + dishes.toString());
	}finally{
		lock.unlock();
	}
}
```
wait()대신 forCook.await()와 forCust.await()를 사용함으로써 대기와 통지의 대상이 명확히 구분된다.

```java
public class ThreadWaitEx1 {

	public static void main(String[] args) throws Exception{
		Table table = new Table();	//여러 쓰레드가 공유하는 객체
		
		new Thread(new Cook(table), "COOK1").start();
		new Thread(new Customer(table, "donut"), "CUST1").start();
		new Thread(new Customer(table, "burger"), "CUST2").start();
		
		Thread.sleep(2000);	
		System.exit(0);
	}
}
class Customer implements Runnable	{
	private Table table;
	private String food;
	
	Customer (Table table, String food){
		this.table = table;
		this.food = food;
	}
	public void run() {
		while(true) {
			try {Thread.sleep(100);}catch(InterruptedException e) {}
			String name = Thread.currentThread().getName();
			
			table.remove(food);
			System.out.println(name+ " ate a "+food);
		}
	}
}
class Cook implements Runnable	{
	private Table table;
	
	Cook(Table table){this.table = table;}
	
	public void run() {
		while(true) {
			//임의의 요리를 하나 선택해서 table에 추가한다.
			int idx = (int)(Math.random()*table.dishNum());
			table.add(table.dishNames[idx]);
			try {Thread.sleep(10);}catch(InterruptedException e) {}
		}
	}
}
class Table{
	String[] dishNames = {"donut", "donut", "burger"};//donut이 더 자주 나온다.
	final int MAX_FOOD = 6;//테이블에 놓을 수 있는 최대 음식의 개수
	private ArrayList<String> dishes = new ArrayList<>();
	
	private ReentrantLock lock = new ReentrantLock();
	private Condition forCook = lock.newCondition();
	private Condition forCust = lock.newCondition();
	
	public void add(String dish) {
		lock.lock();
		
		try {
			while(dishes.size()>=MAX_FOOD) {
				String name = Thread.currentThread().getName();
				System.out.println(name+" is waiting. ");
				
				try {
					forCook.await();//wait(); COOK쓰레드를 기다리게 한다.
					Thread.sleep(500);
				} catch (InterruptedException e) {}
			}
			
			dishes.add(dish);
			forCust.signal();//notify();	기다리고 있는 CUST를 깨우기 위함.
			System.out.println("Dishes: "+ dishes.toString());
		} finally {
			lock.unlock();
		}
	}
	public void remove(String dishName) {
		lock.lock();
		String name = Thread.currentThread().getName();
		
		try {
			while(dishes.size() == 0) {
				System.out.println(name + " is waiting. ");
				try {
					forCust.await();//wait();	CUST쓰레드를 기다리게 한다.
					Thread.sleep(500);
				} catch (InterruptedException e) {}
			}
			while(true) {
				for(int i = 0 ; i<dishes.size(); i++) {
					if(dishName.equals(dishes.get(i))) {
						dishes.remove(i);
						forCook.signal();//notify();	잠자고 있는 COOK을 깨움
						return;
					}
				}
				try {
					System.out.println(name + " is waiting.");
					forCust.await();//wait();	CUST쓰레드를 기다리게 한다.
					Thread.sleep(500);
				} catch (InterruptedException e) {}
			}
		} finally {
			lock.unlock();
		}
	}
	public int dishNum() {return dishNames.length;}
}
```
이전 예제와 달리 요리사 쓰레드가 통지를 받아야하는 상황에서 손님 쓰레드가 통지를 받는 경우가 없어졌다.     
기아 현상이나 경쟁 상태가 개선된 것이다.      
그래도 쓰레드의 종류에 따라 구분하여 통지를 할 수 있게 된 것일 뿐,     
여전히 특정 쓰레드를 선택할 수 없기 때문에 같은 종류의 쓰레드간의 기아 현상이나 경쟁 상태가 발생할 가능성은 남아있다.    
손님이 원하는 음식의 종류별로 Condition을 더 세분화하면 통지를 받고도 원하는 음식이 없어서 다시 기다리는 일이 없도록 할 수 있다.

## volatile

코어는 메모리에서 읽어온 값을 캐시에 저장하고 캐시에서 값을 읽어서 작업한다.    
다시 같은 값을 읽어올 때는 먼저 캐시에 있는지 확인하고 없을 때만 메모리에서 읽어온다.    
그러다보니 도중에 메모리에 저장된 변수의 값이 변경되었는데도 캐시에 저장된 값이 갱신되지 않아서 메모리에 저장된 값이 다른 경우가 발생한다.     
그래서 변수 stopped의 값이 바뀌었는데도 쓰레드가 멈추지 않고 계속 실행되는 것이다.        
> ThreadEx16 예제에서 멀티 코어 프로세서의 경우 그러하다.   

```java
boolean suspended = false;
boolean stopped = false;
//>>>>>>>>>>>>
volatile boolean suspended = false;
volatile boolean stopped = false;
```

그러나 위와 같이 변수 앞에 volatile을 붙이면, 코어가 변수의 값을 읽어올 때      
캐시가 아닌 메모리에서 읽어오기 때문에 캐시와 메모리간의 값의 불일치가 해결된다.    
변수에 volatile을 붙이는 대신에 synchronized블럭을 사용해도 같은 효과를 얻을 수 있다.     
쓰레드가 synchronized블럭으로 들어갈 때와 나올 때, 캐시와 메모리간의 동기화가 이루어지기 때문에 값의 불일치가 해소되기 때문이다.     

#### volatile로 long과 double을 원자화
JVM은 데이터를 4byte단위로 처리하기 때문에, int와 int보다 작은 타입들은 한 번에 읽거나 쓰는 것이 가능하다.    
즉, 단 하나의 명령어로 읽거나 쓰기가 가능하다는 뜻이다.     
하나의 명령어는 더 이상 나눌 수 없는 최소의 작업단위이므로, 작업의 중간에 다른 쓰레드가 끼어들 틈이 없다.     
그러나, 크기가 8byte인 long과 double타입의 변수는 하나의 명령어로 값을 읽거나 쓸 수 없기 때문에,     
변수의 값을 읽는 과정에 다른 쓰레드가 끼어들 여지가 있다. 다른 쓰레드가 끼어들지 못하게 하려고 변수를 읽고 쓰는 모든 문장을     
synchronized블럭으로 감쌀 수도 있지만, 더 간단한 방법이 있다. 변수를 선언할 떄 volatile을 붙이는 것이다.    
> 상수에는 volatile을 붙일 수 없다. 즉, 변수에 final과 volatile을 같이 붙일 수 없다.     
> 사실 상수는 변하지 않는 값이므로 멀티쓰레드에 안전하다. 그래서 volatile을 붙일 필요가 없다.     

```java
volatile long sharedVal;
//long타입의 변수를 원자화
volatile double shareVal;
//double타입의 변수를 원자화
```

volatile은 해당 변수에 대한 읽거나 쓰기가 원자화된다. 원자화라는 것은 작업을 더 이상 나눌 수 없게 한다는 의미인데,     
synchronized블럭도 일종의 원자화라고 할 수 있다.      
즉, synchronized블럭은 여러 문장을 원자화함으로써 쓰레드의  동기화를 구현한 것이라고 보면 된다.      
volatile은 변수의 읽거나 쓰기를 원자화 할 뿐, 동기화하는 것은 아니라는 점에 주의하자.     
동기화가 필요할 때 synchronized블럭 대신 volatile을 쓸 수 없다.     

```java
volatile long balance;	//인스턴스 변수 balance를 원자화 한다.

synchronized int getBalance(){	//balance의 값을 반환한다.
	return balance;
}

synchronized void withdraw(int money) {	//balance의 값을 변경
	if(balance>=money) {
		balance -= money;
	}
}
```

위와 같은 코드가 있을 때, 인스턴스변수 balance를 volatile로 원자화했으니까,       
이 값을 읽어서 반환하는 메서드 getBalance()를 동기화 할 필요가 없다고 생각할 수 있다.    
그러나 getBalance()를 synchronized로 동기화하지 않으면,      
withdraw()가 호출되어 객체에 lock을 걸고 작업을 수행하는 중인데도 getBalance()가 호출되는 것이 가능하다.     
출금이 진행 중일 때는 기다렸다가 출금이 끝난 후에 잔고를 조회할 수 있도록 하려면 getBalance()에 synchronized를 붙여서 동기화해야 한다.     

## fork & join 프레임웍
fork & join 프레임웍은 하나의 작업을 작은 단위로 나눠서 여러 쓰레드가 동시에 처리하는 것을 쉽게 만들어 준다.     
먼저 수행할 작업에 따라 RecursiveAction과 RecursiveTask, 두 클래스 중에서 하나를 상속받아 구현해야한다.      
> RecursiveAction : 반환값이 없는 작업을 구현할 때 사용        
> RecursiveTask : 반환값이 있는 작업을 구현할 때 사용          

두 클래스 모두 compute()라는 추상 메서드를 가지고 있는데, 우리는 상속을 통해 이 추상 메서드를 구현하기만 하면 된다.     

```java
public abstract class RecursiveAction extends ForkJoinTask<void> {
	...
	protected abstract void compute();	//상속을 통해 이 메서드를 구현해야 한다.
	...
}
public abstract class RecursiveTask<V> extends ForkJoinTask<V> {
	...
	V result;
	protected abstract V compute();		//상속을 통해 이 메서드를 구현해야 한다.
	...
}
```

예를 들어 1부터 n까지의 합을 계산한 결과를 돌려주는 작업의 구현은 다음과 같이 한다.     

```java
class SumTask extends RecursiveTask<Long> {	//RecursiveTask를 상속받는다.
	long from, to;
	
	SumTask(long from, long to) {
		thisl.from = from;
		this.to = to;
	}
	public Long compute() {
		//처리할 작업을 수행하기 위한 문장을 넣는다.
	}
}
```
그 다음에는 쓰레드풀과 수행할 작업을 생성하고, invoke()로 작업을 시작한다.     
쓰레드를 시작할 때 run()이 아니라 start()를 호출하는 것처럼, fork&join프레임웍으로 수행할 작업도 compute()가 아닌 invoke()로 시작한다.    

```java
ForkJoinPool pool = new ForkJoinPool();	//쓰레드 풀을 생성
SumTask task = new SumTask(from, to);	//수행할 작업을 생성

Long result = pool.invoke(task);		//invoke()를 호출해서 작업 시작
```

ForkJoinPool은 fork&join프레임웍에서 제공하는 쓰레드 풀thread pool로,      
지정된 수의 쓰레드를 생성해서 미리 만들어 놓고 반복해서 재사용할 수 있게 한다.    
그리고 쓰레드를 반복해서 생성하지 않아도 된다는 장점과 너무 많은 쓰레드가 생성되어 성능이 저하되는 것을 막아준다는 장점이 있다.      
쓰레드 풀은 쓰레드가 수행해야하는 작업이 담긴 큐를 제공하며, 각 쓰레드는 자신의 작업 큐에 담긴 작업을 순서대로 처리한다.     
> 쓰레드 풀은 기본적으로 코어의 개수와 동일한 개수의 쓰레드를 생성한다.     

#### compute()의 구현
compute()를 구현할 때는 수행할 작업 외에도 작업을 어떻게 나눌 것인가에 대해서도 알려줘야 한다.

```java
public Long compute() {
	long size = to-from+1;	//from <= i <= to
	
	if(size < 5)	//더할 숫자가 5개 이하면
		return sum();		//숫자의 합을 반환. sum()은 from부터 to까지의 수를 더해서 반환
	
	//범위를 반으로 나눠서 두 개의 작업을 생성
	long half = (from+to)/2;
	
	SumTask leftSum = new SumTask(from, half);
	SumTask rightSum = new SumTack(half+1, to);
	
	leftSum.fork();	//작업leftSum을 작업 큐에 넣는다.
	
	return rightSum.compute() + leftSum.join();
}
```

실제 수행할 작업은 sum()뿐이고 나머지는 수행할 작업의 범위를 반으로 나눠서 새로운 작업을 생성해서 실행시키기 위한 것이다.     
복잡해 보이지만, 작업의 범위를 어떻게 나눌 것인지만 정의해 주면 나머지는 항상 같은 패턴이다.      

> compute()의 구조는 일반적인 재귀호출 메서드와 동일하다.     

#### 다른 쓰레드의 작업 훔쳐오기
fork()가 호출되어 작업 큐에 추가된 작업 역시, compute()에 의해 더 이상 나눌 수 없을때까지 반복해서 나뉘고,     
자신의 작업 큐가 비어있는 쓰레드는 다른 쓰레드의 작업 큐에서 작업을 가져와서 수행한다.     
이것을 작업 훔쳐오기work stealing라고 하며, 이 과정은 모두 쓰레드풀에 의해 자동적으로 수행된다.    
> 작업의 크기를 충분히 작게 해야 각 쓰레드가 골고루 작업을 나눠가질 수 있다.    

#### fork()와 join()
fork()는 작업을 쓰레드의 작업 큐에 넣는 것이고, 작업 큐에 들어간 작업은 더 이상 나눌 수 없을 때까지 나뉜다.    
즉, compute()로 나누고 fork()로 작업 큐에 넣는 작업이 계속해서 반복된다.     
그리고 나눠진 작업은 각 쓰레드가 골고루 나눠서 처리하고, 작업의 결과는 join()을 호출해서 얻을 수 있다.     
fork()와 join()의 중요한 차이점은 fork()는 비동기 메서드asynchronized method이고 join()은 동기 메서드synchronized method라는 것이다.     
> fork() : 해당 작업을 쓰레드 풀의 작업 큐에 넣는다. 비동기 메서드          
> join() : 해당 작업의 수행이 끝날 때까지 기다렸다가, 수행이 끝나면 그 결과를 반환한다. 동기 메서드          

비동기 메서드는 일반적인 메서드와 달리 메서드를 호출만 할 뿐, 그 결과를 기다리지 않는다.     
(내부적으로는 다른 쓰레드에게 작업을 수행하도록 지시만 하고 결과를 기다리지 않고 돌아오는 것이다.)      
그래서 아래의 코드에서, fork()를 호출하면 결과를 기다리지 않고 다음 문장인 return문으로 넘어간다.      
return문에서 compute()가 재귀호출될 때, join()은 호출되지 않는다.     
그러다가 작업을 더 이상 나눌 수 없게 되었을 때, compute()의 재귀호출은 끝나고 join()의 결과를 기다렸다가 더해서 결과를 반환한다.     
재귀호출된 compute()가 모두 종료될 때, 최종 결과를 얻는다.     

```java
public long compute() {
	...
	SumTask leftSum = new SumTask(from, half);
	SumTask rightSum = new SumTask(half+1, to);
	leftSum.fork();	//비동기 메서드. 호출 후 결과를 기다리지 않는다.
	
	return rightSum.compute() + leftSum.join();	//동기 메서드. 호출결과를 기다린다.
}
```

```java
public class ForkJoinEx1 {

	static final ForkJoinPool pool = new ForkJoinPool();	//쓰레드 풀을 생성
	
	public static void main(String[] args) {
		long from = 1L, to = 100_000_000L;
		
		SumTask task = new SumTask(from, to);
		long start = System.currentTimeMillis();
		long result = pool.invoke(task);
		System.out.println("Elapsed time(4 core) : " + (System.currentTimeMillis()-start));
		
		System.out.printf("sum of %d~%d = %d%n",from,to,result);
		System.out.println();
		
		result = 0L;
		start= System.currentTimeMillis();
		
		for(int i = 0; i<=to; i++)
			result+=i;
		
		System.out.println("Elapsed time(1 core): " + (System.currentTimeMillis()-start));
		System.out.printf("sum of %d~%d = %d%n",from,to,result);
	}
}
class SumTask extends RecursiveTask<Long>	{
	long from, to;
	
	SumTask(long from, long to){
		this.from = from;
		this.to = to;
	}
	
	public Long compute() {
		long size = to-from+1;
		
		if(size<=5)
			return sum();
		
		long half = (from+to)/2;
		
		SumTask leftSum = new SumTask(from, half);
		SumTask rightSum = new SumTask(half+1, to);
		
		leftSum.fork();
		
		return rightSum.compute() + leftSum.join();
	}
	
	long sum() {
		
		long tmp = 0L;
		
		for(long i = from; i<=to ; i++)
			tmp += i;

		return tmp;
	}
}

Elapsed time(4 core) : 715
sum of 1~100000000 = 5000000050000000

Elapsed time(1 core): 115
sum of 1~100000000 = 5000000050000000
```
실행결과를 보면, fork&join프레임웍으로 계산한 결과보다 for문으로 계산한 결과가 시간이 덜 걸린 것을 알 수 있다.    
왜냐하면, 작업을 나누고 다시 합치는데 걸리는 시간이 있기 때문이다.    
재귀호출보다 for문이 더 빠른 것과 같은 이유인데, 항상 멀티쓰레드로 처리하는 것이 빠르다고 생각해서는 안 된다.    
반드시 테스트해보고 이득이 있을 때만, 멀티쓰레드로 처리해야 한다.   

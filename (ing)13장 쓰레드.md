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


> static Thread currentThread()	현재 실행중인 쓰레드의 참조를 반환한다.
> String getName()	쓰레드의 이름을 반환한다.

그래서 Thread를 상속받은 ThreadEx1_1에서 간단히 getName()을 호출하면 되지만,    
Runnable을 구현한 ThreadEx1_2에는 멤버가 run()밖에 없기 때문에 Thread클래스의 getName()을 호출하려면,     
Thread.currentThread().getName()와 같이 해야 한다.     


참고로 쓰레의 이름은 다음과 같은 생성자나 메서드를 통해 지정 또는 변경할 수 잏ㅆ따.

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

사실 start()가 호출되었다고 해서 바로 실행되는 것이 아니라, 일단 실행대기 상태에 있다가 자신의 차례까 되어야 실행된다.     
만약 실행대기중인 쓰레드가 하나도 없다면 곧바로 실행상태가 된다.     

> 쓰레드의 실행순서는 OS의 스케쥴러가 작성한 스케쥴에 의해 결정된다.

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

이 예젠ㄴ 하나의 쓰레드로 사용자의 입력을 받는 작업과 화면에 숫자를 출력하는 작업을 처리하기 때문에      
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
public static final int NOR_PRIORITY = 10	//보통우선순위
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
//쓰레드 그룹에 포함된 활성상태ㅔ 있는 쓰레드 그룹의 수를 반환
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


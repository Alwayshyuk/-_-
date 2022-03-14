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
				Thread.sleep(2*1000);
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
쓰레드 프로그래밍이 어려운 이유는 동기화syncronization와 스케줄링scheduling 때문이다.      
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

####join()-다른 쓰레드의 작업을 기다린다.
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

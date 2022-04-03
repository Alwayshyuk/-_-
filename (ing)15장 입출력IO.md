# 자바에서의 입출력

## 입출력이란?

I/O란 Input과 Output의 약자로 입력과 출력을 뜻한다.     
입출력은 컴퓨터 내부 또는 외부의 장치와 프로그램간의 데이터를 주고받는 것을 말한다.    

## 스트림(stream)
자바에서 입출력을 수행하려면, 즉 어느 한쪽에서 다른 쪽으로 데이터를 전달하려면,     
두 대상을 연결하고 데이터를 전송할 수 있는 무언가가 필요한데 이것을 스트림stream이라고 정의했다.     

> 스트림이란 데이터를 운반하는데 사용되는 연결통로이다.   

스트림은 연속적인 데이터의 흐름을 물에 비유해서 붙여진 이름인데, 여러 가지로 유사한 점이 많다.    
물이 한쪽 방향으로만 흐르는 것과 같이 스트림은 단방향통신만 가능하기 때문에 하나의 스트림으로 입력과 출력을 동시에 처리할 수 없다.   
그래서 입력과 출력을 동시에 수행하려면 입력을 위한 입력스트림input stream과 출력을 위한 출력스트림output stream이 모두 필요하다.     
스트림은 먼저 보낸 데이터를 먼저 받게 되어 있으며 중간에 건너뜀 없이 연속적으로 데이터를 주고받는다.   
큐Queue와 같은 FIFO구조로 되어있따고 생각하면 된다.   

## 바이트기반 스트림-IputStream, OutputStream

스트림은 바이트 단위로 데이터를 전송하며 입출력 대상에 따라 다음과 같은 입출력스트림이 있다.     

```
입력스트림: FileInputStream
출력스트림: FileOutputStream
입출력 대상의 종류: 파일

입력스트림: ByteArrayInputStream
출력스트림: ByteArrayOutputStream
입출력 대상의 종류: 메모리(byte 배열)

입력스트림: PipedInputStream
출력스트림: PipedOutputStream
입출력 대상의 종류: 프로세스(프로세스간의 통신)

입력스트림: AudioInputStream
출력스트림: AudioOutputStream
입출력 대상의 종류: 오디오장치
```

위와 같이 여러 종류의 입출력 스트림이 있으며, 어떠한 대상에 대해서 작업을 할 것인지     
그리고 입력을 할 것인지 출력을 할 것인지에 따라서 해당 스트림을 선택해서 사용하면 된다.     
예를 들어 어떤 파일의 내용을 읽고자하면 FileInputStream을 사용하면 될 것이다.    
이들은 모두 InputStream 또는 OutputStream의 자손들이며, 각각 읽고 쓰는데 필요한 추상메서드를 자신에 맞게 구현해 놓았다.     
자바에서는 java.io패키지를 통해서 많은 종류의 입출력관련 클래스들을 제공하고 있으며,    
입출력을 처리할 수 있는 표준화된 방법을 제공함으로써 입출력의 대상이 달라져도    
동일한 방법으로 입출력이 가능하기 때문에 프로그래밍을 하기에 편리하다.    

```java
InputStream: abstract int read()
OutputStream: abstract void write(int b)

InputStream: int read(byte[] b)
OutputStream: void write(byte[] b)

InputStream: int read(byte[] b, int off, int len)
OutputStream: void write(byte[] b, int off, int len)

inputStream과 OutputStream에 정의된 읽기와 쓰기를 수행하는 메서드
```
> read의 반환타입이 byte가 아니라 int인 이유는 read()의 반환값의 범위가 0~255와 -1이기 때문이다.    

InputStream의 read()와 OutputStream의 write(int b)는 입출력의 대상에 따라 읽고 쓰는 방법이 다를 것이기 때문에     
각 상황에 알맞게 구현하라는 의미에서 추상메서드로 정의되어 있다.    
read()와 write(int b)를 제외한 나머지 메서드들은 추상메서드가 아니니까 굳이 추상메서드인 read()와 write(int b)를    
구현하지 않아도 이들을 사용할 수 있을 것이라고 생각할 수 있겠지만 사실 추상메서드인 read()와 write(int b)를 이용해서     
구현한 것들이기 때문에 read()와 write(int b)가 구현되어 있지 않으면 이들은 아무런 의미가 없다.     

```java
public abstract class InputStream{
	...
	//입력스트림으로부터 1byte를 읽어서 반환한다. 읽을 수 없으면 -1을 반환한다.
	abstract int read();
	
	//입력스트림으로부터 len개의 byte를 읽어서 byte배열 b의 off위치부터 저장한다.
	int read(byte[] b, int off, int len){
		...
		for(int i = off; i<off+len; i++){
			//read()를 호출해서 데이터를 읽어서 배열을 채운다.
			b[i] = (byte)read();
		}
	...
	//입력스트림으로부터 byte배열 b의 크기만큼 데이터를 읽어서 배열 b에 저장한다.
	int read(byte[] b){
		return read(b, 0, b.length);
	}
	...
}
```

이 코드는 InputStream의 실젲 소스코드의 일부를 이해하기 쉽게 약간 변형한 것인데,   
여기서 read(byte[] b, int off, int len)의 코드를 보면 read()를 호출하는 것을 알 수 있다.     
read()가 추상메서드이지만 이처럼 read(byte[] b, int off, int len)의 내에서 read()를 호출할 수 있다.     
메서드는 선언부만 알고 있어도 호출이 가능하기 때문에, 추상메서드를 호출하는 코드를 작성할 수 있다.    
실제로는 추상클래스를 상속받아서 추상메서드를 구현한 클래스의 인스턴스에 대해서 추상메서드가 호출될 것이기 떄문에    
추상메서드를 호출하는 코드를 작성해도 아무런 문제가 되지 않는다.    

## 보조 스트림

위에서 언급한 스트림 외에도 스트림의 기능을 보완하기 위한 보조스트림이 제공된다.   
보조스트림은 실제 데이터를 주고받는 스트림이 아니기 때문에 데이터를 입출력할 수 있는 기능은 없지만,    
스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있다.    
그래서 보조스트림만으로는 입출력을 처리할 수 없고, 스트림을 먼저 생성한 다음에 이를 이용해서 보조스트림을 생성해야한다.    
예를 들어 test.txt라는 파일을 읽기위해 FileIntputStream을 사용할 때,     
입력 성능을 향상시키기 위해 버퍼를 사용하는 보조스트림인 BufferedInputStream을 사용하는 코드는 다음과 같다.    

```java
//먼저 기반스트림을 생성한다.
FileInputStream fis = new FileInputStream("text.txt");

//기반스트림을 이용해서 보조스트림을 생성한다.
BufferedInputStream bis = new BufferedInputStream(fis);

//보조스트림인 BufferedInputStream으로부터 데이터를 읽는다.
bis.read();
```

코드 상으로는 보조스트림인 BufferedInputStream이 입력기능을 수행하는 것처럼 보이지만,    
실제 입력기능은 BufferedInputStream과 연결된 FIleInputStream이 수행하고,     
보조스트림인BufferedInputStream은 버퍼만을 제공한다.    
버퍼를 사용한 입출력과 사용하지 않은 입출력간의 성능차이는 상당하기 때문에 대부분의 경우에 버퍼를 이용한 보조스트림을 사용한다.    
BufferedInputStream, DataInputStream, DigestInputStream, LineNumberInputStream,     
PushbackInputStream은 모두 FilterInputStream의 자손들이고,     
FilterInputStream은 InputStream의 자손이라서 결국 모든 보조스트림 역시     
InputStream과 OutputStream의 자손들이므로 입출력방법이 같다.    

```
보조스트림의 종류

입력: FilterInputStream
출력: FilterOutputStream
설명: 필터를 이용한 입출력 처리


입력: BufferedInputStream
출력: BufferedOutputStream
설명: 버퍼를 이용한 입출력 성능향상

입력: DataInputStream
출력: DataOutputStream
설명: int, float과 같은 기본형 단위primary type로 데이터를 처리하는 기능

입력: SequenceInputStream
출력: 없음
설명: 두 개의 스트림을 하나로 연결

입력: LineNumberInputStream
출력: 없음
설명: 읽어 온 데이터의 라인 번호를 카운트(JDK 1.1부터 LineNumberReader로 대체

입력: ObjectInputStream
출력: ObjectOutputStream
설명: 데이터를 객체단위로 읽고 쓰는데 사용, 주로 파일을 이용하며 객체 직렬화와 관련있음

입력: 없음
출력: PrintStream
설명: 버퍼를 이ㅛㅇ하며 추가적인 print 관련 기능 print,printf,println메서드

입력: PushbackInputStream
출력: 없음
설명: 버퍼를 이용해서 읽어 온 데이터를 다시 되돌리는 기능unread, push back to buffer
```

## 문자기반 스트림 - Reader, Writer
Java에서는 한 문자를 의미하는 char형이 1byte가 아니라 2byte이기 때문에      
바이트기반의 스트림으로 2byte인 문자를 처리하는데는 어려움이 있다.      
이 점을 보완하기 위해서 문자기반의 스트림이 제공된다.     
문자데이터를 입출력할 때는 바이트기반 스트림 대신 문자기반 스트림을 사용하자.    

> InputStream - Reader    
> OutputStream - Writer

```
바이트기반 스트림: FileInputStream, FileOutputStream
문자기반 스트림: FileReader, FileWriter

바이트기반 스트림: ByteArrayInputStream, ByteArrayOutputStream
문자기반 스트림: CharArrayInputStream, CharArrayOutputStream

바이트기반 스트림: PipedInputStream, PipedOutputStream
문자기반 스트림: PipedReader, PipedWriter

바이트기반 스트림:StringBufferInputStream(deprecated), StringBufferOutputStream(deprecated) 
문자기반 스트림: StringReader, StringWriter
```

문자기반 스트림의 이름은 바이트기반 스트림의 이름에서 InputStream은 Reader로 OutputStream은 Writer로 바꾸면 된다.     
단, ByteArrayInputStream에 대응하는 문자기반 스트림은 char배열을 사용하는 CharArrayReader이다.    


아래는 바이트기반 스트림과 문자기반 스트림의 읽기와 쓰기에 사용되는 메서드를 비교한 것인데     
byte배열 대신 char배열을 사용한다는 것과 추상메서드가 달라졌다.     
Reader와 Writer에서도 역시 추상메서드가 아닌 메서드들은 추상메서드를 이용해서 작성되었으며,     
프로그래밍적인 관점에서 볼 때 read()를 추상메서드로 하는 것보다 int read(char[] cbuf, int off, int len)를    
추상메서드로 하는 것이 더 바람직하다.    

```
바이트기반 스트림과 문자기반 스트림의 읽고 쓰는 메서드 비교

InputStream
abstract int read()
int read(byte[] b)
int read(byte[] b, int off, int len)

Reader
int read()
int read(char[] cbuf)
abstract int read(char[] cbuf, int off, int len)

OutputStream
abstract void write(int b)
void write(byte[] b)
void write(byte[] b, int off, int len)

Writer
void write(int c)
void write(char[] cbuf)
abstract void write(char[] cbuf, int off, int len)
void write(String str)
void write(String str, int off, int len)
```

보조 스트림 역시 다음과 같은 문자기반 보조스트림이 존재하며 사용목적과 방식은 바이트기반 보조스트림과 다르지 않다.    

```
바이트기반 보조스트림과 문자기반 보조스트림

바이트기반 보조스트림: BufferedInputStream, BufferedOutputStream
문자기반 보조스트림: BufferedReader, BufferedWriter

바이트기반 보조스트림: FilterInputStream, FilterOutputStream
문자기반 보조스트림: FilterReader, FilterWriter

바이트기반 보조스트림: LineNumberInputStream(deprecated)
문자기반 보조스트림: LineNumberReader

바이트기반 보조스트림: PrintStream
문자기반 보조스트림: PrintWriter

바이트기반 보조스트림: PushbackInputStream
문자기반 보조스트림: PushbackReader
```

# 바이트기반의 보조스트림

## FilterInputStream과 FilterOutputStream

FilterInputStream/FilterOutputStream은 InputStream/OutputStream의 자소이면서 모든 보조스트림의 조상이다.    
보조스트림은 자체적으로 입출력을 수행할 수 없기 때문에 기반스트림을 필요로 한다.   
아래는 FilterInputStream/FilterOutputStream의 생성자다.    

> protected FilterInputStream(InputStream in)     
> public FilterOutputStream(OutputStream out)

FilterInputStream/FilterOutputStream의 모든 메서드는 단순히 기반스트림의 메서드를 그대로 호출할 뿐이다.    
FilterInputStream/FilterOutputStream자체로는 아무런 일도 하지 않음을 의미한다.    
FilterInputStream/FilterOutputStream은 상속을 통해 원하는 작업을 수행하도록 읽고 쓰는 메서드를 오버라이딩해야 한다.    

```java
public class FilterInputStream extends InputStream{
	protected volatile InputStream in;
	protected FilterInputStream(InputStream in) {
		this.in = in;
	}
	public int read() throws IOException{
		return in.read();
	}
	...
}
```
생성자 FilterInputStream(InputStream in)은 접근 제어자가 protected이기 때문에      
인스턴스를 생성해서 사용할 수 없고 상속을 통해서 오버라딩되어야 한다.    
FilterInputStream/FilterOutputStream을 상속받아서 기반스트림에 보조기능을 추가한 보조스트림 클래스는 다음과 같다.    

> FilterInputStream의 자손: BufferedInputStream, DataInputStream, PushbackInputStream 등    
> FilterOutputStream의 자손: BufferedOutputStream, DataOutputStream, PrintStream 등

## BufferedInputStream과 BufferedOutputStream
BufferedIntputStream/BufferedOutputStream은 스트림의 입출력 효율을 높이기 위해 버퍼를 사용하는 보조스트림이다.     
한 바이트씩 입출력하는 것 보다는 버퍼(바이트배열)를 이용해서     
한 번에 여러 바이트를 입출력하는 것이 빠르기 때문에 대부분의 입출력 작업에 사용된다.      

```
BufferedInputStream의 생성자

생성자: BufferedInputStream(InputStream in, int size)
설명: 주어진 InputStream인스턴스를 입력소스(input source)로 하며 지정된 크기(byte단위)의 버퍼를 갖는 BufferedInputStream인스턴스를 생성한다.

생성자: BufferedInputStream(InputStream in)
설명: 주어진 InputStream의 인스턴스를 입력소스로 하며 버퍼의 크기를 지정해주지 않으므로 기본적으로 8192byte 크기의 버퍼를 갖게 된다
```

BufferedInputStream의 버퍼크기는 입력소스로부터 한 번에 가져올 수 있는 데이터의 크기로 지정하면 좋다.     
보통 입력소스가 파일인 경우 8192(8k)정도의 크기로 하는 것이 보통이며, 버퍼의 크기를 변경해가면서 테스트하면 최적의 버퍼크기를 알아낼 수 있다.    
프로그램에서 입력소스로부터 데이터를 읽기 위해 처음으로 read메서드를 호출하면,     
BufferedInputStream은 입력소스로 부터 버퍼 크기만큼의 데이터를 읽어다 자신의 내부 버퍼에 저장한다.     
이제 프로그램에서는 BufferedInputStream의 버퍼에 저장된 데이터를 읽으면 되는 것이다.    
외부의 입력소스로 부터 읽는 것보다 내부의 버퍼로 부터 읽는 것이 훨씬 빠르기 때문에 그만큼 작업 효율이 높아진다.    
프로그램에서 버퍼에 저장된 모든 데이터를 다 읽고 그 다음 데이터를 읽기위해 read메서드가 호출되면,     
BufferedInputStream은 입력소스로부터 다시 버퍼크기 만큼의 데이터를 읽어다 버퍼에 저장해 놓는다. 이 작업이 계속해서 반복된다.    

```
BufferedOutputStream의 생성자와 메서드

생성자: BufferedOutputStream(OutputStream out, int size)
설명: 주어진 OutputStream인스턴스를 출력소스(Output Source)로하며 지정된 크기(단위 byte)의 버퍼를 갖는 BufferedOutputStream인스턴스를 생성한다.

생성자: BufferedOutputStream(OutputStream out)
설명: 주어진 OutputStream인스턴스를 출력소스로 하며 버퍼의 크기를 지정해주지 않으므로 기본적으로 8192byte 크기의 버퍼를 갖게 된다.

메서드: flush()
설명: 버퍼의 모든 내용을 출력소스에 출력한 다음, 버퍼를 비운다.

메서드: close()
설명: flush()를 호출해서 버퍼의 모든 내용을 출력소스에 출력하고, BufferedOutputStream인스턴스가 사용하던 모든 자원을 반환한다.
```

BufferedOutputStream 역시 버퍼를 이용해서 출력소스와 작업을 하게 되는데,    
입력소스로부터 데이터를 읽을 때와는 반대로 프로그램에서 write메서드를 이용한 출력이 BufferedOutputStream의 버퍼에 저장된다.     
버퍼가 가득 차면, 그때 버퍼의 모든 내용을 출력소스에 출력한다.    
그리고는 버퍼를 비우고 다시 프로그램으로부터의 출력을 저장할 준비를 한다.   
버퍼가 가득 찼을 때만 출력소스에 출력을 하기 때문에, 마지막 출력부분이 출력소스에 쓰이지 못하고        
BufferedOutputStream의 버퍼에 남아있는 채로 프로그램이 종료될 수 있다는 점을 주의해야한다.     
그래서 프로그램에서 모든 출력작업을 마친 후 BufferedOutputStream에 close()나 flusch()를 호출해서     
마지막에 버퍼에 있는 모든 내용이 출력소스에 출력되도록 해야 한다.     

> BufferedOutputStream의 close()는 flush()를 호출하여 버퍼의 내용을 출력 스트림에 쓰도록 한 후,     
> BufferedOutputStream인스턴스의 참조변수에 null을 지정함으로써 사용하던 자원들이 반환되게 한다. 

```jaca
import java.io.*;

public class BufferedOutputStreamEx1 {

	public static void main(String[] args) {
		try {
			FileOutputStream fos = new FileOutputStream("123.txt");
			//BufferedOutputStream의 버퍼 크기를 5로 한다.
			BufferedOutputStream bos = new BufferedOutputStream(fos, 5);
			//파일 123.txt에 1부터 9까지 출력한다.
			for(int i = '1'; i<='9'; i++)
				bos.write(i);		
			
			fos.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
123.txt: 12345
```

크기가 5인 BufferedOutputStream을 이용해서 파일 123.txt에 1부터 9까지 출력하는 예제인데    
결과를 보면 5까지만 출력된 것을 알 수 있다.     
그 이유는 버퍼에 남아있는 데이터가 출력되지 못한 상태로 프로그램이 종료되었기 때문이다.     
이 예제에서 fos.close()를 호출해서 스트림을 닫아주기는 했지만,     
이렇게 해서는 BufferedOutputStream의 버퍼에 있는 내용이 출력되지 않는다.    
bos.close(); 와 같이 해서 BufferedOutputStream의 close()를 호출해 주어야 버퍼에 남아있던 모든 내용이 출력된다.     
BufferedOutputStream의 close()는 기반 스트림인 FileOutputStream의 close()를 호출하기 때문에     
FileOutputStream의 close()는 따로 호출해주지 않아도 된다.    


아래의 코드는 BufferedOutputStream의 조상인 FilterOutputStream의 소스코드인데    
FilterOutputStream에 정의된 close()는 flush()를 호출한 다음에 기반스트림의 close()를 호출하는 것을 알 수 있다.     
BufferedOutputStream은 FilterOutputStream의 close()를 오버라이딩없이 그대로 상속받는다.   

```java
public class FilterOutputStream extends OutputStream {
	protected OutputStream out;
	public FilterOutputStream(OutputStream out) {
		this.out = out;
	}
	...
	public void close() throws IOException {
		try{
			flush();
		} catch(IOException ignored) {}
		out.close();	//기반스트림의 close()를 호출한다.
	}
}
```
이처럼 보조스트림을 사용한 경우에는 기반스트림의 close()나 flush()를 호출할 필요없이     
단순히 보조스트림의 close()를 호출하기만 하면 된다.

# 컬렉션 프레임웍Collenctions Framework
컬렉션 프레임웍이란, 데이터 군을 저장하는 클래스들을 표준화한 설계를 뜻한다.     
컬렉션은 데이터 그룹, 프레임웍은 표준화된 프로그래밍 방식을 의미한다.   
> Vector와 같이 다수의 데이터를 저장할 수 있는 클래스를 '컬렉션 클래스'라고 한다.

## 컬렉션 프레임웍의 핵심 인터페이스
컬렉션 프레임웍에서는 컬렉션데이터 그룹을 3가지 타입이 존재한다고 인식하고    
각 컬렉션을 다루는데 필요한 기능을 가진 3개의 인터페이스를 정의하였다.     
> 그리고 인터페이스 List와 Set의 공통된 부분을 뽑아서 새로운 인터페이스인 Collection을 추가로 정의했다.

* List : 순서가 있는 데이터의 집합. 데이터의 중복을 허용한다.
> 구현클래스: ArrayList, LinkedList, Stack, Vector 등
* Set : 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다.
> 구현클래스: HashSet, TreeSet 등
* Map : 키key와 값value의 쌍pair으로 이루어진 데이터의 집합. 순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.
> 구현클래스: HashMap, TreeMap, Hashtable, Properties 등

#### Collection인터페이스
List와 Set의 조상인 Collection인터페이스에는 다음과 같은 메서드들이 정의되어 있다.
```java
boolean add(Object o)            
boolean addAll(Collection c)
//지정된 객체o 또는 Collection(c)의 객체들을 Collection에 추가한다.
void clear()                     
//Collection의 모든 객체를 삭제한다.
boolean contains(Object o)     
boolean containsAll(Collection c)
//지정된 객체o 또는 Collection의 객체들이 Collection에 포함되어 있는지 확인한다.
boolean equals(Object o)          
//동일한 Collection인지 비교한다.
int hashCode()                     
//Collection의 hash code를 반환한다.
boolean isEmpty()                  
//Collection이 비어있는지 확인한다.
Iterator iterator()                
//Collection의 iterator를 얻어서 반환한다.
boolean remove(Object o)           
//지정된 객체를 삭제한다.
boolean removeAll(Collection c)    
//지정된 Collection에 포함된 객체들을 삭제한다.
boolean ratailAll(Collection c)
//지정된 Collection에 포함된 객체만을 남기고 다른 객체들은 Collection 에서 삭제한다.
//이 작업으로 인해 Collection에 변화가 있으면 true 없으면 false를 반환한다.
int size()
//Collection에 저장된 객체의 개수를 반환한다.
Object[] toArray()
//Collection에 저장된 객체를 객체배열(Object[])에 반환한다.
Object[] toArray(Object[] a)
//지정된 배열에 Collection의 객체를 저장해서 반환한다.
```

#### List인터페이스
 List인터페이스는 중복을 허용하면서 저장순서가 유지되는 컬렉션을 구현하는데 사용된다.
 ```java
 void add(int index, Object element)
 boolean addAll(imt index, Collection c)
 // 지정된 위치index에 객체element 또는 컬렉션에 포함된 객체들을 추가한다.
 Object get(int index)
 // 지정된 위치index에 있는 객체를 반환한다.
 int indexOf(Object o)
 // 지정된 객체의 위치index를 반환한다.(List의 첫 번째 요소부터 순방향으로 찾는다.)
 int lastIndexOf(Object o)
 // 지정된 객체의 위치 index를 반환한다.(List의 마지막 요소부터 역방향으로 찾는다.)
 ListIterator listIterator()
 ListIterator listIterator(int index)
 //List의 객체에 접근할 수 있는 ListIterator를 반환한다.
 Object remove(int index)
 //지정된 위치index에 객체element를 저장한다.
 void sort(Comparator c)
 //지정된 비교자comparator 로 List를 정렬한다.
 List subList(int fromIndex, int toIndex)
 //지정된 범위 (fromIndex부터 toIndex까지)에 있는 객체를 반환한다.
 ```
 
 #### Set인터페이스
  Set인터페이스는 중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용된다.    
  
  #### Map인터페이스
  키key와 값value을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용된다.    
  키는 중복될 수 없지만 값은 중복을 허용한다.      
  기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게된다.
  ```java
  void clear()
  //Map의 모든 객체를 삭제한다.
  boolean containsKey(Object o)
  //지정된 key객체와 일치하는 Map의 key객체가 있는지 확인한다.
  boolean containsValue(Object value)
  //지정된 value객체와 일치하는 Map의 value객체가 있는지 확인한다.
  Set entrySet()
  //Map에 저장되어 있는 key-value쌍을 Map.Entry타입의 객체로 저장한 Set으로 반환한다.
  boolean equals(Object o)
  //동일한 Map인지 비교한다.
  Object get(Object key)
  //지정한 key객체에 대응하는 value객체를 찾아서 반환한다.
  int hashCode()
  //해시코드를 반환한다.
  boolean isEmpty()
  //Map이 비어있는지 확인한다.
  Set keySet()
  //Map에 저장된 모든 key객체를 반환한다.
  Object put(Object key, Object value)
  //Map에 value객체를 key객체에 연결mapping하여 저장한다.
  void putAll(Map t)
  //지정된 Map의 모든 key-value쌍을 추가한다.
  Object remove(Object key)
  //지정한 key객체와 일치하는 key-value객체를 삭제한다.
  int size()
  //Map에 저장된 key-value쌍의 개수를 반환한다.
  Collection values()
  //Map에 저장된 모든 value객체를 반환한다.
  ```
  > Map인터페이스에서 값value는 중복을 허용하기 때문에 Collection타입으로 반환하고   
  > 키key는 중복을 허용하지 않기 때문에 Set타입으로 반환한다.

#### Map.Entry인터페이스
Map.Entry인터페이스는 Map인터페이스의 내부 인터페이스이다.   
내부 클래스와 같이 인터페이스도 인터페이스 안에 인터페이스를 정의하는 내부 인터페이스를 정의하는 것이 가능하다.
```java
public interface Map{
  ...
   public static interface Entry {
      Object getKey();        //Entry의 key객체를 반환한다.
      Object getValue();      //Entry의 value객체를 반환한다.
      Object setValue(Object value);     //Entry의 value객체를 지정된 객체로 바꾼다.
      boolean equals(Object o);          //동일한 Entry인지 비교한다.
      int hashCode();                    //Entry의 해시코드를 반환한다.
 ```
 
 ## ArrayLisst
 ArrayList는 List인터페이스를 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용한다.     
 기존의 Vector를 개선한 것으로 Vector와 구현원리와 기능적인 측면에서 동일하다고 할 수 있다.     
 >ArrayList는 Object배열을 이용해서 데이터를 순차적으로 저장한다.    
 >더 이상 저장할 공간이 없으면 보다 큰 배열을 생성해서 기존의 배열에 저장된 내용을 복사한 다음 저장된다.
 ```java
 ArrayList()                                
 //크기가 10인 ArrayList를 생성
 ArrayList(Collection c)                    
 //주어진 컬렉션이 저장된 ArrayList를 생성
 ArrayList(int initialCapacity)             
 //지정된 초기용량을 갖는 ArrayList를 생성
 boolean add(Object c)                      
 //ArrayList의 마지막에 객체를 추가. 성공하면 true
 void add(int index, Object element)        
 //지정된 위치index에 객체를 저장
 boolean addAll(Collection c)               
 //주어진 컬렉션의 모든 객체를 저장
 boolean addAll(int index, Collection c)
 //지정된 위치부터 주어진 컬렉션의 모든 객체를 저장
 void clear()
 //ArrayList를 완전히 비운다.
 Object clone()
 //ArrayList를 복제한다.
 boolean contains(Object o)
 //지정된 객체o가 ArrayList에 포함되어 있는지 확인
 void ensureCapacity(int minCapacity)
 //ArrayList의 용량이 최소한 minCapacity가 되도록 한다.
 Object get(int index)
 //지정된 위치index에 저장된 객체를 반환한다.
 int indexOf(Object o)
 //지정된 객체가 저장된 위치를 찾아서 반환한다.
 boolean isEmpty()
 //ArrayList가 비어있는지 확인한다.
 Iterator iterator()
 //ArrayList의 Iterator객체를 반환
 int lastIndexOF(Object o)
 //객체o가 저장된 위치를 끝부터 역방향으로 검색해서 반환
 ListIterator listIterator()
 //ArrayList의 ListIterator를 반환
 ListIterator listIterator(int index)
 //ArrayList에 지정된 위치부터 시작하는 ListIterator를 반환
 Object remove(int index)
 //지정된 위치index에 있는 객체를 제거한다.
 boolean remove(Object o)
 //지정한 객체를 제거한다.
 boolean removeAll(Collection c)
 //지정한 컬렉션에 저장된 것과 동일한 객체들을 ArrayList에서 제거한다.
 boolean retainAll(Collection c)
 //ArrayList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만을 남기고 나머지는 삭제한다.
 Object set(int index, Object element)
 //주어진 객체element를 지정된 위치index에 저장한다.
 int size()
 //ArrayList에 저장된 객체의 개수를 반환한다.
 void sort(Comparator c)
 //지정된 정렬기준c으로 ArrayList를 정렬
 List subList(int froomIndex, int toIndex)
 //fromIndex부터 toIndex사이에 저장된 객체를 반환한다.
 Object[] toArray()
 //ArrayList에 저장된 모든 객체들을 객체배열로 반환한다.
 Object[] toArray(Object[] a)
 //ArrayList에 저장된 모든 객체들을 객체배열 a에 담아 반환한다.
 void trimToSize()
 //용량을 크기에 맞게 줄인다.(빈 공간을 없앤다)
 ```
 ```java
 public class ArrayListEx1 {

	public static void main(String[] args) {
		
		ArrayList list1 = new ArrayList();
		list1.add(new Integer(5));
		list1.add(new Integer(4));
		list1.add(new Integer(2));
		list1.add(new Integer(0));
		list1.add(new Integer(1));
		list1.add(new Integer(3));
		
		ArrayList list2 = new ArrayList(list1.subList(1, 4));
		print(list1, list2);
//		list1: [5, 4, 2, 0, 1, 3]
//		list2: [4, 2, 0]
						
		Collections.sort(list1);
		Collections.sort(list2);
		print(list1, list2);
//		list1: [0, 1, 2, 3, 4, 5]
//		list2: [0, 2, 4]

		
		System.out.println("list1.containsAll(list2): "+list1.containsAll(list2));
		
		list2.add("B");
		list2.add("C");
		list2.add(3, "A");
		print(list1, list2);
//		list1.containsAll(list2): true
//		list1: [0, 1, 2, 3, 4, 5]
//		list2: [0, 2, 4, A, B, C]
				
		System.out.println("list1.retainAll(list2):  " + list1.retainAll(list2));
		print(list1, list2);
//		list1.retainAll(list2):  true
//		list1: [0, 2, 4]
//		list2: [0, 2, 4, A, B, C]
				
		for(int i = list2.size()-1; i>=0; i--) {
			if(list1.contains(list2.get(i)))
				list2.remove(i);
		}
		//만일 변수i를 증가시켜가면서 삭제하면, 한 요소가 삭제될 때마다 빈 공간을 채우기 위해
		//나머지 요소들이 자리이동을 하기 때문에 올바른 결과를 얻을 수 없다.
		print(list1, list2);
//		list1: [0, 2, 4]
//		list2: [A, B, C]

		
		
	}
	static void print(ArrayList list1, ArrayList list2) {
		System.out.println("list1: "+list1 );
		System.out.println("list2: "+list2 );
		System.out.println();
	}

}
```
```java

public class ArrayListEx2 {

	public static void main(String[] args) {
		final int LIMIT = 10;			//자르려는 글자 개수 지정
		String source = "0123456789abcdefghijABCDEFGHIJ!@#$%^&*()ZZZ";
		int length = source.length();	//43
		
		List list = new ArrayList(length/LIMIT + 10);
		// 지정 크기보다 더 많은 객체를 저장하면 자동적으로 크기가 늘어나지만
		//이 과정에서 처리시간이 많이 소요되므로 여유있게 지정하는 것이 좋다.
		for(int i = 0; i<length; i+=LIMIT) {
			if(i+LIMIT < length)
				list.add(source.substring(i, i+LIMIT));
			else
				list.add(source.substring(i));
		}
		
		
		for(int i = 0; i<list.size();i++)
			System.out.println(list.get(i));
//		0123456789
//		abcdefghij
//		ABCDEFGHIJ
//		!@#$%^&*()
//		ZZZ
	}

}
```java
package practice;

import java.util.Vector;

public class VectorEx1 {

	public static void main(String[] args) {
		Vector v = new Vector(5);	//용량capacity이 5인 Vector를 생성한다.
		v.add("1");
		v.add("2");
		v.add("3");
		print(v);		//[1, 2, 3]
						//size: 3
						//capacity: 5
		
		v.trimToSize();//빈 공간을 없앤다.	배열은 크기를 변경할 수 없기 때문에 새로운 배열을 생성해서 그 주소값을 v에 할당한다.
		print(v);		//[1, 2, 3]		기존의 Vector인스턴스는 더 이상 사용할 수 없으며, 후에 가비지컬렉터에 의해 메모리에서 제거된다.
						//size: 3
						//capacity: 3
		
		v.ensureCapacity(6); 		//용량을 최소 6으로 늘린다. 이미 용량이 6을 넘는다면 아무 일도 일어나지 않는다.
		print(v);		//[1, 2, 3]
						//size: 3
						//capacity: 6
		
		v.setSize(7);				//사이즈를 7로 만든다. capacity는 항상 기존의 크기보다 2배로 증가하므로 12다.
		print(v);		//[1, 2, 3, null, null, null, null]
						//size: 7
						//capacity: 12
		
		v.clear();		//모두 삭제한다.
		print(v);		//[]
						//size: 0
						//capacity: 12
	}
	
	static void print(Vector v) {
		System.out.println(v);
		System.out.println("size: "+v.size());
		System.out.println("capacity: " + v.capacity());
	}

}
```
>다음 예제는 Vector클래스의 실제코드를 바탕으로 이해하기 쉽게 재구성한 것이다.
```java
public class MyVector {
	Object[] data = null;	//객체들 담기 위한 객채배열을 선언한다.
	int capacity = 0;
	int size = 0;
	
	public MyVector(int capacity) {
		if(capacity<0)
			throw new IllegalArgumentException("유효하지 않은 값입니다." + capacity);
		
		this.capacity = capacity;
		data = new Object [capacity];
	}
	public MyVector() {
		this(10);
	}
	
	//최소한의 저장공간(capacity)를 확보하는 메서드
	public void ensureCapacity(int minCapacity) {
		if(minCapacity-data.length>0)
			setCapacity(minCapacity);
	}
	
	public boolean add(Object obj) {
		//새로운 객체를 저장하기 전에 저장할 공간을 확보한다.
		ensureCapacity(size+1);
		data[size++] = obj;
		return true;
	}
	public Object get(int index) {
		if(index<0 || index>=size)
			throw new IndexOutOfBoundsException("범위를 벗어났습니다.");
		return data[index];
	}
	public Object remove(int index) {
		Object oldObj = null;
		
		if(index<0 || index>=size)
			throw new IndexOutOfBoundsException("범위를 벗어났습니다.");
		oldObj = data[index];
		
		//삭제하고자 하는 객체가 마지막 객체가 아니라면, 배열복사를 통해 빈자리를 채워줘야 한다.
		if(index != size-1)
			System.arraycopy(data, index+1, data, index, size-index-1);
		
		//마지막 데이터를 null로 한다. 배열은 0부터 시작하므로 마지막 요소는 index가 size-1이다.
		data[size-1] = null;
		size--;
		return oldObj;
	}
	public boolean remove(Object obj) {
		for(int i = 0; i<size; i++) {
			if(obj.equals(data[i])) {
				remove(i);
				return true;
			}
		}
		return false;
	}
	public void trimToSize() {
		setCapacity(size);
	}
	private void setCapacity(int cpapcity) {
		if(this.capacity == capacity)	return;
		
		Object[] tmp = new Object[capacity];
		System.arraycopy(data, 0, tmp, 0, size);
		data = tmp;
		this.capacity = capacity;
	}
	public void clear() {
		for(int i = 0; i<size; i++)
			data[i] = null;
		size = 0;
	}
	public Object[] toArray() {
		Object[] result = new Object[size];
		System.arraycopy(data, 0, result, 0, size);
		
		return result;
	}
	public boolean isEmpty() { return size==0;}
	public int capacity()	{return capacity;}
	public int size()		{return size;}
	
	//List인터페이스로부터 상속받은 메서드들
	
	//public int size();
	//public boolean isEmpty();
	public boolean contains(Object o)	{return false;}
	public Iterator iterator()	{return null;}
	//public Object[] toArray();
	public Object[] toArray(Object a[]) {return null;}
	//public boolean add(Object o);
	//public boolean remove(Object o);
	public boolean containsAll(Collection c) { return false; }
	public boolean addAll(Collection c)	{return false;}
	public boolean addAll(int index, Collection c)	{return false;}
	public boolean removeAll(Collection c)	{return false;}
	public boolean retainAll(Collection c)	{return false;}
	//public void clear();
	public boolean equals(Object o)	{return false;}
	//public int hashCode();
	//public Object get(int index);
	public Object set(int index, Object element)	{return null;}
	public void add(int index, Object element) {}
	//public Object remove(int index);
	public int indexOf(Object o) {return -1;}
	public int lastIndexOf(Object o)	{return -1;}
	public ListIterator listIterator()	{return null;}
	public ListIterator listIterator(int index)	{return null;}
	public List subList (int fromIndex, int toIndex)	{return null;}
	
	default void sort(Comparator c)	{////}
	default Spliterator spliterator()	{////}
	default void replaceAll(UnaryOperator operator)	{////}
	}
}
```

> ArrayList나 Vector 같이 배열을 이용한 자료구조는 데이터를 읽어오고 저장하는 데는 효율이 좋지만,    
> 용량을 변경해야할 때는 새로운 배열을 생성한 후 기존의 배열로부터 데이터를 복사해야하기 때문에 효율이 떨어진다.

## LinkedList
배열은 가장 기본적인 형태의 자료구조로 구조가 간단하며 사용하기 쉽고     
데이터를 읽어오는데 걸리는 시간(접근시간)이 가장 빠르다는 장점을 갖고 있다.       
하지만 크기를 변경할 수 없다는 단점과 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다는 단점이 있다.
> 이러한 배열의 단점을 보완하기 위해서 Linked list라는 자료구조가 고안되었다.    
> 배열은 모든 데이터가 연속적으로 존재하지만       
> 링크드 리스트는 불연속적으로 존재하는 데이터를 서로 연결한 형태로 구성되어있다.
```java
class Node{
	Node next;		//다음 요소의 주소를 저장
	Object obj;	}	//데이터를 저장
```
삭제하고자 하는 요소의 이전요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하면 요소가 삭제된다.      
새로운 데이터를 추가할 때는 새로운 요소를 생성한 다음 추가하고자 하는 위치의 이전요소의 참조를       
새로운 요소에 대한 참조로 변경해주고, 새로운 요소가 그 다음 요소를 참조하도록 변경하면 되므로 처리 속도가 빠르다.
>링크드 리스트는 이동방향이 단방향이기 때문에 다음 요소에 대한 접근은 쉽지만 이전요소에 대한 접근은 어렵다.      
>이 점을 보완한 것이 더블 링크드 리스트doubly linked list 다.     
더블 링크드 리스는 단순히 링크드 리스트에 참조변수를 하나 더 추가하여    
다음 여소에 대한 참조뿐 아니라 이전 요소에 대한 참조가 가능하도록 했다.
```java
class Node {
	Node next;		//다음 요소의 주소를 저장
	Node previos;		//이전 요소의 주소를 저장
	Object obj;	}	//데이터를 저장
```

>더블 링크드 리스트의 접근성을 보다 향상시킨 것이 더블 써큘러 링크드 리스트doubly circular linked list 이다.     
>단순히 더블 링크드 리스트의 첫 번째 요소와 마지막 요소를 서로 연결시킨 것이다.

LinkedList클래스는 링크드 리스트의 단점인 낮은 접근성을 높이기 위해 더블 링크드 리스트로 구현되어있다.
```java
LinkedList()				//LinkedList 객체 생성
LinkedList(Collection c)		//주어진 컬렉션을 포함하는 LinkedList객체 생성
boolean add(Object o)			//지정된 객체o를 LinkedList의 끝에 추가
void add(int index, Object element)	//지정된 위치index에 객체element 추가
boolean addAll(collection c)		//주어진 컬렉션에 포함된 모든 요소를 LinkedList의 끝에 추가
boolean addAll(int index, Collection c)	//지정된 위치index에 주어진 컬렉션에 포함된 모든 요소를 추가
void clear()				//LinkedList의 모든 요소를 삭제
boolean contains(Object o)		//지정된 객체가 LinkedList에 포함되었는지 알려줌
boolean containsAll(Collection c)	//지정된 컬렉션의 모든 요소가 포함되었는지 알려줌
Object get(int index)			//지정된 위치index의 객체를 반환
int indexOf(Object o)			//지정된 객체가 저장된 위치를 반환
boolean isEmpty()			//LinkedList가 비었는지 알려준다.
Iterator iterator()			//Iterator를 반환한다.
int lastIndexOf(Object o)		//지정된 객체의 위치index를 반환(역순 검색)
ListIterator listIterator()		//ListIterator를 반환
ListIterator listIterator(int index)	//지정된 위치부터 시작하는 ListIterator 반환
Object remove(int index)		//지정된 위치index의 객체를 LinkedList에서 제거
boolean removeAll(Collection c)		//지정된 컬렉션의 요소와 일치하는 요소를 모두 삭제
boolean retainAll(Collection c)		//지정된 컬렉션의 모든 요소가 포함되어 있는지 확인
Object set(int index, Object element)	//지정된 위치index의 객체를 주어진 객체로 변경
int size()				//LinkedList에 저장된 객체의 수를 반환
List subList(int fromIndex, int toIndex)//LinkedList의 일부를 List로 반환
Object[] toArray()			//LinkedList에 저장된 객체를 배열로 반환
Object[] toArray(Object[] a)		//LinkedList에 저장된 객체를 주어진 배열에 저장하여 반환
Object element()			//LinkedList의 첫 번째 요소를 반환
boolean off(Object o)			//지정된 객체o를 LinkedList의 끝에 추가
Object peek()				//LinkedList의 첫 번째 요소를 반환
Object poll()				//LinkedList의 첫 번째 요소를 반환, LinkedList에서는 제거된다
Object remove()				//LinkedList의 첫 번째 요소를 제거
void addFirst(Object o)			//LinkedList의 맨 앞에 객체o를 추가
void addLast(Object o)			//LinkedList의 맨 끝에 객체o를 추가
Iterator descendingIterator()		//역순으로 조회하기 위한 DescendingIterator를 반환
Object getFirst()			//LinkedList의 첫 번째 요소를 반환
Object getLast()			//LinkedList의 마지막 요소를 반환
Object pop()				//removeFirst()와 동일
void push(Object o)			//addFirst()와 동일
Object removeFirst()			//LinkedList의 첫번째 요소 제거
Object removeLast()			//LinkedList의 마지막 요소를 제거
boolean removeFirstOccurrence(Object o)	//LinkedList에서 첫번째로 일치하는 객체를 제거
boolean removeLastOccurrence(Object o)	//LinkedList에서 마지막으로 일치하는 객체를 제거
```
#### ArrayList와 LinkedList 성능비교
```java

		ArrayList al = new ArrayList(20000000);
		LinkedList ll = new LinkedList();
		
		System.out.println("순차적 추가");
		System.out.println(add1(al));		//188
		System.out.println(add1(ll));		//223
		
		System.out.println("중간에 추가");
		System.out.println(add2(al)); 		//2823
		System.out.println(add2(ll)); 		//15
		
		System.out.println("중간에서 삭제");
		System.out.println(remove2(al));	//2185
		System.out.println(remove2(ll));	//141
		
		System.out.println("순차적 삭제");	
		System.out.println(remove1(al));	//11
		System.out.println(remove1(ll));	//38

	}
}
```
>순차적으로 추가/삭제하는 경우에는 ArrayList가 LinkedList보다 빠르다.
만일 ArrayList의 크기가 충분하지 않으면, LinkedList가 더 빠를 수도 있다.    
순차적으로 삭제한다는 것은 마지막부터 삭제한다는 것을 의미하므로    
ArrayList는 마지막 데이터부터 삭제할 경우 각 요소들의 재배치가 필요없어 상당히 빠르다. null로만 바꾸면 된다.

>중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠르다.
이 경우 LinkedList는 각 요소간의 연결만 변경해주면 되므로 처리속도가 빠르다.     
반면 ArrayList는 각 요소들을 재배치하여야하므로 느리다.   
```java
ArrayList al = new ArrayList(1000000);
		LinkedList ll = new LinkedList();

		add(al);
		add(ll);
		
		System.out.println("접근시간테스트");
		System.out.println(access(al));		//1
		System.out.println(access(ll));		//140
```
배열의 경우 만일 인덱스가 n인 요소의 값을 얻어 오고자 한다면 단순히 아래와 같은 수식을 계산하면 된다.
> 인덱스가 n인 데이터의 주소 = 배열의 주소 + n X 데이터 타입의 크기    
LinkedList는 불연속적으로 위치한 각 요소들이 서로 연결된 것이라 처음부터 n번째 데이터까지    
차례대로 따라가야만 원하는 값을 얻을 수 있다.   
그래서 LinkedList는 저장해야하는 데이터의 개수가 많아질수록 데이터를 읽어오는 시간 즉, 접근시간이 길어진다.
>ArrayList 읽기(접근시간): 빠르다   추가/삭제: 느리다.      순차적인 추가삭제는 더 빠르나 비효율적인  메모리 사용               
>LinkedList 읽기(접근시간): 느리다  추가/삭제: 빠르다.      데이터가 많을수록 접근성이 떨어짐

다루고자 하는 데이터의 개수가 변하지 않는 경우라면, ArrayList가 최상의 선택이나    
데이터 개수의 변경이 잦다면 LinkedList를 사용하는 것이 더 나은 선택이다.    
처음 작업하기 전에 데이터를 저장할 때는 ArrayList를 사용한 다음,     
작업할 때는 LinkedList로 데이터를 옮겨서 작업하면 좋은 효율을 얻을 수 있을 것이다.

## Stack과 Queue
>스택은 마지막에 저장한 데이터를 가장 먼저 꺼내는 LIFO구조로 되어있다. 넣은 순서와 꺼낸 순서가 뒤집어진다.      
>큐는 처음에 저장한 데이터를 가장 먼저 꺼내는 FIFO구조로 되어있다. 넣은 순서와 꺼낸 순서가 같다.     
스택 순차적으로 데이터를 추가하고 삭제하는 ArrayList가 적절하고,     
큐는 데이터를 꺼낼 때 항상 첫 번째 저장된 데이터를 삭제하므로 데이터의 추가/삭제가 용이한 LinkedList가 적합하다.

```java
boolean empty()			//Stack이 비어있는지 알려준다.
Object peek()			//Stack의 맨 위에 저장된 객체를 반환. pop과 달리 꺼내지는 않는다.
Object pop()			//Stack의 맨 위에 저장된 객체를 꺼낸다.
Object push(Object item)	//Stack에 객체item를 저장한다.
int search(Object o)		//Stack에서 주어진 객체o를 찾아서 그 위치를 반환, 없으면 -1, 배열과 달리 위치는 1부터 시작
// Stack의 메서드
boolean add(Object o)		//지정된 객체를 Queue에 추가
Object remove()			//Queue에서 객체를 꺼내 반환
Object element()		//삭제없이 요소를 읽어온다. peek과 달리 Queue가 비었을 때 NoSuchElementException 발생
boolean offer(Object o)		//Queue에 객체를 저장
Object poll()			//Queue에서 객체를 꺼내서 반환. 비어있으면 null을 반환
Object peek()			//삭제없이 요소를 읽어 온다. Queue가 비어있으면 null 반환
//Queue의 메서드
```
```java
		Stack st = new Stack();
		Queue q = new LinkedList();
		
		st.push("0");
		st.push("1");
		st.push("2");
		
		q.offer("0");
		q.offer("1");
		q.offer("2");

		System.out.println("stack");
		while(!st.empty()) {
			System.out.println(st.pop());		//210
		}
		
		System.out.println("Queue");
		while(!q.isEmpty()) {
			System.out.println(q.poll());		//012
		}
```
#### Stack 직접 구현하기
Stack은 Vector로부터 상속받아 구현하였다.     
```java
import java.util.*;

public class MyStack extends Vector{
	public Object push(Object item) {
		addElement(item);
		return item;
	}
	public Object pop() {
		Object obj = peek();	//Stack에 저장된 마지막 요소를 읽어온다.
		//만일 Stack이 비어있으면 peek() 메서드가 EmptyStackException을 발생시킨다.
		//마지막 요소를 삭제한다. 배열의 index가 0부터 시작하므로 1을 빼준다.
		removeElementAt(size()-1);
		return obj;
	}
	public Object peek() {
		int len = size();	
		if(len == 0)
			throw new EmptyStackException();
		//마지막 요소를 반환한다. 배열의 index가 0부터 시작하므로 1을 빼준다.
		return elementAt(len-1);
	}
	public boolean empty() {
		return size() == 0;
	}
	public int search(Object o) {
		int i = lastIndexOf(o);		//끝에서부터 객체를 찾는다.
						//반환값은 저장된 위치(배열의 index) 이다.
		if(i>=0)	//객체를 찾은 경우
			return size() - 1;	//Stack은 맨 위에 저장된 객체의 index를 1로 정의하므로
						//계산을 통해서 구한다.
		return -1;	//헤당 객체를 찾지 못하면 -1을 반환한다.
	}
}
```
EmptyStackException은 RuntimeException이므로 따로 예외처리를 해주지 않아도 된다.

#### 스택과 큐의 활용
>스택의 활용 예 - 수식계산, 수식괄호검사, 워드프로세서의 undo/redo, 웹브라우저의 뒤로/앞으로              
>큐의 활용 예 - 최근사용문서, 인쇄작업 대기목록, 버퍼buffer
```java
import java.util.*;

public class StackEx1 {

	public static Stack back = new Stack();
	public static Stack forward = new Stack();

	public static void main(String[] args) {

		goURL("1.네이트");
		goURL("2.야후");
		goURL("3.네이버");
		goURL("4.다음");
		
		printStatus();
		
		goBack();
		System.out.println("= '뒤로' 버튼을 누른 후=");
		printStatus();
		
		goBack();
		System.out.println("= '뒤로' 버튼을 누른 후=");
		printStatus();

		goForward();
		System.out.println("= '앞으로' 버튼을 누른 후=");
		printStatus();

		goURL("chobo.com");
		System.out.println("= 새로운 주소로 이동 후=");
		printStatus();
	}

	public static void printStatus() {
		System.out.println("back:" + back);
		System.out.println("forward:" + forward);
		System.out.println("현재 화면은 '" + back.peek() + "'입니다.");
		System.out.println();
	}

	public static void goURL(String url) {
		back.push(url);
		if (!forward.empty())
			forward.clear();
	}

	public static void goForward() {
		if (!(forward.empty())) {
			back.push(forward.pop());
		}
	}

	public static void goBack() {
		if (!back.empty())
			forward.push(back.pop());
	}
}
```
> 웹 브라우저의 '뒤로', '앞으로' 버튼의 기능을 구현한 것이다.
```java
import java.util.*;

public class ExpValidCheck {

	public static void main(String[] args) {
		if (args.length != 1) {
			System.out.println("Usage: java ExpValidCheck \"EXPRESSION\"");
			System.out.println("Example : java ExpValidCheck\"((2+3)*1)+3\"");
			System.exit(0);
		}

		Stack st = new Stack();
		String expression = args[0];

		System.out.println("expression: " + expression);

		try {
			for (int i = 0; i < expression.length(); i++) {
				char ch = expression.charAt(i);

				if (ch == '(') {
					st.push(ch + "");
				} else if (ch == ')') {
					st.pop();
				}
			}
			if (st.empty()) {
				System.out.println("괄호 일치");
			} else {
				System.out.println("괄호 불일치");
			}
		} catch (EmptyStackException e) {
			System.out.println("괄호 불일치");
		}
	}
}
```
>입력한 수식의 괄호가 올바른지를 체크하는 예제다.    
>'('를 만나면 스택에 넣고 ')'를 만나면 스택에서 '('를 꺼낸다.
```java
import java.util.*;

public class QueueEx1 {

	static Queue q = new LinkedList();
	static final int MAX_SIZE = 5;

	public static void main(String[] args) {
		System.out.println("help를 입력하면ㄴ 도움말을 볼 수 있습니다.");

		while (true) {
			System.out.println(">>");

			try {
				// 화면으로부터 라인 단위로 입력받는다.
				Scanner s = new Scanner(System.in);
				String input = s.nextLine().trim();

				if ("".equals(input))
					continue;

				if (input.equalsIgnoreCase("q")) {
					System.exit(0);
				} else if (input.equalsIgnoreCase("help")) {
					System.out.println("help-도움말을 보여줍니다.");
					System.out.println("q 또는 Q - 프로그램을 종료합니다.");
					System.out.println("history - 최근에 입력한 명령어를 " + MAX_SIZE + "개 보여줍니다.");
				} else if (input.equalsIgnoreCase("history")) {
					int i = 0;
					// 입력받은 명령어를 저장하고,
					save(input);

					// LinkedList의 내용을 보여준다.
					LinkedList tmp = (LinkedList) q;
					ListIterator it = tmp.listIterator();

					while (it.hasNext())
						System.out.println(++i + "." + it.next());
				} else {
					save(input);
					System.out.println(input);
				} // if(input.equalsIgnoreCase("q")) {
			} catch (Exception e) {
				System.out.println("입력 오류");
			}
		}
	}
	public static void save(String input) {
		// queue에 저장한다.
		if (!"".equals(input))
			q.offer(input);

		// queue의 최대크기를 넘으면 제일 처음 입력된 것을 삭제한다.
		if (q.size() > MAX_SIZE)
			q.remove();
	}
}
```
> 유닉스의 history명령어를 Queue를 이용해서 구현한 것이다.

#### PriorityQueue
저장한 순서에 관계없이 우선순위priority가 높은 것부터 꺼내게 된다는 특징이 있다.    
그리고 null은 저장할 수 없고, 저장한다면 NullPointerException이 발생한다.     
PriorityQueue는 저장공간으로 배열을 사용하며, 각 요소를 '힙'이라는 자료구조의 형태로 저장한다.    
힙은 이진 트리의 한 종류로 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다는 특징이 있다.(자료구조의 힙과는 다른 것이다.)

```java
import java.util.*;

public class PriorityQueueEx {

	public static void main(String[] args) {
		Queue pq = new PriorityQueue();
		pq.offer(3);	//pq.offer(new Integer(3));  오토박싱
		pq.offer(1);
		pq.offer(5);
		pq.offer(2);
		pq.offer(4);
		
		System.out.println(pq); 			//[1,2,3,4,5]
		System.out.println(pq.poll());			//1
		
		Object obj = null;
		
		//PriorityQueue에 저장된 요소를 하나씩 꺼낸다.
		while((obj = pq.poll()) != null)	//2 3 4 5 (1은 이미 꺼냄)
			System.out.println(obj);
	}

}
```
저장순서가 3,1,5,2,4 인데도 출력결과는 1,2,3,4,5 이다.    
우선순위는 숫자가 작을수록 높은 것이므로 1이 가장 먼저 출력된 것이다.    
참조변수 pq를 출력하면 PriorityQueue가 내부적으로 가지고 있는 배열의 내용이 출력되는데,    
저장한 순서와 다르게 저장되었다. 이는 힙이라는 주료구조의 형태로 저장된 것이라 그렇다.

#### Deque(Double - Ended Queue)
Queue의 변형으로, 한 쪽 끝으로만 추가/삭제할 수 있는 Queue와 달리, Deque은 양쪽 끝에 추가/삭제가 가능하다.     

>Deque: offerLast()(끝에 저장), pollLast(끝에서 삭제), pollFirst(앞에서 삭제), offerFirst(앞에 저장)     
>Stack: push()(끝에 저장),      pop()(끝에서 추출),  peek()(끝에서 반환)     
>Queue: offer()(끝에 저장), poll()(앞에서 추출),  peek()(앞에서 반환)     

## Iterator, ListIterator, Enumeration
Iterator, ListIterator, Enumeration 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스다.     
Enumeration은 Iterator의 구버젼이며, ListIterator는 Iterator의 기능을 향상시킨 것이다.

#### Iterator 
컬렉션 프레임웍에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하였다.       
컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator인터페이스를 정의하고,       
Collection인터페이스에는 Iterator(Iterator를 구현한 클래스의 인스턴스)를 반환하는 iterator()를 정의하고 있다.     

> 컬렉션 클래스에 대해 iterator()를 호출하여 Iterator를 얻은 다음       
> 반복문, 주로 while문을 사용해서 컬렉션 클래스의 요소들을 읽어 올 수 있다.     

boolean hasNext(): 읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환한다.     
Object next(): 다음 요소를 읽어온다. next()를 호출하기 전에 hasNext()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다.     
void remove(): next()로 읽어 온 요소를 삭제한다. next()를 호출한 다음에 remove()를 호출해야 한다.(선택적 기능)

```java
Collection c = new ArrayList();
Iterator it = c.iterator();

while(it.hasNext()){
	System.out.println(it.next());	}
```
>ArrayList에 저장된 요소들을 출력하기 위한 코드는 이렇게 작성할 수 있다.

ArrayList대신 Collection인터페이스를 구현한 다른 컬렉션 클래스에 대해서도 이와 동일한 코드를 사용할 수 있다.    
> Iterator를 이용해서 컬렉션의 요소를 읽어오는 방법을 표준화했기 때문에 이처럼 코드의 재사용성을 높이는 것이 가능하다.     
> 이처럼 공통 인터페이스를 정의해서 표준을 정의하고 구현하여 표준을 따르도록 함으로써       
> 코드의 일관성을 유지하여 재사용성을 극대화하는 것이 객체지향 프로그래밍의 중요한 목적 중의 하나이다.

Map인터페이스를 구현한ㄹ  컬렉션 클래스는 키key와 값value을 쌍pair으로 저장하고 있기 때문에 iterator()를 직접 호출할 수 없고,     
그 대신 keySet()이나 entrySet()과 같은 메서드를 통해서 키와 값을 각각 따로 Set의 형태로 얻어 온 후에 다시 iterator()를 호출해야 Iterator를 얻을 수 있다.
```java
Map map = new HashMap();
	...
Iterator it = map.entrySet().iterator();	//아래 두 문장을 합친 것이다.
//Set eSet = map.entrySet();
//Iterator it = eSet.iterator();
```
```java
import java.util.*;

public class IteratorEx1 {

	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add("1");
		list.add("2");
		list.add("3");
		list.add("4");
		list.add("5");

		Iterator it = list.iterator();

		while (it.hasNext()) {
			Object obj = it.next();
			System.out.println(obj);	//1 2 3 4 
		}
	}
}
```
>List클래스들은 저장순서를 유지하기 때문에 Iterator를 이용해서 읽어 온 결과 역시     
>저장 순서와 동일하지만 Set클래스들은 각 요소간의 순서가 유지 되지 않기 때문에     
>Iterator를 이용해서 저장된 요소들을 읽어 와도 처음에 저장된 순서와 같지 않다.

#### ListIterator와 Enumeration
Enumeration은 Itrator의 구버젼이므로 가능하면 Iterator를 사용하는 것이 좋다.     
ListIterator는 Iterator를 상속받아서 기능을 추가한 것으로, 컬렉션의 요소에 접근할 때     
Iterator는 단방향으로만ㄴ 이동할 수 있는데 반해 ListIterator는 양방향으로의 이동이 가능하다.     
다만 ArrayList나 LinkedList와 같이 List인터페이스를 구현한 컬렉션에서만 사용할 수 있따.

* Enumeration인터페이스의 메서드
```java
boolean hasMoreElements()	//읽어 올 요소가 남아있는지 확인한다. Iterator의 hasNext()와 같다.
Object nextElement()		//다음 요소를 읽어온다. nextElement()를 호출하기 전에 hasMoreElements()를 호출해서 
				//읽어올 요소가 남았는지 확인하는 것이 안전하다. Iterator의 next()와 같다.
```
>Enumeration과 Iterator는 메서드이름만 다를 뿐 기능은 같다.

* ListIterator의 메서드
```java
void add(Object o)	//컬렉션에 새로운 객체o를 추가한다(선택적 기능)
boolean hasNext()	//읽어 올 다음 요소가 남았는 지 확인한다.
boolean hasPrevious()	//읽어 올 이전 요소가 남았는 지 확인한다.
Object next()		//다음 요소를 읽어온다. 호출 전 hasNext()를 호출해서 확인해보는 것이 안전하다.
Object Previous()	//이전 요소를 읽어온다. 호출 전 hasPrevious()를 호출해서 확인해보는 것이 안전하다.
int nextIndex()		//다음 요소의 index를 반환한다
int previousIndex()	//이전 요소의 index를 반환한다.
void remove()			//선택적 기능
//next() 또는 previous()로 읽어 온 요소를 삭제한다. 반드시 next()나 previous()를 먼저 호출하고 이 메서드를 호출해야한다.
void set(Object o)		//선택적 기능
//next()나 previous()로 읽어 온 요소를 지정된 객체o로 변경한다. 
//반드시 next()나 previous()를 먼저 호출하고 이 메서드를 호출해야한다.
```
>선택적 기능이라고 표시된 것들은 반드시 구현하지 않아도 된다.    
>예를 들어, Iterator인터페이스를 구현하는 클래스에서 remove()는 선택적인 기능이므로 구현하지 않아도 된다.     
>그렇다하더라도 인터페이스로부터 상속받은 메서드는 추상메서드라 메서드의 몸통을 반드시 만들어 주어야 한다
```java
public void remove(){
	throw new UmsupportedOperationException();	}
```
> 단순히 public void remove(){}; 와 같이 구현하는 것보다 예외를 던져 구현되지 않았음을 호출하는 쪽에 알리는 것이 좋다.     
> Iterator의 remove()는 단독으로 쓰일 수 없고, next()와 같이 써여한다.   
> 특정위치의 요소를 삭제하는 것이 아니라 읽어 온 것을 삭제하기 때문이다. next()없이 호출하면 IllegalStateException이 발생한다.     
```java
import java.util.*;

public class ListIteratorEx1 {

	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		
		list.add("1");
		list.add("2");
		list.add("3");
		list.add("4");
		list.add("5");
		
		ListIterator it = list.listIterator();
		
		while(it.hasNext()) {
			System.out.print(it.next());		//12345
		}
		System.out.println();
		while(it.hasPrevious()) {
			System.out.print(it.previous());	//54321
		}
	}
}
```
> Iterator는 단방향으로만 이동하기 때문에 컬렉션의 마지막 요소에 다다르면 더 이상 사용할 수 없지만,    
> ListIterator는 양방향으로 이동하기 때문에 각 요소간의 이동이 자유롭다.
```java
import java.util.*;

public class IteratorEx2 {

	public static void main(String[] args) {
		ArrayList original = new ArrayList(10);
		ArrayList copy1 = new ArrayList(10);
		ArrayList copy2 = new ArrayList(10);

		for(int i = 0 ; i<10; i++)
			original.add(i+"");
		
		Iterator it = original.iterator();
		
		while(it.hasNext())
			copy1.add(it.next());
		System.out.println(original);		//[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
		System.out.println(copy1);		//[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
		
		it = original.iterator();		//Iterator는 재사용이 안되므로, 다시 얻어와야 한다.
		
		while(it.hasNext()) {
			copy2.add(it.next());
			it.remove();
		}
		System.out.println(original);		//[]
		System.out.println(copy2);			//[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	}
}
```
```java
package practice;
import java.util.*;

public class MyVector2 extends MyVector implements Iterator{
		int cursor = 0;
		int lastRet = -1;
		
		public MyVector2(int capacity) {
			super(capacity);
		}
		public MyVector2() {
			super(10);
		}
		public String toString() {
			String tmp = "";
			Iterator it = iterator();
			
			for(int i = 0; it.hasNext(); i++) {
				if(i!=0) tmp+=", ";
				tmp += it.next();		//tmp+=next().toString();
			}
			return "[" + tmp + "]";
		}
		public Iterator iterator()	{
			cursor = 0;			//cursor와 lastRet을 초기화 한다.
			lastRet = -1;
			return this;
		}
		public boolean hasNext() {
			return cursor != size();
		}
		public Object next() {
			Object next = get(cursor);
			lastRet = cursor++;
			return next;
		}
		public void remove() {
			//더 이상 삭제할 것이 없으면 IllegalStateException을 발생시킨다.
			if(lastRet == -1) {
				throw new IllegalStateException();
			} else {
				remove(lastRet);
				cursor--;				//삭제 후에 cursor의 위치를 감소시킨다.
				lastRet = -1;				//lastRet의 값을 초기화 한다.
			}
		}
}
```
cursor는 앞으로 읽어 올 요소의 위치를 저장하는데 사용되고, lastRet은 마지막으로 읽어 온 요소의 위치index를 저장하는데 사용된다.     
그래서 lastRet은 항상 cursor보다 1 작은 값이 저장되고 remove()를 호출하면 이미 next()를 통해서 읽은 위치의 요소,      
즉 lastRet에 저장된 값의 위치에 있는 요소를 삭제하고 lastRet의 값을 -1로 초기화 한다.      
만일 next()로 호출하지 않고 remove()를 호출하면 lastRet의 값은 -1이 되어 IllegalStateException이 발생한다.    
remove()는 next()로 읽어 온 객체를 삭제하는 것이기 때문에 remove()를 호출하기 전에는 반드시 next()가 호출된 상태여야 한다.
```java
public class MyVector2Test {

	public static void main(String[] args) {
		MyVector2 v = new MyVector2();
		v.add("0");
		v.add("1");
		v.add("2");
		v.add("3");
		v.add("4");

		System.out.println(v);
		Iterator it = v.iterator();	//[0,1,2,3,4]
		it.next();
		it.remove();
		it.next();
		it.remove();
		System.out.println(v);		//[2,3,4]
	}
}
```
## Arrays
Arrays클래스에는 배열을 다루는데 유용한 메서드가 정의되어 있다.   
같은 기능의 메서드가 배열의 타입만 다르게 오버로딩되어 있어서 많아 보이지만, 실제로는 그리 많지 않다.     

#### 배열의 복사 copyOf(), copyOfRange()
copyOf()는 배열 전체를, copyOfRange()는 배열의 일부를 복사해서 새로운 배열로 반환한다.
```java
int[] arr = {0,1,2,3,4};
int[] arr2 = Arrays.copyOf(arr, arr.length);	//01234
int[] arr3 = Arrays.copyOf(arr, 3);		//012
int[] arr4 = Arrays.copyOf(arr, 7);		//0123400
int[] arr5 = Arrays.copyOfRange(arr, 2, 4)	//23
int[] arr6 = Arrays.copyOfRange(arr, 0, 7)	//0123400
```

#### 배열 채우기 - fill(), setAll()
fill()은 배열의 모든 요소를 지정된 값으로 채운다.     
setAll()은 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다.     
이 메서드를 호출할 떄는 함수형 인터페이스를 구현한 객체를 매개변수로 지정하던가 람다식을 지정해야한다.
```java
int[] arr = new int[5];
Arrays.fill(arr, 9);		//arr = 9,9,9,9,9
Arrays.setAll(arr, () -> (int)(Math.random()*5)+1);	//arr = 1,5,2,1,1
```
setAll()메서드는 람다식이 반환한 임의의 정수로 배열을 채운다.

#### 배열의 정렬과 검색 sort(), binarySearch()
sort()는 배열을 정렬할 때, binartSearch()는 배열에 저장된 요소를 검색할 때 사용한다.     
binarySearch()는 배열에서 지정된 값이 저장된 위치index를 찾아서 반환하는데, 배열이 정렬된 상태여야 올바른 결과를 얻는다.    
그리고 만일 검색한 값과 일치하는 요소들이 여러 개 있다면, 이 중에서 어떤 것의 위치가 반환될지는 알 수 없다.
```java
int[] arr={3,2,0,1,4};
int idx = Arrays.binarySearch(arr, 2);		//idx = -5(잘못된 결과)

Arrays.sort(arr);
System.out.println(Arrays.toString(arr));	//[0,1,2,3,4]
int idx = Arrays.binarySearch(arr, 2);		//idx = 2
```
배열의 첫 번째 요소부터 순서대로 하나씩 검색하는 '순차 검색linear search'는    
배열이 정렬되어 있을 필요는 없지만 배열의 요소를 하나씩 비교하기 때문에 시간이 많이 걸린다.     
반면 이진 검색binary search은 배열의 검색할 범위를 반복적으로 절반씩 줄여가면서 검색하기 때문에 검색속도가 상당히 빠르다.      
단, 배열이 정렬되어 있는 경우에만 사용할 수 있다는 단점이 있다.

#### 배열의 비교와 출력 equals(), toString()
toString()은 배열의 모든 요소를 문자열로 편하게 출력할 수 있다.    
toString()은 일차원 배열에만 사용할 수 있으므로, 다차원 배열에는 deepToString()을 사용해야 한다.     
deepToString()은 배열의 모든 요소를 재귀적으로 접근해서 문자열을 구성하므로 2차원뿐 아니라 3차원 이상의 배열에서도 동작한다.
```java
int[] arr = {0,1,2,3,4};
int[][] arr2D = {{11,12}, {21,22}};

System.out.println(Arrays.toString(arr));	//[0,1,2,3,4]
System.out.println(Arrays.deepToString(arr2D));	//[[11, 12], [21, 22]]
```
equals()도 일차원 배열에서만 사용이 가능하고 다차원 배열의 비교에서는 deepToEquals()를 사용해야 한다.
```java
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Arrays.equals(str2D, str2D2));	//false
System.out.println(Arrays.deepToEquals(str2D, str2D2));	//true
```
equals()는 문자열을 비교하는 것이 아니라 배열에 저장된 배열의 주소로 비교하므로    
서로 다른 배열은 항상 주소가 다르므로 false가 나온다.

#### 배열을 List로 반환 asList(Object...a)
asList()는 배열을 List에 담아서 반환한다. 매개변수의 타입이 가변인수라서 배열 생성없이 저장할 요소들만 나열하는 것도 가능하다.
```java
List list = Arrays.asList(new Integer[]{1,2,3,4,5});	//list = {1,2,3,4,5}
List list = Arrays.asList(1,2,3,4,5);			//list = {1,2,3,4,5}
list.add(6);	//UnsupportedOperationException 예외 발생
```
> asList()가 반환한 List의 크기는 변경할 수 없으므로 주의하여야 한다.     
> 즉, 추가 또는 삭제가 불가능하고 저장된 내용을 변경하는 것은 가능하다.     
> 크기를 변경할 수 있는 List가 필요하다면 다음과 같이 하면 된다.
```java
List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```

#### paralleXXX(), spliterator(), stream()
이 외에도 parallel로 시작하는 이름의 메서드들이 있는데, 보다 빠른 결과를 얻기 위해 여러 쓰레드가 작업을 나누어 처리하도록 한다.    
spliterator()는 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 Spliterator를 반환하며,       
stream()은 컬렉션을 스트림으로 변환한다.
```java
public class ArraysEx {

	public static void main(String[] args) {
		int[] arr = {0,1,2,3,4};
		int[][] arr2D = {{11,12,13}, {21,22,23}};
		
		System.out.println(Arrays.toString(arr));				//[0, 1, 2, 3, 4]
		System.out.println(Arrays.deepToString(arr2D));			//[[11, 12, 13], [21, 22, 23]]
		
		int[] arr2 = Arrays.copyOf(arr, arr.length);	
		int[] arr3 = Arrays.copyOf(arr, 3);					
		int[] arr4 = Arrays.copyOf(arr, 7);						
		int[] arr5 = Arrays.copyOfRange(arr, 2, 4);
		int[] arr6 = Arrays.copyOfRange(arr, 0, 7);
		System.out.println(Arrays.toString(arr2));				//[0, 1, 2, 3, 4]
		System.out.println(Arrays.toString(arr3));				//[0, 1, 2]
		System.out.println(Arrays.toString(arr4));				//[0, 1, 2, 3, 4, 0, 0]
		System.out.println(Arrays.toString(arr5));				//[2, 3]
		System.out.println(Arrays.toString(arr6));				//[0, 1, 2, 3, 4, 0, 0]
		
		int[] arr7 = new int[5];
		Arrays.fill(arr7, 9);
		System.out.println(Arrays.toString(arr7));				//[9,9,9,9,9]
		
		Arrays.setAll(arr7, i -> (int)(Math.random()*6)+1);
		System.out.println(Arrays.toString(arr7));					//[1, 1, 2, 1, 1]
		
		for(int i : arr7) {
			char[] graph = new char[i];
			Arrays.fill(graph, '*');
			System.out.print(new String(graph)+i);					//*1 *1 **2 *1 *1
		}
		
		String[][] str2D = new String[][] {{"aaa", "bbb"}, {"AAA", "BBB"}};
		String[][] str2D2 = new String[][] {{"aaa", "bbb"}, {"AAA", "BBB"}};
		
		System.out.println(Arrays.equals(str2D, str2D2));			//false
		System.out.println(Arrays.deepEquals(str2D, str2D2));		//true
		
		char[] chArr = {'A', 'D', 'C', 'B', 'E'};
		
		System.out.println(Arrays.toString(chArr));					//[A,D,C,B,E]
		System.out.println(Arrays.binarySearch(chArr, 'B'));		//-2
		Arrays.sort(chArr);
		System.out.println(Arrays.toString(chArr));					//[A,B,C,D,E]
		System.out.println(Arrays.binarySearch(chArr, 'B'));		//1
	}
}
```
## Comparator와 Comparable
Arrays.sort()는 Character클래스의 Comparable의 구현에 의해 정렬된 것이다.    
Comparator와 Comparable은 모두 인터페이스로 컬렉션을 정렬하는데 필요한 메서드를 정의하고 있다.
```java
public interface Comparator{
	int compare (Object o1, Object o2);
	boolean equals(Object obj);	}
	
public interface Comparable{
	public int compareTo(Object o);	}
```
> Comparable은 java.lang패키지에 있고, Comparator는 java.util패키지에 있다.

compare()과 compareTo()는 선언 형태와 이름이 약간 다를 뿐 두 객체를 비교한다는 같은 기능을 목적으로 고안된 것이다.     
둘 다 객체를 비교해서 음수, 0, 양수를 반환하도록 구현해야한다.     
```java
public final class Integer extends Number implements Comparable {
...
  public int compareTo(Object o)   {
  	return compareTo((Integer)o);
  }
  public int compareTo(Integer anotherInteger)  {
  	int thisVal = this.value;
	int anotherVal = anotherInteger.value;
	//비교하는 값이 크면 -1, 같으면 0, 작으면 1을 반환한다.
	return (thisVal<anoterVal? -1 : (thisVal == anotherVal ? 0 : 1);
  }	}
  ```
위의 코드는 Integer클래스의 일부인데 Comparable의 compareTo(Object o)를 구현한 것을 볼 수 있다.     
Comparable을 구현한 클래스들이 기본적으로 오름차순으로 정렬되어 있지만,    
내림차순으로 정렬한다던가 아니면 다른 기준에 의해 정렬되도록 하고 싶을 때 Comparator를 구현해서 정렬기준을 제공할 수 있다.
>Comparable 기본 정렬기준을 구현하는데 사용.     
>Comparator 기본 정렬기준 외에 다른 기준으로 정렬하고자할 때 사용.

```java
public class ComparatorEx {
	public static void main(String[] args) {
		String[] strArr = {"cat", "Dog", "lion", "tiger"};
		
		Arrays.sort(strArr);	//String의 Comparable구현에 의한 정렬
		System.out.println(Arrays.toString(strArr));		//[Dog, cat, lion, tiger]
		
		Arrays.sort(strArr, String.CASE_INSENSITIVE_ORDER);		//대소문자 구분안함
		System.out.println(Arrays.toString(strArr));		//[cat, Dog, lion, tiger]
		
		Arrays.sort(strArr, new Descending());
		System.out.println(Arrays.toString(strArr));		//[tiger, lion, cat, Dog]
	}
}
class Descending implements Comparator{
	public int compare(Object o1, Object o2) {
		if(o1 instanceof Comparable && o2 instanceof Comparable) {
			Comparable c1 = (Comparable) o1;
			Comparable c2 = (Comparable) o2;
			return c1.compareTo(c2) * -1;		//-1을 곱해서 기본정렬방식의 역으로 변경한다.
								//또는 c2.compareTo(c1)와 같이 비교 객체의 순서를 바꿔도 된다.
		}
		return -1;
	}
}
```
> Arrays.sort()는 배열을 정렬할 때, Comparator를 지정해주지 않으면 저장하는 객체(주로 Comparable을 구현한 개체)에 구현된 내용에 따라 정렬된다.

```java
static void sort(Object[] a)	//객체 배열에 저장된 객체가 구현한 Comparable에 의한 정렬
static void sort(Object[] a, Comparator c)	//지정한 Comparator에 의한 정렬

public static final Comparator CASE_INSENSITIVE_ORDER
```
String의 Comparable구현은 문자열이 사전 순으로 정렬되도록 작성되어 있다.      
대소문자를 구분하지 않고 비교하는 Comparator를 상수의 형태로 제공한다.     
이 Comparator를 이용하면, 문자열을 대소문자 구분없이 정렬할 수 있다.     

## HashSet
 set인터페이스를 구현한 컬렉션이며, set인터페이스의 특징대로 HashSet은 중복된 요소를 저장하지 않는다.     
 HashSet에 새로운 요소를 추가할 때 add메서드나 addAll메서드를 사용하는데     
 만일 HashSet에 이미 저장되어 있는 요소와 중복된 요소를 추가하고자 한다면 이 메서드들은 false를 반환한다.    
 이러한 HashSet의 특징을 이용하면 컬렉션 내의 중복 요소들을 쉽게 제거할 수 있따.     
 ArrayList와 같이 List인터페이스를 구현한 컬렉션과 달리 HashSet은 저장순서를 유지하지 않으므로 저장 순서를 유지하고자 한다면 LinkedHashSet을 이용한다.
 
 ```java
 HasdhSet()				//HashSet객체를 생성한다.
 HashSet(Collection c)			//주어진 컬렉션을 포함하는 HashSet객체를 생성한다.
 HashSet(int initialCapacity)		//주어진 값을 초기용량으로하는 HashSet객체를 생성한다.
 HashSet(int initialCapacity, float loadFactor)		/초기 용량과 load factor를 지정하는 생성자.
 boolean add(Object o)			//새로운 객체를 저장한다.
 boolean addAll(Collection c)		//주어진 컬렉션에 저장된 모든 객체들을 추가한다(합집합)
 void clear()				//저장된 모든 객체를 삭제한다.
 Object clone()				//HashSet을 복제해서 반환한다.(얕은 복사)
 boolean containsAll(Collection c)	//주어진 컬렉션에 저장된 모든 객체들을 포함하고 있는지 알려준다.
 boolean isEmpty()			//HashSet이 비어있는지 알려준다.
 Iterator iterator()			//Iterator를 반환한다.
 boolean remove(Object o)		//지정된 객체를 HashSet에서 삭제한다.
 boolean removeAll(Collection c)	//주어진 컬렉션에 저장된 모든 객체와 동일한 것들을 HashSet에서 삭제한다.
 boolean retainAll(Collection c)	//주어진 컬렉션에 저장된 객체와 동일한 것만 남기고 삭제한다.(교집합)
 int size()				//저장된 객체의 개수를 반환한다.
 Object[] toArray()			//저장된 객체들을 객체배열의 형태로 반환한다.
 Object[] toArray(Object[] a)		//저장된 객체들을 주어진 객체배열에 담는다.
 ```
 >load factor는 컬렉션 클래스에 저장공간이 가득 차기 전에 미리 용량을 확보하기 위한 것으로 0.8로 지정하면 80%찻을 때 용량을 두배로 늘린다.      
```java
import java.util.*;

public class HashSetEx1 {

	public static void main(String[] args) {

		Object[] arr = {"1", new Integer(1), "2", "2", "3", "3", "4","4","4"};
		Set set = new HashSet();
		
		for(int i = 0; i<arr.length; i++) {
			set.add(arr[i]);				//HastSet에 arr 요소들을 저장한다.
		}
		System.out.println(set);			//[1,1,2,3,4}
	}
}
```
> set을 구현한 컬렉션 클래스는 List를 구현한 컬렉션 클래스와 달리 순서를 유지하지 않기 때문에 저장한 순서와 다를 수 있다.      
> 중복을 제거하는 동시에 저장한 순서를 유지하고자 한다면 HashSet대신 LinkedHashSet을 사용해야한다.
```java
public class HashSetLotto {

	public static void main(String[] args) {
		Set set = new HashSet();
		
		for(int i = 0 ; set.size()<6 ; i++) {
			int num = (int)(Math.random()*45)+1;
			set.add(new Integer(num));
		}
		List list = new LinkedList(set);		//LinkedList(Colletion c)
		Collections.sort(list);					//Collections.sort(List list)
		System.out.println(list);				//[8, 17, 19, 20, 21, 26]
	}
}
```
번호를 크기순으로 정렬하기 위해서 COllections클래스의 sort(List list)를 사용했다.     
이 메서드는 인자로 List인터페이스 타입을 필요로 하기 때문에       
LinkedList클래스의 생성자 LinkedList(Collection c)를 이용해서 HashSet에 저장된 객체들을 LinkedList에 담아서 처리했다.      
```java
public class Bingo {

	public static void main(String[] args) {
		Set set = new HashSet();
		//Set set = new LinkedHashSet();
		
		int[][] board = new int[5][5];
		
		for(int i = 0; set.size()<25; i++) {
			set.add((int)(Math.random()*50)+1+ "");
		}
		
		Iterator it = set.iterator();
		
		for(int i =0; i<board.length; i++) {
			for(int j = 0; j<board[i].length; j++) {
				board[i][j] = Integer.parseInt((String)it.next());
				System.out.print((board[i][j]<10? "  " : " ")+ board[i][j]);
			}
			System.out.println();
		}
	}
}
```
next()는 반환값이 Object타입이므로 형변환해서 원래의 타입으로 되돌려 놓아야 한다.     
```java
public class HashSetEx3 {

	public static void main(String[] args) {
		
		HashSet set = new HashSet();
		
		set.add("abc");
		set.add("abc");
		set.add(new Person("David",10));
		set.add(new Person("David",10));
		
		System.out.println(set);			//[David:10, abc, David:10]
	}
}
class Person{
	String name;
	int age;
	
	Person(String name, int age){
		this.name = name;
		this.age = age;
	}
	public String toString() {
		return name + ":" + age;
	}
}
```
실행 결과를 보면 두 인스턴스의 name과 age가 같음에도 서로 다른 것으로 인식하여 두번 출력되었다.     
```java
public class HashSetEx4 {

	public static void main(String[] args) {
		HashSet set = new HashSet();
		
		set.add(new String("abc"));
		set.add(new String("abc"));
		set.add(new Person2("David", 10));
		set.add(new Person2("David", 10));
		
		System.out.println(set);			//[abc, David:10]
	}
}
class Person2{
	String name;
	int age;
	
	Person2(String name, int age){
		this.name = name;
		this.age = age;
	}
	public boolean equals(Object obj) {
		if(obj instanceof Person2) {
			Person2 tmp = (Person2)obj;
			return name.equals(tmp.name) && age == tmp.age;
		}
		return false;
	}
	public int hashCode() {
		return (name + age).hashCode();
		//return Object.hash(name, age);	//int hash(Object...values) 이걸 쓰는 걸 권장 
	}
	public String toString(){
		return name +":"+age;
	}
}
```
HashSet의 add메서드는 새로운 요소를 추가하기 전에 기존에 저장된 요소와 같은 것인지 판별하기 위해      
추가하려는 요소의 equals()와 hashCode()를 호출하기 때문에 equals()와 hashCode()를 목적에 맞게 오버라이딩해야 한다.      
* 오버라이딩을 통해 작성된 hashCode()는 다음의 세 조건을 만족시켜야 한다.       
> 1. 실행 중인 애플리케이션 내의 동일한 객체에 대하여 여려번 hashCode()를 호출해도 동일한 int값을 반환해야 한다.     
> 실행시마다 동일한 int값을 반환할 필요는 없다.(equals메서드의 구현에 사용된 멤버변수의 값이 바뀌지 않는다고 가정한다)      
```java
Person2 p = new Person2("David", 10);
Person2 p1 = new Person2("David", 10);
int hashCode1 = p.hashCode();
int hashCode2 = p.hashCode();
p.age = 20;
int hashCode3 = p.hashCode();

boolean b  = p.equals(p2);
int hashCode3 = p.hashCode();
int hashCode4 = p.hashCode();
```
hashCode1과 hashCode2의 값은 항상 일치해야하지만 두 값이 매번 실행때마다 반드시 같은 값일 필요는 없다.     
> String클래스는 문자열의 내용으로 해시코드를 만들어 내기 때문에 내용이 같은 문자열에 대한 hashCode() 호출은 항상 동일한 해시코드를 반환한다.     

>2. equals메서드를 이용한 비교에 의해서 true를 얻은 두 객체에 대해 각각 hashCode()를 호출해서 얻은 결과는 반드시 같아야 한다.
>3. equals메서드를 호출했을 때 false를 반환하는 두 객체는 hashCode() 호출에 대해 같은 int값을 반환하는 경우가 있어도 괜찮지만,     
>해싱hashing을 사용하는 컬렉션의 성능을 향상시키기 위해서는 다른 int값을 반환하는 것이 좋다.
서로 다른 객체에 대해서 해드코드값(hashCode()를 호출한 결과)이 중복되는 경우가 많아질수록      
해싱을 사용하는 Hashtable, HashMap과 같은 컬렉션의 검색속도가 떨어진다.
```java
public class HashSetEx5 {

	public static void main(String[] args) {
		HashSet setA = new HashSet();
		HashSet setB = new HashSet();
		HashSet setHab = new HashSet();
		HashSet setKyo = new HashSet();
		HashSet setCha = new HashSet();
		
		setA.add("1"); setA.add("2"); setA.add("3");
		setA.add("4"); setA.add("5");
		System.out.println(setA);			//[1,2,3,,4,5]
		
		setB.add("4"); setB.add("5"); setB.add("6");
		setB.add("7"); setB.add("8");
		System.out.println(setB);			//[4,5,6,7,8]
		
		Iterator it = setB.iterator();
		while(it.hasNext()) {
			Object tmp = it.next();
			if(setA.contains(tmp))
				setKyo.add(tmp);
		}
		it = setA.iterator();
		while(it.hasNext()) {
			Object tmp = it.next();
			if(!setB.contains(tmp))
				setCha.add(tmp);
		}
		it = setA.iterator();
		while(it.hasNext())
			setHab.add(it.next());
		it = setB.iterator();
		while(it.hasNext())
			setHab.add(it.next());
		
		System.out.println(setKyo);		//[4,5]
		System.out.println(setHab);		//[1,2,3,4,5,6,7,8]
		System.out.println(setCha);		//[1,2,3]
	}
}
```

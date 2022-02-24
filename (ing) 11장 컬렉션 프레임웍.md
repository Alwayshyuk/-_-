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
		
		System.out.println(pq); 				//[1,2,3,4,5]
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

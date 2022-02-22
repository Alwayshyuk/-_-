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

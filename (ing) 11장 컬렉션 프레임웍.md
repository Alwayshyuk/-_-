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

자바의 자료구조 중에 HashMap이라는 자료구조가 있다.
먼저 HashMap에 있는 `Map`에 먼저 알아보자.


# `Map<K, V>`
Map은 `Key`와 `Value`를 가진 집합이면 중복을 허용하지 않는다.
여기서 중복은 `Key`에만 해당 되지 않는다. `Value`는 중복이 가능하다. 
Map의 구현하고 있는 자식들은 `TreeMap`, `HashMap`, `HashTable`과 같은 다양한 자료구조가 있다.

그러면 `Map`을 사용해보자!
```java
public static void main(String[] args) {
	Map<String, Integer> m = new Map<>(); // error
}
```
이렇게 쓰면 될까?

물론 `에러`가 난다.  `Map클래스` 안으로 들어가보자. 
```java
public interface Map<K, V> {
	int size();
	boolean isEmpty();

	// ... 나머지 코드 ...
}
```
`Map`은 인터페이스(interface)다.

그러면 어떻게 Map을 쓰나요?
물론 우리가 상속을 받아서 `오버라이딩` `public class A implements Map<K, V> {...}`을 해서 직접 써도 되지만 자바에서 이미 구현 해둔 자료구조를 쓰면 된다!  앞에 설명한 `TreeMap`, `HashMap`, `HashTable`을 쓰면 된다. 그리고 이글은 `HashMap`을 다루는 글이기 때문에 `HashMap`에 대해 설명을 해보겠다.

### HashMap

내부구조
```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
    // .... 나머지 코드 ....
}
```
다음과 같이 `HashMap`은 Map을 상속 받아 구현한다.

사용법 예제
```java
Map<String, Object> map1 = new HashMap<>();

HashMap<String, Object> map2 = new HashMap<>();
```
두 개의 코드의 차이점은 뭘까?
선언을 인터페이스로 한것과 인스턴스로 생성한것의 차이다.

하지만 Map 인터페이스를 사용하여 코드를 작성하면 유연성이 높아진다! 나중에 가면 TreeMap, HashTable 같은 Map을 구현한 인스턴스들로 변경이 쉽기 때문이다.

```java
Map<String, Object> map1 = new HashMap<>();
Map<String, Object> map1 = new TreeMap<>();
Map<String, Object> map1 = new HashTable<>();

HashMap<String, Object> map2 = new HashMap<>();
```

아무튼 HashMap은 키-값 쌍을 저장하는 수 있는 자료구조이다. 
그리고 데이터를 빠르게 검색할 수 있다는 장점이 있다.

## 얼마나 빠르나요?
`HashMap`은 대부분의 경우 `O(1)`의 시간 복잡도로 값을 검색한다.


- - -
참고 </br>
[Map과 HashMap 차이](https://wooktae.tistory.com/38)</br>
[Map과 Hash Map 의 차이는??](https://velog.io/@nike7on/Map%EA%B3%BC-Hash-Map-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%8A%94)</br>

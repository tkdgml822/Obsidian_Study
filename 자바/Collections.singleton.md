# 개요
스프링부트를 하면서 Collections.singleton에 대한 코드를 봤다. 

## 싱글톤
싱글톤이라는 개념은 알고 있었다. 

싱글톤: 특정 클래스들의 인스턴스를 1개만 생성되는 것을 보장하는 디자인 패턴이다. **메모리 낭비**를 방지
참고! : `스프링은 기본적으로 싱글톤을 보장해준다.`

## Collections.singleton
코드 내부로 들어가 보니
```java
public static <T> Set<T> singleton(T o) {  
    return new SingletonSet<>(o);  
}
```
Set을 반환 하는거였다. 그래서 의문점이 생겼다. 왜 Set이지?

## Set
그 이유는 간단한 이유 였다.
이유를 알려면 일단 Set의 특성을 알아야 된다. 먼저 간단한게 말하자면 `중복 허용 X`, `단일성 보장`, `Collection<T>`의 자식이다. 

Collections.singleton으로 생성된 Set은 오직 하나의 요소만을 가지며, 이 요소는 중복될 수 없고 추가나 삭제가 불가능하다. 
이 특성을 활용하면, 특정 상황에서 단일 객체만 있어야 하는 요구사항을 쉽게 충족시킬 수 있다.
그리고 Collections의 자식이기 때문에 기존 API와의 호환성에서도 좋다. 여러개의 객체로 확장되 가능하며 여러가 이점이 많다.

## 불변성
내가 봤을때는 이게 제일 이점이 큰거 같다. stackoverflow에서는 불변성을 유지한다고 했다.
그 이유도 간단하다! 싱글톤을 쓰기 때문이다. 
불변성을 유지하면 Thread-Safe하여 병렬 프로그래밍 유용 및 동기화를 고려화하지 않아도 된다.

> size가 1로 고정됨 (지정된 단일 객체를 가르키는 주소값을 가지기 때문)


만약 값을 변경하면? 오류가 난다.

```java
Set<Employee> ceo = Collections.singleton(new Employee("Tim Cook" ));

ceo.add( … ) ;     // Fails
ceo.clear() ;      // Fails
ceo.remove( … ) ;  // Fails

someReport.processEmployees( ceo ) ;
```

## 요약
Set으로 하면 다른 API와의 확장성도 좋고 Collections.singleton으로 생성하는 Set은 오직 하나의 요소만을 가진다. 
즉, **불변성**을 유지 할 수 있다.

## 참고
[What is the benefit for Collections.singleton() to return a Set instead of a Collection?](https://stackoverflow.com/questions/31599467/what-is-the-benefit-for-collections-singleton-to-return-a-set-instead-of-a-col)
[Java 단일 요소의 배열 Collections.singletonList vs Arrays.asList](https://prohannah.tistory.com/85)
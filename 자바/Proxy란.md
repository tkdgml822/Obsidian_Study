# Proxy란?

Java에서 Proxy는 이전에 다뤘던 Reflection과 연결된다.

`Proxy` 란 사전적으로 대리인이란 뜻을 가지고 있다. Java에서 프록시는 RealSubject는 자신의 기능에만 집중을 하고 그 이외 부가 기능을 제공하거나 접근을 제어하는 역할을 Proxy 객체에서 위임한다.

이로 인해 `SOLID` 원칙 중 `SRP`를 지향하는 코드로 작성이 가능해진다.

Proxy 패턴은 아래와 같은 구조를 가지고 있는데 Client는 Interface type의 Subject를 의존하고 있고 해당 Interface를 Proxy와 RealSubject가 구현을 하고 있다.

---

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/dbc105ba-4b63-44eb-86ff-fb345594947e/f1613648-3cc9-499d-8489-1dc46fcb7f79/Untitled.png)

프록시 Pattern의 특징은 다음과 같다.

- RealSubject와 같은 이름의 메소드를 구현한다. (interface를 구현)
- RealSubject에 대한 참조 변수를 가진다.
- RealSubject의 method를 호출하고 그 결과를 클라이언트에게 반환한다.
- RealSubject 메서드의 호출 전, 후에 별도의 로직을 수행할 수 있다.

# 예제 코드 (Pure java)

먼저 Client가 호출 할 interface를 정의한다

```java
public interface BookService {
	void printTitle(Book book);
}
```

interface를 구현하고 있는 RealSubject를 구현한다.

```java
public class BookServiceImpl implements BookService {
	@Override
	public void printTitle(Book book) {
		System.out.println("book.getTitle() = " + book.getTitle());
 	}
}
```

interface를 구현하면 `RealSubject`를 참조하고 있는 `Proxy` 객체를 정의한다.

이 때의 `Proxy` 안에서 많은 작업들을 할 수 있지만 간단한 logging을 남기는 작업을 진행하겠다.

```java
public class BookServiceImplProxy implements BookService [
	BookService bookService = new BookServiceImpl();
	
	@Override
	public void printTitle(Book book) {
		System.out.println("prev prev method");
		bookService.printTitle(book);
		System.out.println("prev after method");
	}
}
```

이 후 `BookService`의 인스턴스를 생성 할 때 `BookServiceImplProxy`와 같은 Proxy 객체를 넣어준 후 Client는 해당 interface를 사용하게 된다.

```java
public static void main(String[] args) {
	BookService BookService = new BookServiceImplProxy();
	bookServiceImplProxy.printTitle(new Book("foobar");
}
```

### proxy 패턴을 이용할 때 고려해야 할 점들...

위의 예제 코드와 같이 Proxy 객체를 직접 생성하는 일은 많은 불편함을 가져다 준다. (`~~Proxy`를 적용하려는 interface의 method가 1개라면 괜찮을 수도 있겠지만?~~)

target이 여러개이고 대부분 동일한 기능을 제공해야 하는 `Proxy`를 정의해야 한다면? 중복 코드는 무척이나 발생할 것이며 생성성도 떨어 질 것이다.

이러한 점들을 이 후 알아볼 `Dynamic Proxy`라는 개념을 통해 개선해나가보도록 하자! (Java Reflection package에서 제공 -> Dynamic Proxy도 Reflection의 연장선의 개념이다.)
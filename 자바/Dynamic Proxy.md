# Dynamic Proxy란?

`Dynamic Proxy`란 이전과 같이 `Proxy` 객체를 직접 생성을 하는 것이 아니라 **Runtime(애플리케이션이 실행되는 중)에 Interface를 구현하는 Class or 인스턴스를 만들어내는 것을 이야기 한다.**

Java `java.lang.reflect.Proxy` package에서 제공해주는 API를 이용하여 `Dynamic Proxy`를 이용할 수 있다.

API에서 제공해주는 Syntax는 다음과 같다.

```java
public static Object newProxyInstance(
    ClassLoader loader, // 1
    Class<?>[] interfaces, // 2
    InvocationHandler h // 3
) throws IllegalArgumentException
```

1. `Proxy`객체 정의하기 위한 `Class Loader`를 지정한다. (`Proxy` 객체가 구현할 Interface에 `Class Loader`를 얻어오는 것이 일반적)
2. `newProxyInstance()`를 통해 생성 될 `Proxy` 객체가 구현할 Interface를 정의한다.
3. 메소드 호출을 디스패치하기 위한 호출 핸들러. (디스패치: 어떤 메소드를 호출할 것인가를 결정하여 그것을 실행하는 과정을 이야기함.)

위에 정의된 대로 매개변수를 지정하여 API를 호출하면 return 되는 값은 다음과 같다고 문서에서 정의하고 있다.

> Returns: a proxy instance with the specified invocation handler of a proxy class that is defined by the specified class loader and that implements the specified interfaces

직역하자면 지정된 `Class Loader`에 의해 정의된 Interface를 구현한 `Proxy` 객체의 `InvocationHandler`를 가지고 있는 `Object`타입의 `Proxy`인스턴스라는 것이다.

## 예제 코드

위에서는 API의 스팩에 대하여 알아봤으니 한번 코드로 작성해보자.

Java에서 제공하는 `Dynamic Proxy`는 Interface 기반이기 때문에 Interface를 정의한다.

```java
public interface BookService {
    void printTitle(Book book);
}
```

해당 Interface를 구현하는 구현 Class를 정의한다.

```java
public class BookServiceImpl implements BookService{

    @Override
    public void printTitle(Book book) {
        System.out.println("book.getTitle() = " + book.getTitle());
    }
}
```

`newProxyInstance()`를 이용하여 Interface기반의 `Proxy` 인스턴스를 생성한다.

```java
 public static void main(String[] args) {
        // interface 기반으로 해당 구현체의 인스턴스를 생성하였다.
        // (pure 자바에서 제공하는 reflect.Proxy는 Class 기반의 Proxy를 만들지 못한다.)
        BookService bookService = (BookService) Proxy.newProxyInstance(
        	BookService.class.getClassLoader(),
           new Class[]{BookService.class},
           (proxy, method, arguments) -> {
            	BookService realSubject = new BookServiceImpl();
  		          System.out.println("prev method invoke");
                Object invoke = method.invoke(realSubject, arguments);
                System.out.println("after method invoke");
                return invoke;
        	});

				// class com.sun.proxy.$Proxy0
        System.out.println("bookService.getClass() = " + bookService.getClass());

        Book book = new Book("foobar");
        bookService.printTitle(book);

        // output
        // prev method invoke
        // bookService.getClass() = foobar
        // after method invoke
    }
```

위의 예제 코드와 같이 `java.lang.reflect.Proxy`에서 제공해주는 API를 이용하면 런타임에도 해당 Interface를 구현하는 **Proxy객체의 인스턴스를 생성할 수 있다.**
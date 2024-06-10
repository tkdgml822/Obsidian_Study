# 정의

Java 코드는 컴파일이 되면 바이트 코드(.class)로 변환이 된다. 하지만 가끔씩 애플리케이션 실행 중에(런타임) 일부 class, 인터페이스, 필드, 메서드 등 변경을 해야랄 때가 필요하다.

이렇게 애플리케이션 실해중 코드를 변경할 수 있돌고 Java에서는 Reflection를 제공해준다.

추가적으로 Reflection을 이용하면 런타임에 객체를 동적으로 생성할 수도 있다.

# 요약

Reflection은 런타임에 클래스를 동작을 검사하거나 조작할 때 사용되는 프로세스이다.

# Java.lang.Class

Java에서는 제공하는 Reflection을 이용하기 위해 살펴봐야 할 라이브러리는 `Java.lang.Class`이다.

`Java.lang.Class` 를 얻어 올 수 있는 방법은 3가지이다.

# .class

인스턴스가 존재하지 않을 경우에는 사용자 정의 타입에 `.class`를 붙어 Class 객체를 얻어올 수 있다.

```java
public class Member {
	private String name;
	private int age;
}

public static void main(String[] args) {
	Class<Member> memberClass = Member.class;
}
```

위와 같이 타입을 통해 `Class` 객체를 얻어올 수 있다.

# Object.getClass()

인스턴스가 존재할 경우 `Object.getClass`를 이용해서 `Class` 객체를 얻어올 수 있다.

모든 `Class`는 Object를 상속받고 있기 때문에 해당 method는 제공된다.

```java
public class Member {
	private String name;
	private int age;
}

public static void main(String[] args) {
	Member member = new Member();
	member.getClass();
	Class<? extends Member> memberClass = member.getClass();
}
```

# Class.forName()

패키지명이 포함된 클래스명을 통해서도 `Class` 객체를 얻어올 수 있다.

```java
public class Member {
	private String name;
	private int age;
	
	public static void main(String[] args) {
		Class<?> memberClass = Class.forName("com.example.Member");
	}
```

하지만 위와 같은 경우 `Class`를 찾지 못한다면 `ClassNotFoundException`를 발생 시키기 때문에 예외처리가 강제가 된다.

# 요약

1. `.class`
2. `.getClass()`
3. `Class.forName()` (`ClassNotFoundException` 예외 처리)

---

## Hello Reflection

이번 글에서는 Reflection을 통해 인스턴스를 생성하고 method를 실행하는 방법만을 담으려고 한다.

### Reflection을 이용한 인스턴스 생성하기

이전에는 `Class` 객체에서 제공해주는 `newInstance()`를 통해 `Class` 객체에서 바로 인스턴스를 생성할 수 있었다.

하지만 현재 `newInstance()`는 Java9부터 Deprecated되었다.

```java
@CallerSensitive
@Deprecated(since="9")
public T newInstance(){
	//...
}
```

이로 인해 인스턴스 생성하는 순서는 다음과 같다.

1. `Class` 객체 얻어오기
2. 해당 `Class`에 대한 생성자 얻어오기
3. 생성자를 통해 인스턴스 생성

다음과 같은 class가 있다고 해보자.

```java
public class Member {

    private String name;
    private int age;

    public Member() {
    }

    public Member(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

먼저 위와 같은 class가 있다고 할 때 Reflection을 이용하여 기본생성자를 호출하여 인스턴스를 생성하는 로직은 다음과 같다.

```java
public static void main(String[] args){
	// 1. 클래스 객체 가져오기
	Class<Member> memberClass = Member.class;
	// 2. 해당 Class에 대한 생성자 얻어오기
  Constructor<Member> noArgConstructor = memberClass.getConstructor();
  // 3. 생성자를 통해 인스턴스 생성
  Member member = noArgConstructor.newInstance();
}
```

기본 생성자이기 때문에 `getConstructor()` 를 할 때 파라미터로 어떤한 type도 지정해주지 않는다. 지정해주지 않으면 Member Class의 기본 생성자가 호출이 된다.

그럼 매개변수가 있는 생성자를 호출 할때는 어떻게 해야 할까?

```java
public static void main(String[] args){
	// 1. 클래스 객체 가져오기
	Class<Member> memberClass = Member.class;
	// 2. 해당 Class에 대한 생성자 얻어오기 (매개변수 -> String, int)
	Constructor<Member> argConstructor = memberClass.getConstructor(String.Class, int.Class);
	// 3. 생성자를 통해 인스턴스 생성
	Member member = argConstructor.newInstance("leewoooo", 27);
}
```

위의 코드와 같이 매개변수가 있는 생성자를 Reflection을 이용하여 가져올 때 파라미터의 **타입을 지정해주어야 한다.**

공통적으로는 만약 해당하는 생성자를 찾지 못하면 `NoSuchMethodException`이 발생된다.

# 메소드 실행하기

메소드를 실행하는 것도 인스턴스를 생성하는 순서와 비슷하다.

1. `Class` 객체 얻어오기
2. 해당 `Class에서 method` 얻어오기
3. 얻어온 method를 `invoke()` API를 이용하여 실행하기

다음과 같은 Member method가 있다고 해보자.

```java
public class Member {
    //...
    public int sum(int left, int right){
        return left + right;
    }
    
    public static int staticSum(int left, int right){
        return left + right;
    }
}
```

인스턴스를 생성할 때 처럼 `Class` 객체를 얻어와서 method를 얻어온다.

```java
public static void main(String[] args){
	// 1. Class 객체 얻어오기
	Class<Member> memberClass = Member.class;
	// 메서드 얻어오기 여기에서 얻어오는 메서드는 "sum"
	Method sum = memberClass.getMethod("sum", int.class, int.class)
}
```

`getMethod()`를 실행할 때 필요한 필요한 것은 아래와 같다.

1. method 명
2. method의 매개변수 타입들

이미 위에 예제 코드에서 본 것 처럼 매개변수 있는 생성자를 얻어올 때와 동일하다.

만약 매개변수가 없는 메소드라면 메소드 명만 입력해주면 되면 해당 정보를 메소드를 찾지 못할 경우 동일하게 `NoSuchMethodException`가 발생한다.

실행은 `Method`에서 제공하는 `invoke()`를 호출하여 실행하면 된다. 여기서 `static` 메소드와 `static`이 아닌 method로 나뉘게 되는데

`static` 메소드는 `invoke()`를 실행 할 때 해당 `Class` 타입의 `Object` 즉 인스턴스가 필요 없지만 `static` 이 아닌 method는 실행할 때 인스턴스가 필요하다.

```java
Method sum = memberClass.getMethod("sum", int.class, int.class);
int result = (int) sum.invoke(memberWithAge, 1, 2);
System.out.println("result = " + result);

Method staticSum = memberClass.getMethod("staticSum", int.class, int.class);
int staticResult = (int) staticSum.invoke(null, 1, 2);
System.out.println("staticResult = " + staticResult);
```

# 요약

Reflection을 이용하여 `Class` 객체를 이용하여 해당 인스턴스를 생성도 할 수 있으며 메소드도 실행할 수 있다.

생성자를 얻어올 때, 메소드를 얻어올 때 매개변수가 있다면 매개변수의 **타입을 지정해야 한다.**

만약 찾으려는 생성자나 메소드가 존재하지 않을 경우 `NoSuchMethodException`가 발생한다.

`static` 메소드가 아닌 경우 `invoke()`를 실행할 때 첫번 째 인자로 **인스턴스를 넣어줘야 한다.**
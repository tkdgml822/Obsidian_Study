- static 메소드의 참조
    
    ```java
    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;
    import java.util.function.Consumer;
    
    class ArrageList {
        public static void main(String[] args) {
            List<Integer> ls = Arrays.asList(1, 3, 5, 7, 9);
            ls = new ArrayList<>(ls);
    
            Consumer<List<Integer>> c = l -> Collections.reverse(l); // 람다식
            c.accept(ls); // 순서 뒤집기
            System.out.println(ls); // 출력
        }
    }
    ```
    
    이 예제의 핵심은 다음과 같다.
    
    ```java
    Consumer<List<Integer>> c = l -> Collections.reverse(l);
    ```
    
    그리고 만약에 Collections 클래스의 다음 메소드를 몰랐다면, 위와 같이 간단한 람다식의 작성은 불가능했을 것이다.
    
    ```java
    public static void reverse(List<?> list) // 저장 순서를 뒤집는다.
    ```
    
    이 메소드는 저장순서를 뒤집어주는 역할을 한다.
    
    그리고 자바 8에서는 람다식을 작성할 필요없이 다음과 같이 작성할수도 있다.
    
    ```java
    Consumer<List<Integer>> c = Collections::reverse;
    ```
    
    즉 static 메소드 참조 방법은 다음과 같다. 그리고 아래에서 :: 은 자바 8에서 추가된, 메소드 참조를 의미하는 연산자이자.
    
    ```java
    ClassName::staticMethodName
    ```
    
    정리하면, 다음과 같이 ‘메소드 참조’를 기반으로 람다식을 대신할 수 있다.
    
    ```java
    Consumer<List<Integer>> c = l -> Collections.reverse(1);
    -> Consumer<List<Integer>> c = Collections::reverse;
    ```
    
    위의 ‘메소드 참조’에서 람다식에는 있는 인자 전달에 대한 정보를 생략할 수 있는 이유는 다음 약속에 근거한다.
    
    “accept 메소드 호출 시 전달되는 인자를 reverse 메소드를 호출하면서 그대로 전달한다.”
    
    ```java
    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;
    import java.util.function.Consumer;
    
    class ArrageList2 {
        public static void main(String[] args) {
            List<Integer> ls = Arrays.asList(1, 3, 5, 7, 9);
            ls = new ArrayList<>(ls);
    
            Consumer<List<Integer>> c = Collections::reverse; // 메소드 참조
            c.accept(ls); // 전달 인자 ls를 reverse에 그대로 전달하게 된다.
            System.out.println(ls); // 출력
        }
    }
    ```
    
- 인스턴스 메소드의 참조 1 : 인스턴스가 존재하는 상황에서 참조
    
    ```java
    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;
    import java.util.function.Consumer;
    
    class JustSort {
        public void sort(List<?> lst) {
            Collections.reverse(lst);
        }
    }
    
    class ArrangeList3 {
        public static void main(String[] args) {
            List<Integer> ls = Arrays.asList(1, 3, 5, 7, 9);
            ls = new ArrayList<>(ls);
            JustSort js = new JustSort();
    
            Consumer<List<Integer>> c = e -> js.sort(e); // 람다식 기반
            System.out.println(ls);
        }
    }
    ```
    
    위에서 봐야할것은 다음과 같다.
    
    ```java
    Consumer<List<Integer>> c = e -> js.sort(e); // 람다식 기반
    ```
    
    같은 지역 내에 선언된 참조변수 js에 접근하고 있다.
    
    람다식에서는 같은 지역에 선언된 참조변수에 접근하는 것은 가능하다. 단 다음 조건을 만족해야 가능하다.
    
    “람다식에서 접근 가능한 참조변수는 final로 선언되었거나 effectively final이어야 한다.”
    
    effectively final이라는 것은 “사실상 final 선언이 된 것과 다름없음”을 뜻한다. 위 예제에서 참조하는 js는 effectively final이다. si가 참조하는 대상을 수정하지 않기 때문이다. 그렇다면 중간에 값을 바꾸면 어떤 일이 일어날까? 컴파일 에러가 난다.
    
    ```java
    public static void main(String[] args) {
    	....
    	JustSort js = new JustSort();
    	js = new JustSort(); // 다른 인스턴스를 참조
    	Consumer<List<Integer>> c = l -> js.sort(l);
    	....
    }
    ```
    
    js에 null을 대입해도 컴파일 오류로 이어지게 된다. 왜냐하면 js가 effectively final이 아니기 때문이다.
    
    ```java
    public static void main(String[] args) {
    	....
    	JustSort js = new JustSort();
    	js = new JustSort(); // 다른 인스턴스를 참조
    	Consumer<List<Integer>> c = l -> js.sort(l);
    	....
    	js = null;
    }
    ```
    
    이유: final이나 effectively final이 아닌 참조변수를 참조하게 하는 것은 논리적 혼란을 일으키거나 예특 불가능한 상황으로 이어질 수 있어서 이러한 제한을 두는 것이다. 그럼 다음로 다시 예제로 돌아와서 예시를 보자 위 예제에서 등장한 람다식은 다음과 같이 ‘메소드 참조’로 대신할 수 있다.
    
    ```java
    Consumer<List<Integer>> c = e -> js.sort(e);
    ```
    
    →
    
    ```java
    Consumer<List<Integer>> c = js:sort;
    ```
    
    ReferenceName::instanceMethodName
    
    ```java
    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;
    import java.util.function.Consumer;
    
    class JustSort {
        public void sort(List<?> lst) {
            Collections.reverse(lst);
        }
    }
    
    class ArrangeList3 {
        public static void main(String[] args) {
            List<Integer> ls = Arrays.asList(1, 3, 5, 7, 9);
            ls = new ArrayList<>(ls);
            JustSort js = new JustSort();
    
            Consumer<List<Integer>> c = js::sort; // 메소드 참조 기반
    				c.accept(ls);
            System.out.println(ls);
        }
    }
    ```
    
- 인스턴스 메소드의 참조 2: 인스턴스 없이 인스턴스 메소드 참조
    
    ```java
    import java.util.function.ToIntBiFunction;
    
    class IBox {
        private int n;
        public IBox(int i) { n = i; }
        public int larger(IBox b) {
            if(n > b.n) {
                return n;
            }
            else {
                return b.n;
            }
        }
    }
    
    class NoObjectMethodRef {
        public static void main(String[] args) {
            IBox ib1 = new IBox(5);
            IBox ib2 = new IBox(7);
            
            // 두 상자에 저장된 값 비교하여 더 큰 값 반환
            ToIntBiFunction<IBox, IBox> bf = (b1, b2) -> b1.larger(b2);
            int bigNum = bf.applyAsInt(ib1, ib2);
            System.out.println(bigNum);
        }
    }
    // ToIntBiFunction<T, U> int applyAsInt(T t, U u)
    
    ```
    
    위 예제에서 등장한 람다식은 다음과 같다.
    
    ```java
    ToIntBiFunction<IBox, IBox> bf = (b1, b2) -> b1.larger(b2);
    ```
    
    위 람다식에서 호출하는 메소드 larger가 ‘첫 번째 인자로 전달된 인스턴스의 메소드’라는 사실에 주목하자. 다음과 같이 메소드 참조가 이를 대신할 수 있다.
    
    ```java
    ToIntBiFunction<IBox, IBox> bf = (b1, b2) -> b1.larger(b2);
    ```
    
    ```java
    ToIntBiFunction<IBox, IBox> bf = IBox::larger(b2); // 메소드 참조 방식
    ```
    
- 생성자 참조
    
    람다식을 작성하다 보면 인스턴스를 생성하고 참조 값을 반환해야 하는 경우가 있다. 그리고 이러한 경우에도 ‘메소드 참조’ 방식을 쓸 수 있는데, 이와 관련하여 먼저 다음 예제를 보자.
    
    ```java
    import java.util.function.Function;
    
    class StringMaker {
        public static void main(String[] args) {
            Function<char[], String> f = ar -> {
                return new String(ar);
            };
            char[] src = {'R', 'o', 'b', 'o', 't'};
            String str = f.apply(src);
            System.out.println(str);
        }
    }
    // Function<T, R> R apply(T t)
    
    ```
    
    위 예제에서는 String 클래스의 다음 생성자를 이용한 String 인스턴스의 생성을 보이고 있다.
    
    ```java
    public String(char[] value)
    ```
    
    물론 이 예제에서 중요한 것은 위의 생성자가 아닌 다음 람다식이다.
    
    ```java
    Function<char[], String> f = ar -> {
    	return new String(ar);
    }
    ```
    
    우선 이 람다식은 다음과 같이 줄일 수 있다.
    
    ```java
    Function<char[], String> f = ar → new String(ar);
    ```
    
    그리고 위의 문장을 메소드 참조 방식으로 바꿀 수 있다.
    
    ```java
    Function<char[], String> f = ar → new String(ar);
    ```
    
    ```java
    Function<char[], String> f = ar → new String(ar);
    -> Fuction<char[], String> f = String::new; // 생성자 참조 방식
    ```
    
    정리하면, 람다식을 이루는 문장이 ‘단순이 인스턴스 생성 및 참조 값의 반환’일 경우, 이를 다음 형태의 메소드 참조로 바꿀 수 있다.
    
    ```java
    ClassName::new
    ```
    
    이번에도 이러한 줄임이 가능한 이유는 역으로 생각해보기 위해 다음 코드를 보자.
    
    ```java
    public static void main(String[] args) {
    	Function<char[], String> f = String::new;
    	....
    	String str = f.apply(src);
    	....
    }
    ```
    
    위의 코드에서 f의 참조 대상이 String::new 이므로, f는 String의 생성자를 참조하게 되는데, 참조 변수 f의 자료형이 Function<char[], String> 이므로 매개변수 형이 char[]인 다음 생성자를 참조하게 된다.
    
    ```java
    public String(char[] value)
    ```
    
    따라서 이후에 다음 문장을 실행하게 되면,
    
    ```java
    String str = f.apply(src);
    ```
    
    다음 내용으로 apply 메소드가 호출된다.
    
    ```java
    apply(src) {
    	new String(src);
    }
    ```
    

## Optional 클래스

코드를 작성하다보면 NullPointerException 예외를 접근할 수 있다. 따라서 이에 대한 처리를 고려하고 코드를 작성해야 하는데, 이는 어렵지는 않지만 매우 번거로운 일이다. 그래서 이러한 일을 단순히 처리할 수 있도록 Optional 클래스가 소개되었다.

> 여기서 자바의 100% 호환이 되는 코틀린을 사용하면 Null 안정성이 보장받기 때문에 Optional을 사용 할 필요가 없다.

- NullPointerException 예외의 발생 상황

```java
// 친구 클래스
class Friend {
    String name;
    Company cmp; // 친구의 회사 참조변수

    public Friend(String n, Company c) {
        name = n;
        cmp = c;
    }

    public String getName() {
        return name;
    }

    public Company getCmp() {
        return cmp; // 친구의 회사 리턴
    }
}

// 회사
class Company {
    String cName; // 이름
    ContInfo cInfo; // null 인줄 알수 없음, 정보

    public Company(String cn, ContInfo ci) {
        cName = cn;
        cInfo = ci;
    }

    public String getCName() {
        return cName;
    }

    // 정보 리턴
    public ContInfo getContInfo() {
        return cInfo;
    }
}

// 정보
class ContInfo {
    String phone; // null 일 수 있음
    String adrs; // null 일 수 있음

    public ContInfo(String ph, String ad) {
        phone = ph;
        adrs = ad;
    }

    public String getPhone() {
        return phone;
    }

    public String getAdrs() {
        return adrs;
    }
}

class NullPointerCaseStudy {
    // 매개변수 친구 Friend f
    public static void showCompAddr(Friend f) {
        String addr = null;

        // 친구가 Null이 아니면
        if (f != null) {
            // 친구의 회사 정보를 가져옴
            Company com = f.getCmp();
            // 회사 정보가 Null이 아니면
            if (com != null) {
                // 정보를 가져옴
                ContInfo info = com.getContInfo();
                // 정보가 null이 아니면
                if (info != null) {
                    // 주소를 가져옴
                    addr = info.getAdrs();
                }
            }
        }

        // addr이 null이 아니면 즉, 성공적으로 모든 정보들이 null 아니면 null이 아니다.
        if (addr != null) {
            // 주소 출력
            System.out.println(addr);
        }
        // 어느 정보 하나라도 없다면
        else {
            System.out.println("There's no address information");
        }

    }

    public static void main(String[] args) {
        // 핸드폰과, 주소입력
        ContInfo ci = new ContInfo("321-444-577", "Republic of Korea");
        // 회사 이름과, 정보(핸드폰, 주소) 입력
        Company cp = new Company("YaHo Co., Ltd.", ci);
        // 친구의 이름과, 친구의 회사 정보(정보(핸드폰, 주소)) 입력
        Friend frn = new Friend("Lee Su", cp);
        showCompAddr(frn); // 친구가 다니는 회사 주소 출력
    }
}
```

예제에서 보이듯이 정의 클래스 멤버 중 일부에는 다음과 같이 null이 저장될 수 있다.

```java
class ContInfo {
	String phone; // 전화번호 정보가 없을 수 있다.
	String adrs; // 주소 정보가 없을 수 있다.
}
```

실제로 친구가 휴직 상태이거나 회사의 재직 중임에도 불구하고 개인에게 해당되는 업무용 전화번호가 없다면 해당 멤버는 null 일 수 있다. 하지만 예제와 같이 회사의 출력하는 일이 매우 복잡해졌다.

다음과 같이 생각할 수 도 있다. “이렇게 Null 생길 수 있는 멤버들은 하나하나 if 문을 써서 검사해줘야 되나? 가독성도 떨어지고 화살표 모양이 되고 있네 뭔가 따로 만들어 놓은 것이 없을까?” 라고 말이다. 그것이 이번 장에서 배우는 Optional이다.

- Optional 클래스의 기본적인 사용 방법
    
    Optional 클래스는 java.util 패키지로 묶여 있으면, 다음과 같이 정의되어있다.
    
    ```java
    public final class Optional<T> extends Object {
    	private final T value; // 이 참조변수를 통해 저장을 한다.
    	....
    }
    ```
    
    Optional은 멤버 value에 인스턴스를 저장하는 일종의 래퍼(Wrapper) 클래스이다. 그럼 지금부터 작은 예제들을 소개 하겠다.
    
    ```java
    class StringOptional1 {
    	public static void main(String[] args) {
    		Optional<String> os1 = Optional.of(new String("Toy1"));
    		Optional<String> os2 = Optional.ofNullable(new String("Toy2"));
    
    		if (os1.isPresent())
    			System.our.println(os1.get());
    
    		if (os2.isPresent())
    				System.our.println(os1.get());
    		}
    }
    ```
    
    ```java
    Optional<String> os1 = Optional.of(new String("Toy1"));
    Optional<String> os2 = Optional.ofNullable(new String("Toy2"));
    ```
    
    두 메소드 of와 ofNullable의 차이점은 null의 허용 여부에 있다. ofNullable의 인자로는 null을 전달 할수 있다. 반면 of 메소드에는 null을 인자로 전달할 수 없다. null을 전달할 경우 NullPointerException이 발생한다. 그리고 Optional 인스턴스를 대상으로 다음과 같이 존재 여부를 확인할 수 있다. 또 해당 내용물을 꺼낼 수 있다.
    
    ```java
    if (os1.isPresent())
    			System.our.println(os1.get());
    ```
    
    위 예제는 Optional의 매력을 찾을 수 없다. 그러나 다음 예제에서는 Optional의 매력을 조금 보여준다.
    
    ```java
    import java.util.Optional;
    
    class StringOptional2 {
        public static void main(String[] args) {
            Optional<String> os1 = Optional.of(new String("Toy1"));
            Optional<String> os2 = Optional.ofNullable(new String("Toy2"));
    
            // 람다식 버전
            os1.ifPresent(s -> System.out.println(s));
    
            // 메소드 참조 버전
            os2.ifPresent(System.out::println);
        }
    }
    ```
    
    위 예제에서 호출하는 있는 메소드는 ifPresent의 매개변수 형은 Consumer이다.
    
    ```java
    public void ifPresent(Consumer<? super T> consumer)
    ```
    
    따라서 다음 메소드 accept의 구현에 해당하는 람다식을 ifPresent 호출 시 인자로 전달해야 한다.
    
    ```java
    Consumer<T> void accept(T t)
    ```
    
    → Optional<String>의 T가 String이므로 void accept(String t)
    
    그러면 ifPresent가 호출되었을 때, Optional 인스턴스가 저장하고 있는 내용물이 있으면, 이 내용물이 인자로 전달되면서 accept 메소드가 호출된다. (다시 말해서 전달된 람다식이 실행된다.) 반면 내용물이 없으면 아무 일도 일어나지 않는다. 따라서 다음의 if 문을
    
    ```java
    if (os1.isPresent())
    	System.out.prntln(os1.get());
    ```
    
    다음과 같이 줄이수 있다.
    
    ```java
    os1.ifPresent(s -> System.out.println(s));
    ```
    
- Optional 클래스를 사용하면 if ~ else문을 대신할 수 있다: map 메소드의 소개
    
    if뿐만 아니라 if ~ else도 사용하지 않아도 된다.
    
    ```java
    class ContInfo {
        String phone;   // null 일 수 있음
        String adrs;    // null 일 수 있음
    
        public ContInfo(String ph, String ad) {
            phone = ph;
            adrs = ad;
        }
    
        public String getPhone() { return phone; }
        public String getAdrs() { return adrs; }
    
    }
    
    class IfElseOptional {
        public static void main(String[] args) {
            ContInfo ci = new ContInfo(null, "Republic of Korea");
            
            String phone;
            String addr;
    
            if(ci.phone != null)
                phone = ci.getPhone();
            else
                phone = "There is no phone number.";
    
            if(ci.adrs != null)
                addr = ci.getAdrs();
            else
                addr = "There is no address.";
              
            System.out.println(phone);
            System.out.println(addr);
        }
    }
    ```
    
    위에서 정의한 다음 클래스의 멤버 둘은 null이 될 수 있다고 가정하였다.
    
    ```java
    class ContInfo {
    	String phone; // 전화정보 정보가 없을 수 있음
    	String adrs;  // 주소 정보가 없을 수 있음 
    	....
    }
    ```
    
    따라서 멤버가 null인 경우와 null이 아닌 경우의 실행 코드를 구분할려면, 위 예제와 같이 if ~ else를 문을 이용해야 한다. 그러나 Optional 클래스를 활용하면 if ~ else 없이도 이러한 코드를 작성할 수 있다.
    
    ```java
    import java.util.Optional;
    
    class OptionalMap {
        public static void main(String[] args) {
            Optional<String> os1 = Optional.of("Optional String");
            Optional<String> os2 = os1.map(s -> s.toUpperCase());
            System.out.println(os2.get());
    
            Optional<String> os3 = os1.map(s -> s.replace(' ', '_'))
                                      .map(s -> s.toLowerCase());
            System.out.println(os3.get());
        }
    }
    ```
    
    그리고 Map 자세히 보면 map은 제네릭 클래스에 정의된 제네릭 메소드임을 알 수 있다.
    
    ```java
    public <U> Optional<U> map(Function<? super T, ? extends U> mapper)
    ```
    
    따라서 다음 메소드의 apply의 구현에 해당하는 람다식을 map 호출 시 인자로 전달해야 한다.
    
    ```java
    Function<T, U>   U apply(T t)
    ```
    
    - 해석
        
        여기서
        
        ```java
        Optional<String> os1 = Optional.of("Optional String");
        Optional<String> os2 = os1.map(s -> s.toUpperCase());
        ```
        
        을 하면 일어나는 일을 알아보자.
        
        ```java
        Optional<String> os1 = Optional.of("Optional String");
        ```
        
        여기서 .of는 null이 전달되면 NullPointerExceptio이 일어나는 메소드이다. 여기서 두번째 Optional을 보자.
        
        ```java
        Optional<String> os2 = os1.map(s -> s.toUpperCase());
        ```
        
        그러 내부 map을 보자.
        
        ```java
        public <U> Optional<U> map(Function<? super T, ? extends U> mapper)
        ```
        
        일단 우리는 제네릭 제한을 String을 받는다고 설정해놨다. 그럼 다음과 같이 된다.
        
        ```java
        public Optional<String> map(Function<? super T, ? extends U> mapper)
        ```
        
        그리고 여기서 Function 함수형 인터페이스가 나온다. 함수형의 인터페이스는 다음과 같이 생겼다.
        
        ```java
        Function<T, U> U apply(T t)
        ```
        
        여기서 제네릭을 보면 상한 하한 제한을 해놨는데 여기서 `<? super T>` 은 쓰기만 하겠다는 뜻이다. String의 부모만 받겠다는 뜻이다. `<? extends U>` 은 읽기만 하겠다는 뜻이다. String을 상속받은 애들만 쓰겠다는 것이다. 여기서 그럼 Function함수형 인터페이스는 `<? extends U>`을 반환하겠다고 되어있다. 왜 이것을 반환하겠다고 되어있냐면 `<? extends U>`은 읽기만 가능하기 때문이다
        
        그리고 여기서 여기서 `s -> s.toUpperCase()` 을 썼다. 그러면 Function apply에서 s.toUpperCase()이 전달이 된다. <? extends U> 를 썼기 때문에 String의 상속 받았거나 String을 반환한다. s.toUpperCase은 return 값으로 String을 썻기때문에 쓸 수 있다.
        
    
    그런데 예제에서는 Optional<String>의 인스턴스를 대상으로 map 메소드를 호출하므로, 다음 메소드의 구현에 대한 람다식을 작성하면 된다. 즉 T는 Optional<String> 인스턴스 생성 시 String으로 이미 결정이 난 상태이다.
    
    ```java
    U apply(String t)
    ```
    
    그러면 map 메소드가 하는 일이 뭘까?
    
    “apply 메소드가 반환하는 대상을 Optional 인스턴스에 담아서 반환한다.”
    
    > 여기서 map이 두번 쓴 이유도 다음과 같다. `public <U> Optional<U> map(Function<? super T, ? extends U> mapper)` 여기서 반환값이 Optional<U>이라서 연속해서 map을 쓸 수 있다.
    
    **참고**
    
    Optional<T>는 제네릭 클래스이다. 따라서 Optional<String>의 인스턴스 메소드인 map은 다음과 같다.
    
    ```java
    public <U> Optional<U> map(Fuction<? super String, ? extends U> mapper)
    ```
    
    그리고 이는 제네릭 메소드이므로 U는 메소드 호출 시 결정된다. 예를 들면 다음과 같다.
    
    ```java
    Optional<String> os2 = os1.<String>map(s -> s.toUpperCase());
    ```
    
    위 문장에서는 map 호출 시 U를 String으로 지정하였다. 그러나 String 인스턴스의 참조 값이 반환된다는 사실이 이미 람다식에 반영되어 있으므로 위 문장에서 U에 대한 정보를 생략하여 다음과 같이 문장을 수성할 수 있다. 그러면 컴파일러가 문장을 분석하여 U를 String으로 결정해준다.
    
    ```java
    Optional<String> os2 = os1.map(s -> s.toUpperCase());
    ```
    
    지금까지 설명한 Optional 클래스의 map을 잘 이해했다면, 다음 문장이 어떻게 동작하는지 이해할 수 있다.
    
    ```java
    Optional<String> os3 = os1.map(s -> s.replace(' ', '_')).map(s -> s.toUpperCase());
    ```
    
    위 문장에서 다음 메소드 호출이 먼저 진행된다.
    
    ```java
    os1.map(s -> s.replace(' ', '_'))
    ```
    
    따라서 위의 map 메소드가 반환하는 것은 문자열 “Optional_String”을 감싼 Optional 인스턴스의 참조 값이다. 따라서 이 반환 값을 대상으로, 이어서 다음 메소드를 호출 할 수 있다.
    
    ```java
    map(s -> s.toLowerCase());
    ```
    
- Optional 클래스를 사용하면 if ~ else문을 대신할 수 있다: orElse 메소드의 소개
    
    Optional 클래스에는 Optional 인스턴스에 저장된 내용물을 반환하는 get 메소드가 존재한다. 그리고 이와 유사한 기능의 orElse 메소드도 존재한다. 즉 orElse 메소드도 Optional 인스터에 저장된 내용물이 없을 때, 대신해서 반환할 대상을 지정할 수 있다는 점에서 get 메소드와 차이가 있다.
    
    ```java
    import java.util.Optional;
    
    class OptioonalElse {
        public static void main(String[] args) {
            // empty()을 사용하면 내용이 없는 값인 Optional을 반환한다.
            Optional<String> os1 = Optional.empty();
            // of는 깂이 없으면 NullPointerException을 일으킴
            Optional<String> os2 = Optional.of("So Basic");
    
            String s1 = os1.map(s -> s.toString())
                    .orElse("Empty");
    
            String s2 = os2.map(s -> s.toString())
                    .orElse("Empty");
    
            System.out.println(s1);
            System.out.println(s2);
        }
    }
    ```
    
    ```java
    Empty
    So Basic 
    ```
    
    위에 예제에서 empty메소드를 호출하면 저장하고 있는 내용물이 없는, 빈 Optional 인스턴스가 생성되어 반환된다.
    
    ```java
    Optional<String> os1 = Optional.empty();
    ```
    
    →
    
    ```java
    Optional<String> os1 = Optional.ofNullable(null)
    ```
    
    - 차이점
        
        두 개의 메소드는 같은 기능을 하는데 차이점이 뭘까? **`Optional.ofNullable()`: 주어진 값이 `null`이면 빈 `Optional` 객체를 반환하고, `null`이 아니면 해당 값으로 채워진 `Optional` 객체를 생성합니다.**
        
        **`Optional.empty()`: 이 메소드는 항상 빈 `Optional` 객체를 반환합니다. 이 객체는 `null` 값을 안전하게 다루기 위한 방법 중 하나입니다.**
        
    
    그리고 이 참조변수를 대상으로 다음 문장을 실행하면 map 메소드가 호출되고, 이어서 orElse 메소드가 호출되어 그 반환 값이 s1에 저장된다.
    
    ```java
    String s1 = os1.map(s -> s.toString()).orElse("Empty");
    ```
    
    그런데 os1이 참조하는 Optional 인스턴스는 비어있다. 이러한 경우 map은 빈 Optional인스턴스를 생성하여 반환한다. 결국 위의 문장은 map이 반환한 빈 Optional 인스턴스를 대상으로 orElse 메소드를 호출하게 된다. 그리고 이렇게 빈 Optional 인스턴스를 대상으로 orElse 메소드를 호출하면, orElse를 호출하면서 전달된 인스턴스가 대신 반환된다. 즉 위의 문장이 실행되면 s1은 문자열 “Empty”를 참조하게 된다.
    
- Optional 클래스의 map과 orElse를 사용하여 if ~ else 문을 대신한 결과
    
    Optional 클래스의 map과 orElse의 사용 방법을 설명하였는데, Optional 클래스를 사용하는 수준은 사실 이 정도이다. 그럼 앞서 소개한 예제 IfElseOptional.java를 Optional 클래스 기반으로 개선해보겠다.
    
    ```java
    import java.util.Optional;
    
    class ContInfo {
        String phone;
        String adrs;
    
        public ContInfo(String ph, String ad) {
            phone = ph;
            adrs = ad;
        }
    
        public String getPhone() { return phone; }
        public String getAdrs() { return adrs; }
    }
    
    class MapElseOptional {
        public static void main(String[] args) {
            Optional<ContInfo> ci = Optional.of(new ContInfo(null, "Republic in Korea"));
    
            String phone = ci.map(c -> c.getPhone())
                    .orElse("There is no phone number");
            String adrs = ci.map(c -> c.getAdrs())
                    .orElse("There is no address");
    
            System.out.println(phone);
            System.out.println(adrs);
        }
    }
    ```
    
    ```java
    There is no phone number
    Republic in Korea
    ```
    
    이전 예제에 있는 if ~ else문이 사라졌지만 내용은 동일하다. 특히 코드의 양이 많이 줄었고 이로 인해 코드의 가독성도 높아졌다.
    
- 예제 NullPointerCaseStudy.java의 개선 결과
    
    ```java
    import java.util.Optional;
    
    class Friend {
        String name;
        Company cmp;
    
        public Friend(String n, Company c) {
            name = n;
            cmp = c;
        }
    
        public String getName() {
            return name;
        }
    
        public Company getCmp() {
            return cmp;
        }
    }
    
    class Company {
        String cName;
        ConInfo cInfo;
    
        public Company(String name, ConInfo cInfo) {
            this.cName = name;
            this.cInfo = cInfo;
        }
    
        public String getName() {
            return cName;
        }
    
        public ConInfo getCInfo() {
            return cInfo;
        }
    }
    
    class ConInfo {
        String phone;
        String adrs;
    
        public ConInfo(String phone, String adrs) {
            this.phone = phone;
            this.adrs = adrs;
        }
    
        public String getPhone() {
            return phone;
        }
    
        public String getAdrs() {
            return adrs;
        }
    }
    
    class NullPointerException {
    
        public static void showCompAddr(Optional<Friend> f) {
            String addr = f.map(Friend::getCmp)
                    .map(Company::getCInfo)
                    .map(ConInfo::getAdrs)
                    .orElse("There's no address information");
    
            System.out.println(addr);
        }
        public static void main(String[] args) {
            ConInfo ci = new ConInfo("321-444-577", "Republic of Korea");
            Company cp = new Company("YaHo Co., Ltd.", ci);
    //        Friend frn = new Friend("LEE SU", cp);
            Friend frn = new Friend("LEE SU", null);
            showCompAddr(Optional.of(frn));
        }
    	}
    ```
    
- Optional 클래스의 flatMap 메소드
    
    위의 예제 NullPointerCaseStudy2.java 에서는 Optional의 사용 범위를 showCompAddr로 제한하였다. 즉 이 메소드의 정의와 이를 호출하는 문장을 제외하고는 Optional 관련 코드를 볼 수 없었다. 그러나 경우에 따라서 Optional 클래스를 코드 전번에 걸쳐 사용하기도 한다.
    
    Optional 클래스를 코드 전반에 사용하기 위해서는 map 메소드와 성격이 유사한 flatMap 메소드를 알아야 한다. 따라서 다음 예제를 먼저 설명하겠다.
    
    ```java
    import java.util.Optional;
    
    class OptionalFlatMap {
        public static void main(String[] args) {
            Optional<String> os1 = Optional.of("Optional String");
            Optional<String> os2 = os1.map(s -> s.toUpperCase());
            System.out.println(os2.get());
    
            Optional<String> os3 = os1.flatMap(s -> Optional.of(s.toUpperCase()));
            System.out.println(os3.get());
        }
    }
    ```
    
    위 예제에서 보이는 다음 문장을 비교해보자. 이 문장의 비교를 통해 flatMap을 이해할 수 있다.
    
    ```java
    Optional<String> os2 = os1.map(s -> s.toUpperCase());
    Optional<String> os3 = os1.flatMap(s -> Optional.of(s.toLowerCase());
    ```
    
    위의 두 문장에서 호출하는 map과 flatMap 모두 Optional 인스턴스를 반환한다. 다만 map은 람다식이 반환하는 내용물을 Optional 인스턴스로 감싸는 일을 알아서 해주지만, flatMap은 알아서 해 주지 않기 때문에 이 과정을 람다식이 포함하고 있어야 한다. 이것이 두 메소드의 차이점이다.
    
    그렇다면 flatMap은 언제 유용하게 사용할 수 있을까? 다음 예제에서 보이듯이 Optional 인스턴스를 클래스의 멤버로 두는 경우에 유용하게 사용할 수 있다.
    
    ```java
    import java.util.Optional;
    
    class ContInfo {
        Optional<String> phone;   // null 일 수 있음
        Optional<String> adrs;    // null 일 수 있음
    
        public ContInfo(Optional<String> ph, Optional<String> ad) {
            phone = ph;
            adrs = ad;
        }
        public Optional<String> getPhone() { return phone; }
        public Optional<String> getAdrs() { return adrs; }
    }
    
    class FlatMapElseOptional {
        public static void main(String[] args) {
            Optional<ContInfo> ci = Optional.of(
                new ContInfo(Optional.ofNullable(null), Optional.of("Republic of Korea"))
            );
            
            String phone = ci.flatMap(c -> c.getPhone())
                             .orElse("There is no phone number.");
    
            String addr = ci.flatMap(c -> c.getAdrs())
                            .orElse("There is no address.");
              
            System.out.println(phone);
            System.out.println(addr);
        }
    }
    ```
    
    먼저 다음 클래스 정의를 보자.
    
    ```java
    class ConInfo {
    	Optional<String> phone; // null 일 수 있음
    	Optional<String> adrs; // null 일 수 있음
    	...
    }
    ```
    
    위의 클래스와 같이 멤버를 Optional로 두면 이 멤버와 관련된 코드 전반에 걸쳐서 코드의 개선을 기대 할 수 있다. 그런데 이렇게 멤버를 Optional로 두는 경우네는 map보다는 flatMap이 더 어울린다.
    
    만약에 예제의 다음 문장을 map 메소드를 호출하는 형태로 작성한다면,
    
    ```java
    String phone = ci.flatMap(c -> c.getPhone())
    								.orElse("There is no phone number");
    ```
    
    Optional로 감싸서 반환하는 map 메소드의 특성상 다음과 같이 get 메소드 호출을 통해서 꺼내는 과정을 거쳐야 하기 때문이다.
    
    ```java
    String phone = ci.map(c -> c.getPhone()).get()
    								.orElse("There is no phone number");
    ```
    
    그럼 전반에 걸쳐서 Optional 클래스를 적용한 다음 예제를 보이면 Optional에 대한 설명을 마무리 하고자 한다. 이 결과를 Optional 클래스의 사용에 대한 가이드 역할을 할 것으로 믿는다.
    
    ```java
    import java.util.Optional;
    
    class Friend {
        String name;
        Optional<Company> cmp;    // null 일 수 있음
    
        public Friend(String n, Optional<Company> c) {
            name = n;
            cmp = c;
        }
        public String getName() { return name; }
        public Optional<Company> getCmp() { return cmp; }
    }
    
    class Company {
        String cName;
        Optional<ContInfo> cInfo;    // null 일 수 있음
    
        public Company(String cn, Optional<ContInfo> ci) {
            cName = cn;
            cInfo = ci;
        }
        public String getCName() { return cName; }
        public Optional<ContInfo> getCInfo() { return cInfo; }
    
    }
    
    class ContInfo {
        Optional<String> phone;   // null 일 수 있음
        Optional<String> adrs;    // null 일 수 있음
    
        public ContInfo(Optional<String> ph, Optional<String> ad) {
            phone = ph;
            adrs = ad;
        }
        public Optional<String> getPhone() { return phone; }
        public Optional<String> getAdrs() { return adrs; }
    
    }
    
    class NullPointerCaseStudy3 {
        public static void showCompAddr(Optional<Friend> f) {
    
            String addr = f.flatMap(Friend::getCmp)
                    .flatMap(Company::getCInfo)
                    .flatMap(ContInfo::getAdrs)
                    .orElse("There's no address information.");
    
            System.out.println(addr);    
        }
    
        public static void main(String[] args) {
            Optional<ContInfo> ci = Optional.of(
                        new ContInfo(Optional.ofNullable(null), Optional.of("Republic of Korea"))
            );
            Optional<Company> cp = Optional.of(new Company("YaHo Co., Ltd.", ci));
            Optional<Friend> frn = Optional.of(new Friend("LEE SU", cp));
    
            // 친구 정보에서 회사 주소를 출력
            showCompAddr(frn);
        }
    }
    ```
    

## OptionalInt, OptionalLong, OptionalDouble 클래스

- Optional과 OptionalXXX와의 차이점
    
    OptionalXXX 클래스들은 Optional 클래스보다 기능이 제한적이다. 그래서 Optional을 대신하는 경우는 많지 않다. 하지만 다음 Chapter에서 소개하는 ‘스트림(Stream)’을 공부하다 보면, 이 클래스들을 조금 접하게 된다. 따라서 알아 두는 것이 학습에 도움이 된다. Optional을 기반으로 작성한 예제와 이를 OptionalInt 기반으로 수정한 예제를 제시하여 이들의 차이점을 확인해 보겠다.
    
    ```java
    import java.util.Optional;
    
    class OptionalBase {
        public static void main(String[] args) {
            Optional<Integer> oi1 = Optional.of(3);
            Optional<Integer> oi2 = Optional.empty();
            
            System.out.print("[Step 1.] : ");
            oi1.ifPresent(i -> System.out.print(i + "\\t"));
            oi2.ifPresent(i -> System.out.print(i));
            System.out.println();
    
            System.out.print("[Step 2.] : ");
            System.out.print(oi1.orElse(100) + "\\t");
            System.out.print(oi2.orElse(100) + "\\t");
            System.out.println();
        }
    }
    ```
    
    ```java
    [Step 1.] : 3
    [Step 2.] : 3   100
    ```
    
    이어서 다음을 보자
    
    ```java
    import java.util.OptionalInt;
    
    class OptionalIntBase {
        public static void main(String[] args) {
            OptionalInt oi1 = OptionalInt.of(3);
            OptionalInt oi2 = OptionalInt.empty();
            
            System.out.print("[Step 1.] : ");
            oi1.ifPresent(i -> System.out.print(i + "\\t"));
            oi2.ifPresent(i -> System.out.print(i));
            System.out.println();
    
            System.out.print("[Step 2.] : ");
            System.out.print(oi1.orElse(100) + "\\t");
            System.out.print(oi2.orElse(100) + "\\t");
            System.out.println();
        }
    }
    ```
    
    ```java
    [Step 1.] : 3
    [Step 2.] : 3   100
    ```
    
    두 예제를 통해서 Optional<T>에서 T를 Integer로 구체화한 것이 OptionalInt 임을 짐작할 수 있을 것이다. 이로써 OptionalXXX 클래스들에 대한 설명을 마치고자 한다. Optional<T>에서 T를 int로 구체화한 것을 OptionalInt인 것처럼 OptionalLong과 OptionlDouble을 이해하고 활용하면 된다. (단, OptionalXXX 클래스들은 map과 flatMap 메소드가 정의되어 있지 않다.)
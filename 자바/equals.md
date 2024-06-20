# equals
자바에서는 `==`이라는 비교 연산자가 있다 이 연사자는 두 `피 연산자`의 값이 같은지를 검사한다.
일단 `==` 연산자는 주소의 값을 비교한다.
그러면 equals뭘까?
<U>equals() 연산자는 객체끼리 내용을 비교하는 메소드이다.</U>

```java
public class Test() {
	public static void main(String[] args) {
		String str1 = "123";
		String str2 = "123";
		String str3 = str1;
		String srt4 = new String("123");
		
		// == : 주소를 비교
		System.out.println(str1 == str2); // true
		System.out.println(str1 == str3); // true
		System.out.println(str1 == str4); // false : 주소 값이 다름

		// equals(): 내용을 비교
		System.out.println(str1.equals(str2)); // true
 		System.out.println(str1.equals(str3)); // true
	} 
}
```

<details>
	<summary>str1이랑 str2는 따로 생성했는데 다른 주소가 안 나오나요?</summary>
		자바를 잘 알고 계신분들은 알고 알고있겠지만 혹시 궁금한분들이 있을수도 있어 적어본다.
		자바는 String을 생성하게 된다면 해당 String 값은 Heap 영역내에 "String Pool"이라는 영역에 들어가게 된다. 

		 윗 예제로 예를 들면 "123"은  "String Pool" 영역에 들어가서 다 같이 사용한다.
		 하지만 new로 생성하면 그렇지 않다. 아래는 참고 그림이다.
</details>
![자바Pool](자바/images/java-string-pool.png)		

`equals()`을 사용하면 객체 간의 내용을 비교한다. 정확히 말하면 객체간의 **동등성**을 비교하는데 사용된다.

# equals() 오버라이드

`equals()`는 종종 오버라이드(재정의)를 해서 객체의 상태를 비교하는데 사용한다.
현재 윗 코드에서는 우리가 String만 비교해서 간단해 보이지만 만약 다음과 같은 객체끼리는 어떻게 비교할 것인가?

```java
public class Cat() {
	private String name; // 고양이 이름
	private String type; // 고양이 종
	private String age; // 고양이 나이

	// 생성자
	public Cat(String name, String type, String age) {
		this.name = name;
		this.type = type;
		this.age = age;
	}
}
```
이런 객체들은 어떤씩으로 비교하고 판단해야 판단해야한다. 
만약에 Cat 객체로 코드를 짜다가 갑자기 2개의 Cat 객체들의 종을 비교하고 싶으면?

```java
public class Main {  
  
    public static void main(String[] args) {  
        Cat cat1 = new Cat("나비", "먼치킨", "2");  
        Cat cat2 = new Cat("보리", "먼치킨", "4");  
  
        System.out.println(cat1.equals(cat2)); // fasle
    }  
  
    static class Cat {  
        private String name; // 고양이 이름  
        private String type; // 고양이 종  
        private String age; // 고양이 나이  
  
        // 생성자        public Cat(String name, String type, String age) {  
            this.name = name;  
            this.type = type;  
            this.age = age;  
        }  
    }  
}
```
이런씩으로 하면 될까? 물론 `false`라는 결과가 나온다.
우리는 기준을 정해줘야 한다. 그러면 답은 간단하다 오버라이드(재정의) 해줘서 기준을 짜면 된다.

```java
import java.util.Objects;  
  
public class Main {  
  
    public static void main(String[] args) {  
        Cat cat1 = new Cat("나비", "먼치킨", "2");  
        Cat cat2 = new Cat("보리", "먼치킨", "4");  
  
        System.out.println(cat1.equals(cat2));  // true
    }  
  
    static class Cat {  
        private String name; // 고양이 이름  
        private String type; // 고양이 종  
        private String age; // 고양이 나이  
  
        // 생성자        
        public Cat(String name, String type, String age) {  
            this.name = name;  
            this.type = type;  
            this.age = age;  
        }  
  
        @Override  
        public boolean equals(Object obj) {  
            if (this == obj) {  
                return true;  
            }  
            if (obj == null || getClass() != obj.getClass()) {  
                return false;  
            }  
            Cat cat = (Cat) obj;  
            return Objects.equals(type, cat.type);  
        }  
    }  
}
```

오버라이드 해준 결과다. 

`if (this == obj)` : 같은 객체가 들어오면 true `cat1.equals(cat1)`와 같이 들어오면 true가 된다.
`if (obj == null || getClass() != obj.getClass())`: null이 들어오거나 getClass()로 현재 참조 중인 클래스와 비교할 클래스가 다르면 `false` 가 된다.
`Cat cat = (Cat) obj` :  Object 클래스를 Cat 객체로 다운 캐스팅을 해주고 대입해줍니다.
`return Objects.equals(type, cat.type)`: 그리고 Objects.equals()로 type이랑 다운 캐스팅을 해준 Cat에 있는 type이랑 비교해줍니다.


# hashCode()
아마 전글이 `hashCode()`에 관한 글이다. 
[HashCode](https://github.com/tkdgml822/Obsidian_Study/blob/main/자바/hashCode.md)를 잘 모르면 참고해주시면 감사하겠습니다.
- - -
`equals()`을 오버라이딩 할때 hashCode()을 함께 오버라이딩하는 것이 일반적이다. 
이는 `equals()` 메소드가 true를 반환하는 두 객체는 동일한 해쉬 코드를 반환해야 하기 때문이다.

**안하면?**
해시 기반의 컬렉션(HashMap, HashSet)에서 예상치 못한 동작이 발생 할 수 있다.


참고 </br>
[Java(자바)- 이항연산자 3.비교 연산자(==, !=, <, <=, >, >=)](https://park-youjin.tistory.com/15)</br>
[Java 문자열(String) 비교 방법 ==, equals() 차이](https://milku.tistory.com/112)
[Java 자바 문자열 비교 equals(), == 사용법 및 차이점](https://lnsideout.tistory.com/entry/JAVA%EC%9E%90%EB%B0%94-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%B9%84%EA%B5%90-equals-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%B0%A8%EC%9D%B4%EC%A0%90)</br>
[Java String Pool](https://www.javastring.net/java/string/pool)</br>
[String Constan Pool이란?](https://starkying.tistory.com/entry/what-is-java-string-pool) </br>
**GPT**

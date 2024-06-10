## 메소드 오버로딩(Methid Overloding)

한 클래스 내에 동일한 이름의 메소드를 둘 이상 정의하는 것은 허용되지 않는다. 그러나 매개변수의 선언이 다르면 가능하다. 이것을 메소드 오버로딩이라고 한다.

- 메소드 오버로딩의 조건
    
    호출한 메소드를 찾을 때 다음 두 가지 정보를 참조하여 메소드를 찾게 된다.
    
    - 메소드의 이름
    - 메소드의 매개변수 정보
    
    예를 들어서 다음 메소드의 호출문을 보자.
    
    ```java
    MyHome home = new MyHome();
    home.mySimpleRoom(3, 5);
    ```
    
    위의 문장에서 호출하는 메소드를 찾을 때 다음 두 가지 정보가 사용된다.
    
    - 메소드의 이름은 mySimpleRoom이다.
    - 3과 5를 인자로 전달받을 수 있는 메소드이다.
    
    따라서 MyHome 클래스 내에 다음과 같이 mySimpleRoom 메소드가 둘 이상 존재해도 문제가 되지 않는다. 매걔변수의 선언이 다르면 호출된 메소드의 구분이 가능하기 때문이다.
    
    ```java
    class MyHome {
    	void mySimpleRoom(int n) {...}
    	void mySimpleRoom(int n1, int n2) {...}
    	void mySompleRoom(double d1, double d2) {...}
    ```
    
    정리하면, 메소드의 이름이 같아도 매개변수 선언이 다르면 메소드 호출문의 전달인자를 통해서 호출된 메소드를 호출된 메소드를 구분할 수 있다. 때문에 매개변수의 선언이 다르면 동일한 메소드 정의를 허용하는데, 이를 가르켜 ‘메소드 오버로딩’이라 한다.
    
    메소드 오버로딩이 성립할려면 매개변수 선언이 달라야 한다고 했는데, 구체적으로 매개변수의 수 또는 형(type)이 달라야 한다.
    
    하지만 반환형이 다른 경우는 오버로딩이 성립하지 않는다.
    
    ```java
    int simpleMethod() {...}
    double simpleMethod() {...}
    ```
    
- 애매한 상황
    
    다음과 같은 클래스가 있다.
    
    ```java
    class AAA {
    	void simple(int p1, int p2) {...}
    	void simple(int p1, double p1) {...}
    }
    ```
    
    그런데 다음과 같이 선언이 되었다.
    
    ```java
    AAA inst = new AAA();
    inst.simple(7, 'K');
    ```
    
    위의 호출 애매하다. 이유는 메소드의 인자 전달 과정에서 발생하는 형 변환 때문이다. 사실 AAA 클래스에서 정수 10과 문자 ‘K’를 인자로 전달받는 simple 메소드가 정의되어 있지 않다. 때문에 자동 형 변환 규칙을 적용했을 때 호출이 가능하다는데 있다. 그래서 위의 메소드 호출은 애매하다고 하는 것이다. 자동 현 변환 규칙을 적용하되 가장 가까운 위치에 놓여있는 자료형으로의 형 변환을 우선 시도한다. 때문에 위의 메소드 호출문에 의해 호출되는 메소드는 다음과 같다.
    
    ```java
    void simple(int p1, int p2) {...}
    ```
    
    오버로딩 된 메소드를 호출할 때는 전달인자의 자료형과 매개변수의 자료형을 일치시키는 것이 좋다. 그래서 애매한 상황을 만들지 않는 것이 좋다.
    
- 생성자도 오버로딩 대상이 됩니다.
    
    ```java
    class Person {
        private int regiNum;     // 주민등록 번호
        private int passNum;     // 여권 번호
        
        Person(int rnum, int pnum) {
            regiNum = rnum;
            passNum = pnum;
        }
        
        Person(int rnum) {
            regiNum = rnum;
            passNum = 0;
        }
        
        void showPersonalInfo() {
            System.out.println("주민등록 번호: " + regiNum);
            
            if(passNum != 0)
                System.out.println("여권 번호: " + passNum + '\\n');
            else
                System.out.println("여권을 가지고 있지 않습니다. \\n");
        }
    }
    
    class ConOverloading {
        public static void main(String[] args) {
            // 여권 있는 사람의 정보를 담은 인스턴스 생성
            Person jung = new Person(335577, 112233);
            
            // 여권 없는 사람의 정보를 담은 인스턴스 생성
            Person hong = new Person(775544);
    
            jung.showPersonalInfo();
            hong.showPersonalInfo();
        }
    }
    ```
    
    위의 person클래스에 정의된 두 생성자는 다음과 같다. 생성자의 매개변수 선언이 다르므로 이 둘은 오버로딩 관계에 있다.
    
- 키워드 this를 이용한 다른 생성자의 호출
    
    앞서 봤던 생성자를 다시 자세히 보자
    
    ```java
    class Person {
        private int regiNum;     // 주민등록 번호
        private int passNum;     // 여권 번호
        
        Person(int rnum, int pnum) {
            regiNum = rnum;
            passNum = pnum;
        }
        
        Person(int rnum) {
            regiNum = rnum;
            passNum = 0;
        }
    	...
    }
    ```
    
    이 중에 두번째 생성자를 다음과 같이 표현 할 수 있다.
    
    ```java
    Person(int rnum, int pnum) {
    	this(rnum, 0); // rnum과 0을 인자로 받는 다른 생성자를 호출
    }
    ```
    
    위에 사용된 this는 ‘오버로딩 된 다른 생성자’를 의미한다.
    
    결구 위와 같이 생성자를 정의하면 이 생성자는 초기화할 값을 전달받는 역할만 하고, 실제 초기화는 첫 번째로 정의된 생성자를 통해서 진행하는 형태가 된다.
    
- 키워드 this를 이용한 인스턴스 변수의 접근
    
    ```java
    class SimpleBox {
        private int data;
        
        SimpleBox(int data) {
            this.data = data;
        }
        
        void setData(int data) {
            this.data = data;
        }
    
        int getData() {
            return this.data;
        }
    }
    
    class ThisInst {
        public static void main(String[] args) {
            SimpleBox box = new SimpleBox(99);
            System.out.println(box.getData());
            
            box.setData(77);
            System.out.println(box.getData());
        }
    }
    ```
    
    클래스 내부를 보면 인스턴스 변수로 data가 선언되어있다.
    
    만약에 생성자안에 매개변수가 data가 된다면 문제가 생긴다.
    
    ```java
    class SimpleBox {
        private int data;
        
        SimpleBox(int data) {
    			// 이 안에 선언된 data는 전부 매개변수 data를 의미한다.
        }
    ```
    
    인스턴스 메소드 내에서, 그리고 생성자 내에서 data라는 이름의 변수에 접근하면, 이는 변수 data의 접근으로 이어진다. 위와 같이 동일한 매개변수의 이름이 인스턴스 변수의 이름과 동일하게 선언된 경우, 선언된 지역 내에서의 해당 이름은 매개변수를 의미하게 된다. 하지만 this 키워드를 이용하게 된다면 이 영역 안에서도 인스턴스 변수에 접근하게 된다.
    
    ```java
    class SimpleBox {
        private int data;
        
        SimpleBox(int data) {
    			// 여기서의 this.data는 인스턴스 변수 data를 의미한다.
        }
    ```
    
    즉 this.data에서 this가 의미하는 것은 ‘이 문장이 속한 인스턴스’이다. 따라서 this.data는 인스턴스 변수 data를 의미한다. 따라서 다음과 같은 생성자의 정의가 가능하다.
    
    ```java
    class SimpleBox {
    	private int data;
    	SimpleBox(int data) {
    	 this.data = data;
    	}
    }
    ```
    

## String 클래스

- String 클래스의 인스턴스 생성
    
    문자열 표현을 위한 String 인스턴스의 생성 방법은 다음과 같다. 일반적인 인스턴싀 생성 방법과 차이가 없다.
    
    ```java
    String str = new String("Simple Java");
    ```
    
    이렇게 인스턴스가 생성되면 str이 참조하는 String 인스턴스의 내부에는 문자열 “Simple Java”이 담기게 되고, 이는 다음과 같이 출력하여 그 내용을 확인할 수 있다.
    
    ```java
    System.out.println(str);
    ```
    
    지금까지 많이 호출해왔던 System.out.println 메소드는 다음과 같이 정의되어 있기 때문에 String 인스턴스의 참조 값이 인자로 전달 가능하다.
    
    ```java
    public void println(String s) {...}
    ```
    
    그리고 다음과 같이 생성할 수 도있다. new를 이용한 방법보다 보편적인 String 인스턴스의 생성 방법이다.
    
    ```java
    String str = "Simple String";
    ```
    
    다음 예제 코드를 보자.
    
    ```java
    class StringInst {
        public static void showString(String str) {
            System.out.println(str);
            System.out.println(str.length());
        
        }
    
        public static void main(String[] args) {
            String str1 = new String("Simple String");
            String str2 = "The Best String";
            
            System.out.println(str1);
            System.out.println(str1.length());
            
            System.out.println();
    
            System.out.println(str2);
            System.out.println(str2.length());
    
            System.out.println();
            
            showString("Funny String");
        }
    }
    ```
    
    예제에서는 다음 두 가지의 String 인스턴스 생성 방법을 보였다.
    
    ```java
    String str1 = new String("Simple String");
    String str2 = "The Best String";
    ```
    
    그리고 String 클래스에 다음과 같이 정의된 lenth 메소드를 호출하여 문자열의 길이를 반환하고 이를 호출하였다.
    
    ```java
    public int length() {...} // 문자열 길이를 반환한다.
    ```
    
    그런데 여기서 의문점이 있다. length 반환 값이 어떻게 println 메소드의 인자가 될 수 있을까? 메소드 println은 다음과 같이 오버로딩이 되어 있기 때문에 가능하다. 특히 인자를 전달하지 않고도 호출이 가능한데 이럴 경우 단순히 “개 행”을 하게 된다.
    
    ```java
    void println() {...}
    void println(int x) {...}
    void println(String x) {...}
    ```
    
    pritnln 메소드는 보다 다양한 인자를 전달받을 수 있도록 오버로딩 되어 있다. 아까 코드를 보았을 때 문잘이 아닌 “Funny String”을 대상으로 만들어진 String 인스턴의 참조 값이 전달이 된다.
    
    ```java
    showString("Funny String")'
    ```
    
    위의 문장이 실행되면 일단 “Funny String”을 대상으로 String 인스턴스가 만들어진다. 그리고 이어서 이 인스턴스의 참조 값이 문자열 선언을 대신하게 된다. 예를 들어서 위의 문자열 선언을 통해 생성된 인스턴스의 참조 값이 0x1234라고 하면, 위의 문장은 메소드 호출 전(String 인스턴스 생성 후) 다음과 같은 상황에 놓이게 된다.
    
    ```java
    showString("Funny String");
    -> showString(0x1234);
    ```
    
    그래서 다음과 같이 매개변수 선언이 String형 참조변수로 선언된 메소드는 문자열을 인자로 전달 받을 수 있다.
    
    ```java
    public static void showString(String str) {...}
    ```
    
    끝따옴표롤 묶인 문자열로 선언만으로도 String 인스턴스 생성된다.
    
- 문자열 생성을 위한 두 가지 방법의 차이점은?
    
    ```java
    String str1 = new String("My String");
    String str2 = "My String";
    ```
    
    다음은 예제 코드이다.
    
    ```java
    class ImmutableString {
        public static void main(String[] args) {
            String str1 = "Simple String";
            String str2 = "Simple String";
            
            String str3 = new String("Simple String");
            String str4 = new String("Simple String");
            
            if(str1 == str2)
                System.out.println("str과 str2는 동일 인스턴스 참조");
            else
                System.out.println("str1과 str2는 다른 인스턴스 참조");
    
            if(str3 == str4)
                System.out.println("str3과 str4는 동일 인스턴스 참조");
            else
                System.out.println("str3과 str4는 다른 인스턴스 참조");
        }
    }
    ```
    
    결과
    
    ```
    str과 str2는 동일 인스턴스 참조
    str3과 str4는 다른 인스턴스 참조
    ```
    
    위의 == 연산은 ‘참조변수의 참조 값’에 대한 비교 연산을 진행한다. 예를 들어서 아래 코드를 실행 할 경우 == 연산의 결과로 true가 출력된다.
    
    ```java
    public static void main(String[] args) {
    	AAA inst1 = new AAA();
    	AAA inst2 = inst1; // 두 참조변수는 동일 인스턴스 참조
    	System.out.println(inst1 == inst2); // 같은 인스턴스 참조하므로 true 출력
    }
    ```
    
    반면 다음의 코드를 실행할 경우 == 연산의 결과로 false가 출려된다.
    
    ```java
    public static void main(String[] args) {
    	AAA inst1 = new AAA();
    	AAA inst2 = new AAA();
    	System.out.println(inst1 == inst2); // 다른 인스턴스 참조하므로 false 출력
    }
    ```
    
    같은 인스턴스
    
    ```java
    String str1 = "Simple String";
    String str2 = "Simple String";
    ```
    
    다른 인스턴스
    
    ```java
    String str3 = new String("Simple String");
    String str4 = new String("Simple String");
    ```
    
    이런한 차이점을 보이는 이유는 무엇일까? 가장 핵심이 되는 이유는 다음과 같다.
    
    “String 인스턴스 Immutable 인스턴스입니다.”
    
    사전적으로는 Immutable은 “변결할 수 없는”의 뜻을 지닌다. 그리고 String 인스턴스에서 변경할 수 없는 것은 인스턴스가 갖는 문자열 내용이다. 예를 들어서 다음과 같이 String 인스턴스를 생성하면,
    
    ```java
    String str = "AtoZ";
    ```
    
    이때 생성된 인스턴스는 내부에 문자열 “AtoZ”을 지니게 되고 이 내용은 인스턴스가 소멸할 때까지 바꿀 수 없다. 따라서 다음과 같이 생성하게 되면
    
    ```java
    String str1 = "AtoZ";
    String str2 = "AtoZ";
    ```
    
    “문자열 내용이 같기 때문에” 다음과 같이 하나의 인스턴스를 생성해서 이를 공유하는 방식으로 코드를 처리한다.
    
    ```java
    String str1 = "AtoZ";
    String str2 = str1;
    ```
    
    이렇게 하나의 인스턴스를 공유해도 문제가 되지 않을까? 대부분의 경우 문제가 되지 않는다. 이유는 String 인스턴스는 그 안에 저장된 데이터를 수정할 수 없는, 참조만 가능한 인스턴스이기 때문이다.
    
- String 인스턴스를 이용한 switch문의 구성
    
    ```java
    public static void main(Stringp[] args) {
    	String str = "two";
    	
    	switch(str) {
    		case "one":
    			System.out.println("one");
    			break;
    		case "two":
    			System.out.println("two");
    			break;
    		default:
    			System.out.println("default");
    	}
    }
    ```
    
    switch문의 구성이 일반적이지는 않으나, 각 case 영역을 설명하는 문장을 구성할 수 있다는 측면에서 긍정적인 부분도 존재한다.
    

## String 클래스의 메소드

- 문자열 연결시키기: Concatenating
    
    String 클래스의 다음 메소드를 이용하면 두 문자열을 연결시킨 문자열을 결과로 얻을 수 있다.
    
    ```java
    public String concat(String str)
    ```
    
    이 메소드의 사용 방법은 다음과 같다.
    
    ```java
    String1.concat(String2); // String1과 String2을 연결한 결과를 반환
    ```
    
    위와 같이 호출되면 String1, String2가 갖는 문자열을 연결한 새로운 문자열이 만들어진다. 이렇게 만들어진 인스턴스의 참고 값이 반환된다.
    
    ```java
    class StringConcat {
        public static void main(String[] args) {
            String st1 = "Coffee";
            String st2 = "Bread";
            
            String st3 = st1.concat(st2);
            System.out.println(st3);
    
            String st4 = "Fresh".concat(st3);
            System.out.println(st4);
        }
    }
    ```
    
    다음과 같은 메소드 호출이 가능한 이유는 큰따옴표를 이용한 문자열 표현은 String 인스턴스의 생성으로 이어지고, 이 문자열의 위치에, 생성된 인스턴스의 참조 값이 반환된다. 따라서 위와 같은 형태의 문자 구성이 가능하다.
    
    ```java
    String st4 = "Fresh".concat(st3);
    ```
    
- 문자열의 일부를 추출하기
    
    String 클래스의 다음 메소드를 이용하면 문자열의 뒷부분을 별도의 문자열로 추출할 수 있다.
    
    ```java
    public String subString(int beginIndex) // beginIndex ~ 끝까지 추출
    ```
    
    ```java
    String str = "abcdefg";
    ```
    
    메소드 subString의 인자로 전달되는 숫자는 ‘인덱스 값’이다. 그래서 다음과 두 개의 인자를 전달 받는 메소드가 정의되어 있으며, 이 메소드를 호출하여 문자열의 중간 부분을 추출할 수 있다.
    
    ```java
    public String subString(int beginIndex, int endIndex) // beginIndex ~ endIndex 사이 추출
    ```
    
    메소드 사용법
    
    ```java
    Sting str = "abcdefg";
    str.substring(2, 4); // 인덱스 2 ~ 3에 위치한 내용의 문자열 반환
    ```
    
- 문자열의 내용 비교
    
    ```java
    public boolean equals(Object anObject)
    ```
    
    윗 메소드를 사용하면 두 개의 String 인스턴스가 지니는 문자열의 내용을 비교할 수 있다.
    
    메소드 사용법
    
    ```java
    String str = "my house";
    boolean isSame = str.equals("my house");
    ```
    
    두 문자열을의 내용 비교를 ‘사전 편찬 순서’를 기준으로 조금 더 구체적으로 진행하고 싶다면 다음 메소드의 호출을 고료할 수 있다.
    
    ```java
    public int compareTo(String anotherString)
    ```
    
    이 메소드는 ‘사전 편창 상’ 순서를 비교한다.
    
    - 두 무자열의 내용이 일치하면 0을 반환
    - 호출된 인스턴스의 문자열이 인자로 전달된 문자열보다 앞서면 0보다 작은 값 반환
    - 호출된 인스턴스의 문자열이 인자로 전달된 문자열보다 뒤서면 0보다 작은 값 반환
    
    따라서 코드를 작성할 때에도, 인자로 전달된 문자열이 사전 편차 순서상 뒤에 위치하면 ‘0보다 작은 값’이 반환된다는 사실에 근거해서 코드를 작성해야 한다.
    
    ```java
    class CompString {
        public static void main(String[] args) {
            String st1 = "Lexicographically";
            String st2 = "lexicographically";
            int cmp;
    
            if(st1.equals(st2))
                System.out.println("두 문자열은 같습니다.");
            else
                System.out.println("두 문자열은 다릅니다.");
    
            cmp = st1.compareTo(st2);
    
            if(cmp == 0)
                System.out.println("두 문자열은 일치합니다..");
            else if (cmp < 0)
                System.out.println("사전의 앞에 위치하는 문자: " + st1);
            else
                System.out.println("사전의 앞에 위치하는 문자: " + st2);
    
            
            if(st1.compareToIgnoreCase(st2) == 0)
                System.out.println("두 문자열은 같습니다.");
            else
                System.out.println("두 문자열은 다릅니다.");
        }
    }
    ```
    
- 기본 자료형의 값을 문자열로 바꾸기
    
    ```java
    static String valueOf(boolean b)
    static String valueOf(char c)
    static String valueOf(double d)
    static String valueOf(float f)
    static String valueOf(int i)
    static String valueOf(long l)
    ```
    
    이 메소드를 호출하면 기본 자료형의 값을 문자열로 바꿀 수 있다.
    
    이 메소드의 사용 방법은 다음과 같다.
    
    ```java
    double e = 2.2143546576;
    String se = String.valueOf(e);
    ```
    
- 문자열로 대상으로 하는 + 연산과 += 연산
    
    다음과 같은 형태로 문자열을 연결하였다.
    
    ```java
    System.out.println("funny" + "camp"); // 문자열 + 문자열
    ```
    
    +연산이 가능한 이유는 컴파일러가 + 연산이 다음과 같이 concat메소드의 호출로 바뀌기 때문이다.
    
    ```java
    System.out.println("funny".concat("camp"));
    ```
    
    다음과 같이 += 연산도 사용할 수 있다.
    
    ```java
    String str = "funny";
    str += "camp";
    ```
    
    그렇다면 다음과 같은 문장도 될까?
    
    ```java
    String str = "age :" + 17;
    ```
    
    일단 된다 그러면 다음 문장을 보자
    
    ```java
    String str = "age :".concat(17);
    ```
    
    이것은 컴파일 오류가 난다. concat 메소드는 String 인스턴스의 참조값을 인자로 요구하기 때문이다. 따라서 위의 문장을 대신해 다음과 같이 처리가 된다.
    
    ```java
    String str = "age : ".concat(String.valueOf(17));
    ```
    
- concat 메소드는 이어서 호출이 가능하다.
    
    ```java
    String str = "AB".concat("CD").concat("EF");
    //		-> str = "ABCDEF";
    ```
    
    위 호출이 가능한 이유는 무엇일까? 이에 대한 이해를 돕기 위해 위 문장에 소괄호를 추가하면 다음과 같다.
    
    ```java
    String str = ("AB".concat("CD")).concat("EF");
    ```
    
    즉, 왼편에 위치한 concat 메소드가 먼저 호출되고, 그 결과로 문자열 “ABCD”가 만들어져 다음의 상태가 된다.
    
    ```java
    String str = "ABCD".concat("EF");
    ```
    
    참고로 다음과 같은 빈번한 생성은 자바의 성능에 좋지 않은 영향을 준다.
    
- 문자열 결합의 최적화
    
    ```java
    String birth = "<양>" + 7 + '.' + 16;
    ```
    
    이 코드는 컴파일 했을때 다음과 같이 된다.
    
    ```java
    String birth = 
    "<양>".contcat(String.valueOf(7)).concat(String.valueOf('.')).concat(String.valueOf(16));
    ```
    
    기본 자료형의 값을 문자열로 반환하는 과정을 여러번 거쳐야 하는 문제가 있다.
    
    이러한 문제점을 위해서 `StringBuilder` 이 제공된다.
    
    ```java
    String birth = "<양>" + 7 + '.' + 16;
    -> String birth = (new StringBuilder("<양>").append(7).append('.').append(16)).toString();
    ```
    
    위의 방법으로 구성하면 기본 자료형의 값을 문자열로 변환할 필요가 없다. 그리고 concat 메소드는 호출될 때마다 새로운 String 인스턴스를 생성하는데, 위의 경우에는 그러한 인스턴스의 생성도 일어나지 않는다. 그럼 위 문장의 이해를 위해 StringBuilder 클래스를 살펴보자.
    
- StringBuilder 클래스
    
    StringBuilder 클래슨는 내부적으로 문자열을 저장하기 위한 메모리 공간을 지닌다. 그리고 이 메모리 공간은 String 클래스의 메모리 공간과 달리 문자를 추가하거나 삭제하는 것이 가능하다. 따라서 수정하면서 유지해야 할 문자열이 있다면 이 클래스에 그 내용을 담아서 관리하는 것이 효율적이다.
    
    StringBuilder 클래스의 대표적인 메소드
    
    ```java
    public StringBuilder append(int i)
    -> 기본 자료형 데이터를 문자열 내용에 추가
    ```
    
    ```java
    public StringBuilder delete(int start, int end)
    -> 인덱스 start에서부터 end 이전까지의 내용을 삭제
    ```
    
    ```java
    public StringBuilder insert(int offset, String str);
    -> 인덱스 offset의 위치에 str에 전달된 문자열 추가
    ```
    
    ```java
    public StringBuilder replace(int start, int end, String str)
    -> 인덱스 start에서부터 end이전까지의 내용을 str의 문자열로 대체
    ```
    
    ```java
    public StringBuilder reverse()
    -> 저장된 문자열의 내용을 뒤집는다.
    ```
    
    ```java
    public String subString(int start, int end)
    -> 인덱스 start에서부터 end 이전까지의 내용만 담은 String 인스턴스의 생성 및 반환
    ```
    
    ```java
    public String toString()
    -> 저장된 문자열의 내용을 담은 String 인스턴스의 생성 및 반환
    ```
    
    이 중에서 몇몇 메소드는 다영한 인자를 받을 수 있게 오버로딩 되어있다. 특히 append 메소드는 다음과 같이 매우 다양하게 오버로딩 되어있다.
    
    ```java
    StringBuilder append(boolean b)
    StringBuilder append(char c)
    StringBuilder append(double d)
    StringBuilder append(float f)
    StringBuilder append(int i)
    StringBuilder append(long lng)
    StringBuilder append(Object obj)
    StringBuilder append(String str)
    ```
    
    StringBuilder 인스턴스 내부에는 문자열 관리를 위한 메모리 공간이 존재하는데, 이 공간의 크기를 인스턴스 생성 과정에서 다음과 같이 지정할 수 있다.
    
    ```java
    StringBuilder stbuf = new StringBuilder(64);
    -> 생성자의 인자로 전달된 숫자의 크기만큼 문자를 저장할 공간 마련
    ```
    
    물론 StringBuilder 인스턴스는메모리 공간을 스스로 관리한다. 즉 부족하면 공간을 늘린다. 그러나 이는 소모가 많은 작업이다. 따라서 사용계획에 따라 적절한 크기를 초기에 만들면 그만큼의 성능 향상을 기대할 수 있다.
    
    ```java
    public StringBuilder()
    -> 16개의 문자를 저장할 수 있는 메모리 공간 확보
    ```
    
    ```java
    public StringBuilder(int capacity)
    -> capacity개의 문자를 저장할 수 있는 메모리 공간 확보
    ```
    
    ```java
    public StringBuilder(String str)
    -> 전달되는 문자열과 16개의 문자를 저장할 수 있는 메모리 공간 확보
    ```
    
    ```java
    public StringBuilder append(int i)
    public StringBuilder delete(int start, int end)
    public StringBuilder insert(int offset, String str)
    ```
    
    위의 메소드들은 무엇을 반환하는 걸까?
    
    ```java
    class ReturnStringBuilder {
        public static void main(String[] args) {
            StringBuilder stb1 = new StringBuilder("123");
            StringBuilder stb2 = stb1.append(45678);
            
            System.out.println(stb1.toString());
            System.out.println(stb2.toString());
    
            stb2.delete(0, 5);
    
            System.out.println(stb1.toString());
            System.out.println(stb2.toString());
    
            if(stb1 == stb2)
                System.out.println("같은 인스턴스 참조");
            else
                System.out.println("다른 인스턴스 참조");
        }
    }
    ```
    
    ```
    12345678
    12345678
    678
    678
    같은 인스턴스 참조
    ```
    
    실행 결과는 다음 문장에서 append 메소드가 반화하는 것은 append 메소드가 호출된 인스턴스의 참조 값 임을 알려준다.
    
    ```java
    StringBuilder stb2 = stb1.append(45678); // 이 경우 stb1의 참조 값이 반환된다.
    ```
    
- StringBuilder 클래스 이전에 사용된 StringBuffer 클래스
    
    이전에는 StringBuffer 클래스가 사용되었다.
    
    StringBuffer과 StringBuilder의 공통점
    
    - 생성자를 포함한 메소드의 수
    - 메소드의 기능
    - 메소드의 이름과 매개변수의 선언
    
    이 세가지가 일차한다는 것은 사실상 같은 클래스임을 의미한다. 하지만 차이점은 존재한다.
    
    “StringBuffer는 쓰레드에 안전하지만, StringBuilder는 쓰레드에 안전하지 않습니다.”
    
    StringBuffer 클래스 라는 것은 멀티 쓰레드 환경에서 안전하게 동작하도록 만들어 졌다. 그런데 이렇게 만들어지면 속도가 느리다.
    
    “멀티 쓰레드에 안전하게 설계된 StringBuffer 클래스는 속도가 느리다.”
    
    그래서 멀티 쓰레드와 상관없는 상황에서는 StringBuilder 클래스를 정의하기에 이른다.
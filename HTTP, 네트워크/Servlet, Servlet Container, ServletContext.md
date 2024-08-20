# 1. Servlet (서블릿) 이란

**클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술**

### 특징
- 클라이언트의 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
- html을 사용하여 요청에 응답한다.
- Java Thread를 이용하여 동작한다.
- MVC 패턴에서 Controller로 이용된다.
- HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받는다.
- UDP보다 처리 속도가 느리다.    
- HTML 변경 시 Servlet을 재컴파일해야 하는 단점이 있다.

일반적인 웹서버는 정적인 페이지만 제공해준다. 동적인 페이지를 제공하기 위해서는 웹서버는 다른 곳에 도움을 요청하여 동적인 페이지를 작성해야 한다. 
동적인 페이지로는 임의의 이미지만을 보여주는 페이지와 같이 사용자가 요청한 시점에 페이지를 생성해서 전달해주는 것을 의미한다.
여기서 웹서버가 동적인 페이지를 제공할 수 있도록 도와주는 어플리케이션이 서블릿, 동적인 페이지를 생성하는 어플리케이션이 CGI이다.

Servlet 동작 방식
![동작방식](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F993A7F335A04179D20)
1. 사용자가 URL을 입력하면 HTTP REQUEST가 Servlet Container로 전송
2. 요청을 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성
3. web.xml을 기반으로 사용자가 요청한  URL이 어는 서블릿에 대한 요청인지 찾는다.
4. 해당 서블릿에서 service메소드를 호출 클라이언트의 GET, POST여부에 따라 doGet(), doPost()를 호출
5. doGet() or doPost() 메서드는 동적 페이지를 생성한 후 HttpServletResponse 객체에 응답을 보낸다.
6. 응답이 끝내면 HttpServletRequets, HttpServletResponse 두 객체를 소멸

### CGI란?
특별한 라이브러리 도구를 의미를 하는 것이 아닌, 별도로 제작된 웹서버와 프로그램간의 교한 방식이다.

CGI방식은 어떠한 프로그래밍언어로도 구현이가능하며, 별도로 만들어 놓은 프로그램에 HTML의 Get or Post 방법으로 클라이언트의 데이터를 환경변수로 전달하고, 프로그램의 표준 출력 결과를 클라이언트에게 전송하는 것입니다.

즉, 자바 어플리케이션 코딩을 하듯 웹 브라우저용 출력 화면을 만드는 방법

# 2. Servlet Container(서블릿 컨테이너)
한마디로 하면 서블릿을 관리하는 컨테이너라고 한다.
서블릿은 스스로 작동하는 것이 아닌 서블릿을 관리해주는 것이 필요하다. 그것이 서블릿 컨테이너이다.
서블릿 컨테이너는 클라이언트의 요청(request)을 받아주고 응답(response)할 수 있게, 웹서버와 소켓으로 통신한다. 대표적인 예로 스프링부트에서 기본적으로 쓰이는 톰캣이 있다.

### 역할
1. 웹서버와의 통신 지원
   서블릿 컨테이너는 서블릿과 웹서버가 손쉽게 통신할 수 있게 해준다. 일반적으로 우리는 소켓을 만들고 lister, accept 등을 해야되지만 서블릿 컨테이너는 이런한 기능을 api를 제공하여 복잡한 과정을 생략할 수 있게 해준다. 그래서 개발자가 서블릿에 구현해야 할 비지니스 로직에 대해선만 집중할 수 있다.
2. 서블릿 생명주기(Life Cycle) 지원
   서블릿 컨테이너는 서블릿의 탄생과 죽음을 관리해준다. 서블릿 클래스를 로딩하여 인스턴스화하고, 초기화 메서드를 호출하고, 요청이 들어오면 서블릿 메서드를 호출한다.
   또한 서블릿이 생명을 다 한 순간에는 적절하게 Garbage Collection(가비지 컬렉션)을 진행하여 편의를 제공
3. 멀티쓰레드 지원 및 관리
   서블릿 컨테이너는 요청이 올 때 마다 새로운 자바 쓰레드를 하나 생성하는데, HTTP 서비스 메소드를 실행하고 나면, 쓰레드는 자동으로 죽게됩니다. 원래는 쓰레드를 관리해야 하지만 서버는 다중 쓰레드를 생성 및 운영해주니 쓰레드의 안정성에 대해서 걱정하지 않아도 된다.
4. 선언적인 보안 관리
   서블릿 컨터이너를 사용하면 개발자는 보안에 관련된 내용을 서블릿 또는 자바 클래스에 구현해 놓지 않아도 된다. 일반적으로 보안관리는 xml 배포 서술자에 다가 기록한다. 보안에 대해 수정할 일이 생겨도 자바 소스 코드를 수정하여 다시 컴파일 하지 않아도 보안관리가 가능합니다.
### Servlet 생명주기
![생명주기](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F991870335A04292F0B)
1. 클라이언트의 요청이 들어오면 컨테이너는 해당 서블릿이 메모리에 있는 확인, 없을 경우 init() 메서드를 호출하여 적재합니다. init() 메소드는 처음 한번만 실행되기 때문에, 서블릿의 쓰레드에서 공톡적으로 사용해야하는 것이 있다면 오버라이딩하여 구현하면 됩니다. 실행 중 서블릿이 변경될 경우, 기본 서블릿을 파괴하고 init()을 통해 새로운 내용을 다시 메모리에 적재합니다.
2. init()이 호출된 후 클라이언트의 요청에 따라서 service() 메소드를 통해 요청이 대한 응답이 doGet()가 doPost()로 분기됩니다. 이때 서블릿 컨테이너가 클라이언트의 요청이 오면 가장 먼저 처리하는 과정으로 생성된 HtppServletRequest, HttpServletResponse에 의해 request와 response객체가 제공
3. 종료 요청을 하면 destory()메소드가 호출되는데 마찬가지로 한번만 실행되며, 종료시에 처리해야하는 작업들을 destory() 메소드를 오버라이딩하여 구현하면 됩니다.
# 3. ServletContext (서블릿 컨텍스트)
서블릿 컨텍스트란 하나의 서블릿이 서블릿 컨테이너와 통신하기 위해서 사용되어지는 메서드들을 가지고 있는 클래스가 바로 ServletContext이다. 
![서블릿 컨테스트](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4yCIH%2FbtrkjXCBdmd%2F23lPq261gtL6hjx1u2AIFk%2Fimg.png)
서블릿 컨테이너는 말 그대로 서블릿들을 보관하는 그릇(컨테이너)라고 생각하면 된다.
즉, web application내에 있는 모든 서블릿들을 관리하며 정보 공유할 수 있게 도와 주는 역할을 담당하는 것이 바로 **ServletContext**다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoXx2P%2FbtrkjXbwMwz%2FFlLyfDeValXnlRzO4iPwp0%2Fimg.png)

# **Application Context(Spring)**

스프링에서 오브젝트의 생성과 관계 설정, 사용, 제거 등의 작업을 애플리케이션에서 개발자가 코드를 직접 구현하는 대신 독립된 컨테이너가 담당하는데 이를 컨테이너가 제어권을 가지고 있다고 해서 IoC라고 부릅니다.

그리고 스프링에서 IoC를 담당하는 컨테이너를 `빈 팩토리 또는 애플리케이션 컨텍스트`라고 부릅니다.
스프링 컨테이너는 DI작업보다 더 많은 일을 하고 이에 해당하는 필요한 여러가지 컨테이너 기능을 추가한 것을 `애플리케이션 컨텍스트라`고 부른다.

실제 ApplicationContext는 인터페이스고, 실제 스프링 컨테이너는 이 ApplicationContext를 구현한 구현체를 말하게 된다.
그래서 애플리케이션 컨텍스트가 스프링 애플리케이션을 만들기 위해 어떤 일을 하는지를 보면

### **POJO 클래스**
POJO 클래스라는 것은 객체지향적인 원리에 충실하면서, 환경과 기술에 종속되지 않고 필요에 따라 재활용 될 수 있는 방식으로 설계된 오브젝트를 말한다.

### **설정 메타정보**
POJO 클래스들 중에 애플리케이션에서 사용할 것을 선정하고 이를 IoC 컨테이너가 제어할 수 있도록 적절한 메타정보를 만들어 제공하는 작업이다.

IoC의 컨테이너의 가장 기초적인 역할은 오브젝트를 생성하고 관리하는 것이고 IoC컨테이너가 관리하는 객체를 빈이라고 설정 메타정보는 이 빈을 어떻게 만들고 어떻게 동작할 것인가에 대한 정보이다.

스프링 설정 메타정보는 BeanDefinition 인터페이스로 표현되는 순수한 추상 정보고 애플리케이션 컨텍스트는 이 BeanDefinition으로 만들어진 메타정보를 담은 오브젝트를 사용해서 IoC와 DI작업을 수행한다고 합니다.

그래서 xml, 애너테이션, 자바코드, 프로퍼티 파일이든 상관없이 BeanDefinition으로 정의되는 스프링의 설정 메타정보의 내용을 표현한 것이면 무엇이든 사용 할 수 있는 것 입니다.

 여러 설정 파일들을 BeanDefinition으로 읽어주는게 BeanDefinitionReader라는게 있고 이거를 구현해서 BeanDefinition 타입으로 정의할 수 있으면 어떤 설정 메타정보는 어떤 형식으로든 작성할 수 있다.
 
 스프링 IoC 컨테이너는 각 빈에대한 정보를 담은 설정 메타정보를 읽어들인 뒤, 이를 참고해서 빈 오브젝트를 생성하고 프로터티나 생성자를 통해 의존 오브젝트를 주입해주는 DI 작업을 수행합니다.
 
 이 작업을 통해 만들어지고, DI로 연결되는 오브젝트들이 모여서 하나의 애플리케이션을 구성하고 동작하게 되는데 이것이 IoC 컨테이너(Application Context)의 역할이 됩니다.
![IoC 컨테이너를 통해 애플리케이션이 만들어지는 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FshuZv%2FbtrkinhK7Sl%2FKnQ4S25sCQizTyvgkQfR41%2Fimg.png)
 결국 스프링 애플리케이션이란 POJO클래스와 설정 메타정보를 이용해서 IoC 컨테이너(Applicaiton Context)가 만들어주는 오브젝트의 조합이라고 합니다.
 
 BeanDefinition에서 정의되는 메타정보는
- 빈 아이디, 이름, 별칭 : 빈 오브젝트를 구분할 수 있는 식별자
- 클래스 or 클래스 이름 : 빈으로 만들 POJO 클래스 또는 서비스 클래스 정보
- 스코프
- 프로퍼티 값 또는 참조
- 생성자 파라미터 값 또는 참조
- 지연 로딩 여부

- 메타정보 리소스 : xml, 애너테이션, properties 등등
- 메타정보 리더 : BeanDefinitionReader라는 인터페이스
- 설정 메타정보: BeanDefinition라는 인터페이스
- POJO 클래스 : 특정 환경이나 규약에 종속되지 않는 재사용이 가능한 자바 오브젝트

# WebApplicationContext(Spring)
WebApplicationContext는 그림과 같이 ApplicationContex를 확장한 인터페이스입니다.
![그림](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fceoir4%2FbtrkiJrkb04%2F9gVqRKN6bsFKUfdgmEY68k%2Fimg.png)
이 WebApplicationContext를 구현한 구현체들이 스프링 애플리케이션에서 가장 많이 사용됩니다.
구현체는 Xml 설정파일을 사용하는 XmlWebApplicationContext, 애너테이션 기반의 설정 리소르를 사용하는 AnnotationConfigWebApplicationContext등이 있습니다.

WebApplicationContext의 사용법을 알려면 IoC 컨테이너를 적용했을 때 애플리케이션을 기동시키는 방법에 대해서 알아야한다.

빈 설정 메타정보를 이용해 빈 오브젝트를 만들고 DI작업을 수행하지만 그것만으로 애플리케이션이 동작하지는 않습니다.

마치 자바 애플리케이션 main() 메서드처럼 어디선가 특정 빈 오브젝트의 메서드를 호출함으로써 애플리케이션이 동작이 되는데요.

보통 이런 기동시키는 역할을 맡은 빈을 사용하려면 IoC 컨테이너에서 요청해서 빈 오브젝트를 가져와야되고 만약 직접 IoC 컨테이너를 셋업햇다면 다음과 같은 코드가 등장한다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeuS0iN%2FbtrkaPNlvch%2FAgY9IbaF0dzy3DtZTkfFC1%2Fimg.png)
적어도 한 번은 IoC 컨테이너에게 요청하면 빈 객체를 가져오고 이 때 getBean()을 사용해서 ApplicationContext안에 있는 객체를 가져온다.

그 이후로도 다시 빈으로 가져오지 않아도 되는데 빈 객체끼리 DI로 서로 연결되어 있으므로 의존관계를 타고 필요한 객체가 호출되면서 애플리케이션이 동작이 된다.

hello.print()를 호출했으면 더 이상 컨테이너의 도움을 받지 않고도 컨테이너 안에 구성된 빈 객체에 의해 애플리케이션이 동작한다.

IoC 컨테이너의 역할은 이렇게 초기에 빈 객체를 생성하고 DI한 후 최초로 애플리케이션을 기동할 빈을 제공해주는 것 까지이다.

그런데 웹 애플리케이션은 동작하는 방법이 다르다. 독립형 자바 프로그램은 자바 Vm에게  main() 메서드를 가진 클래스를 시작 시켜달라고 요총할 수 있지만 웹에서는 main() 메서드를 호출할 방법이 없다.

게다가 사용자도 여러명이 될 수 있고 동시에 웹 애플리케이션을 사용하는데 그래서 웹 환경에서는 main() 메서드 대신 서블릿 컨테이너가 브라우저로부터 오는 HTTP 요청을 받아서 해당 요청에 매핑되어 있는 서블릿을 실행시켜주는 방식으로 동작한다. 서블릿이 일종의 main()의 역할을 하는 셈이다.

### 웹 애플리케이션에서 스프링 스프링 애플리케이션 기동시키는 방법

main() 역할을 하는 서블릿을 만들어두고, 미리 애플리케이션 컨텍스트를 생성해둔 다음, 요청이 서블릿으로 들어올 때마다 getBean()으로 필요한 빈을 가져와서 정해진 메서드를 실행시키면 된다.
![웹 환경에서 스프링 애플리케이션이 기동하는 방식](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbtb9s6%2FbtrkcqGz40r%2FTbtl9Wc6S72KzuTe1jaiL0%2Fimg.png)
main() 메서드안에서 애플리케이션 컨텍스트를 만들고 설정 메타정보를 읽어들여서 초기화하고 POJO 빈을 요청하고 메서드를 실행하는 것과 다를게 없다.

단지 main() 메서드나 테스트 메서드에서 했던 작업을 웹 애플리케이션과 그에 소속된 서블릿이 대신 해줄 뿐입니다.

서블릿 컨테이너는 사용자의 요청을 받아서 서블릿을 동작시키는 일을 맡는다.
서블릿은 웹 애플리케이션이 시작될 때 미리 만들어둔 웹 애플리케이션 컨텍스트에게 빈 객체로 구성된 애플리케이션의 기동 역할을 해줄 빈을 요청해서 받아두고 미리 지정된  메서드를 호출함으로써 스프링 컨테이너가 DI방식으로 구성해둔 애플리케이션 기능이 시작되는 것이다.

그런데 사실 저희는 이런 설정들을 한 적이 없는데 잘 동작하는것은 웹 환경에서 애플리케이션 컨텍스트를 생성하고 설정 메타정보로 초기화해주고, 클라이언트로부터 들어오는 요청마다 적절한 빈을 찾아서 이를 실행해주는 기능을 가진 DispathcerServlet이라는 이름의 서블릿을 스프링에서 제공하고 이거를 저희가 사용하기 때문입니다.

또 저희는 DispatcherServlet을 등록한적이 없는데 어떻게 되는건가? 라고 할수 있는데 과거에는 web.xml 파일에 등록했었고 springboot에서는 SpringAutoConfiguration에 의해서 spring.factories안에 있는 DispathcerServletAutoConfiguration이 명시 되어 있는것을 볼 수 있고 이 객체를 통해 DispatcherServlet을 등록하게 된다.
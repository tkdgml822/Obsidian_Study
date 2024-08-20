# ObjectMapper란?
JSON 형식을 사용할 때, 응답들을 직렬화하고 요청들을 역직렬화 할 때 사용하는 기술

## 직렬화 (Serialize)
데이터를 전송하거나 저장할 때 바이트 문자열이어야 하기 때문에 객체들을 문자열로 바꾸어 주는 것
- Object -> String 문자열

## 역직렬화 (Deserialize)
다시 문자열을 기존의 객체로 회복시켜주는 것
- String 문자열 -> Object


스프링에서 @ResponseBody나 @RestController 또는 ResponseEntity을 사용하는 경우에도 사용된다.

## 주의 사항
```java
@Getter
@NoArgsConstructor
@AllArgsConstrucotor
public calss Person {
	private String name;
	private Integer age;

	public String getNameWithAge() {
		return name "(" + age + ")";
	}
}

```

만약에 Person을 직렬화 해버리면 getNameWithAge() 메서드도 같이 직렬화가 되니 주의하자

## 역직렬화 과정
1. 기본 생성자 객체 생성
2. 필드값을 찾아서 값을 바인딩

가장 먼서 객체를 생성한다. 기본 생성자가 없다면 에러를 발생

### 기본 생성자 없이 우회적으로 객체를 만드는 방법
별도의 2가지 작업이 필요하다.
- ObjectMapper에 ParameterNames 모듈추가
- java 컴파일에 -parameters 옵션 추가
parameters 추가
```gradle
implementation group: 'com.fasterxml.jackson.module', name: 'jackson-module-parameter-names'
```
해당 모듈을 ObjectMapper에 등록을 하면 ObjectMapper가 우회적인 방법을 사용할 수 있다. 
```java
ObjectMapper objectMapper = new ObjectMapper();
objectMapper.registerModule(new ParameterNamesModule());
```
parameters 옵션을 추가해주면 JDK8 이상에서는 컴파일 시에 Reflection API로 파라미터 정보를 가지고 올 수 있도록 컴파일된 클래스에 정보를 추가해준다.

## 스프링부트가 제공해주는 기능
그레이들 기반의 스프링 부트로 개발을 하다 보면 기본 생성자가 없는 경우에도 에러 없이 객체가 정상적으로 만들어준다. 그 이유는 SpringBoot가 추가적인 설정과 플러그인을 제공해주기 때문이다.

# DTO 클래스는 항상 다음과 같이 사용해라
위에 내용은 항상 머리에 담기에는 너무 어렵고 신경쓸일이 많다. 다음과 같이 코드를 작성해버리면 우리는 부담없이 DTO에 일관된 방식을 제공해줄 수 있다. 
```java
@Getter
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class MangKyuRequest {
    private String name;
    private Integer age;
}
```
@ModelAttrubute 사용이 필요하면 @Setter까지 넣어주면 된다.

출처: [https://mangkyu.tistory.com/223](https://mangkyu.tistory.com/223) [MangKyu's Diary:티스토리] </br>
출처: [https://escapefromcoding.tistory.com/341](https://escapefromcoding.tistory.com/341)[너도 나도 함계 성장하자:티스토리]

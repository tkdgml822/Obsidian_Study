토큰을 발급받고 JWT을 이용해서 인증을 하려면 HTTP 요청 헤더 중에 Authorization 키값에 **Bearer + JWT 토큰값**을 넣어 보내야 합니다.

그러면 어떠한 구조로 이루어져 있을까요?
JWT는 . 기준으로 헤더(header), 내용(payload, 서명(signature)으로 이루어져 있습니다.
```text
aaaaa.bbbbb.ccccc
```

### 헤더
토큰의 타입과 해싱 알고리즘을 지정하는 정보를 담습니다. 다음의 경우 JWT 토큰, HS256 해싱 알고리즘을 사용한다는 내용입니다.

```JSON
"typ": "JWT",
"alg": "HS256"
```

| 이름  | 설명                         |
| --- | -------------------------- |
| typ | 토큰의 타입을 지정, JWT라는 문자열이 들어감 |
| alg | 해싱 알고리즘을 지정                |
내용에는 토큰과 관련된 값이 들감, 내용의 한 덩어리를 클레임(claim)이라고 부른다. 클레임은 키값의 한 쌍으로 이루어져 있음
클레임은 등록된 클레임, 공개 클레임, 비공개 클레임으로 나눌 수 있다.

등록된 클레임은 토큰에 대한 정보를 담는 사용한다.
공개 클레임은 공개되어도 상관없는 클레임을 의미, 충돌을 방지할 수 있는 이름을 가져야 하며, 보통 클레임 이름을 URL로 짓습니다. 비공개 클레임은 공개되면 안 되는 클레임을 의미한다. 클라이언트와 서버 간의 통신에 사용됩니다. 
```JSON
{
	"iss": "ajufresh@gmail.com", // 등록된 클레임
	"iat": 1622370878, // 등록된 클레임
	"exp": 1622372678, // 등록된 클레임
	"https://shinsunyoung.com/jwt_claims/is_admin": true, // 공개 클레임
	"email": "ajufresh@gmail.com", // 비공개 클레임
	"hello": "안녕하세요!" // 비공개 클레임
}
```

iss, iat, exp는 JWT 자체에서 등록된 크레임이고, URL로 네이밍된  https://shinsunyoung.com/jwt_claims/is_admin은 공개 클레임이다.그 외에 등록된 클레임도, 공개 클레임도 아닌 email과 hello는 비공개 클레임 값입니다. 

서명은 해당 토큰이 조작되었거나 변경되지 않았음을 확인하는 용도로 사용하며, 헤더의 인코딩 값과 내용의 인코딩값을 합친 후에 주저인 비밀키를 사용해 해시값 생성한다.


실제 코드 예시
```java
public class TokenProvider {
	private String makeToken(Date expiry, User user) {  
	    Date now = new Date();  
  
	    return Jwts.builder()  
	            .setHeaderParam(Header.TYPE, Header.JWT_TYPE) 
			    .setIssuer(jwtProperties.getIssuer()) 
	            .setIssuedAt(now) 
	            .setExpiration(expiry) 
	            .setSubject(user.getEmail()) 
	            .claim("id", user.getId())
	            .signWith(SignatureAlgorithm.HS256, jwtProperties.getSecretKey())  
	            .compact();  
	}
}
```
이 실제 코드에서 각각 Header, Payload, Signature 부분에 대해 알아보자.

#### Header
`setHeaderParam()`을 보면 `Header.Type`, `Header.JWT_TYPE`을 정한다.
Header 같은 경우는 인터페이스로 미리 정의 되어 있는 문자열이다. Header 인터페이스에 들어가 보면
```java
public interface Header<T extends <T>> extends Map<String, Object> {
	String JWT_TYPE = "JWT";  
	String TYPE = "typ";  
	String CONTENT_TYPE = "cty";  
	String COMPRESSION_ALGORITHM = "zip";
	// ....
}
```
그러므로 Type 같은 경우는 "typ"이고 JWT_TYPE은  "JWT"이다.
`"typ": "JWT"` 을 정한 것이다.
#### Payload
payload에는 Claim이라는 사용자 또는 코튼에 대한 property를 key-value의 형태로 저장된다.  
한마디로 토큰에서 사용할 사용자 정보의 조각이다.  
*  iss (issuer) : 토큰 발급자  
*  sub (subject) : 토큰 제목 - 토큰에서 사용자에 대한 식별 값이 됨  
*  aud (Audience) : 토큰 대상자  
*  exp (Expiration Time) : 토큰 만료 기간  
*  nbf (Not Before) : 토큰 활성 날짜 (이 날짜 이전의 토큰은 활성화되지 않음을 보장)  
*  iat (Issued At) 토큰 발급 시간  
*  jti (JWT id) : JWT 토큰 식별자 (issuer가 여러 명일 때 이를 구분하기 위한 값)

iss에 해당하는 부분이 코드에서 `setIssuer()`이다.
sub 해당하는 부분은 `setSubject()`이다.
exp는 `setExpiration()`이다.
iat는 `setIssuedAt()`이다.
그런데 코드에서  aud, nbf, jti 부분이 없다.
JWT에서는 payload 내에 총 7가지 표준 속성(property)을 제공하며, 이들은 선택적으로 사용할 수 있다. 선택 사항이지만, 보안을 강화하고 토큰의 유효성을 보장하기 위해 모든 속성을 구현하는 것이 권장된다. 
#### Signature
```java
.signWith(SignatureAlgorithm.HS256, jwtProperties.getSecretKey())
```
알고리즘을 지정하고, 비밀키를 설정합니다. 비밀키는 매우 중요합니다. 나중에 복호화 작업을 할때도 사용하고 사용자들을 구별할때 쓰입니다.
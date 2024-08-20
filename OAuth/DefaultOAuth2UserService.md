# DefaultOAuth2UserService이란
**DefaultOAuth2UserService**은 Spring Security에서 지원해주는 클래스입니다.
특별한 설정없이도 작동되지만, 특정 요구사항을 맞게 사용자 정보를 커스터마이징을 할려면 DefaultOAuth2UserService 상속해서 커스터마이징된 서비스를 구현할 수 있습니다.

```java
public class CustomOAuth2UserService extends DefaultOAuth2UserService {
	@Override
	public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
		OAuth2User oAuth2User = super.loadUser(userRequest);
		// 사용자 정보를 커스터마이징 하는 코드
		return oAuth2User;
	}
}
```

## 실행 시점
  
1. 사용자가 소셜 로그인 버튼 클릭  
2. 사용자 인증 및 권한 승인: 사용자는 소셜 로그인 제공자에게 로그인하고 애플리케이션에 필요한 권한을 승인합니다.  
3. Authorization Code 발급: 사용자가 로그인 권한을 승인하면, 소셜 로그인 제공자는 애플리케이션에 Authorization Code를 발급합니다. 이 코드는 인증된 사용자를 식별하기 위한 코드입니다.  
4. Authorization Code를 사용해 Access Token 요청: Authorization Code를 사용해서 소셜 로그인 제공자에 Access Token을 요청  
5. DefaultOAuth2UserService 실행: 소셜 로그인 제공자가 Access Token 발급 후 Access Token을 사용해 해당 사용자에 대한 프로필 정보를 요청, 이때 DefaultOAuth2UserService가 실행된다.     OAuth2User 객체를 반환한다.  
6. 인증 완료 및 사용자 세션 생성: OAuth2User 객체는 스프링 시큐리티에 의해 사용자의 세션에 저장되어, 이 때 사용자는 애플리케이션 내에서 인증된 상태로 접근할 수 있게 됩니다.
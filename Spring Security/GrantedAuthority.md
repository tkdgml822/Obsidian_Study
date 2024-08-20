# GrantedAuthority
GrantedAuthority는 현재 사용자(Principle)가 가지고 있는 권한을 의미한다. 
`ROLE_*`같은 형태를 가지고 있으면 GrantedAuthority는 UserDetailsService에 의해 불러올 수 있다.
그리고 GrantedAuthority는 인터페이스며 이 인터페이스를 구현하고 있는 구현체는 총 3가지이다.
![구현체](https://velog.velcdn.com/images/lsvk9921/post/b592e472-1a44-446b-8b48-db365998bb27/image.png)
그 중에서 SimpleGrantedAuthority을 살펴보자.
```java
public final class SimpleGrantedAuthority implements GrantedAuthority {
	private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;

	private final String role;

	public SimpleGrantedAuthority(String role) {
		Assert.hasText(role, "A granted authority textual representation is required");
		this.role = role;
	}

	// ....
}
```
String 으로 권한을 넣는 곳이 있다.
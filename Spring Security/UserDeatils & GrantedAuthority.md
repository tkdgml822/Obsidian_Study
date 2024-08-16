# UserDeatils
Spring Security에서 계정 객체를 자바로 정의하기 위해서는 org.springframework.security.core.userdetails.UseraDetails 인터페이스를 이해해야 한다.
이 인터페이스를 구현한 클래스는 Spring Security에서는 사용자라고 보고 작업을 하게 된다.

| return 타입                                | 메소드명                      | 설명                                                |
| ---------------------------------------- | ------------------------- | ------------------------------------------------- |
| String                                   | getUsername()             | 계정의 이름을 리턴합니다.                                    |
| String                                   | getPassword()             | 계정이 만료되지 않았는지를 리턴합니다. (true: 만료되지 않음)             |
| boolean                                  | isAccountNonExpired()     | 계정이 잠겨있지 않은지를 리턴합니다. (true:  계정이 잠겨 있지 않음)        |
| boolean                                  | isAccountNonLocked()      | 계정의 비밀번호가 만료되지 않았는지를 리턴합니다. (true: 패스워드가 만료되지 않음) |
| boolean                                  | isCredentialsNonExpired() | 계정이 사용 가능한지를 리턴합니다 (true: 사용 가능한 계정임)             |
| boolean                                  | isEnabled()               | 계정이 사용 가능한지를 리턴(true: 사용 가능한 계정 임)                |
| `Collection<? extends GrantedAuthority>` | getAuthorities()          | 계정이 가지고 있는 권한 목록을 리턴합니다.                          |
# GrantedAuthority (권한 클래스)

Spring Security에서 권한 객체는 org.springframework.security.core.GrantedAuthority 인터페이스를 구현한 클래스 객체로 만들면 됩니다. 

#### SimpleGrantedAuthority
- 위의 인터페이스를 구현한 클래스 중 하나 입니다.
- 권한을 저장하기 위해서는 저장하고자 하는 권한 문자열 값을 SimpleGrantedAuthority의 생성자 파라미터에 넣어주면 됩니다. 이것으로 권한 객체 생성은 끝이 납니다.


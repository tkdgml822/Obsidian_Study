# **TLS(Transport Layer Security)이란?**

전송 계층 보안(TLS)은 인터넷 상의 커뮤니케이션을 위한 개인 정보와 데이터 보안을 용이하게 하기 위해 설계되어 널리 채택된 보안 [프로토콜](https://www.cloudflare.com/learning/network-layer/what-is-a-protocol/)입니다. TLS의 주요 사용 사례는 웹 사이트를 로드하는 웹 브라우저와 같이 웹 응용 프로그램과 서버 간의 커뮤니케이션을 암호화하는 것입니다. TLS는 또한 이메일, 메시지, [보이스오버 IP(VoIP)](https://www.cloudflare.com/learning/video/what-is-voip/) 등 다른 커뮤니케이션을 암호화하기 위해 사용됩니다. 이 문서에서는 [웹 애플리케이션 보안](https://www.cloudflare.com/learning/security/what-is-web-application-security/)에서 TLS의 역할에 주목하겠습니다.

TLS는 국제 표준 기구인 IETF(Internet Engineering Task Force)에 의해 제안되었으며 프로토콜의 첫 번째 버전은 1999년에 발표되었습니다. 가장 최신 버전은 2018년에 발표된 [TLS 1.3](https://www.cloudflare.com/learning/ssl/why-use-tls-1.3/)입니다.

# **TLS와 SSL의 차이점은 무엇입니까?**

TLS는 Netscape가 개발한 [SSL](https://www.cloudflare.com/learning/ssl/what-is-ssl/)(Secure Sockets Layer)이라고 불리는 이전의 암호화 프로토콜에서 발전한 것입니다. TLS 버전 1.0은 SSL 버전 3.1로서 개발을 시작했지만 Netscape와 더 이상 연관이 없음을 명시하기 위해 발표 전에 프로토콜의 이름이 변경되었습니다. 이러한 역사 때문에 용어 TLS와 SSL은 가끔 서로 바꿔서 사용됩니다.

[HTTPS](https://www.cloudflare.com/learning/ssl/what-is-https/)는 [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) 프로토콜 상위에서 TLS 암호화를 구현한 것으로 모든 웹 사이트와 다른 웹 서비스에서 사용됩니다. 따라서 HTTPS를 사용하는 웹 사이트는 TLS 암호화를 이용합니다.

# 사용 이유

TLS 암호화는 데이터 유출 및 기타 공격으로부터 웹 애플리케이션을 보호하는 데 도움 될 수 도있다. 오늘날 TLS로 보호되는 HTTPS는 웹 사이트의 표준 관행이다.

# 역할

암호화, 인증, 무결성

- 암호화: 제 3자로부터 전송되는 데이터를 숨깁니다.
- 인증: 정보를 교환하는 당사자가 요청된 당사자임을 보장
- 무결성: 데이터가 위조되거나 변조되지 않았는지 확인

# TLS 인증서란?

TLS를 사용하기 위해서는 원본 서버에 TLS 인증서가 설치되어 있어야 합니다(SSL 인증서라고도 함). 인증 기관이 도메인을 소유한 사람 혹은 비즈니스에 TL
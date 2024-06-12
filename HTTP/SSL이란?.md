# SSL 이란?

[암호화](https://www.cloudflare.com/learning/ssl/what-is-encryption/) 기반 인터넷 보안 [프로토콜](https://www.cloudflare.com/learning/network-layer/what-is-a-protocol/)입니다. 인터넷 통신의 개인정보 보호, 인증, 데이터 무결성을 보장하기 위해 Netscape가 1995년 처음으로 개발했습니다. SSL은 현재 사용 중인 [TLS](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/) 암호화의 전신입니다.

SSL/TLS를 사용하는 웹사이트의 URL에는 "[HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/)" 대신 "[HTTPS](https://www.cloudflare.com/learning/ssl/what-is-https/)"가 있습니다.

# 어떻게 작동하나요?

- SSL은 높은 수준의 [개인정보 보호](https://www.cloudflare.com/learning/privacy/what-is-data-privacy/)를 제공하기 위해, 웹에서 전송되는 데이터를 암호화합니다. 따라서, 데이터를 가로채려는 자는 거의 해독할 수 없는 복잡한 문자만 보게 됩니다.
- SSL은 두 통신 장치 사이에 [핸드셰이크](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)라는 **인증** 프로세스를 시작하여 두 장치의 ID를 확인합니다.
- SSL은 또한 **데이터 무결성**을 제공하기 위해 데이터에 디지털 서명하여 데이터가 의도된 수신자에 도착하기 전에 조작되지 않았다는 것을 확인합니다.

# **SSL/TLS는 왜 중요합니까?**

원래 웹 상의 데이터는 메시지를 가로채면 누구나 읽을 수 있는 일반 텍스트 형태로 전송됐습니다. 가령 고객이 쇼핑 웹사이트를 방문하여 주문하고, 신용 카드 번호를 입력했다고 하면, 해당 신용 카드 번호가 숨겨지지 않은 채 인터넷을 이동하게 됩니다.

SSL은 이 문제를 바로잡고, 사용자 개인 정보를 보호하기 위해 제작됐습니다. SSL은 사용자와 웹 서버 사이를 이동하는 모든 데이터를 암호화하여, 누군가 데이터를 가로채더라도 무작위 문자만 볼 수 있게 합니다. 이제 고객의 신용 카드 번호는 안전해졌으며, 고객이 번호를 입력한 쇼핑 웹사이트만 이를 볼 수 있습니다.

SSL은 특정한 유형의 사이버 공격도 차단합니다. SSL은 웹 서버를 인증하는데, 공격자들이 사용자를 속여 데이터를 훔치기 위한 가짜 웹사이트를 만드는 일이 있기 때문에, 이러한 인증이 중요합니다. 또한 약병의 조작 방지 봉인처럼, 공격자가 전송 중인 데이터를 조작하지 못하게 막기도 합니다.

# **SSL과 TLS은 같은 것입니까?**

SSL은 TLS(Transport Layer Security)이라는 또 다른 프로토콜의 바로 이전 버전입니다. 1999년 IETF(Internet Engineering Task Force)는 SSL에 대한 업데이트를 제안했습니다. IETF가 이 업데이트를 개발하고 Netscape는 더 이상 참여하지 않게 되면서, 이름이 TLS로 바뀌었습니다. SSL의 최종 버전(3.0)과 TLS 첫 버전의 차이는 크지 않으며, 이름이 바뀐 것은 소유권 변경을 나타내기 위한 것입니다.

이들은 긴밀히 연계되어 있어 두 용어가 혼합되어 사용되는 경우가 많습니다. TLS를 아직 SSL이라 부르기도 하고, SSL의 인지도가 높으므로, ‘SSL/TLS 암호화’라 부르는 경우도 있습니다.

# **지금도 SSL이 최신 상태입니까?**

SSL은 1996년 SSL 3.0 이후 업데이트되지 않았으며, 앞으로 사라지게 될 것으로 여겨지고 있습니다. SSL 프로토콜에는 알려진 취약성이 여러 가지 있으며 보안 전문가들은 SSL의 사용을 중단하라고 권장합니다. 실제로, 최신 웹 브라우저는 대부분 이제 SSL을 지원하지 않습니다.

TLS는 현재 온라인으로 실행되고 있는 최신 암호화 프로토콜인데, 아직 이를 "SSL 암호화"라고 부르는 사람도 있습니다. 이 때문에 보안 솔루션 구매 시 혼란이 일어날 수 있습니다. 실제로, 현재 "SSL"을 제공하는 업체는 사실 상 TLS 보호를 제공하는 것이며, 이는 거의 20년 동안 업계 표준으로 자리 잡고 있습니다. 하지만 아직도 많은 고객이 "SSL 보호"를 찾고 있기 때문에, 많은 제품 페이지에 이 용어가 나타나고 있습니다.

# **SSL 인증이란 무엇입니까?**

SSL은 [SSL 인증서](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/)(공식적으로 "TLS 인증서")가 있는 웹사이트만 실행할 수 있습니다. SSL 인증서는 사람의 신원을 확인하는 신분증이나 배지와 같습니다. SSL 인증서는 웹사이트나 애플리케이션 서버가 웹에 저장하고 표시합니다.

SSL 인증서에 포함된 가장 중요한 정보 중 하나가 웹 사이트의 공개 [키](https://www.cloudflare.com/learning/ssl/what-is-a-cryptographic-key/)입니다. 이 [공개 키](https://www.cloudflare.com/learning/ssl/how-does-public-key-encryption-work/) 덕분에 암호화와 인증이 가능합니다. 사용자의 장치는 공개 키를 보고 이를 이용하여 웹 서버와 안전한 암호화 키를 수립합니다. 한편, 웹 서버에도 기밀로 유지하는 개인 키가 있습니다. 개인 키는 공개 키로 암호화된 데이터를 해독합니다.

CA(인증 기관)는 SSL 인증서 발행을 담당합니다.

# **SSL 인증서에는 어떤 유형이 있습니까?**

다양한 [유형의 SSL 인증서](https://www.cloudflare.com/learning/ssl/types-of-ssl-certificates/)가 있습니다. 하나의 인증서가 하나의 웹사이트에 적용되는지 아니면 여러 개의 웹사이트에 적용되는지에 따라 유형이 달라집니다.

- **단일 도메인:** 단일 도메인 SSL 인증서는 단 하나의 도메인("도메인"은 www.cloudflare.com처럼 웹사이트 이름입니다)에 적용됩니다.
- **와일드카드:** 와일드카드 SSL 인증서도 단일 도메인 인증서처럼 단 하나의 도메인에 적용되지만, 도메인의 하위 도메인도 포함합니다. 예를 들어, 와일드카드 인증서는 [www.cloudflare.com](http://www.cloudflare.com), [blog.cloudflare.com](http://blog.cloudflare.com), developers.cloudflare.com을 포함할 수 있지만, 단일 도메인 인증서는 첫 번째 도메인만 포함할 수 있습니다.
- **멀티 도메인**: 이름이 의미하는 것처럼 멀티 도메인 SSL 인증서는 관련되지 않은 다수의 도메인에 적용될 수 있습니다.

또한, SSL 인증서마다 유효성 검사 수준이 다릅니다. 유효성 검사는 신원 조회와 같은 것이며, 그 수준은 검사의 정도에 따라 다릅니다.

- **도메인 유효성 검사:** 가장 덜 엄격하고 저렴한 수준의 유효성 검사입니다. 기업은 도메인을 관리하고 있다는 것만 증명하면 됩니다.
- **조직 유효성 검사:** 보다 실무적인 프로세스입니다. CA가 담당자나 기업에 인증서를 직접 문의합니다. 이 인증서는 사용자에게 더 많은 신뢰를 제공합니다.
- **확장 유효성 검사:** 조직의 배경을 완전히 검사한 후에 SSL 인증서를 발행할 수 있습니다.

# **기업은 어떻게 SSL 인증서를 확보할 수 있습니까?**

Cloudflare는 모든 기업에 [무료 SSL 인증서](https://www.cloudflare.com/ssl/)를 제공합니다. Cloudflare의 보호를 받는 웹사이트는 클릭 몇 번으로 SSL을 활성화할 수 있습니다. 웹사이트는 [원본 서버](https://www.cloudflare.com/learning/cdn/glossary/origin-server/)에도 SSL 인증서를 설정해야 할 수 있습니다. [추가 지침](https://support.cloudflare.com/hc/en-us/articles/360024787372-How-do-I-add-SSL-to-my-site-)을 검토하시기 바랍니다.
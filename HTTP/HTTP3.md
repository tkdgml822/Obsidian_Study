## 달라진 점

[HTTP](https://namu.wiki/w/HTTP)/1 및 [HTTP/2](https://namu.wiki/w/HTTP/2)가 TCP로 통신하는 것과는 달리 HTTP/3는 [UDP](https://namu.wiki/w/UDP) 기반의 QUIC 프로토콜을 사용하여 통신한다.

HTTP/3의 기반이 되는 QUIC도 역시 웹 표준이며 RFC 9000으로 표준화되어 있다.

### 가장 큰 장점 3가지

- Zero RTT(Round Trip Time)
- 패킷 손실에 대한 빠른 대응
- 사용자 IP가 바뀌어도 연결이 유지되는 것이

차이가 크지 않을 수 있으나, 동영상 서비스 등에서는 큰 차이를 보인다. 예를들어, 모바일 기기처럼 인터넷 연결 상태가 고르지 못한 경우라든지, WiFi에 연결하여 동영상을 시청하는 도중에 자리를 이탈하여 셀룰러 신호로 바뀌더라도 HTTP/3로 연결되어 있으면 영상을 끊김없이 시청할 수 있다.

패킷 전송에 있어서 제약이 거의 없는 비연결성 전송 계층을 기반으로 TCP 프로토콜의 무결성 보장 알고리즘과 SSL이 이식됨으로써 높은 성능과 동시에 충분히 괜찮은 정확성과 부인방지 특성을 충족시켰다.

HTTP/3와 그 기반 기술인 QUIC은 [TLS](https://namu.wiki/w/TLS) 암호화를 기본적으로 사용한다. 다만, TLS가 TCP를 염두에 두고 설계되어 있기 때문에 QUIC의 원저작자인 구글의 알고리즘대로 일부 수정을 거쳐서 사용한다.

UDP와 TLS가 결합된 기술로는 [DTLS](https://namu.wiki/w/DTLS)라는 기술도 있지만 'TCP의 재구현'이 목표 중 하나인 QUIC와는 지향하는 바가 다르다.

[구글](https://namu.wiki/w/%EA%B5%AC%EA%B8%80) 자체 프로토콜이었던 QUIC가 HTTP/3로 표준화됨에 따라서 IETF 측에서는 구글이 독자적으로 개발했던 QUIC 프로토콜의 파편화된 구현 알고리즘을 기존의 다른 프로토콜과 상호운용이 용이하도록 대폭 수정하는 과정을 진행중이다.

QUIC 내에서 IETF 표준 대비 가장 큰 파편화를 보인 부분이 보안 소켓이다. IETF는 구글이 자체적으로 개발한 보안 소켓을 IETF 표준 프로토콜인 DTLS로 대체하는 **QUIC over DTLS** 표준을 만들고 있었지만 QUIC 워킹 그룹에서 다른 방법을 모색하는 것으로 결론짓고 해당 표준의 제작을 중단했다. [#](https://datatracker.ietf.org/doc/html/draft-rescorla-quic-over-dtls-00)성능이 떨어지는 컴퓨터는 QUIC 프로토콜로 인해 크롬 브라우저가 느려진다. 그럴 때는 주소창에 "chrome://flags/"를 입력해 Experiments로 들어간 다음 Experimental QUIC protocol을 검색하여 비활성화해주면 빨라질 수 있다.
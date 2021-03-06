# 터널링 프로토콜 특징 및 비교
터널링이란 송신자와 수신자 사이의 `전송로`에 `외부`로부터의 `침입`을 막기 위하여 일종의 파이프를 구성하는 기술을 말한다. 터널링되는 데이터를 `페이로드(Payload)`라고 부르며 터널링 구간에서 `페이로드`는 전송되는 데이터로만 취급이 되며 그 내용은 변하지 않는다.

## PPTP(Point to Point Tunneling Protocol) - 2계층
`PPTP`는 `MS사가 개발한 방법`으로 IP, IPX 또는 NetBEUL 트래픽을 제안한 레이어 2 터널링 프로토콜로 이동 사용자가 홈서버에 접속하도록 구성되었다.

## L2TP(Layer 2 Tunneling Protocol) - 2계층
`PPTP(Microsoft)` + `L2F(Cisco)`의 장점을 수용하여 결합한 프로토콜이다. `2계층 프로토콜`이며 `L2 레이어 업계 표준`이다.

## Sock v5 - 5계층
`Sock v5`는 `세션레이어(5계층)`에서 프록시 프로토콜로 사용이 되며 SOCK v4의 확장형태라 할 수 있다. `클라이언트 인증`, `암호화`에 프록시 등 보`안기능이 추가`되었다. 응용계층에서 필터링을 지원하며 `SSL/TLS`에 결합사용이 가능하다.

## IPSec - 3계층
`IPSec`는 `IETF`에 의해 IP 계층 보안을 위한 개방형 구조로 설계된 VPN의 `3계층 프로토콜`로써 암호화 알고리즘을 수용할 수 있을 뿐 아니라 새로운 알고리즘도 수용할 수 있다. `IPSec`는 보안 서비스 제공을 위하여 `AH(Authentication Header)`와 `ESP(Encapsulation Security Payload)` 두 가지 프로토콜과 `IPSec` 보안구조와 관련된 데이터베이스를 이용한다.

### IPSec 프로토콜
- AH
    - `데이터 무결성`과 `IP 패킷의 인증` 제공, MAC 기반
    - Replay Attack으로부터의 보호 기능(순서번호 사용)을 제공
    - 인증 시 MD5, SHA-1 인증 알고리즘을 이용하여 Key 값과 IP 패킷의 데이터를 입력한 인증 값을 계산하여 인증 필드에 기록
    - 수신자는 같은 키를 이용하여 인증 값을 검증
- ESP
    - 전송 자료를 암호화하여 전송하고 수신자가 받은 자료를 복호화하여 수신
    - IP 데이터그램에 제공하는 기능으로서, 데이터의 선택적 `인증`, `무결성`, `기밀성`, `Replay Attack 방지`를 위해 사용
    - `AH`와 달리 `암호화`를 `제공`(대칭키, DES, 3-DES 알고리즘)
    - `TCP/UDP` 등의 `Transport` 계층까지 암호화할 경우 `Transport` 모드
    - 전체 IP 패킷에 대해 암호화할 경우 터널 모드를 사용
- IKE
    - VPN에서 `키 교환`을 위해 사용되는 `프로토콜`
    - `RFC 2409`에 규정되어 있으며, `IPSec`를 암호화하는 데 사용됨
    - 송신측에서 수신측이 생성한 암호키를 상대방에게 안전하게 송신하기 위한 방법
    - `RSA법`과 `Diffie-Hellman법` 등의 암호 기술을 사용
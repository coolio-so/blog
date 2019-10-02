# DoS(Denial of Service) Attack

# DoS(Denial of Service) Attack
> 공격자의 컴퓨터로부터 표적 시스템과 그 시스템에 속한 네트워크에 과다한 데이터를 보냄으로써 대역폭, 프로세스, 처리능력, 기타 시스템 자원을 고갈시킴으로써 정상적인 서비스를 할 수 없도록 하는 행위

## Dos 공격형태

### 시스템 과부학 공격
- 프로세스 & 네트워크 고갈공격
- 디스크 채우기 공격

### 네트워크 서비스 방해 공격
- SYN
- TCP/IP Flooding
- Smurf
- Land Attack

## DoS 공격유형

### TCP SYN Flooding
> 서비스 방해공격의 한 방법으로 전형적인 `3Way 핸드쉐이킹`인데 공격자가 임의로 `자신의 IP주소`를 속여서 다량으로 서버에 보내면 서버는 클라이언트에 `SYN/ACK`를 보내고 이에 대한 클라이언트 응답 `ACK`를 받기 위해 대기상태에 놓이게 되는 공격

- TCP 패킷의 SYN Bit를 이용한 공격방법이다. 많은 연결 요청이 오도록하여 대상 시스템이 Flooding하게 함으로써 메모리가 바닥나게 하는 공격
- 동시 사용자 연결 수를 존재하지 않는 클라이언트가 접속한 것처럼 하여 다른 사용자가 서비스를 받지 못하도록 하는 공격

### UDP Floding
> 서비스 방해공격의 한 종류로 UDP 프로토콜을 이용하여 클라이언트가 서버에 가상의 데이터를 연속적으로 보내어 서버의 부하 및 네트워크 오버로드를 발생시켜 정상적인 서비스를 하지 못하도록 하는 공격

### Teardrop Attack(IP Fragmentation - Ping of Death)
> 네트워크 패킷은 `MTU(Maximum Transmission Unit)` 보다 큰 패킷이 오면 분할(Fragmentation)하고 분할 된 정보를 `flags`와 `offset`이 가지고 있다. 이 때 `offset`을 임의로 조작하여 다시 조립할 수 없도록 하는 공격으로 `Fragment`를 조작하여 패킷 필터링 장비나 IDS를 우회하여 서비스 거부를 유발시킨다.  
이 공격수법은 헤더가 조작된 일련의 IP 패킷조각(`IP Fragments`)들을 전송함으로써 공격이 이루어진다.  
이 수법으로 공격당한 시스템은 네트워크 연결이 끊어지거나 일명 "죽음의 푸른 화면(`Blue Screen of Death`)"이라 불리는 오류 화면을 표시하면서 중단된다.

#### Ping of Death
> `ping`을 이용하여 `ICMP` 패킷을 규정된 길이 이상으로 큰 IP 패킷을 전송하여 수신 받은 OS에서 처리하지 못함으로써 시스템을 마비시키는 공격

#### Tear Drop
> `Fragment` 재조합 과정의 취약점을 이용한 공격으로 목표시스템 정지나 재부팅을 유발하는 공격. `TCP Header` 부분의 `Offset Field` 값이 중첩되는 데이터 패킷을 대상 시스템에 전송


### Smurf Attack(ICMP Flooding)
> `ping`을 이용한 공격으로 `ICMP_ECHO_REPLY`를 이용한 공격이라고 할 수 있다. 출발지 주소를 속여서 네트워크의 `Broadcast` 주소로 `ICMP_ECHO_REQUEST`를 전달하는 경우 출발지로 응답에 대한 `traffic`이 증가되어 정상적인 네트워크 서비스가 이루어지지 않을 수 있다.

### ARP Spoofing
- 로컬 통신 과정에서 서버와 클라이언트는 `IP`와 `MAC 주소`로 통신을 수행한다.
- 클라이언트의 `MAC 주소`를 중간에 공격자가 자신의 `MAC 주소`로 변조하여 마치 서버와 클라이언트가 통신하는 것처럼 속이는 공격이다. 이러한 공격은 `Fragrouter`를 통하여 연결이 끊어지지 않도록 `Release`를 해주어야 한다.

### Land Attack
> `IP Header`를 변조하여 인위적으로 송신자 `IP 주소` 및 `PORT 주소`를 수신자의 `IP 주소`와 `PORT 주소`로 설정하여 트래픽을 전송하는 공격 기법이다. 송신자와 수신자의 `IP 주소`와 `PORT 주소`가 동일하기 때문에 네트워크 장비에 부하를 유발한다.
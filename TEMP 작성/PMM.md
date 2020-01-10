# PMM(Percona)

## pmm-admin service default products

Service Name | Method | Service Port
---------|----------|---------
 General OS metrics | linux:metrics | 42000
 MySQL metrics | mysql:metrics | 42002
 MongoDB metrics | mongodb:metrics | 42003
 ProxySQL metrics | proxysql:metrics | 42004
 PostgreSQL metrics | postgresql:metrics | 42005

정보에 대한 부분이 제대로 갱신되지 않을 경우에는 서버와의 통신 상태를 체크한다.
```
$ pmm-admin check-network
```

## PMM Client 설치
기존 PMM Client가 설치되어 있다면 삭제 후에 설치. `PMM2` Client는 서버 접속시에 SSL로만 접속이 가능하기에 SSL 접속이 제한되면 `PPM1` 버젼을 사용하는 것을 추천


### PMM2 Repo 추가
```
$ yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm
```

### Percona Release 설정
```
$ percona-release disable all
$ percona-release enable original release
```

### PMM1 Client 설치
```
$ rpm -ivh https://www.percona.com/downloads/pmm-client/1.8.1/binary/redhat/7/x86_64/pmm-client-1.8.1-1.x86_64.rpm
```

### PMM2 Client 설치
```
$ yum install pmm2-client
```

### PMM1 Client Configuration
PPM 서버를 설정하는 과정
```
$ ppm-admin config --client-name {Client Name} --server {Host IP or URL} --client-address {Client IP}
```
설정이 완료되면 모니터링하고자 하는 서비스를 등록 한다.

#### `Linux`만 등록 할 때
```
$ ppm-admin add linux:metrics
```

#### 등록 상태 확인
```
$ ppm-admin check-network
```
ppm-admin check-network 명령 결과에서 status가 Down으로 표시되면 방화벽을 체크해야 한다. 통신을 하는 해당 포트는 방화벽에서 오픈을 해줘야지만 모니터링이 가능함.
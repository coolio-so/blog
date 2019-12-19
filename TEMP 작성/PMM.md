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
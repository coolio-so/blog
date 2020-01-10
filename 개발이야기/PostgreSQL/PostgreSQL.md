# PostgreSQL
> ANSI/ISO SQL 표준을 모두 만족하고 확장 가능한 관계형 데이터베이스를 만들기 위해서 고안되었으며, 이로 인해 매우 강력하면서도 다양항 써드파티 도구와 라이브러리가 존재합니다. 또한 표준 SQL을 `MVCC(Multiversion Concurrency Control)`라고 불리는 기술의 구현으로 읽기 잠금(read lock) 없이 동시성을 보장합니다.  
> 반면, 일반적인 온라인 트랜잭션 처리(Online Transaction Processing, OLTP)에 있어서 MySQL에 비해 다소 성능이 떨어진다고 알려져 있으며 동시성 보장을 위한 아키텍처로 인해서 리플리케이션(Replication)을 구성하는 것이 좀 더 어렵습니다.  
> SQL이나 프로지서(Procedure)를 이용하여 복잡한 질의나 연산이 필요한 애플리케이션을 개발하거나 데이터의 무결성이 매우 중요한 경우 유료 상용 데이터베이스의 합리적인 대안이 될 수 있습니다. 반면, 애플리케이션의 성능이 매우 중요하거나 PostgreSQL을 비롯한 데이터베이스 관리/운영 경험이 풍부한 엔지니어가 없는 경우 좀 더 신중하게 고려해 보는 것이 좋습니다.

## PostgreSQL의 라이센스
BSD(Berkeley Distribution Software)

## 외부 접속을 위해서 설정

### postgresql.conf 변경
```
listen_addresses = '*'
```
### pg_hba.conf 변경
```
# TYPE      DATABASE        USER        CIDR-ADDRESS      METHOD
  host      all             all         0.0.0.0/0         md5
```

지원되는 Method 인증 방법 : GSSAPI, SSPI, LDAP, RADIUS, PAM, md5


## Graphical administration tools

### 무료 관리 툴
- pgAdmin3
- phpPgAdmin

### 사용 관리 툴
- Navicat
  - http://pgsql.navicat.com
- EMS SQLManager
  - http://www.sqlmanager.net/products/studio/postgresql
- LightningAdmin
  - http://www.amsoftwaredesign.com

## 터미널 접속 방법

```
psql -h {hostname} -p {5432} -d {dbname} -U {username} -W
```

## DATA 폴더 기본 위치
### Debian or Ubuntu 9.0 이하
```
/var/lib/postgresql/R.r/main
```
### Debian or Ubuntu 9.0 이상
```
/etc/postgresql/R.r/main
```

### Red Hat RHEL, CentOS or Fedora
```
/var/lib/pgsql/data
```

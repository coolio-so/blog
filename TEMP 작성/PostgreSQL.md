# PostgreSQL

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

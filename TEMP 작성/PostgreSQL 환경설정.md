# PostgreSQL 환경변수 정보

## PostgreSQL 관련 설정 파일
- `postgresql.conf`
  - 실행 환경에 관련된 설정 파일
- `pg_hba.conf`
  - 접속 관련 설정 파일
- `pg_ident.conf`
  - 보안 관련 설정 파일

## 운영중에 변경된 환경설정 값 로딩 방법
```
pg_ctl -D data reload
```
`postgresql.conf`파일을 리로딩하여 운영중인 `PostgreSQL`에 반영됨.

## 메모리 사이즈에 따른 설정값 변경
PostgreSQL의 메모리가 32MB가 이상이라면 `shared_buffers`의 값을 기본값보다 높게 설정한다. 또한 `shared_buffers`의 값을 변경하였다면, 시스템의 `SHMMAX`의 시스템 변수값을 변경한다. 

### LINUX / Mac OS / FreeBSD
해당 시스템에 대해서는 환경 변수를 `/etc/sysctl.conf` 파일을 수정하여 시스템 변수를 설정 할 수 있다. 재부팅시에도 반영될 수 있도록 해당 파일에 값을 설정하는 것이 좋다.

#### LINUX
- `kernel.shmmax=value`
#### Mac OS
- `kern.sysv.shmmax=value`
#### FreeBSD
- `kern.ipc.shmmax=value`

## 수동 실행
|        OS        | Command                               |
|:----------------:|---------------------------------------|
| UBUNTU / DEBIAN  | `pg_ctlcluster 9.0 main reload`       |
| RED HAT / REDORA | `pg_ctl -D /var/lib/pgsql/data start` |
| RED HAT / REDORA | `service postgresql start`            |
|     SOLARIS      | `pg_ctl -D /var/lib/pgsql/data start` |
|      MAC OS      | `pg_ctl -D /var/lib/pgsql/data start` |
|     FREEBSD      | `pg_ctl -D /var/lib/pgsql/data start` |
|     WINDOWS      | `net start postgres`                  |

## 안전 실행 수동 정지
`pg_ctl -D datadir -m fast stop`  
`-m fast` 옵션을 활용하면 사용중인 사용자를 대기하지 않고 강제로 바로 종료 할 수 있다.
|       OS        | Command                               |
|:---------------:|---------------------------------------|
| UBUNTU / DEBIAN | `pg_ctlcluster 9.0 main stop --force` |

# 실무 예제로 배우는 Elasticsearch 검색엔진
> 한빛 출판사 도서로 1.0.0 버젼으로 추후에 내용은 업데이트해야 함.

## Elasticsearch와 RDBMS 용어 비교

| Elasticsearch | RDBMS    |
|---------------|----------|
| index         | database |
| document type | table    |
| document      | row      |
| field         | column   |

\* Index + Document Type + Document ID = Unique ID of document

## Elasticsearch 디렉터리 구조
elasticsearch 설치 파일의 압축을 풀면 처음에는 bin, config, lib의 세 개 디렉터리만 존재하고, 나머지 디렉터리는 실행할 때 생성된다.

| 디렉터리 | 설명                                                                  |
|----------|-----------------------------------------------------------------------|
| bin      | elasticsearch 실행에 필요한 스크립트와 플러그인 설치 스트립트가 있다. |
| config   | elasticsearch.yml과 logger.yml 파일이 있다.                           |
| lib      | 검색엔진에서 사용하는 라이브러리가 있다.                              |
| data     | 별도 path를 지정하지 않으면 기본 index store의 위치가 된다.           |
| logs     | 검색엔진에서 기록하는 로그파일의 위치다.                              |
| plugins  | 검색엔진에서 사용하는 모든 플러그인이 설치되는 위치다.                |
| work     | 임시 파일 경로다.                                                     |

## Elasticsearch 실행 옵션

옵션|설명
---|---
-Des.config|elasticsearch.yml 파일을 지정한다.
-Des.pidfile|실행된 pid를 저장할 파일을 지정한다.
-Des.foreground|foreground로 실행할지 지정한다.
-Des.path.home|elasticsearch의 home 디렉터리를 지정한다.

## 실행 스크립트 구성하기

#### start.sh
```shell
#!/bin/bash

export ES_HEAP_SIZE=256m
export ES_HEAP_NEWSIZE=128m
export JAVA_OPT="-server -XX:+AggressiveOpts -XX:UseCompressdOops -XX:MaxDirectMemorySize -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:+CMSParallelRemarkEnabled -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly"

ES=/home/es/app/elasticsearch
$ES/bin/elasticsearch -Des.pidfile=$ES/bin/es.pid -Des.config=$ES_NODE/config/elasticsearch.yml -Djava.net.preferIPv4Stack=true -Des.max-open-files=true > /dev/null 2>&1 &
```

#### stop.sh
```shell
#!/bin/bash

ES=/home/es/app/elasticsearch
/bin/kill `cat < $ES/bin/es.pid`
```

## 설정 값

### Index 기타 설정

| 기본 설정 값                  | 설명                                                         |
|-------------------------------|--------------------------------------------------------------|
| index.mapper.dynamic:true     | 자동으로 필드 매핑을 설정한다.                               |
| index.refresh_interval:"1s"   | 색인 operation에 대한 실시간 검색 반영을 위한 주기 설정이다. |
| action.auto_create_index:true | 자동 인덱스 생성에 대한 설정으로 패턴을 지원한다.            |
| action.disable_shutdown:true  | REST API를 이용하여 노드 shutdown 기능을 활성화한다.         |

### Network 설정

| 기본 설정 값                | 설명                                        |
|-----------------------------|---------------------------------------------|
| network.host:localhost      | IP 정보로 설정한다.                         |
| transport.tcp.port:9300     | TCP 기본 포트로, 변경할 수 있다.            |
| transport.tcp.compress:true | TCP 통신 시 데이터 압축을 설정한다.         |
| http.port:9200              | http(REST API) 기본 포트로, 변경할 수 있다. |
| http.enabled:true           | http(REST API) 사용을 활성화한다.           |

## Elasticsearch 기본 REST API
RESt API | 설명
---|---
http://localhost:9200/_cluster |클러스터와 관련된 작업 수행
http://localhost:9200/_nodes |노드와 관련된 작업 수행
http://localhost:9200/_aliases |index alias와 관련된 작업 수행
http://localhost:9200/_analyze |analyzer 관련 테스트 수행
http://localhost:9200/_cache |캐시와 관련된 작업 수행
http://localhost:9200/_flush | 트랜잭션 로그나 Memory free 작업 수행
http://localhost:9200/_optimize | 세그먼트 파일에 대한 병합 작업 수행
http://localhost:9200/_stats | 시스템 또는 색인에 대한 통계 정보
http://localhost:9200/_status | 클러스터와 노드 등에 대한 상태 점검
http://localhost:9200/_validate | 쿼리에 대한 유효성 점검
http://localhost:9200/_bulk | 벌크 색인에 대한 작업 수행
http://localhost:9200/_count | 문서 카운트 작업 수행
http://localhost:9200/_mget | 멀티 데이터 패치 수행
http://localhost:9200/_search | 검색 질의 수행
http://localhost:9200/_msearch | 멀티 검색 질의 수행
http://localhost:9200/_suggest | 검색어 자동완성 또는 추천 수행
http://localhost:9200/{index}/{type}|{id} | URI 기본 구조

## 검색 결과 속성

속성|설명
---|---
took|검색 질의응답 시간(milliseconds)
timed_out|boolean 값으로 검색엔진 내부에서 질의 실행에 대한 timeout 여부
_shards | 검색을 수행한 샤드
_shards.total|검색을 수행한 총 샤드 수
_shards.successful|검색 수행을 성공한 샤드 수
_shards.failed|검색 수행을 실패한 샤드 수
hits|검색 매칭 결과 루트
hits.total|검색 매칭된 도큐먼트 총수
hits.max_score|매칭된 도큐먼트 중 가장 높은 점수
hits.hits|매칭된 도큐먼트 결과
hits.hits._index|매칭된 인덱스 명
hits.hits._type|매칭된 도큐먼트 타입
hits.hits._id|매칭된 도큐먼트 아이디
hits.hits._score|매칭된 도큐먼트의 점수
hits.hits._source|출력 필드를 지정하지 않았을 경우 리턴(모든 필드 목록 포함)
hits.hits._source.fileds|출력 필드 목록 포함(fields 사용시 _source는 출력되지 않음)
hits.hits.highlight|강조 필드 목록 포함

## Elasticsearch의 검색 방법

### Term Query
> Term 쿼리는 검색 대상 필드에 질의를 위해 입력한 질의 텍스트를 분석하여 추출된 검색 Term이 포함된 문서를 검색한다.

### Terms Query
> Term 쿼리와 같은 쿼리로 대상 필드에 복수 갱의 Term이 포함된 도큐먼트를 검색한다. 이 쿼리는 minimum_match 설정을 통해 AND와 OR 연산을 적용할 수 있다.  
> 여러 개의 Term이 모두 포함된 문서만 찾고 싶으면 minimum_match를 Term 크기만큼 지정하고, 그 중 하나의 term이라도 포함된 문서를 찾고 싶다면 minimum_match를 1로 설정한다.

### Match Query
> Terms 쿼리와 비슷한 기능으로, 지정한 필드에서 쿼리 스트링 형태로 문서를 검색한다. Terms 쿼리와는 다르게 쿼리에 대한 분석 작업이 필요하다.

### Multi Match Query
> Match 쿼리와 방법은 같으나 하나의 필드가 아니라 여러 개의 필드에서 쿼리 스트링으로 검색한다.

### Query String Query
> 전통적으로 많이 사용하는 쿼리로, 대상 필드에 쿼리 스트링 질의(Query String Parsing)로 문서를 검색한다. 이 쿼리는 쿼리 스트링 구문분석을 위해 쿼리 구문분석기(Query Parser)를 사용하는 것이 특징이다.  
> 사용하려면 루씬의 Query Parser Syntax를 잘 알아야 한다.

### Identifiers Query
> 도큐먼트의 primary 또는 Unique key에 해당하는 값을 이용해서 검색하는 쿼리로 _id 필드에 도큐먼트 id 목록을 요청하여 도큐먼트 정보를 구한다. _id필드는 인덱스 속성을 not_analyzed로 설정해야 한다는 점을 주의한다. 이 쿼리는 일반적으로 도큐먼트 정보를 빠르게 검색하여 상세페이지를 구성하는 데 유용하게 사용된다.

### Prefix Query
> 이 쿼리는 텍스트의 앞 문자열에 대한 term의 일치 여부에 따라 검색 결과를 가져온다. Prefix 쿼리를 이용하면 자동완성 기능을 쉽게 구현할 수 있다. 이 쿼리에 대한 검색 대상 필드는 인덱스 속성이 not_analyzed로 설정되어야 하고, 다른 속성 값을 가진 필드로 검색을 실행하면 잘못된 결과가 나오게 된다.

### Match All Query
> 이 쿼리는 `SELECT * FROM TABLE` SQL문처럼 모든 도큐먼트를 구해오는 쿼리다. 검색 서비스에서는 거의 사용하지 않고, 보통 관리 목적으로 사용한다.

### Range Query
> Term 쿼리와 비슷하게 가장 많이 사용하는 쿼리로, number와 date, ip 등의 데이터형을 갖는 필드에 대한 범위를 지정하여 도큐먼트를 검색한다. 필드 특성은 인덱스 속성을 not_analyzed로 설정해야 한다.

### Bool Query(must, should)
> bool 쿼리의 must는 반드시 검색하려는 term이 도큐먼트에 포함되어야만 하는 조건을 만들고, should는 조건 속성에 따라 다르지만 최소 하나 이상의 term이 포함된 문서를 검색하는 조건을 만든다. 즉 must는 AND 조건을 만들고 should는 OR 조건을 만든다고 이해하면 쉽다.

### Bool Query(must_not)
> 이 쿼리는 검색어 제외 기능으로, 지정한 검색 필드에서 term이 포함된 도큐먼트를 제외하고 결과를 구한다.


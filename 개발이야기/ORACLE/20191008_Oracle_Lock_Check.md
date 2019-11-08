# 오라클 LOCK 걸린 개체 확인 및 LOCK 해제

### 1. LOCK 걸린 개체 확인

```sql
SELECT OBJECT_ID
     , SESSION_ID
     , ORACLE_USERNAME
     , OS_USER_NAME
FROM V$LOCKED_OBJECT;
```

### 2. 해당 SESSION_ID와 SERIAL 번호로 락걸린 개체 확인

```sql
SELECT A.SID 
     , A.SERIAL#
     , OBJECT_NAME
     , A.SID || ', ' || A.SERIAL# AS KILL_TASK
FROM V$SESSION A
INNER JOIN V$LOCK B ON A.SID = B.SID
INNER JOIN DBA_OBJECTS C ON B.ID1 = C.OBJECT_ID
WHERE B.TYPE  = 'TM'
```

### 3. 락 발생 사용자 및 OBJECT 조회 + 어떤 SQL를 실행중인지 확인

> Lock을 해제하기전에 Lock을 유발시킨 SQL이 어떤것인지를 확인하여 실행되고 있는지 아니면 정말로 Lock 인지를 확인해야 함.

```sql
SELECT DISTINCT T1.SESSION_ID
     , T2.SERIAL#
     , T4.OBJECT_NAME
     , T2.MACHINE
     , T2.TERMINAL
     , T2.PROGRAM
     , T3.ADDRESS
     , T3.PIECE
     , T3.SQL_TEXT
FROM V$LOCKED_OBJECT T1
     , V$SESSION T2
     , V$SQLTEXT T3
     , DBA_OBJECTS T4
WHERE T1.SESSION_ID = T2.SID
  AND T1.OBJECT_ID = T4.OBJECT_ID
  AND T2.SQL_ADDRESS = T3.ADDRESS
ORDER BY T3.ADDRESS, T3.PIECE;
```

### 4. SID와 시리얼 번호로 세션 해제

```sql
ALTER SYSTEM KILL SESSION ${2번에서 KILL_TASK로 조회한 결과값};
```

### 참고사이트

[오라클 LOCK 걸린 개체 확인 및 LOCK 해제](https://hello-nanam.tistory.com/23)
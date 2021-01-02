# 정리
## 가끔사용하는 SQL형태

```sql

CREATE TABLE SB_HISTORY NOT EXISTS BITCOIN (
  orderId INT(10) NOT NULL,
  clientOrderId VARCHAR(24) NOT NULL,
  transactTime LONG,
  PRIMARY KEY (id)
);
  

```

## 유니크인덱스 생성
> url : https://huskdoll.tistory.com/605 <br>

```
1. 따로생성

  정의
  CREATE INDEX [인덱스명] ON [테이블명][(컬럼명,...)];

  예시,
  CREATE INDEX old_history_idx ON old_history(id,name);

2. 테이블 생성시 생성
  CREATE TABLE old_history (
    id varchar(20) not null,
    //...
    system_date datetime default null,
    
    index 'old_history_idx' (id, name) -- 이렇게 넣어주면 됨
  );
  
```

## mysql 스키마의 테이블사이즈 조회
> url : https://www.lesstif.com/dbms/mysql-17105786.html <br>

```sql
--찾아본 사이트 중에,
--가장 간결하고 오류가 없는 쿼리라서 추가한다.


SELECT TABLE_NAME AS "Tables",
                     round(((data_length + index_length) / 1024 / 1024), 2) "Size in MB"
FROM information_schema.TABLES
WHERE table_schema = "bitcoin"
ORDER BY (data_length + index_length) DESC;


```

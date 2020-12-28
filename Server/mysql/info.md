# 참고 
## nodejs, mysql 연동
> url : https://poiemaweb.com/nodejs-mysql <br>

## docker로 mysql 띄우기
> url : https://www.hanumoka.net/2018/04/29/docker-20180429-docker-install-mysql/ <br>

```

1. docker run으로 띄우되, -d옵션을 통해 백그라운드로 띄운다.
2. docker exec명령어로 mysql컨테이너에 접속 
   docker exec -it mysql_test bash
   
3. mysql컨테이너 내에서,
   mysql -u root -p password
   
   -u : 유저
   -p : 비밀번호
   
4. sql날려서 테이블 생성하든 말든 하면 된다.
  (근데, 여기서는 불편하다. 사실상...)
  
  * mySql workbench를 사용하면 된다. 링크 참조.
```

## mysql DB접근 계정생성
> url : https://2dubbing.tistory.com/13 <br>

```

1. root로 접속 (mysql -u -p)
2. use mysql
3. 계정테이블은 user
4.1. create user '계정ID'@localhost identified by '비밀번호'; // 내부IP만 접속가능
4.2. create user '계정ID'@'%' identified by '비밀번호'; // 외부IP 접속가능
    (ex. create user genUser@'%' identified by 'genUserPassword'; )

5. 추가로, 
5.1. 계정삭제
   delete from user where user='계정ID';
   
5.2. 스키마 제한
   a. 먼저, 대상 스키마 생성 (create schema openDB;)
     (다른 스키마가 있으면 생략)
     
   b. use openDB로 스키마 바라보게 설정
   c. grant all privileges on '스키마명'.'테이블명' to '계정명'@'호스트' identified by '계정비밀번호' with grant option;
     (ex. grant all privileges on openDB.* to genUser@'%' identified by 'genUserPassword' with grant option; )
     
     * all 대신 select나 기타 다른걸 사용해도 된다.
     
   d. flush privileges; 로 권한 적용
   
   e. show grants for '계정명'@'호스트'; 를 입력해 실제 권한 부여확인
     (ex. show grants for genUser@'%';)
     
6. 스키마 권한 제거
   revoke all on '스키마명'.'테이블명' from '계정명'@'호스트';
   (ex. revoke all on openDB.* from genUser@'%'; )
   
   * all 대신 select나 기타 다른걸 사용해도 된다.

```

# 참고 
## 자잘한 사항

|명칭 | 설명 |
|---|---|
|연동에러 해결|https://m.blog.naver.com/PostView.nhn?blogId=kkj6369&logNo=221407127618&proxyReferer=https:%2F%2Fwww.google.com%2F|
|버전체크 | SELECT VERSION(); `/* OR */` <br/>SHOW VARIABLES LIKE 'version'; |
## nodejs, mysql 연동
> url : https://poiemaweb.com/nodejs-mysql <br>

```
// nodejs의 ER_NOT_SUPPORTED_AUTH_MODE 에러 확인

google에서 검색하니, 이런 사례가 생각보다 많이 발견되었다.
npm의 보안프로토콜 버전이 mysql8을 못따라와서 생긴문제라고 legecy 처리를 하면 해결된다는 의견이 있다.
그 중 하나를 링크를 위에 두었다.

...생각해보니, 도커쓰고 있는데, 그러면 도커의 mysql버젼을 낮춰야하나...?

더 찾아보니. SQL문으로 해결할 수 있었다.
alter user 'genUser'@'%' identified with mysql_native_password by 'genUser';

use mysql;
select * from user;
로 확인결과

user테이블의 plugin, authentication_string 컬럼값이 변경되었다.

plugin은 'mysql_native_password'
authentication_string은 16진수값

이렇게 바뀌어야 nodejs와 연동이 가능한 것으로 추정된다. (해결!)(node server.js 된다)
mysql_native_password ==>  *4ACFE3202A5FF5CF467898FC58AAB1D615029441
caching_sha2_password ==>  $A$005$A;>{)YiHA+'UT*HjoPPUiwbP0Iyli7SbweBWEuJlQ7A2M/GVp08yD/Fy66


mysql_native_password가 뭔가 보안면에서 아쉬운 점으로 인해 최신Mysql버전에서 sha2를 적용했을지도 모르겠는데,
이건 시간을 두고 node측에서 패치가 이루어지던가 하겠지.

```

## nodejs, mysql DML문법 기술
> url : https://www.w3schools.com/nodejs/nodejs_mysql_insert.asp <br>

```
위의 사이트를 참고하여 작성했긴 했는데,
Insert쿼리가 미묘하게 달랐다.

(X)  var sql = "INSERT INTO customers (name, address) VALUES ?"; 
(O)  var sql = "INSERT INTO customers (name, address) VALUES (?);";

'?'에 아래 값이 들어가는데, 그냥 ?만 있으면 syntax에러뜬다. 이거때문에 조금 헤멨다.

들어갈 예시값
  var values = [
    ['John', 'Highway 71'],
    //...
    ['Chuck', 'Main Road 989'],
  ];
  
connection.query(sql, values, function(err, result) {...} );

```

## docker로 mysql 띄우기
> url : https://www.hanumoka.net/2018/04/29/docker-20180429-docker-install-mysql/ <br>
> url : http://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/ <br/>

```bash

1. docker run으로 띄우되, -d옵션을 통해 백그라운드로 띄운다.
   docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --name autobitdb mysql
   
   * 외부에 데이터를 저장해야하는 경우. ( mysql이 저장하는 경로는 /var/lib/mysql 하위에 있다.)
     docker내 경로와 실제 물리경로를 마운트해야한다.
     
     예) docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --name bitcoindb -v /home/commuser/mysqldata:/var/lib/mysql mysql:8.0.22
   
   mysql 이미지가 없을경우, 알아서 받는다.
   하지만, 미리 버전을 결정하고( docker pull mysql:8.0.22 ) 진행하는게 좋다.

2. docker exec명령어로 mysql컨테이너에 접속 
   docker exec -it mysql_test bash
   
3. mysql컨테이너 내에서,
   mysql -u root -p 
   
   -u : 유저
   -p : 비밀번호 입력
   
4. sql날려서 테이블 생성하든 말든 하면 된다.
  (근데, 여기서는 불편하다. 사실상...)
  
  * mySql workbench를 사용하면 된다. 링크 참조.
```

## mysql DB접근 계정생성
> url : https://2dubbing.tistory.com/13 <br>
> url : https://m.blog.naver.com/PostView.nhn?blogId=platinasnow&logNo=220281629640&proxyReferer=https:%2F%2Fwww.google.com%2F <br>

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
     ==> '%'까지만 적고 나머지는 지운다.
         (ex. grant all privileges on openDB.* to genUser@'%' )
     
     * all 대신 select나 기타 다른걸 사용해도 된다.
     
   d. flush privileges; 로 권한 적용
   
   e. show grants for '계정명'@'호스트'; 를 입력해 실제 권한 부여확인
     (ex. show grants for genUser@'%';)
     
6. 스키마 권한 제거
   revoke all on '스키마명'.'테이블명' from '계정명'@'호스트';
   (ex. revoke all on openDB.* from genUser@'%'; )
   
   * all 대신 select나 기타 다른걸 사용해도 된다.
   
   
7. 정리
계정생성 : create user [계정ID]@'%' identified by [패스워드]; // 그전에 use mysql로 진입해야함.
부여권한보기 : show grants for [계정ID]@'%';      // '%'대신 호스트IP 적어도됨.
권한부여 : grant all privileges on [스키마명].[테이블명] to [계정ID]@'%';
권한제거 : revoke all on [스키마명].[테이블명] from [계정ID]@'%';

grant의 all privilegest on 대신 ->  select on 이나 update on 으로 변경해도됨.
revoke도 동일하게 all 대신 select등으로 변경

```

## SQL대소문자 구분
> url : https://bizadmin.tistory.com/entry/mysql-%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9D%B4%EB%A6%84-%EB%8C%80%EC%86%8C%EB%AC%B8%EC%9E%90-%EB%B3%80%EA%B2%BD <br>
> mysql은 OS에 따라 기본설치시 대소문자 구분Flag를 다르게 주고 있다. <br>

```

mysql은 OS마다 대소문자 구분옵션이 다르다. 
대소문자 구분없이 진행하려면 `select @@lower_case_table_names`로 조회되는 값을 1로 변경해야한다.

변경방법
1. 우리는 docker를 사용하므로, docker exec명령어를 통해 컨테이너 내부로 들어간다.
  (컨테이너 내부에 vim 안깔려 있으면, 설치하면 된다.)
  (mainOs는 centos인데, docker-mysql은 데비안 계열이라 apt-get명령어로 설치함.)
  (apt-get update -> apt-get install vim)
  
2. cd /etc/mysql/
   vim my.cnf
   
   [mysqlId] 하위에 
   
   lower_case_table_names = 1 추가(있으면 변경)
   
3. docker-mysql컨테이너 재시작
   docker stop -> docker start 
   
   (당연히, 컨테이너ID를 뒤에 적어야 한다.)
   
   * start후, 컨테이너가 바로 종료된다면, docker logs로 문제점을 파악한다.
     필자의 경우는 
     
     "Different lower_case_table_names settings for server('1') and data dictionary('0')"
     ==> stackoverflow를 찾아보니, 변경후 비슷한 문제를 겪는게 보여서 일단 변경은 보류.
     
     가 로그로 찍혀있었다.
     
4. 적용이 되었...어야하는데, 오류가 나서 일단 보류중.
   이런경우 도커디버깅이 필요하다.
   아래링크 참고.
   
   https://github.com/sonsangjun/SimpleTechDesc/blob/master/Server/docker/info.md
   
```

## mysql table생성
> (데이터형)URL : https://m.blog.naver.com/PostView.nhn?blogId=dudwo567890&logNo=220847437396&proxyReferer=https:%2F%2Fwww.google.com%2F <br>

<p/>

## docker상의 mysql데이터 백업
> url : https://mungto.tistory.com/329 <br>

```
컨테이너를 띄워놓은 상태이므로,
당연히 컨테이너 내 데이터를 백업해야한다.
```

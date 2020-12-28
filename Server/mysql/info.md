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

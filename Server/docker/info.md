# 정보
## 참고사이트
> setup : https://niceman.tistory.com/36 <br>
> setup(en) : https://www.liquidweb.com/kb/how-to-install-docker-on-centos-7/ <br>
> (참고만)url : https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html <br>
> VSCode-Docker연동 : https://89douner.tistory.com/123 <br>
> (명령어)url : https://brunch.co.kr/@hopeless/10 <br>
> (명령어,exec)url : http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter20/08 <br>
> (도커컴포즈)url : https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose#%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A5%BC-%EB%B3%B4%EC%A1%B4%ED%95%98%EA%B8%B0 <br>
> (도커디버깅)url : https://www.44bits.io/ko/post/docker-container-trouble-shooting-by-exec-and-commit <br>

<p/>

## 간단 명령어

|명칭 | 설명 |
|--|--|
|docker version | 도커 현재 버젼확인 client/server <br>만약, server정보가 없으면 docker deamon이 떠 있는지 확인<br>안떠있으면, setup링크를 참고해 띄울 것(systemctl명령어로 등록 및 서비스 시작해야함)|
|docker run | 도커 실행명령어.(ex. docker run -i -t centos /bin/bash)<br>-option설명<br>`-d : 백그라운드 모드 실행`<br>`-it : 터미널 입력상태로 컨테이너 실행`<br>`--rm 프로세스 종료시 컨테이너 자동으로 삭제`<br>다양한 옵션은 위의 링크 참고.<br> |
|docker stop | docker ps 으로 조회된 프로세스 중, 살아있는 (STATUS가 Up 1 Minutes 이런것) 걸 죽이는 명령어<br>docker stop ${CONTAINER ID}<br>ex. docker stop ba435cdc3368|
|docker ps | 동작중인 컨테이너 목록<br>-a 컨테이너 전체목록|
|docker rm | 컨테이너 삭제<br>(ex. docker rm 3ad434)<br>(ex. docker rm 3ad434, 72d342d)<br>(도커 전체삭제. ex. docker rm 'docker ps -a -q')<br>컨테이너ID전부를 입력할 필요 없다.|
|docker image | docker로 생성 및 local에 받은 이미지 목록 조회 |
|docker exec | 도커로 띄운 컨테이너 안에 명령을 실행하는 것.<br>(docker `옵션` `컨테이너이름/ID` `명령` `매개변수`)  대개 아래예시로 사용.<br>(ex. docker exec -it mysql bash)<br>(ex. docker exec -it a483b3 bash)==>이 경우, 컨테이너ID를 전부입력하지 않아도 된다. |

## VSCode-Docker연동
```

1. VSCode 설치
2. extension 들어가서
   Remote Development 설치
   Docker, Docker Explorer 설치
   

```

## docker이미지 생성(nodejs기준)
> url : https://velog.io/@koozzi/Docker-Node.js%EB%A1%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80%EB%A5%BC-%EC%83%9D%EC%84%B1%ED%95%98%EA%B3%A0-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%8B%A4%ED%96%89%ED%95%B4%EB%B3%B4%EA%B8%B0 <br>
> DetailDesc : https://javacan.tistory.com/entry/docker-start-7-create-image-using-dockerfile <br>

```

// 도커이미지 생성에 대한 설명이 DetailDesc 잘 되어있다.

1. server.js (app.listen하는 js파일) 위치의 동일레벨에 파일 두개 생성
   Dockerfile , .dockerignore (대소문자 철저히 구분할 것)

2. Dockerfile, .dockerignore 각 설명
2.1. Dockerfile
     도커 이미지 생성 프로세스에 대한 정의
     
     ex.
     // Dockerfile
      FROM node:12

      WORKDIR /Users/chihunkoo/Desktop/test
      // 현재 위치(경로)를 확인하려면 server.js가 위치한 디렉토리에서 'pwd' 명령어를 입력하면 된다.

      COPY package*.json ./

      RUN npm install
      COPY . .

      EXPOSE 3000
      CMD [ "node", "server.js" ]
     
2.2. .dockerignore
     이미지 생성시 무시할 파일 형식
     
     ex.
      // .dockerignore
      node_modules
      npm-debug.log
     
3. Docker image 빌드

   linux기준.
   
   docker build --tag [docker hub ID]/[Image Name]:[tag_name]
   docker build --tag auto/bitcoin:1.0
   
   버젼이 변경시, autobit:1.1 이런식으로 다시 빌드한다.
   (docker images로 체크하니 용량이 그대로 복사되던데 이건 체크 필요)

   build가 끝나면, docker images로 실제 이미지가 존재하는지 체크.
   
4. Docker run
   
   // nodejs기준. ==> 이걸로 실행하지말고,
   docker run --rm -d autobit:1.0 
   nodejs로 설정된 프로젝트를 띄울시, -d옵션을 주지 않으면 헤어나올수 없다.
   포그라운드로 띄우니까 컨트롤C도 안먹고 식겁했다.
   (물론, 그래도 나올 방법은 있겠지만, 난 모르겠다.)
   
   서버가 동작하고, 접근하기 위해서는 그 컨테이너의 고유IP를 확인해야한다.
   
   
   // 아래 명령어로 기동할 것
   // (참고) http://labs.brandi.co.kr/2018/05/25/kangww.html (nodejs클러스터링)
   docker create --name autobit -p 8001:8001 autobit:1.0
   docker start autobit
   
   docker ps로 확인시 autobit가 활성화 된 걸 확인할 수 있고, listenPost가 0.0.0.0:8001인걸 확인할 수 있다.
   이러면 외부에서 linux서버IP로 접근이 가능하다.
   
   docker stop도 docker run으로 띄운것과 동일하게 docker ps목록에 나타난 CONTAINER ID로 죽일 수 있다.

```

## 도커 디버깅과 docker commit
> `도커디버깅`에 관한 내용을 찾던 중. 알게된 사항. <br>

```

원래 도커 내부를 보기위해서 docker exec 명령어를 사용했는데
아예 뜨지 않는 도커를 탐색하는 방법은 없더라.

그래서, 찾아보던 중, docker commit를 통해 죽을때의 상태로 만들어
그걸 다시 띄워서 탐색했다.

mysql의 lower적용하다가 죽었는데,
해당명령어로 발생의 근원을 수정하고 다시 이미지를 떴다.

docker commit [컨테이너ID] [tag]
(ex. docker commit a3432v autobit:killed)

docker run -it autobit:killed bash

혹은 start후에 exec명령어로 접근하여 내용을 확인하면 된다.


```

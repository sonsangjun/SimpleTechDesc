# 정보
## 참고사이트
> setup : https://niceman.tistory.com/36 <br>
> setup(en) : https://www.liquidweb.com/kb/how-to-install-docker-on-centos-7/ <br>
> (참고만)url : https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html <br>
> VSCode-Docker연동 : https://89douner.tistory.com/123 <br>
<p/>

## 간단 명령어

|명칭 | 설명 |
|--|--|
|docker version | 도커 현재 버젼확인 client/server <br>만약, server정보가 없으면 docker deamon이 떠 있는지 확인<br>안떠있으면, setup링크를 참고해 띄울 것(systemctl명령어로 등록 및 서비스 시작해야함)|
|docker run | 도커 실행명령어.(ex. docker run -i -t centos /bin/bash)<br>-option설명<br>`-d : 백그라운드 모드 실행`<br>`-it : 터미널 입력상태로 컨테이너 실행`<br>`--rm 프로세스 종료시 컨테이너 자동으로 삭제`<br>다양한 옵션은 위의 링크 참고.<br> |
|docker stop | docker ps 으로 조회된 프로세스 중, 살아있는 (STATUS가 Up 1 Minutes 이런것) 걸 죽이는 명령어<br>docker stop ${CONTAINER ID}<br>ex. docker stop ba435cdc3368|
|docker image | docker로 생성 및 local에 받은 이미지 목록 조회 |

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
   docker build --tag autobit:1.0
   
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

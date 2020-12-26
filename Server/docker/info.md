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

## VSCode-Docker연동
```

1. VSCode 설치
2. extension 들어가서
   Remote Development 설치
   Docker, Docker Explorer 설치
   

```

## docker이미지 생성(nodejs기준)
> url : https://velog.io/@koozzi/Docker-Node.js%EB%A1%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80%EB%A5%BC-%EC%83%9D%EC%84%B1%ED%95%98%EA%B3%A0-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%8B%A4%ED%96%89%ED%95%B4%EB%B3%B4%EA%B8%B0 <br>

```

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

```

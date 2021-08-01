# 참고 
> url(Ngnix) : https://myjamong.tistory.com/9 <br/>
> url(Ngnix) : https://velog.io/@jeff0720/2018-11-18-2111-%EC%9E%91%EC%84%B1%EB%90%A8-iojomvsf0n <br/>
> url(nodejsSSO) : https://m.blog.naver.com/PostView.nhn?blogId=scw0531&logNo=221175584994&proxyReferer=https:%2F%2Fwww.google.com%2F <br/>
> url(Ag9Proxy) : https://daddyprogrammer.org/post/4245/angular2-httpclient-proxy/ <br/>
> url(dockerNgnix) : https://frontmulti.tistory.com/69 <br/>
> url(nginxDetail) : https://gigas-blog.tistory.com/233 <br/>

## ngnix(Web)와 nodejs(Was) 연동
> 예전에는 막연하다고 생각했는데, 의외로 설정이 간단하다. <br/>
> ng serve로 angular테스트시, proxy.json을 설정하는 경우가 있을 것이다. <br/>

```
ng serve --proxy-config proxy.json

// proxy.json 내용.
{
  "/api/*" : {
    "target" : "http://127.0.0.1:8011/",
    "secure" : false,
    "logLevel" : "debug"
  },
  
  "/mail/*" : {...블라블라...}
}

요런식으로 설정했었지...

```

> ngnix도 비슷하게 설정한다. <br/>
> 설치방법은 위의 링크를 참고하면 된다. <br/>
> (나는 docker로 설정하여, 스크립트에 집어넣던가 할 계획.) <br/>

```
// 이부분은 도커스크립트에 잘 넣어서 docker build시 적용되도록 한다.

// 대상파일
// .../nginx/conf/nginx.conf

// 변경부분

...
server {
  listen 80;
  server_name localhost;
  
  location /deal { ... }
  location /mail { ... }

}

// location 객체의 상세내용
// 1.
location /{블라블라} {
    proxy_set_header    Host $http_host;
    proxy_set_header    X-Real-IP $remote_addr;
    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header    X-Forwarded-Proto $scheme;
    proxy_set_header    X-NginX-Proxy true;

    proxy_pass http://127.0.0.1:8001;
    proxy_redirect      off;
    charset utf-8;

}

// 2. 
location /{블라블라} {
    proxy_pass http://127.0.0.1:8001;
}


// 1.은 프록시 관련 헤더설정이 있는데, 커스텀헤더인지, 기본헤더인지 찾아봐야겠다.
// 가능하다면, 헤더에 SSO값을 넣어서 비 로그인시 접근을 차단해버리던가 해야지.


```


## docker N Nginx
> URL을 참고해서 진행하지만, run을 하지 않고, <br/>
> create 및 build후 생긴 컨테이터ID를 기준으로 start을 진행한다.
>> 1. 쉘 스크립트 작성
```bash
homepath='/home/abcdefh'     # cd 입력시 들어가는 계정 최상위 경로.
workspace='devWorkspace'     # dev or real
projectName='webserver'      # bitcoinWebserver
dockerName='webserver_real'  # _dev or _real
timestamp=`date +%Y%m%d%H%M` # 현시간
listenport=80:80             # 리스닝포트

# 도커내 시간대 설정위한 작업
ln -sf /usr/share/zoneinfo/Asia/Seoul $homepath/etc/localtime
rm -rf $homepath/tmp/nginx

# 빌드를 위해 파일이 떨어지는 임시경로 설정
cd $homepath/workspace/$workspace/$projectName/
mkdir -p $homepath/tmp/nginx

cp -r * $homepath/tmp/nginx
cd $homepath/tmp/nginx

# dockerfile 분기처리
mv $homepath/tmp/nginx/Dockerfiles/Cloud/Dockerfile $homepath/tmp/nginx/Dockerfile

# build and Timezon Linking
# -v host경로 docker경로
# -v 경로는 절대경로를 삽입해야함.
## !주의 '\'로 이어지는 명령어 사이에 `#`같은 주석을 넣으면 쉘 실행시 에러남.
## (이어지던 명령어가 #을 기준으로 끊어져서 `-e`는 독립된 명령어로 인식함.)
docker build --tag auto/$dockerName:$timestamp .
docker create --name $dockerName -p $listenport \
 -v $homepath/log/nginx/:$homepath/log/nginx/ \
 -v $homepath/etc/localtime:/etc/localtime:ro \
-e TZ=Asia/Seoul \
auto/$dockerName:$timestamp 

echo '['$dockerName'] docker build complete.'
```

>> 2. dockerfile세팅
```docker
# 서버는 Dockerfile 이미지 사용
FROM nginx

WORKDIR /home/abcdefh/usr/tmp/nginx

COPY . .

ENV NODE_ENV production

ENV PORT 80
EXPOSE 80
```
>> 3. 쉘 실행 및 결과체크
```bash
[abcdefh@orderbitcoin Cloud]$ sh realBuild.sh
Sending build context to Docker daemon  10.75kB
Step 1/6 : FROM nginx
 ---> 08b152afcfae
Step 2/6 : WORKDIR /home/abcdefh/usr/tmp/nginx
 ---> Running in 94d84c367ed4
Removing intermediate container 94d84c367ed4
 ---> 73a9af37e27c
Step 3/6 : COPY . .
 ---> d38e1e62fa04
Step 4/6 : ENV NODE_ENV production
 ---> Running in 8c7455b7f1ce
Removing intermediate container 8c7455b7f1ce
 ---> 6c3962cbea43
Step 5/6 : ENV PORT 80
 ---> Running in c1c9ea37a864
Removing intermediate container c1c9ea37a864
 ---> d896c4994d6c
Step 6/6 : EXPOSE 80
 ---> Running in b6e09033a180
Removing intermediate container b6e09033a180
 ---> 2853f60cad90
Successfully built 2853f60cad90
Successfully tagged auto/webserver_real:202108010659
9647908b190e3e9d1e4afafd7a0bf897687ccbcf75f7bf8000b8a3e582cd6e70
```
>> 4. docker start `컨테이너ID` 시작
```cmd
docker start 9647 
```
>> 5. 완료 (컨테이너ID:9647...)
```cmd
CONTAINER ID   IMAGE                                      COMMAND                  CREATED          STATUS             PORTS                            NAMES
9647908b190e   auto/webserver_real:202108010659           "/docker-entrypoint.…"   23 seconds ago   Up 4 seconds       0.0.0.0:80->80/tcp        webserver_real
3af7e1a2572e   auto/autobit_fu_moni_real:202105311347     "docker-entrypoint.s…"   2 months ago     Up 2 days          0.0.0.0:9999->9999/tcp    autobit_fu_moni_real
```


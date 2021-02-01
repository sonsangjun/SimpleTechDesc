# 참고 
> url(Ngnix) : https://myjamong.tistory.com/9 <br/>
> url(Ngnix) : https://velog.io/@jeff0720/2018-11-18-2111-%EC%9E%91%EC%84%B1%EB%90%A8-iojomvsf0n <br/>
> url(Ag9Proxy) : https://daddyprogrammer.org/post/4245/angular2-httpclient-proxy/ <br/>

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



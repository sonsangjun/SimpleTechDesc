## nodejs 설치

```
# 설치전 먼저 진행.
yum update
yum install epel-release

# 완료후 nodejs 설치 진행.
yum install nodejs

```

<br/>

## 개발/운영 환경분리
> url : https://devhyun.com/blog/post/23 <br>
> url : https://qastack.kr/programming/11104028/process-env-node-env-is-undefined <br>
```

NODE_ENV값에 의해 개발/운영 환경이 결정된다.

NODE_ENV값은 서버의 환경변수로 설정되어 있어,
개발이나 운영서버 배포시 환경변수만 세팅하면 
node에 굳이 파라메터를 주거나, 신경을 쓸필요없이 바로 서버를 띄울수 있다.

다만, 로컬이나 개발/운영 스위칭이 조금 불편하다.
IDE내에서 stg/dev/real 바꿀 수 있으면 좋은데...
(BAT나 sh로 만들어야지.)


여튼,

개발 : 
window [set  NODE_ENV=development ]
linux  [export NODE_ENV=development] OR
       [NODE_ENV=development node server.js]

운영
window [set  NODE_ENV=production ]
linux  [export NODE_ENV=production]
       [NODE_ENV=production node server.js]

*주의
Windows '='사이 공백이나 '"' 까지도 값으로 포함하므로 주의

```

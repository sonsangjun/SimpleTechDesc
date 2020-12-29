## 개발/운영 환경분리
> url : https://devhyun.com/blog/post/23 <br>

```

NODE_ENV값에 의해 개발/운영 환경이 결정된다.

NODE_ENV값은 서버의 환경변수로 설정되어 있어,
개발이나 운영서버 배포시 환경변수만 세팅하면 
node에 굳이 파라메터를 주거나, 신경을 쓸필요없이 바로 서버를 띄울수 있다.

다만, 로컬이나 개발/운영 스위칭이 조금 불편하다.
IDE내에서 stg/dev/real 바꿀 수 있으면 좋은데...
(BAT나 sh로 만들어야지.)


여튼,

window : 
set NODE_ENV = 'production'
set NODE_ENV = 'development'

linux : 
export NODE_ENV = 'production'
export NODE_ENV = 'development'

아니면, $NODE_ENV = 'production' node server.js
이렇게도 사용할 수 있다.

```

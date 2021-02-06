# 정보

url : https://ebbnflow.tistory.com/209?category=745851 <br>
Server데몬 : https://november11tech.tistory.com/146 <br>
(서버를 로그아웃 하더라도 떠있을 수 있게 설정)<br>
express설치 : https://junspapa-itdev.tistory.com/7 <br>
(생각보다 쓰이는 웹 프레임워크)<br>
route분리 : https://backback.tistory.com/341 <br>
(controller, service 처럼 분리느낌이라 생각했는데, 내가 아는 spring controller와는 조금 다른 느낌) <br>

## package.json
> url : https://hyunjun19.github.io/2018/03/23/package-lock-why-need/ <br>

```
당연히, package.json만 커밋하고 node_modules나 package-lock.json은 무시처리했는데,

node_modules를 npm install로 다시 설치하는 환경이라면 
package-lock.json이 필요하네.

```

## module 구성
> 코드가 길어지고, 여러기능이 혼재하면 하나 둘 분리하기 마련이다. <br>
> 그 와중에 실수나 몰랐던 사항이 있어 적어둔다. <br>

```javascript
//////////////////////////////////////////////////////////
// aaaSql.js

const logger = require('../conf/winston');

// ...

let sqlObj = {};

sqlObj.aaa = function(){...} ;
sqlObj.bbb = function(){...} ;

// ...

module.exports = sqlObj;

//////////////////////////////////////////////////////////
// aaa.js
const aaaSql = require('../sql/aaaSql');
const express = require('express');
let router = express.Router();

router.get('/aaa',function(req, res, next){
  aaaSql.aaa(); // aaaSql.js에서 정의한 Method
  //...
});

//...

module.exports = router

```
> 마지막 줄의 `module.exports = sqlObj;`을 정의하지 않으면 <br>
> aaaSql을 require하는 코드는 aaaSql내 정의된 Method를 사용할 수 없다. <br>
> 생각보다 잘 까먹는 점이므로 꼭 상기하기.<br>

## nodejs 서버 보안
> url : https://m.blog.naver.com/PostView.nhn?blogId=cck223&logNo=221019399455&proxyReferer=https:%2F%2Fwww.google.com%2F <br>
> url(ssl) : https://blog.naver.com/cck223/220959771753 <br>

```
향후, 프론트앤드도 붙이면서
외부에서 보려고 페이지를 노출시키려는 경우, 

미리고려해봐야할 사항이다.
누가 하꼬페이지를 공격하겠냐만은 서버로그보면 24시간 내내 서버 비밀번호가
틀렸다는 메시지가 뜨는걸로 봐서는...


```

## nodejs 스케쥴링.

> url : https://velog.io/@filoscoder/%EC%8A%A4%EC%BC%80%EC%A4%84-%EC%97%85%EB%AC%B4-%EC%9E%90%EB%8F%99%ED%99%94-Node-cron-vs-Node-schedule-%EB%B9%84%EA%B5%90-clk4dyynve <br/>
> url(node-cron) : https://miiingo.tistory.com/180 <br/>
```
nodejs의 반복작업을 settimeout으로 돌리고 있는데, 좀 더 안정적으로 돌려야해서, 라이브러리를 사용하기로 함.
(가끔 새벽에 반복작업이 멈춰버리는 경우가 있어, 심히 곤란.)


```

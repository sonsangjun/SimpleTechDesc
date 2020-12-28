# 정보

url : https://ebbnflow.tistory.com/209?category=745851 <br>
Server데몬 : https://november11tech.tistory.com/146 <br>
(서버를 로그아웃 하더라도 떠있을 수 있게 설정)<br>
express설치 : https://junspapa-itdev.tistory.com/7 <br>
(생각보다 쓰이는 웹 프레임워크)<br>
route분리 : https://backback.tistory.com/341 <br>
(controller, service 처럼 분리느낌이라 생각했는데, 내가 아는 spring controller와는 조금 다른 느낌) <br>


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

```
> 마지막 줄의 `module.exports = sqlObj;`을 정의하지 않으면 <br>
> aaaSql을 require하는 코드는 aaaSql내 정의된 Method를 사용할 수 없다. <br>
> 생각보다 잘 까먹는 점이므로 꼭 상기하기.<br>

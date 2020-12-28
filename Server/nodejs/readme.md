# 정보

url : https://ebbnflow.tistory.com/209?category=745851 <br>
Server데몬 : https://november11tech.tistory.com/146 <br>
(서버를 로그아웃 하더라도 떠있을 수 있게 설정)<br>
express설치 : https://junspapa-itdev.tistory.com/7 <br>
(생각보다 쓰이는 웹 프레임워크)<br>
route분리 : https://backback.tistory.com/341 <br>
(controller, service 처럼 분리느낌이라 생각했는데, 내가 아는 spring controller와는 조금 다른 느낌) <br>

## Router분리
> app.use외에 여러옵션이 있어 정리해둔다.<br>

|명칭|설명|
|--|--|
|app| const app = require('express'); <br>아마 이런식으로 프레임워크 import |
|app.use | 대상 : 모든 HttpMethod<br>Url만 맞으면 실행 |
|app.get | 대상 : Get HttpMethod |
|app.post | 대상 : Post HttpMethod |
|app.patch | 대상 : Patch HttpMethod |
|app.delete | 대상 : Delete HttpMethod |

> 이건, route.js 정의시에도 동일하게 적용된다. <br>

```javascript
// routes/exam.js

const express = require('express');
const router = express.Router();

router.get('/member', function(req, res, next) {...} );
router.post('/person', function(req, res, next) {...});
router.delete('/aabb', function(req, res, next) {...});

// ... 이렇게 쓸 수 있다.

```

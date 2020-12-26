# route분리
> server.js에만 담기에는 너무 코드가 길어지므로, <br>
> java service파일 분리하듯이, 분리가 필요하다. <br>

```javascript
////////////////////////////////////////////
// server.js
const listPort = 8001;

var express = require('express');
var http = require('http');
var app = express();

app.set('port', listPort);
// ...

var examRouter = require('./routes/examRoute');

app.use('/exam',examRouter);

////////////////////////////////////////////
// examRoute.js
var express = require('express');
var router = express.Router();
var XMLHttpRequest =require('xhr2');

router.get('/member', function(req, res, next){
  res.send('member~~');
});

module.exports = router;

```
> 추가로.<br>
app.use 대신 'get/post/patch/delete'를 사용할 수 있다.<br>
위 모두 restfulAPI에서 언급하는 HttpMethod인데, <br>
클라에서 호출하는 방식에 따라, 서버에서 받고 싶은 HttpMethod만 받도록 route할 수 있다.<br>

```javascript

// 예.
// 테스트시, curl -X GET "Https://www.exam.co.kr/api/v1/memter?a=1"
app.get('/exam',examRouter); // HttpMethod가 Get인 녀석만 받음. 

// 테스트시, curl -X POST "Https://www.exam.co.kr/api/v1/memter/" -d "a=1&b=2"
app.post('/exam',examRouter); // HttpMethod가 Post인 녀석만 받음. 

// 기타등등

```

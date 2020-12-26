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

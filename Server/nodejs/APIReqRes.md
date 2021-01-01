# 정보
> url : https://lightcode.tistory.com/21 <br>

<p/>

## nodejs의 API호출시
> HttpMethod에 GET/POST/PATCH/PUT/DELETE 등이 있지만. <br>
> GET/POST만 일단설명<br>

```
API를 개발하다보니, GET에도 parameter를 싣어야 하는 상황.

express 프레임워크로 개발.
const router = express.Router();

// local nodemon을 띄웠다.
// URL : http://localhost/api/v1/exam?paramval=babo&paramval2=100

router.get('/exam',function(req, res, next){
  // 두가지 방식이 존재
  // 1.
  let paramval = req.query.paramval;    // 'babo' 
  let paramval2 = req.query.paramval2;  // 100
  
  // 2. (20210102, 이거는 deprecated라 가능하면 req.query나 req.body를 권장했다.)
  paramval = req.params('paramval');  // 'babo'
  paramval2 = req.params('paramva2'); // 100
  
  res.send('response success'); // 원래는 클라와 약속한 response를 넣어야 (json형태로)
});


```

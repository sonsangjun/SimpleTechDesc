# 정보
## nodejs 서버기동시, 로그남기기
> URL1 : https://dololak.tistory.com/126 <br>
> URL2 : https://basketdeveloper.tistory.com/42 <br>
> URL3 : https://velog.io/@ash/Node.js-%EC%84%9C%EB%B2%84%EC%97%90-logging-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-winston-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0 <br>
> URL4 : https://mungmungdog.tistory.com/34 <br>
winston이란 모듈을 사용한다. <br>
URL3은 Logging설정에 대한 체계적인 정보<br>

```
1. 설치
  npm install winston --save
  npm install winston-daily-rotate-file --save
  npm install moment --save
  
  (--save하고, 나중에 package.json을 통해서 받으면 된다.)
  
2. Logger 설정
  로거 설정하고, require를 통해 사용하는 방식인 것 같다.
  URL2를 참고하면 좋고.
  
  winston.js에 logger를 정의하고,
  다른 routes같은 js에서 require를 통해 import하여 사용하면 된다.

* 적용시, syntax에러가 발생하면, 오류지점의 개행된 코드를 일자로 정렬한다.
  (nodejs로 동작시, 코드 해석하는 방식이 브라우저와 살짝 다른것 같다.)
  
* 로그레벨 설정방법
  생각보다 로그레벨 설정하는 위치를 공유하는 사이트는 잘 없더라.
  그냥, 레벨이 조정가능하다. 정도만 적어놓을뿐
  
  선언위치는 createLogger의 json파라메터 최상단이다.
  
  winston.createLogger({
  //...
  
    level : 'debug',
  
  //...
  });
```

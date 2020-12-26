# 정보
## nodejs 서버기동시, 로그남기기
> URL1 : https://dololak.tistory.com/126 <br>
> URL2 : https://basketdeveloper.tistory.com/42 <br>

winston이란 모듈을 사용한다.

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

```

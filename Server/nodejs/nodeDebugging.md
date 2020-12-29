# 정보
> 설치 <br>
> url : https://meyouus.tistory.com/62 <br>
> <br>디버깅<br>
> url : https://meyouus.tistory.com/63 <br>
> url : https://nerv2000.tistory.com/105 <br>
> <br>
> 서버에서만 디버깅을 진행했는데, 이제는 <br>
> PC에서도 디버깅을 해야할 것 같아 세팅법을 간단히 기술한다. <br>

```

1. nodejs 설치(부수적인것 전부설치할 것)
2. node --inspect server.js 
   //(server.js는 내가 서버테스트 중이라서, 원래 디버그 대상을 입력하면된다.)
   
3. chrome://inspect로 접근하면, 안드로이드 디버깅 하듯이 잠시후에 목록에 나타난다.
3.1. vsCode 디버깅 방법...은 확인중이다. (일단 cmd가 편해서 이걸로 하다가 찾아봐야지)
```

## nodemon을 이용한 실시간 변경
> url : https://blog.outsider.ne.kr/649 <br>

```
 소스 변경시 node를 재시작하지 않고 바로 확인할 수 있는 기능
 1. npm install nodemon -g 
   (--save 옵션을 넣어 package.json에 포함시켜도 된다.)
   
 2. 설치완료하면, nodemon 입력
   (nodemon server.js, nodemon --inspect server.js 등등 편한대로 실행)
   
```

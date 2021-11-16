## 개요
> vue 관련 내용을 정리한 폴더이다.

## 초기설정
> URL
```
(vue)        https://doncolmi.github.io/Vue-1/
(웹팩)       https://www.youtube.com/watch?v=40KdBVS2UP4&list=PLCu4XCCcMtSLOmEBy-3_wGkHOgO-_hJMY
(Plugin설명) https://joshua1988.github.io/web-development/vuejs/boost-productivity/
```

설정은 VSCode기준으로 진행했다.  
nodejs를 이용한 서버를 작성한 시점이라서, node설정에 관한 내용은 URL참고할 것.  
<br/>

### npm을 통한 설치 대상
```
목록
webpack
webpack-cli
vue
vue-loader
vue-templete-compiler

명령어
npm install webpack webpack-cli -D
npm install vue 
npm install vue-loader vue-templete-compiler -D
```


* D는 개발시에만 사용하겠다고 약속 (package.json에 devDependencies에 추가됨).  
* npm 을 통한 설치전에, npm init를 통해 package.json을 생성하는 초기화 과정이 필수.  
* vue/vue-template-compiler 버젼은 모두 동일하게 유지해야하며, 버전 미 기록시 `최신 버전으로 설치된다.`  
  (설치시 버전설정은 `vue@2.6.0` `vue-template-compiler@2.6.0` 처럼 하면 된다.)  
* 간단하게 제작하려면, webpack 설치하지 말고, html에 templete/script를 모두 작성하면 된다.  
  (vue는 `<script src='https://cdn.jsdelivr.net/npm/vue@2.6.0'></script>`로 삽입하면 된다.)  
  (정말 테스트 용으로만 사용하고, 실제 개발에는 권하지 않는다. 너무 손이 많이가...)  

### VSCode 설정시, 사용한 플러그인 항목

|명칭|설명|
|--|--|
|Live Server|로컬에 nginx나 was따로 안띄워도 된다. 이걸로 그냥 퉁치면 됨|
|Prettier | 코드 작성시, 자동 정리(포맷팅)
|Vetur | 개발툴
|vue | Vue 문법 검사
|Vue VSCode Snippets | 자동완성

> Live Server  
```
html로 작성하고, 해당 페이지의 오른쪽 마우스 클릭후 나타나는 메뉴에서

`Open With Live Server`

클릭하면, 로컬에 웹서버가 기동된다. (동시에, 브라우저에 나타난다.)
페이지 변경시 바로바로 반영된다.

* webpack을 통해 작업하면, 새로 반영된 부분은 npm run build 전까지는 볼 수 없다.
  (vue 컴파일 및 스크립트 병합이 끝나고 나서야 볼 수 있다.)
```

![image](https://user-images.githubusercontent.com/12105781/141991296-60a65ef5-af79-4a1f-9e27-7c7612b0ad1e.png)


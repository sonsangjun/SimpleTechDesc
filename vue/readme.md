## 개요
> vue 관련 내용을 정리한 폴더이다.

## 설정방법
### 직접설정(vue init 미사용)
> URL
```
(vue)        https://doncolmi.github.io/Vue-1/
(웹팩)       https://www.youtube.com/watch?v=40KdBVS2UP4&list=PLCu4XCCcMtSLOmEBy-3_wGkHOgO-_hJMY
(Plugin설명) https://joshua1988.github.io/web-development/vuejs/boost-productivity/
```

설정은 VSCode기준으로 진행했다.  
nodejs를 이용한 서버를 작성한 시점이라서, node설정에 관한 내용은 URL참고할 것.  
<br/>

#### npm을 통한 설치 대상
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


* -D는 개발시에만 사용하겠다고 약속 (package.json의 devDependencies에 추가됨).  
* npm 을 통한 설치전에, npm init를 통해 package.json을 생성하는 초기화 과정이 필수.  
* vue/vue-template-compiler 버젼은 모두 동일하게 유지해야하며, 버전 미 기록시 `최신 버전으로 설치된다.`  
  (설치시 버전설정은 `vue@2.6.0` `vue-template-compiler@2.6.0` 처럼 하면 된다.)  
* 간단하게 제작하려면, webpack 설치하지 말고, html에 templete/script를 모두 작성하면 된다.  
  (vue는 `<script src='https://cdn.jsdelivr.net/npm/vue@2.6.0'></script>`로 삽입하면 된다.)  
  (정말 테스트 용으로만 사용하고, 실제 개발에는 권하지 않는다. 너무 손이 많이가...)  

#### VSCode 설정시, 사용한 플러그인 항목

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



### Vue init을 통한 설정 (작업중..., 올바르지 않은 내용입니다. 참고하지 마세요!)

> Vue-cli 3버전대부터 이렇게 설정이 가능하다.  
> 다만, webpack.config.js를 root에 두는게 아니라, 내부적으로 관리한다구 한다.  
> 직접 수정은 좀 그렇고, `vue.config.js`파일이 있어, 그걸 수정하면 된다.  
> 위의 설정방법은, webpack-vue가 어떻게 연동되는지(구조)를 알 수 있는 좋은 방식이다.  

```
프로젝트 만들고, 아래 명령어를 입력한다.
(물론, 프로젝트 경로 root에 들어와야 한다. VSCode에서 터미널을 열면 바로 위치해있고,
 아니라면, cd명령어로 찾아들어오면 된다.)
 
 vue init
 ```
 
 > vue init 실행중 보안오류 발생시. 
 > 참고 : https://www.inflearn.com/questions/17423
 > 참고(@의미) : https://stackoverflow.com/questions/36667258/what-is-the-meaning-of-the-at-prefix-on-npm-packages
 ```
 아래 명령어 실행할 것. (윈도우 환경만)
 vue.cmd create vue-cli
 
 그런데, vue-cli 버젼이 3미만이면 아래 메시지가 추가로 나타난다.
  vue create is a Vue CLI 3 only command and you are using Vue CLI 2.9.6.
  You may want to run the following to upgrade to Vue CLI 3:

  npm uninstall -g vue-cli
  npm install -g @vue/cli
  
  
~~

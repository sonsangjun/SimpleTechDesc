# 정보
> url : https://nubiz.tistory.com/703 <br>
> url : https://m.blog.naver.com/umpirage98/221457951923 <br>
> url(oauth2) : https://velog.io/@piecemaker/OAuth2-%EC%9D%B8%EC%A6%9D-%EB%B0%A9%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90 <br>
> url(oauth2 Detail) : https://kibua20.tistory.com/70 <br>
```

Gmail을 통한 메일전송방법이 암호를 평문으로 넣는 예제만 존재해서,
고민이 많았다.

내 비밀번호를 평문으로 노출해...? 다른사이트에서 사용하는데.


찾아보니, oauth2 인증방식이 있어 기술한다.

1. https://console.developers.google.com/apis 접속
1.1. gmail API 생성
1.2. oAuth 동의페이지 생성(필수값은 적당히 맞춰서 넣으면 된다.)
     사용처는 '웹 서버' , 1개 이상사용함.
     
2. oAuth 동의페이지 생성되었으면, oAuth 클라이언트ID 생성
   리다이렉트URL : https://developers.google.com/oauthplayground/
   자바스크립트는 등록할 필요없다.
   
3. 클라ID, 클라비번이 생성된다.
   
   접속 : https://developers.google.com/oauthplayground/ 
   톱니바퀴를 클릭하여,
   
   하단 Use your own OAuth credentials 체크
   후,
   
   클라ID / 클라Secret 입력
   
4. Step1, Step2를 따라가며 Authorization Code / Refresh token / Access token을 차례대로 발급받아 저장
5. nodejs에 가서 미입력항목 입력하기. (access token이 한시간마다 만료되는데, 정말 인증갱신이 되는지 모르겠다.)
   (https://blog.eunsatio.io/develop/nodemailer%EC%99%80-gmail%EB%A1%9C-%EB%A9%94%EC%9D%BC-%EB%B0%9C%EC%86%A1%ED%95%98%EA%B8%B0-%E3%85%A1-OAuth2)



```

## 인증과 권한
> 정말 단어가 헷갈리는데, 어정쩡하게 알 수는 없는 노릇이니.   
> 참고로 등록해둔다.   
<br>

> 참고(인증과 권한) : https://hanee24.github.io/2018/04/21/authentication-authorization/ <br>
> SSO인증방식 : https://toma0912.tistory.com/75 <br>
<br>

> 2020년에 진행한 프로젝트에서 SSO인증을 통한 Login을 적용했는데,   
> 여기는 SSO서버가 따로 존재한다.   
<br>

> 인증대행모델 or 인증정보전달모델 두가지중 구체적으로는 잘 모르겠다.   
> Session이나 클라가 던진 API의 request body에 ssoToken이 존재하고,   
> ssoToken의 만료(expire)여부에 따라 request를 거부할지 말지를 결정한다.   
> (채널) -> (기간계)에서 기간계 접속전에 끊기는 걸로 봐서는   
> 인증대행모델에 가까워보인다.   
> 기간계까지 접근해서, 각 시스템이 인증토큰을 비교하는 Sql로그를 본적이 없다.   

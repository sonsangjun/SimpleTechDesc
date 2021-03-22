# Slack 설명
## Slack API 연동방법
> 참고 <br/>
> https://gaemi606.tistory.com/187 <br/>
> https://miaow-miaow.tistory.com/148 <br/>
> http://labs.brandi.co.kr/2019/01/30/kwakjs.html <br/>

```
1. 접속 https://api.slack.com/apps 
   생성한 이력이 없는 경우 
   초록색 버튼으로 
   
   'Create an App'
   
   단추가 있을 것이다.
   
2. AppName과 Development Slack Workspace 를 적당히 입력하시고.

3. Add features and functionality 항목에서 Bots 클릭
   (작성자는 Bots을 사용할 목적)
   
4. Features의 AppHome으로 이동한다면, 순서대로 따르는게 좋다.
   a. First, assign a scope to your bot token
      우선, 봇토큰의 스코프를 할당해줘.
      봇에게 어디까지 활동시킬건지 범위를 지정해주는듯 하다.
      
      'Review Scopes to Add'
      
      클릭시, Features의 oAuth & Permission 페이지로 이동한다.
   
   b. 페이지에서 쭉 내리면, 'Scopes'가 있다.
      Bot token의 범위(scopes)를 지정하면 된다.
      
      'Add an OAuth Scope'를 클릭해 범위를 추가한다.
      영어로 된 스코프 설명을 일일이 읽어보면 좋겠지만, 그럴시간이...
      
      'chat:write'        : Send message as 채널. 
                            ( 채널로 메시지를 보낸다. )
      'app_mentions:read' : View messages that directly mention 채널 in conversations that the app is in.
                            (채널이 직접적으로 언급된 메시지를 앱내 대화창에 보여준다.)
                            
      두 개 옵션을 주면, 
      채널로 메시지를 보내고 (채널을 직접언급한)
      채널을 언급한 메시지를 대화창에 보여준다.
      
      로 보면 되지 않을까 싶은데...
      
   c. 스코프를 추가하면, 상단의 Install to workspace 버튼이 활성화 된다.
      그거 '누'르면 된다.
      
   d. '허용'할지 말지 물어본다. 빨랑 허용하자.
   
   e. OAuth Tokens for Your Team 한 토큰이 발행되었다.
   
5. Slack에 App추가
   a.0. Url로 직접들어가려면, https://app.slack.com/client/블라블라...
        모르겠으면.
   
   a.1. https://slack.com/intl/ko-kr/
        로 들어가서 로그인 하시고.
   
        우측상단에 SLACK실행 클릭해서 당신의 Workspace를 들어가면 된다.
        (아까 앱만들때 선택한 워크스페이스를 선택하면 된다.)
        
   b.   왼쪽 하단에 '+앱 추가' 버튼을 클릭한다.
        
   c.   아까 만든 앱 이름을 검색한다.
        딱, 한개 뜰것이다.
        
        누르면, 워크스페이스에 추가된다.
   
6. Nodejs의 Slack모듈설치
   궁극적인 목표는 Nodejs서버에서 메시지를 보내는 것이다.
   
   Nodejs로 구성된 프로젝트 폴더로 이동하여 (package.json이 있는 폴더)
   아래 명령어를 실행한다.
   
   npm install slack-node --save
   
7. JS코드에서 테스팅.
   
8.  


```

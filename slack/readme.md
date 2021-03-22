# Slack 설명
## Slack API 연동방법
> 참고 <br/>
> https://gaemi606.tistory.com/187 <br/>
> https://miaow-miaow.tistory.com/148 <br/>

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
   
5. 
      


```

# Slack 설명
## Slack API 연동방법
> 참고 <br/>
> https://gaemi606.tistory.com/187 <br/>
> https://miaow-miaow.tistory.com/148 <br/>
> https://slack.dev/node-slack-sdk/web-api <br/>

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
   
   d.   추가한 앱이 채널에 메시지를 뿌리기 위해서는 채널에 들어가있어야한다.
        추가한 앱을 클릭하고,
        
        
        `여기에서는 메시지를 보내고 @{AppName}와(과) 파일을 공유할 수 있습니다.`
        
        
        라고 메시지가 떠있을텐데... 그 사이에 있는 @{AppName}을 클릭한다.        
        을 클릭한다. (ex. 앱이름이 myFirstApp라면 @myFirstApp를 클릭)
        
        (앱을 채널에 추가안하면, Nodejs에서 Api호출시, invaild_auth 에러가 발생한다.)
   
   
6. Nodejs의 Slack모듈설치
   궁극적인 목표는 Nodejs서버에서 메시지를 보내는 것이다.
   
   Nodejs로 구성된 프로젝트 폴더로 이동하여 (package.json이 있는 폴더)
   아래 명령어를 실행한다.
   
   npm install @slack/web-api
   
7. JS코드에서 테스팅.
   테스트코드 작성은 어렵지 않다.
   슬랙 공식홈에서 설명해주고 있기도 하고,
   
```

```javascript
   대략적으로 적어보자면, 아래 꼴과 같다.
   
   //////////////////////////////////////////////////
   // 선언
   // 토큰은 dotenv같은걸로 관리해도 되고, DB에 저장하고 긁어와도 된다.
   const { WebClient } = require('@slack/web-api');

   // Read a token from the environment variables
   const token = process.env.SLACK_TOKEN;

   // Initialize
   const web = new WebClient(token);
   
   //////////////////////////////////////////////////
   // 호출
     const result = await web.chat.postMessage({
       text: 'Hello world!',
       channel: conversationId,
     });
     
     console.log(`Successfully send message ${result.ts} in conversation ${conversationId}`);
```

```json
8. 호출시, slack에 'Hello world'가 찍히면 성공이다.

   덤으로 성공시 json결과
   
   result==> {
     ok: true,
     channel: 'C01RL24BXXX
     ts: '1616423135.001300',
     message: {
       bot_id: 'B01S1KN5XXX
       type: 'message',
       text: 'Hello world!',
       user: 'U01RLL3DXXX
       ts: '1616423135.001300',
       team: 'T01HSNV3XXX
       bot_profile: {
         id: 'B01S1KN5XXX
         deleted: false,
         name: 'orderMsgXXX
         updated: 1616423XXX
         app_id: 'A01RYB13XXX
         icons: [Object],
         team_id: 'T01HSNV3XXX
       }
     },
     response_metadata: { scopes: [ 'chat:write' ], acceptedScopes: [ 'chat:write' ] }
   }
   
```

## 트러블슈팅
### initSlack fail. Error: An API error occurred: not_in_channel
> 봇이 채널이 속해있지 않다. (invalid_auth 오류도 이와 비슷한 사례인 경우가 있다.) <br/>
> 봇을 채널에 넣어주자. <br/>
> `5. Slack에 App추가` 항목 참고 <br/>


# TCP와 UDP
> 참고 : https://mangkyu.tistory.com/15 <br>
> UDP는 스타크래프트 할 때, IPX에서 UDP로 바뀌어서 그 존재는 알고 있었지만,   
> 대략적으로 알 뿐 자세히 알지는 못했다.   
<br>
링크의 가장 마지막 표를 참고하면 좋을 듯 싶다.   
<br>
내가 알고 있던 상식으로는   

```

TCP는 수신여부 확인 
--> 끊어졌을경우 다시 전송
--> 끊어졌으면 계속 그 지점부터 다시전송(속도느림)
--> 신뢰성 높음   
==> 서로 지속적으로 수신여부 소통하므로 연결성

UDP는 수신여부 X 일단 던지고 봄 
--> 받든 말든 알바아님.
--> 난 모르겠고, 내가 던질건 다 던짐(속도빠름)
--> 신뢰성 낮음
==> 어떻게 보면 일방통행 같아서 비연결형

이라 알고 있었는데,
내용을 보면 더 자세히 나와있다.

```

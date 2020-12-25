# 참고 
> url : https://johngrib.github.io/wiki/curl/ <br>

```

restfulAPI 테스트시 유용함.
binanceAPI 테스트시 사용

curl -H "X-MBX-APIKEY: zhOhErCUCrQ1I11bpGok2I1zMEdqrVbSZYCUHRImvZ5NfsM5bSgpsbUvqw8cFwWi" -X POST 'https://www.binance.kr/open/v1/orders' -d 'symbol=BTC_USDT&side=0&type=1&quantity=0.16&price=7500&timestamp=1581720670624&recvWindow=5000&signature=444a8f29979245878165cfe55e432cebc2b69b9f199097c90109c98d1646f2b5'

-H 헤더 따로 세팅(이러면 커스텀 헤더가 됨. 어차피 서버와 약속한 사항이라면 커스텀설정)
-X 보내! 여기서는 post 보냄(create)(HttpMethod)
-d requestBody설정

API호출테스트 말고도,
실제 서버 접속을 위한 방화벽테스트로도 사용된다.

```

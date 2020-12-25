# 참고
> url : https://m.blog.naver.com/PostView.nhn?blogId=seongjeongki7&logNo=220815883064&proxyReferer=https:%2F%2Fwww.google.com%2F <br>
> url : http://bahndal.egloos.com/422524 <br>
<p>

```
바이낸스API 테스트 중,
아래 명령어를 처음 사용해봐서 내용에 대해 기술함.

echo -n "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=30000&timestamp=1608874033454" | openssl dgst -sha256 -hmac "yQJYNNbblQ3Kd09sLKCBrhr5zKbFGxlJmF6jqFX1E45luxYhyBp4Re3nEjxyqsHP"

openssl hmacSha256값을 생성시.
key : "yQJYNNbblQ3Kd09sLKCBrhr5zKbFGxlJmF6jqFX1E45luxYhyBp4Re3nEjxyqsHP"
msg : "symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=30000&timestamp=1608874033454"
로 받는다.

echo -n의 '-n'은 엔터를 자동으로 적용하라는 의미.
실제, javascript로 작성된 CryptoJS.HmacSHA256(message, key); 를 사용하여.
key -> key
msg -> message에 각각 값을 입력하면 openssl의 결과와 동일함을 알 수 있다.

openssl와 echo -n을 조합사용하는 건 위의 참고페이지 같은 경우에 사용하는게 일반적인 것 같고,
해당 사항은 binanceAPI를 테스트 하던 도중 의문이 생겨 찾은 내용을 기반으로 해석한 것이다.

```

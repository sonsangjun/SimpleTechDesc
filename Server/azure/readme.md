# 정보 
> url : https://gsk121.tistory.com/399 <br/>

## MS Azure
> 마이크로소프트 클라우드 서비스

```
집 컴퓨터가 정말 싸지만, 관리측면에서 힘들어서 클라우드 서비스를 사용하기로 함.

서버 동작동안 벌어들인 이익이 서버유지비용 이상으로 나와서
별문제가 없으면 12개월 무료기간 끝나도 유지할 수 있을듯 싶다.


* 가상머신 생성하면서 생소했던 점.

1. VM을 고르라는데, 하드용량이 너무 작아서 고민했는데,
   하드용량은 따로 설정할 수 있더라. 쓸데없는 고민.
   
2. '진단 스토리지' 계정을 설정시, 웹에서 PUTTY같이 콘솔창이 나타난다.
   이건 정말 신기.

```

### DNS 설정
> 참고URL :  
> https://diane-space.tistory.com/225 <br/>
> https://brunch.co.kr/@topasvga/173 <br/>
> https://velog.io/@ruddms936/Domain-name-%EC%84%A4%EC%A0%95 <br/>
> 
```
 사이트 여러군데를 다 읽어봤는데,
 
 Azure에 나와있는 domain name server 1,2,3,4 의 존재이유를 알게되었다.
 저거를 도메인서버 관리회사에 등록요청을 해야한다.
 (한국은 도레지닷컴 같다.)
 (물론, 그 전에 www같은 URL을 미리 등록하고 진행해야한다.)
 
 물론, 돈이 든다. 
 돈도 돈인데, 이거 은근 귀찮네...
 
 그렇다고 IP로 접근시키기도 참...

```

## 확인사항.

```
1. podman이라는 docker와 비슷한 녀석하에 동작하는 가상머신때문인지.
   도커로 뭐든 컨테이너를 만들기만 하면 start시 Exited 139 에러가 발생한다.
   
   이거 때문에, 환경잡던 가상머신은 OS디스크만 남기고 삭제.
   개발/운영/DB 서버를 각각가져가기로 했다.
   
   나중에 웹단까지 개발한다면, 웹서버용 가상머신도 따로가져가야겠다.
   그러면, 총 4개가 필요하겠네.
   (웹서버도 개발/운영 나눠야하면 5갠데... 허어.)
   
```
> url : https://azuremarketplace.microsoft.com/ko-kr/marketplace/apps/cognosys.docker-ce-with-centos-7-7?tab=Overview <br/>
> url : https://portal.azure.com/?fromAccountsPortal=true#create/cognosys.docker-ce-with-centos-8-1docker-ce-with-centos-8-1 <br/>
```
↑
구독하란 의미다.
애초에 도커를 전용으로 사용할 수 있는 구독이 있었네...아 젠장.

공짜 도커도 있으니까, 찾아서 서버비용만 잘 지불하고 쓰면된다.

* 실제적용결과.
  잘 된다. docker run하니, STATUS가 UP된다. 역시, 전용 구독을 사용해야하네.
```

## 개요
> 웹서버(nginx) 설정시, 작업한 내용을 기술한 문서.

## 도메인 ~ SSL 설정
### 도메인 설정
> SSL을 설정하기 앞서서 먼저 도메인을 설정한다.  
> 그러기 위해, IP를 고정으로 변경한다. (이거는 간단히 변경가능)   
> `(중요!)` 클라우드 플레어 사용예정이라면 `도메인 관리툴`에 Azure네임서버를 등록하지 말고,  
> `클라우드 플레어 네임서버`를 등록해야한다.(레코드 셋은 그대로 설정을 진행한다.)    

```
1. IP를 고정IP( Static IP )로 변경.
   메뉴 잘 찾아가면 있다.
   (Azure기준, 고정IP는 비용이 부과된다. https://azure.microsoft.com/ko-kr/pricing/details/ip-addresses/)

2. 도메인 구입
   (해당 내용은 `도메인 구입방법` 항목참고)

3. 도메인 삽입
   참고 : https://mpain.tistory.com/212

   도메인을 샀으면,
   a. Azure포털에서 DNS Zone 생성
      - 리소스 그룹은 기존에 쓰던 그룹.
      - 이름은 아까 산 도메인을 Name칸에 입력  `ex. ssj.site`
      - 비용은 Azure기준, 약500원/월

   c. 생성완료되면 도메인 네임서버 확인가능 (4개까지 있음)
      
   d. 아까 산 도메인 관리사이트에 네임서버 등록하기.
     (등록후, 최대 48시간 소요됨. 당일에 확인 안될 수 있음.)
     (nslookup으로 확인가능, https://www.44bits.io/ko/keyword/cli--nslookup)
     (nslookup -> set type=ns (네임서버) -> 구매한 도메인 입력)
     (의외로 나는 1시간내 반영되었다. 이게 개인차기 있는 듯 하네.)

   e. DNS 레코드 등록.
      (참고 : https://www.cloudflare.com/ko-kr/learning/dns/dns-records/)
      (A    : 도메인의 IP를 가지고 레코드, 
              특별한 사항이 없으면, IP로 바로 매핑하기.
              https://www.cloudflare.com/ko-kr/learning/dns/dns-records/dns-a-record/)
      (IP는 그냥두지말고, 현재 가상머신의 고정IP를 입력한다.)

      레코드 등록이 끝났으면 다시 nslookup으로 테스트한다.
      (네임서버가 아니므로, set type=ns는 입력하지 않는다.)

      권한 없는 응답 : 
      이름 : ssj.site
      Address : 142.250.199.99

      이런 식으로 나오면 OK (이건 그냥 예시임.)

   
   * 다행히 운이 좋아서,
     모든 과정이 하루안에 끝났다. (오전중에 완료!)

```

### 도메인 구입방법
> 참고 : https://www.hosting.kr/servlet/html?pgm_id=HOSTING000006   
> (와씨, 가비아가 겁나할인하길레 싼줄알았는데, 1년동안만 1900원...)

```
도메인 유지비용이 평균 `1~2만원/년` 듯 하다.
할인하는게 보여서 덥썩 물었더니. 1년 한정이네...

1년동안 잘 테스트 해보고 잘 되면, 좀 더 저렴한 도메인을 사용해야겠다.
아직 SSL도 테스트 전이라서 도메인 구입에 큰 비용을 사용하기 애매하다.

- 첫구매는 가비아에서 진행.
- 두번째는 hosting.kr 에서 진행하자. 상대적으로 저렴하다.

- 구입은 도메인 관리 사이트로 가서, 도메인을 검색해 구입하면 된다.
- 대부분 2만원 안팎이고, 가끔 이벤트 하는 도메인이 있다.
  (shop나 site라던가... shop은 좀 그 단어의 색깔이 너무 확연해서... 쫌.)
  (site는 저렴한 줄 알았는데, 본래가격은 1년에 4만원 헉!)

```

### SSL 설정
> 참고 : (SSL 설정)    https://codens.info/1963, https://jackerlab.com/nginx-https-lets-encrypt/  
> 참고 : (IP우회차단)  https://tech.yeon.me/blog/cloudflare%EA%B3%BC-nginx-%EA%B4%80%EB%A0%A8-%EB%AA%87-%EA%B0%80%EC%A7%80-%ED%8C%81-4-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%9D%B8%EC%A6%9D%EC%84%9C-%EC%9D%B8%EC%A6%9D%EB%90%9C/  
> 참고 : (가입방법)    https://blog.wsgvet.com/cloudflare-sign-in-and-change-nameserver/  
> 참고 : (가입방법)    https://wonderbout.tistory.com/119  
> 참고 : (docker-Port) https://dev.plusblog.co.kr/24  


```
`도메인 설정` 및 `도메인 구입방법` 까지 마쳤다면, 
SSL을 설정할 차례.

SSL은 아직까지 무료인 `클라우드 플레어`를 이용한다.

방법
1. 가입한다.
2. 가입 끝나고, 위에 구매한 도메인을 입력한다. 
   (이전에 Azure에 등록을 완료했다는 전제가 붙는다.)

3. 무료플랜을 가입한다.
4. 레코드 집합등을 퀵스캔 한다.
   아마, A레코드만 덩그러니 있을 것이다. (그것만 만들었으니깐)
   Continue한다.

5. 아...
   이걸 이제 봤네!!
   아주 중요한 스텝이다.

   도메인 관리툴에서 기존 네임서버 지우고,
   클라우드 플레어꺼를 쓰라고 안내한다.

   클라우드 플레어의 최적화를 위해서, (뒤에 나올 SSL사용을 위해서도 뭐)
   어쩐지 너무 쉽게 도메인 설정이 끝나더라니...

   Azure의 네임서버는 뭐 어쩔 수 없지.
   결론적으로, 기존 ns 지우고, 클라우드 플레어꺼로 진행한다.

   페이지에 방법이 상세히 기술되어있으니 눈으로 따라가면 된다.

   (* 가비아에서 네임서버 변경시, 가끔 실패가 뜨는데, 인내심을 가지고 다시 변경하면 된다.)

6. 네임서버 변경이 완료되면 (nslookup으로 확인하기)
   하단의 `Done, check nameservers` 클릭
   (완료되면 가입한 이메일을 통해 `보호중`이라는 메일이 수신된다.)

7. overview에서 SSL/TLS 이동
7.1. 암호화모드 `전체` 선택
7.2. 조금 위에 보면 `원본 서버`란 탭이 있다. 그거 클릭.
7.3. `인증서 생성` 클릭
     Cloudflare로 개인 키 및 CSR 생성 (유형은 2048 그대로)
     아래 유효기간 등등을 확인한다.

7.4. 생성되면 그거 잘 보관한다. (그런거 유출시키는거 아님.)
     두 인증서는 모두 원본서버에 저장한다.

     원본 인증서 : *.crt (or *.origin.pem)
     개인 키     : *.key (or *.private.pem)
     
     (* 는 가능하면 구매한 도메인으로 사용할 것 (ssj.site.crt / ssj.site.key))

7.5. 웹서버 세팅을 해야한다. (nginx기준)
     아마 기존에 listen 80만 있을건데,

     `listen 443 http2; `을 추가한다. 자세한 건 (`SSL설정` 링크를 참고한다.)

      server {
         listen 443 http2;        
         server_name  ssj.site;

         ssl on;         
         ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
         ssl_certificate     /etc/nginx/conf.d/ssj.site.origin.pem;
         ssl_certificate_key /etc/nginx/conf.d/ssj.site.private.pem;
         
         # Block IP access. (using Cloudflare, 필요없으면 주석처리)
         ssl_verify_client on; # Block IP access.
         ssl_client_certificate /etc/nginx/conf.d/cloudflare.crt;
         
        // ...
     }

     * 키/원본이 상대경로에 있으면 거기에 두어도 된다.
     * nginx 구동시, `[warn] the "ssl" directive is deprecated, use the "listen ... ssl"`
       가 나타난다.  ssl on; 이란 옵션 대신 listen ... ssl을 사용하라니 참고 할 것.
       아래는 그 예시다.

     server {
         listen 443 http2 ssl;        
         server_name  ssj.site;

         ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
         ssl_certificate     /etc/nginx/conf.d/ssj.site.origin.pem;
         ssl_certificate_key /etc/nginx/conf.d/ssj.site.private.pem;
         
         # Block IP access. (using Cloudflare, 필요없으면 주석처리)
         ssl_verify_client on; # Block IP access.
         ssl_client_certificate /etc/nginx/conf.d/cloudflare.crt;

         // ...
     }

7.6. 웹서버를 재시작한다. (service restart 나 docker면 docker를 restart한다.)
7.7. ssl포트로 접속한다. (https://)

8.  만약, docker를 통해 nginx를 기동중이라면, 최초 컨테이너 생성시 -p 443:443 설정여부를 확인한다.
    (80:80만 설정시, SSL설정하더라도 서버가 응답하지 않아 CloudFlare 521 응답코드를 받게 될 것이다.)

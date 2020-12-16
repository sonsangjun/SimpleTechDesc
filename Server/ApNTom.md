# 아파치와 톰캣
> 웹서버 아파치, WAS 톰캣   
> 참고(AJP) :    
> https://iam-song.tistory.com/74 <br>
> https://ehdvudee.tistory.com/20 <br>
> AJP : 아파치 Jserv 프로토콜, WebServer에서 WebApplicationServer로 바이너리 전송 약속이다.  


<br>

## WebServer -> WAS로 돌리기
> 참고 : https://www.lesstif.com/system-admin/apache-httpd-tomcat-connector-mod_jk-reverse-proxy-mod_proxy-12943367.html <br>
> 참고(secret) : https://nirsa.tistory.com/158 <br>
> 참고(rewrite) : http://httpd.apache.org/docs/2.0/misc/rewriteguide.html <br>
> 웹서버로 들어온 요청을 WAS로 돌리는 설정이 가물가물해서 기록함.   
> 핵심은 `<VirtualHost>`태그   
> 참고 사이트를 읽어보니, mod_jk말고 mod_proxy 연결방식도 있다.   
> 엔진엑스로 이관시 mod_proxy가 mod_jk보다 간편하다는 '알림'이 있으니 참고 할 것.   
> <br>
> 확실한 구성은 링크나 Google검색 참고.   
> 아래는 약식으로 기술.   
<br>

> mod_jk기준으로 작성   

### Apache 설정
#### httpd.conf

```xml
...

# mod_jk.conf
# 해당 코드안에서 mod_jk.so를 로드한다.
# 이건, conf작성 스타일에 따라 선언 위치가 다른 것 같다.
Include conf/mod_jk.conf

# Virtual hosts
Include conf/extra/httpd-vhosts.conf


```

#### mod_jk.conf

```xml

LoadModule jk_module modules/mod_jk.so

# JkWorkersFile : Where to find workers.properties
# JkShmFile : Where to put jk shared memory
# JkLogFile : Where to put jk logs
# JkLogLevel : Set the jk log level [debug/error/info]
# JkLogStampFormat : Select the timestamp log format
# JkMountFile : url pattern 에 따른 connector mapping

<IfModule jk_module>
  JkWorkersFile conf/workers.properties
  JkShmFile run/mod_jk.shm
  JkLogFile /LOG/apache/..(경로)../mod_jk.log
  JkLogLevel info
  JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "
  JkMountFile conf/uriworkermap.properties
</IfModule>


```


#### workers.properties
> 어떤 요청에 대해 tomcat과 연계할 지 정의.   
> 아래 예시는, 이중화가 아닌, 서로 다른 톰캣을 연계(포트로 구분)   

```xml
# secret은
# secret key로서, tomcat의 server.xml 설정값에 사용된다.
# 서로 일치해야만, 연계가 가능하다.

worker.list=worker1, worker2

# Main Tomcat
worker.worker1.type=ajp13
worker.worker1.host=10.10.20.100
worker.worker1.port=8009
worker.worker1.secret=PasswOrd

# Shield Tomcat
worker.worker2.type=ajp13
worker.worker2.host=10.10.20.100
worker.worker2.port=8209
worker.worker1.secret=PasswOrd

```

#### uriworkermap.properties
> 각 요청에 대한 worker의 동작을 정의
> 아래는 /shield로 시작하는 요청은 worker2로   
> 나머지는 worker1으로 보내라는 정의다.   

```xml
/shield/*=worker2
/*=worker1

```


#### httpd-vhosts.conf
> 파일명은 의미를 부여해 작성   
> 따로 정의하지 않고, httpd.conf에 정의해도 된다.   
> Rewrite는 참고문서 확인하기   

```xml
...

<VirtualHost *:80>
  ServerName example.co.kr
  ProxyRequests Off
  
  # redirect to 443 port
  # rewrite설정(Url을 조작)
  <IfModule mod_rewrite.c>
    ReWriteEngine On
    ReWriteCond %{HTTPS} off
    ReWriteRule .* https://%{SERVER_NAME}%{REQUEST_URI} [R,L]
  </IfModule>
  
  # Connect to Tomcat
  JkMountFile conf/uriworkermap.properties
</VirtualHost>

...

```

### tomcat 설정
> server.xml 수정   

#### server.xml
> 속성이 직관적이라 따로 설명은 필요없어 보인다.   
> 추가로, 8209포트를 바라보는 서버도 아래와 같이 세팅하면 된다.   
```xml
...

<Connector protocal="AJP/1.3"
           address="10.10.20.100"
           port="8009"
           secret="PasswOrd"
           secretRequired="true"
           URIEncoding="EUC-KR"
           rediectPort="8443" />

...

```

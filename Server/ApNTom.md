# 아파치와 톰캣
> 웹서버 아파치, WAS 톰캣   

<br>

## WebServer -> WAS로 돌리기
> 참고 : https://www.lesstif.com/system-admin/apache-httpd-tomcat-connector-mod_jk-reverse-proxy-mod_proxy-12943367.html <br>
> 웹서버로 들어온 요청을 WAS로 돌리는 설정이 가물가물해서 기록함.   
> 핵심은 `<VirtualHost>`태그   
> 참고 사이트를 읽어보니, mod_jk말고 mod_proxy 연결방식도 있다.   
> 엔진엑스로 이관시 mod_proxy가 mod_jk보다 간편하다는 '알림'이 있으니 참고 할 것.   
> <br>
> 확실한 구성은 링크나 Google검색 참고.   
> 아래는 약식으로 기술.   
<br>

### httpd.conf

```xml


```

### httpd-vhosts.conf
> 파일명은 의미를 부여해 작성   
> 따로 정의하지 않고, httpd.conf에 정의해도 된다.   

```xml


```

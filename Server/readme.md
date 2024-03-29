## 개요
> 서버설정에 대한 정보를 차곡차곡 쌓고있는 폴더이다.

## 참고
> 루트의 파일/폴더 설명  

#### 폴더
 
|명칭| 설명 |
|----| ---- |
|azure| 마이크로소프트에서 제공하는 클라우드서비스를 설정하면서 알게된 내용을 정리한 폴더<br/>(중요한건 디스크IO비용이 비싸다!!) |
|docker| linux에 각 업무별로 nodejs기반 WAS를 설정하던 중 알게된 내용을 정리한 폴더.<br/>처음에는 nodejs를 도커 컨테이너를 빌드해 띄우면서 작성했지만, nginx등의 웹서버나 mysql등등도 docker 컨테이너로 띄우고 있다.|
|linux | 서버OS를 linux로 사용해서... 주로, centos를 중심으로 작성하고 있다.|
|mysql | 공짜 RDBMS의 선두에 있는 녀석을 사용하면서 내용을 기술한 폴더이다. |
|nodejs| `javascript 런타임환경` 인데, 그냥 이 녀석을 이용해 javascript기반 WAS를 만드는데 사용 중이다. 그리고, 이 폴더는 그 과정을 진행중에 알게된 내용을 기술한 폴더이다. |


#### 파일 

|명칭| 설명 |
|----| ---- |
|ApNTom| 아파치(HTTPD) 톰캣(WAS) 설정에 관한 내용을 기술한 파일이다. |

### robots.txt 설정
> https://searchadvisor.naver.com/guide/seo-basic-robots <br/>

검색로봇이 웹페이지를 수집하거나 제한하는 `국제 권고안` 이라고 한다.   
이거 루트에 없으면 전부다 긁어간다고 위URL에서 설명하고 있다.
<br/><br/>
혹시모를 사생활이 털릴 수 있으니, 어느정도 검색봇이 긁어갈 수 있는 범위나 권한을 제한하는게 좋다고 생각한다. <br/>
실제로, www.naver.com/robots.txt로 접근하니, 텍스트 파일이 받아진다.
<br/><br/>

> 받아진 파일내용
```txt
User-agent: *
Disallow: /
Allow : /$ 
```

위 내용의 의미는 `사이트의 루트 페이지만 수집허용`을 의미한다.  
더 자세한 작성법은 위 링크를 참고한다.  


# 정보
## 참고사이트
> setup : https://niceman.tistory.com/36 <br>
> setup(en) : https://www.liquidweb.com/kb/how-to-install-docker-on-centos-7/ <br>
> (참고만)url : https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html <br>

<p/>

## 간단 명령어

|명칭 | 설명 |
|--|--|
|docker version | 도커 현재 버젼확인 client/server <br>만약, server정보가 없으면 docker deamon이 떠 있는지 확인<br>안떠있으면, setup링크를 참고해 띄울 것(systemctl명령어로 등록 및 서비스 시작해야함)|
|docker run | 도커 실행명령어.(ex. docker run -i -t centos /bin/bash)<br>-option설명<br>`-d : 백그라운드 모드 실행`<br>`-it : 터미널 입력상태로 컨테이너 실행`<br>`--rm 프로세스 종료시 컨테이너 자동으로 삭제`<br>다양한 옵션은 위의 링크 참고.<br> |

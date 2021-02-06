# 정보
> url : https://proni.tistory.com/entry/%F0%9F%90%B3-Docker-Timezone%EC%8B%9C%EA%B0%84-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0 < br>
> url : https://gogorchg.tistory.com/entry/Docker-Container-%EC%97%90-Timezone-%EC%84%A4%EC%A0%95-%ED%95%98%EA%B8%B0 <br>
```
서버를 띄우다 보니,
전혀 생각치 못한 부분에서 놀랐다.

시간대가 +9:00 가 아니라, +0.00 이다.
자꾸 왜, 정해진 시간에 이메일이 안오나 싶었...

여튼,
```
> 직접 컨테이너 쳐들어가 세팅
```
설정방법
난 그냥 패키지 설치방법을 진행했다.

1. docker exec명령어로 컨테이너 접근.
2. apt-get update
   (대부분 컨테이너 내부에서 설치한 경험이 없어서 package정보가 갱신안되어있을걸)
   
3. apt-get install tzdata
4. dpkg-reconfigure tzdata

5. 타임존을 선택하면 된다. 
   Asia -> seoul
   
   컨테이너를 재시작한다. (docker stop -> docker start)
   
6. 컨테이너 다시 쳐들어가면 
   시간대가 바뀐걸 알 수 있다.

```

<p/>

> sh를 만들어서 실행

```
개발에 자꾸 배포하다 보니,
그때그때 들어가서 apt-get하는게 너무 귀찮았다.

그래서,
sh로 만들어 docker build, create을 함과 동시에 타임존까지 세팅하도록 작성했다.
(타임존은 위의 링크를 참고했다.)

// --.sh
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

//..

timestamp=`date +%Y%m%d%H%M`

# build and Timezon Linking
# -v host물리경로:컨테이너경로
docker build --tag auto/ee:$timestamp .
docker create --name autodev -p 18001:18001 -v /etc/localtime:/etc/localtime:ro \
-e TZ=Asia/Seoul \
auto/ee:$timestamp 

// docker create의 파라메터가 핵심이다.(-v, -e)

```

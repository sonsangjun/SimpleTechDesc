# 정보
## 자잘한 정보

|명칭|설명|
|----|----|
|yum패키지삭제|https://zetawiki.com/wiki/CentOS_yum_%ED%8C%A8%ED%82%A4%EC%A7%80_%EC%82%AD%EC%A0%9C|

## 해상도변경
> url : https://hihighlinux.tistory.com/7 <br>
> url : https://hyunsoft.tistory.com/1 <br>

```

1. cd /boot/grub2/
2. vi grub.cfg

3. :set nu으로 라인활성화
4. linux16 /boot/vmlinuz...으로 
5. LANG뒤에 vga값 변경
5.1. vga있으면, vga=771 --> 변경
5.2. vga없으면, vga추가

* 해상도코드\(bit)    8     16    24
  1024 x 768         773   791   792
  1280 x 1024        775   794   795 ==> 적용하니 지원안한다고 함. 사전에 되는지 먼저 확인해보기
  
  1152 x 864         353   355
  


```

## 명령어

### cp
> url : https://withcoding.com/93 <br>

```
나름 복사 명령어를 잘 안다고 생각했는데,
디렉토리 전체를 복사하는 명령어도 모르고 있었다.

자세한 건 위의 링크 참고

cp -r . /tmp/aaa

(현재 디렉토리의 하위디렉토리까지 /tmp/aaa 로 복사)

```

### 시간출력
> url : https://euless.tistory.com/48 <br>

```

명령어 까지는 아니고, 그냥 공유사항
가끔 20201231235959 이런식으로 날짜를 표시하고 싶을때

timestamp=`date +%Y%m%d%H%M`

'`'로 감싸줘야 숫자가 나온다. 


```

<p/>

> url : https://www.clien.net/service/board/lecture/13017510 <br>
> url : https://m.blog.naver.com/PostView.nhn?blogId=star_breeze&logNo=220534943357&proxyReferer=https:%2F%2Fwww.google.com%2F <br>
```
유닉스 타임 : 1446994800
가끔, mysql을 뒤져보니 10자리가 아니라 13자리로 되어있는 숫자가 있는데
이거는 밀리초를 포함한 타임스탬프이다.

select FROM_UNIXTIME(타임스탬프값) FROM DUAL;

1446994800      -- 2015-11-08 15:00:00
1609492404222   -- 2021-01-01 09:13:24.2220	2015-11-08 15:00:00

처럼 나온다. 

```

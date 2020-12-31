# 정보

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

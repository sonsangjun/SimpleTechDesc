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
  1280 x 1024        775   794   795
  


```

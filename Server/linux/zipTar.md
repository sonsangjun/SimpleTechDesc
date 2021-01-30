# 참고 
> URL : https://steven-life-1991.tistory.com/9 <br/>
> tar.gz 확장자의 이유를 몰랐는데, 둘이 같이 사용하는 경우가 대부분이라서<br/>


# gz 명령어
> 압축명령어 <br/>

```
gzip [option] [name]

ex. gzip -9 2020-01-30.log     // 최대압축률
ex. gzip -9c 2020-01-30.log    // 최대압축률, (c)원본파일유지
ex. gzip -d 2020-01-30.log.gz  // 압축해제

```

# tar 명령어
> 묶는명령어 <br/>
> 잘모르더라도 tar -xvzf `압축파일명` 이런 방식으로 사용하는걸 많이 봤을것임.<br/>

```
tar -c [option] [name] // 압축시
tar -x [option] [name] // 압축해제시

ex. tar -xvzf 2020-01.tar.gz // x(해제), v(작업내용출력), z(gzip형식압축/압축해제) f(사용할 파일의 tar지정)

```

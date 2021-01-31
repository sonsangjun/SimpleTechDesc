# 정보

## Vmware에 깔린 linux 디스크 용량늘리기
> url : https://m.blog.naver.com/PostView.nhn?blogId=hanajava&logNo=220778605932&proxyReferer=https:%2F%2Fwww.google.com%2F <br>
> (간단)url : https://cragy0516.github.io/Expand-Hard-Disk-in-VMWare/ <br>

```
위의 Url처럼, 나 또한 20GB면 충분하다고 생각했는데,
Log쌓이는 속도나, DB 데이터가 차오르는 속도가 장난아니다.

이 추세라면... 2년안에 풀날것 같은데
일단, 로그양부터 줄이는 고민을 하고,

먼저, 용량확장방법은 위 링크를 참고하면 된다.

두번재 URL은 간단하게 확장했는데,
혹시나 생길 문제를 대비하여 parted 명령어에 대해 찾아보자.
(우분투라, Centos계열은 아니긴 한데, 명령어는 동일한 것 같기도 하고)

```


## 디스크 추가방법(Centos기준)
> url : https://mkklab.tistory.com/6 <br/>
> url : https://www.sysnet.pe.kr/2/0/12070 <br/>

```bash
# 설명은 논리볼륨을 기준으로 진행.
# 이미, VMware에서 용량을 extend했다는 가정(20GB추가)

# 파일시스템 정보확인. (확인해도 늘린 용량이 보이지는 않음.)
> df -h
> fdisk -h
  Disk /dev/sda: 42.9 GB, 42949672960 bytes, 83886080 sectors
  Units = sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disk label type: dos
  Disk identifier: 0x000bcb04

     Device Boot      Start         End      Blocks   Id  System
  /dev/sda1   *        2048     2099199     1048576   83  Linux
  /dev/sda2         2099200    83886079    40893440   83  Linux

  Disk /dev/mapper/centos-root: 39.7 GB, 39724253184 bytes, 77586432 sectors
  Units = sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes


  Disk /dev/mapper/centos-swap: 2147 MB, 2147483648 bytes, 4194304 sectors
  Units = sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes

# 파티션 확장.
> df -Tlh
  Filesystem              Type      Size  Used Avail Use% Mounted on
  devtmpfs                devtmpfs  475M     0  475M   0% /dev
  tmpfs                   tmpfs     487M     0  487M   0% /dev/shm
  tmpfs                   tmpfs     487M  8.0M  479M   2% /run
  tmpfs                   tmpfs     487M     0  487M   0% /sys/fs/cgroup
  /dev/mapper/centos-root xfs        37G  9.4G  8.6G  49% /
  /dev/sda1               xfs      1014M  171M  844M  17% /boot
  tmpfs                   tmpfs      98M     0  98M   0% /run/user/0
  overlay                 overlay    37G  9.4G  8.6G  49% /var/lib/docker/overlay2/a99ccb5c069b2615cf56770ad8f0fecbc765694ce6a998281234778e0306e907/merged
  overlay                 overlay    37G  9.4G  8.6G  49% /var/lib/docker/overlay2/14cf205e22995498a15be8b8ec69c7607d40a26152627c5e885effe636bf98c0/merged
  overlay                 overlay    37G  9.4G  8.6G  49% /var/lib/docker/overlay2/9566136a49a657112e31cd232a20f87562ba4dcb9302dbbdb14d8ca9d8e41900/merged
  overlay                 overlay    37G  9.4G  8.6G  49% /var/lib/docker/overlay2/e75c131c46eace180120537fa060a84c7ec44ada22bea65bdd2ca33578d8fb60/merged
  overlay                 overlay    37G  9.4G  8.6G  49% /var/lib/docker/overlay2/f63422a27819f93ba922cecd849d4ee5b2da62800734ff8e52eb84acee8d94f0/merged

> pvresize /dev/sda2
> pvscan
> lvextend -l +100%FREE /dev/mapper/centos-root #파티션확장 대상이 요놈이다. (df -lh에 나오는 19G짜리)

# Resizing 진행.
## 확장시 Centos7미만이면, 파일시스템이 xfs가 아니라면 아래명령어 실행.
> resize2fs /dev/mapper/centos-root

## Centos7이며, 파일시스템 xfs라면 아래명령어 실행
## resize2fs를 사용하더라도 bad magic number... 에러가 발생하면서 안된다.
> xfs_growfs /dev/mapper/centos-root


# 확장이 안되면 아래 파티션 재생성을 진행후 다시 시도.
# 파티션 재생성
> fdisk /dev/sda
> Command : p
> Command : d #<- 다음에 2 입력 (/dev/sda2 삭제)
> Command : n #<- p를 입력하여 재생성, 그리고 기본값으로 셋팅(Enter)
> Command : w #변경사항 저장.
> reboot      #재부팅

```

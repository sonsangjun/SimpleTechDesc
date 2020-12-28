# 정보
## VM의 linux 고정IP설정
> url : https://skysoo1111.tistory.com/81 <br>

```

과거 기억을 떠올리며 작성.

vmware설정은 링크참고.

linux. (centos7기준)

1. cd /etc/sysconfig/network-scripts/
2. vi ifcfg-ens33
   (해당 파일없으면 ifcfg로 시작하는 가장 가까운 파일을 찾아봐야함.)
   
3. 내용 수정
  (수정) BOOTPROTO="dhcp" --> BOOTPROTO="none" 
  
  (추가)
  #IP ADDRESS
  IPADDR=Vmware의 subnet mask참고
  NETMASK=255.255.255.0
  GATEWAY=Vmware의 GateWayIP참고
  DNS1=8.8.8.8 (구글DNS)
  
  (ex.
   IPADDR=192.168.196.161
   NETMASK=255.255.255.0
   GATEWAY=192.168.196.2
   DNS1=8.8.8.8
  )

```

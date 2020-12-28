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
   IPADDR=192.168.35.100
   NETMASK=255.255.255.0
   GATEWAY=192.168.35.1
   DNS1=8.8.8.8
  )

```

## VM 에 띄운 linux접근.
> url : https://tristan91.tistory.com/238 <br>

```

// 해당 설정이 끝나면,
// 위의 staticIP를 공유기에서 보이는 IP로 설정필요.

vmware의 연결방식을 확인하고,
링크내용을 살펴보니.

bridged 방식으로 접근해야한다.

NAT로 설정하면, 내컴에서 VM linux접근은 가능하지만, 다른 내부PC에서는 접근이 불가능하다.
NAT가 VM내부 dhcp로 할당한 IP를 공유기 레벨에서 접근할 방법이 없기때문.

// 설정방법
1. Edit -> Virtual Network Editor
2. Change Setting -> VMnet0의 Bridged 선택
3. 아래 Bridged의 SelectBox에서 현재 공유기와 물려있는 네트워크 장치선택(대부분 Wifi겠지)
4. Apply, OK후 창 닫기
5. 대상 가상OS의 Setting -> Network Adapter
6. Network connection의 Bridged선택(아래 체크옵션은 그냥 두기)
7. linux기동. ifconfig명령어로 dhcp가 할당한 ip확인.
7.1. '#VM의 linux 고정IP설정'를 참고하여 파일수정

여기까지가 VMware설정

8. 공유기 페이지 접속하여, 포트포워딩 메뉴이동.
8.1. 고정IP, 포워딩할 Port입력 (iptime은 찾아보고, 나머지 공유기는 메뉴얼 참고해 진행)
9. 실제 고정IP로 접근이 가능한지 확인. (putty, port:22)
10. 22번 Port는 기본적으로 열려있지만, 나머지 포트는 linux상의 방화벽에서 허용되지 않았을 수 있음.
    이부분은 'centos7 방화벽해제' 를 검색바람.

만약, Vmware OS를 두군데 이상 띄우면 IP충돌이 날 수도 있겠다.

```

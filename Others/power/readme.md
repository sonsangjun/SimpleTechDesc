# 개요
> 전원옵션에 관한 설정을 정리한 페이지.

## 언터볼팅 대신에 전원옵션 설정.
> 참고 : https://www.youtube.com/watch?v=7uX4_S2G8EU <br/>


기존에 언더볼팅으로 발열을 제어했는데,  
전원옵션의 레지스트리를 건드리면 설정이 가능해서 여기에 정리해둔다.

> 레지스트리 설정
> ![image](https://user-images.githubusercontent.com/12105781/132078851-859c35b6-497b-4c1f-9b95-e21b9f162ff1.png)
> 찾아가는 경로 :  
> \HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Power\PowerSettings\54533251-82be-4824-96c1-47b60b740d00\75b0ae3f-bce0-45a7-8c89-c9611c25e100  
> 수정대상 :  
> Attribute : 1 -> 2

> 전원옵션 설정
> ![image](https://user-images.githubusercontent.com/12105781/132078889-537edfb5-a533-47ab-92e0-bafca4dd8f22.png)
> `프로세서 전원 관리`항목에 `최대 프로세스 주파수`가 추가된 걸 알 수 있다.   
> 평소에 전원 상태로 사용하므로, 최대주파수를 확인하고, 그것보다 낮게 설정한다.   
> 이 부분은 각 사용자마다 다르므로, 조금씩 변경해가며 적절한 지점을 맞추면 된다.  

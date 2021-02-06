# 정보
> url : https://medium.com/better-programming/understanding-docker-container-exit-codes-5ee79a1d58f6 <br/>

## Exit 139
> 컨테이너가 SIGSEGV를 받았다. <br/>

```
사실, SIGSEGV가 뭔지 모르겠다.
그래서 좀 더 읽어봤는데, 접근이 불가능한 메모리 위치에 접근했다는 에러라네...
마소 클라우드를 사용하는데, 마소가 막은건가...?

Docker입장에서는 어플문제거나, 그 기본이미지가 문제일 수 있다고 한다.
IMAGE를 확인하니, docker run할때 생성된 이미지가 로컬linux와 MS가 서로 달라서 맞추고 다시 시도해야겠다.

```

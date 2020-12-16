# Gradle
> 참고 : https://tjandroid.blogspot.com/2018/11/api-implementation.html <br>
> 참고(기본설명) : https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09 <br>
> 참고(API vs implementation) : https://jongmin92.github.io/2019/05/09/Gradle/gradle-api-vs-implementation/ <br>
> 참고 : https://writemylife.tistory.com/57 <br>
> <br>
> 
## runtimeOnly, implementation 등등
> 몰랐는데, dependencies 블록 정의시, 더 많은 옵션이 존재한다.   

<br>

#### implementation
> 의존 라이브러리 수정시 본 모듈까지만 재빌드(한국어인가...?)      
> (API vs implementation) 항목 설명을 읽으면 이해가 될 성싶다.(그렇게 느끼는 건가?)   
<br>

> A <- B <- C <- D < E <- ... (좀 극단적이긴 한데)   
<br>

> A모듈을 수정시, B까지만 재빌드. C이후는 알바가 아님.   
> 여기서 B가 본 모듈에 해당한다. A가 의존 라이브러리(A를 누가 의존하다.)   
> C 이후 의존하는 녀석들은 A를 접근할 수 없음.   

<br>

#### api :   
> 의존 라이브러리 수정시 해당 모듈을 의존하고 있는 모듈을 재빌드(이건 좀 알아먹겠는데)   
<br>

> A <- B <- C <- D < E <- ... (좀 극단적이긴 한데)   
<br>

> A모듈을 수정시, B아래로 싹 재빌드.   
> 여기서 B가 본 모듈에 해당한다. A가 의존 라이브러리(A를 누가 의존하다.)   
> 싹 재빌드 했기 때문에, B아래로 다 A를 접근할 수 있다.   

<br>

#### compileOnly :   
> complie 할때만 빌드하고, 빌드결과물(war, apk등등)에 포함하지 않음.   
> 이거는 runtime에 lib가 제공되던가, 거기까지 호출하지는 않을거라 생각할 때,   
> 사용할 옵션같은데,   

> <br>

> CommonProject, OneProject, TwoProject 3개의 프로젝트가 있고,   
> OneProject, TwoProject에서 CommonProject의 빌드결과물(jar)을 포함한다고 할 때,   

> <br>

> One,Two Project에서 CommomProject의 compileOnly로 설정된 lib를 가지고 있는 경우가 있다.   
> 이 경우, CommonProject에서 build결과물에 의존lib를 포함하는게 훨씬 덩치가 크므로,   
> 경량화관점에서 보면 compileOnly옵션을 주는게 당연하다.   
> (실제, CommonProject의 모든 lib를 One,Two Project에서 사용한다는 것도 아닐거고)   
<br>

#### runtimeOnly :   
> runtime시에만 필요한 라이브러리.   
> 말그대로, runtime동안 필요한 라이브러리.   
> gradle build 결과물에 lib는 같이 들어가 있다.   
> compileOnly는 컴파일중에만 참고하고, 빌드산출물(war등)에 포함을 안하는데 반해   
> runtimeOnly는 컴파일중에는 모르겠고, 빌드산출물에는 포함시킨다. 실행중에 참조는 해야하니까   
> (실제 로컬PC의 프로젝트에서 lib하나 멱살잡고 compileOnly, implementation, api, runtimeOnly 모두 바꿔가며 gradle clean build 를 진행하고 내린 결론)   

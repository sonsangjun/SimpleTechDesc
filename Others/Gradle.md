# Gradle
> 참고 : https://tjandroid.blogspot.com/2018/11/api-implementation.html <br>
> 참고(기본설명) : https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09 <br>
> 참고(API vs implementation) : https://jongmin92.github.io/2019/05/09/Gradle/gradle-api-vs-implementation/ <br>
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

#### compileOnly :   

<br>

#### runtimeOnly :   

# Gradle
> 참고 : https://tjandroid.blogspot.com/2018/11/api-implementation.html <br>
> 참고(기본설명) : https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09 <br>
> 참고(API vs implementation) : https://jongmin92.github.io/2019/05/09/Gradle/gradle-api-vs-implementation/ <br>
> 참고 : https://writemylife.tistory.com/57 <br>
> <br>
> 

## 큰 그림
> 참고 : https://jwkim96.tistory.com/111 <br>
> 참고(publishing) : https://docs.gradle.org/current/samples/sample_publishing_credentials.html <
> Android의 gradle 세팅과는 조금 다르네.   
> Android는 buildtype, flavor 이것저것 넣어서 세팅은 했는데   
> WAS에 배포하는 프로젝트는 아래와 같이 큰 블록으로 나뉘어 있다.   

```xml

// 스프링부트를 사용하면, 필수 플러그인 4개가 추가되어야 하는 모양.
// 그 중, 'java', 'eclipse'는 각각SDK,IDE성격으로 추가하고.
// io.spring.dependency-management 는 스프링부트의 의존성을 관리하는 녀석
// 위의 필수요소는 apply plugin 으로 정의하던가, 아래처럼 정의하면 된다.
plugins {
  id 'java'
  id 'eclipse'
  id 'java-library'
}

// 전역변수 설정
ext {
  javaVersion='1.8'
  springVersion='4.3.25.RELEASE'
}

// 자바소스 위치 정의.
// test는 작성한 Java코드의 테스트코드(jUnit등)의 위치
// 따로 정의한 이유는 gradle build시, -x test 옵션을 통해 제외하기 위함도 있음.
sourceSets {
  main { java { srcDir '...(상대경로)...' }
  test { java { srcDir '...(상대경로)...' }
}

// Javadoc뽑으면 한글 왕창깨지는데, 여기 옵션을 줘서 그걸 방지
javadoc { 
  source = sourceSets.main.allJava
  options.addStringOption('encoding', 'UTF-8');
}

// 외부망이 붙지 않는 프로젝트는 아래처럼 제공된 repo주소를 입력하는데,   
// 외부망이 붙는 프로젝트라면 "https://..." 대신,   
// mavenCentral(), jcenter() 라 적을 것이다.
repositories {
  maven {
    url "https://..(주소)../"
  }
}

// 의존성 설정. 이 부분은 필요한 lib등등을 알맞게 정의하면 됨.
// 각 옵션에 대한 설명은 하단 목차 참고.
dependencies {
  ...
  compileOnly("com.ttt.j2obj");
  ...

  implementation("org.springframework:spring-aop:4.3.25.RELEASE");
  ...

  runtimeOnly("org.XXX...");
  ...

}

// gradle 빌드후, 바로 배포하는 경우 아래 항목에 정의된 위치로 배포됨.
// gradle clean build publish -x test의 publish에 해당
// war대신, 공통으로 사용하는 jar를 build하는 프로젝트 옵션기준으로 작성됨.
publishing{
  publications {
    mave(MavenPublication) {
      groupId = myGroupId
      artifactId = myArtifactId
      version = artifactVersion

      artifact('build/libs/' + artifactId + '-' + artifactVersion + 'jar') { extension 'jar' }
    }
  }

  // username, password는 정의된 값이다. 
  // gradle.properties에 정의되어 있는데, 주기적으로 변경되는 값이면
  // 설정값을 서버에 두는게 아니라. 바뀌어도 배포주기에 영향을 주지않는 위치에 둬야할 것 같다.
  // docs.gradle.org를 참고하면, 계정정보를 command상에서 줄수도 있는데, 
  // 이러면 굳이 proerties파일을 바꾸지 않아도 될 것 같다.
  repositories {
    maven {
      credentials {
        username "$mavenUser"
        password "$mavePassword"
      }
      url "https://...(배포할 주소)..."
    }
  }
}

```

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

# RestfulAPI 참고할만한 문서
> HTTP Method : https://developer.mozilla.org/ko/docs/Web/HTTP/Methods <br>
> 설명 :   
> https://spoqa.github.io/2012/02/27/rest-introduction.html   
> https://meetup.toast.com/posts/92   
> <br>
> Http Method 를 통해 행위를 정의해야는데,   
> 내가 수행한 프로젝트는 Url에 행위를 넣어서 구분하고 있다.   
> 실제 필터나 인터셉트에서 그걸 보고 전처리를 어떻게 할지 구분하고,   
> 금융권 일부가 이런건지 모르겠지만, 이런 표준이 자리잡히면 좋겠다.   
> <br>
HttpMethod와 관련된 내용이라 기술한다.   
> 참고 : https://ooeunz.tistory.com/100?category=813313 <br>

```
예전에 Controller에 기술하는 API URL은   
@RequestMapping(value="/member/1" method=RequestMethod.GET) 

spring4.3부터는 아래 어노테이션이 추가되었다.
@GetMapping("/member/1")
@PostMapping("/member/1")
@PutMapping("/member/1")
@DeleteMapping("/member/1")
@PatchMapping("/member/1")

5개가 전부 restful API에서 나오던 Http Method와 연결된다.
restful API 개념을 반영한 어노테이션 같다.
```

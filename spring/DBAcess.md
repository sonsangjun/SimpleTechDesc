# DB접근방식
> 참고 : https://velog.io/@leeinae/Spring-Mybatis-JPA-Hibernate-%EB%B9%84%EA%B5%90 <br>
<br>
스프링을 알게 모르게 많이 사용했지만,   
데이터베이스 접근방식에 대해서는 좁게 알고 있었다.   
<br>

항상 mybatis나 ibatis를 사용해서,   
Mapper.xml과 interface를 생성하고, xml에 쿼리를 작성하여 접근하는 방식.   
이게 당연한 건줄 알았고,   
<br>

다른 개발자분 책상위의 JPA는 JavaPageApplication라는 말도안되는 약자로 알고 있었다.   
나중에서야 **Java Persistent API**라고 알게 되었다.   
<br>

mybatis와 JPA의 가장 큰 차이는   
**SQL문을 직접작성하느냐, 맡기느냐 차이같다.**   
<br>

요즘 트렌드는 금융권마냥 서브쿼리와 조인의 향연은 아니고,   
최대한 단순하게 가자는 것 같은데   
그런의미에서라면 JPA가 가까운 것 같기도   
(실상은 모른다. 까보기 전까지는... 간단하다면서 막 끝을 알수 없는 서브쿼리 지옥이 나타난다거나)   

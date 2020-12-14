# 디자인 패턴
> 참고 : https://gmlwjd9405.github.io/2018/07/06/design-pattern.html <br>
> 참고(생성패턴) : https://tech-people.github.io/2020/01/08/java-design-pattern-creational/ <br>
> <br>
> 2020년 프로젝트 진행하면서 가장 많이 썼던 디자인 패턴이 **singleTonPattern**이다.   
> <br>
> 지속적으로 사용하는 객체지만, 변동성이 거의 없기때문에 굳이 new하면서 GC가 되게 할 이유가 없기 때문이기도 하고   
>
## SingleTon
> 딱 한개만 만들어 사용해도 상관없을때(메모리 낭비, 잦은GC호출방지)   
> 참고(SingleTon생성을 JVM에게 떠넘김) : https://jeong-pro.tistory.com/86 <br>
> 참고(상속) : https://peulblog.tistory.com/13 <br>
> <br>
> singleton의 문제점으로 지목한게, 상속불가, 전역공유문제, 싱글턴보장불확실이 있는데   
> 그 중, **싱글턴보장불확실**은 JVM이 처리할테구.   
> **상속불가**는 애초에 상속안할 생각으로 설계 및 생성하지 않을까 싶다.   
> 자식Class가 부모Class를 상속받을경우, 자식Class instance생성시, 부모Class의 생성자를 호출한다.   
> 그런데, 부모가 싱글턴이라 생성자가 private이면 instance생성자체가 불가능하므로 상속자체가 불가능하게 된다.   
> <br>
> **전역공유문제**는 문제긴 하네. 어디서든 객체를 가져오면 그 안의 변수를 접근할 수 있으니.   
> 최대한 전역공유는 피하는 수밖에.   
> (근데, 사람일이라는게, 피할 수 있으련지 모르겠다. 쓰다보면 귀찮아서 전역에 변수고 상수고 막 박아놓으니)   
> (역시, 처음부터 쓰지 말아야 하는 건가?)   

```java
// ObjSingleton은 클래스 안에 클래스(Holder)을 두는 방법
// 가장 많이 사용하는 방식이라는데, 실제 솔루션측의 코드를 봐도 그런것 같다.

public class Obj { 
    private Obj(){}
    
    private static class ObjSingleton {
      private static final  obj = new Obj();
    }
    
    public static Obj getInstance() {
      return ObjSingleton.obj;
    }
}

// 다른 클래스에서 사용할 때,
...
Obj objInstance = Obj.getInstance();
...

```

<br><br>

## Strategy Pattern(전략패턴)
> 참고 : https://wiserloner.tistory.com/461 <br>
> 참고 : https://niceman.tistory.com/133 <br>
> 하나만 읽어서는 감이 오지 않는데,   
> 여러개를 읽으니 조금 감이 온다.   
> 샘플이지만, 정말 이렇게 인스턴스를 교체해서 사용하나...?   
> 라는 생각이 들지만, 우리가 모르는 사이에 이미 이렇게 쓰고 있을지도 모르겠다.   
> <br>
> **하나의 추상적인 접근점(인터페이스)을 만들어 접근점에서 서로 인스턴스를 교환**   
> 이 문구가 처음보면 와닿지 않은데, 위의 2개 참고 사이트를 읽어보면 의미가 와닿는다.   

```java

/** interface mover
    
    Delegate(델리게이트)
    interface를 implements한 Class가 구현을 하도록 유도(강제)하고
    
    Class의 instance에서 interface의 함수가 호출되면, interface가 직접일하는게아니라.
    interface를 구현한 instance가 일하는 것을 '대리자/델리게이트'라고 한다.

**/
public interface Mover {
    public void moving();
}

/** *********************************************/
/** mover를 구현하라 **/
class GoMover implements Mover {
    @Override
    public void moving() {
        System.out.println("Go!");
    }
}

class BackMover implements Mover {
    @Override
    public void moving() {
        System.out.println("Back!");
    }
}

class StopMover implements Mover {
    @Override
    public void moving() {
        System.out.println("Stop!");
    }
}

/** *********************************************/
/** Mover의 몸통? */
public class Team {
    private Mover mover;
    
    public Team(Mover mover) {
        this.mover = mover;
    }
    
    public callMoving(){
        mover.moving();
    }
    
    /** 여기가 설명에서 얘기하던 '접근점'이 아닐까 싶다. */
    public setMover(Mover mover){
        this.mover = mover;
    }
}

/** *********************************************/
/** main */
public static void main(String[] args){
    Team team = new Team(new GoMover());
    
    team.callMoving();
    
    // Back으로 바꿔치기
    team.setMover(new BackMover());
    team.callMoving();
    
    // Stop으로 바꿔치기
    team.setMover(new StopMover());
    team.callMoving();
}

// 결과
/*
    "Go!"
    "Back!"
    "Stop!"

*/
```


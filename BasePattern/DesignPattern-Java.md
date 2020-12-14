# 디자인 패턴
> 참고 : https://gmlwjd9405.github.io/2018/07/06/design-pattern.html <br>
> <br>
> 2020년 프로젝트 진행하면서 가장 많이 썼던 디자인 패턴이 **singleTonPattern**이다.   
> <br>
> 지속적으로 사용하는 객체지만, 변동성이 거의 없기때문에 굳이 new하면서 GC가 되게 할 이유가 없기 때문이기도 하고   
>
## SingleTon
> 딱 한개만 만들어 사용해도 상관없을때(메모리 낭비, 잦은GC호출방지)   
> 참고(SingleTon생성을 JVM에게 떠넘김) : https://jeong-pro.tistory.com/86 <br>
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

```

## FactoryMethod
> 클래스의 객체 생성을 위한 인터페이스를 제공하되,   
> 하위 클래스에서 어떤 클래스를 생성할지 결정할 수 있도록 도와준다...?   



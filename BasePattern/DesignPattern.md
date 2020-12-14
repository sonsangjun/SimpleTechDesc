# 디자인 패턴
> 참고 : https://gmlwjd9405.github.io/2018/07/06/design-pattern.html <br>
> <br>
> 2020년 프로젝트 진행하면서 가장 많이 썼던 디자인 패턴이 **singleTonPattern**이다.   
> <br>
> 지속적으로 사용하는 객체지만, 변동성이 거의 없기때문에 굳이 new하면서 GC가 되게 할 이유가 없기 때문이기도 하고   
>
## SingleTon
> 딱 한개만 만들어 사용해도 상관없을때(메모리 낭비, 잦은GC호출방지)   

```java
public class Obj { 
    private Obj(){}
    
    private final static class ObjSingleton {
      private final static obj = new Obj();
    }
    
    public static Obj getInstance() {
      return ObjSingleton.obj;
    }
}

// 다른 클래스에서 사용할 때,
...

Obj objInstance = Obj.getInstance();

```

## 

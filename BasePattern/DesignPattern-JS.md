# Javascript패턴
> 참고 : https://itstory.tk/entry/%EA%BC%AD-%EC%95%8C%EC%95%84%EC%95%BC%ED%95%98%EB%8A%94-Javascript-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-4%EA%B0%80%EC%A7%80 <br>
> 참고(https://riptutorial.com/ko/javascript/example/5966/%ED%94%84%EB%A1%9C%ED%86%A0-%ED%83%80%EC%9E%85-%ED%8C%A8%ED%84%B4 <br>
> <br>
대표적으로 4가지가 존재.   
<br>

**Module   
Prototype   
Observer   
Singleton**   

<br>

**Module**   
<br>
> 메모리 덤프뜨면 다 볼수있다지만,   
> (이 말은 너무 냉소적이야...)   
> 클래스처럼 다른 클래스가 접근하지 못하도록 보호하는 캡슐화   
> 아래 간단한 코드가 있다.   

```javascript

// 즉시실행함수(IIFE)
var funObj = 
(function(){
    // private한 필드와 메소드
    let aaa = '0000';
    
    // public한 필드와 메소드
    return {
        getAaa(){ return aaa; },
        setAaa(obj){ aaa = obj} 
    }
})();

// console실행시.
funObj.getAaa(); //==> '0000';
funObj.setAaa('1212'); //== aaa에 '1212' 세팅.
funObj.getAaa(); //==> '1212';

```
<br>

> 추가로 **Revealing Module Pattern** 이 있는데, 나중에 읽어보기   


**Prototype**   
> 프로토 타입 패턴은 프로토 타입 상속을 통해 다른 객체의 청사진으로 사용될 수 있는 개체를...<br>
> <br>
> 음...내가 이해하기엔, 객체의 prototype(초기모델)을 정의하는 정도?   
> 정의된 초기모델을 또 다른 Object가 상속받아 사용할 수 있는 거구   
<br>

```javascript

// prototype만 정의(상속전)
function DefiedObj(initId) {
    this.id = initId;
    var aa = '11';
}

DefiedObj.prototype.callId = function(){
    return 'call : '+this.id;
}

DefiedObj.prototype.echoId = function(){
    return 'echo : ' + this.id;
}

var inst = new DefiedObj('1234');

console.log('Defined :',inst.callId());
console.log('Defined :',inst.echoId());


```

> prototype정의후, 상속

```javascript

// Child에게 상속
function ChildObj(initId){
    this.id = initId;
}

ChildObj.eatId = function(){
    return 'eat : ' + this.id;
}

ChildObj.prototype = Object.create(DefiedObj.prototype);
var chInst = new ChildObj('4321');
console.log('Child :', chInst.callId());
console.log('Child :', chInst.echoId());

// Object생성시 삽입한 id를 child의 prototype에서 접근이 가능
// 표준으로 prototype한개 맹글어놓고,   
// 그걸 여기저기서 상속받으면 같은 Method기능을 그대로 사용할 수 있다.

```

<br>



**Observer**   
> 참고 : https://medium.com/@yeon22/design-pattern-observer-pattern-%EC%9D%B4%EB%9E%80-ef4b74303359 <br>
> <br>
> Angular9으로 UI개발시, Observable Class를 사용했는데, Observable<any>를 return받으면   
> 항상 subscribe Method로 성공/실패시 callback처리를 정의했다.   
> 그 subscribe개념이 여기 패턴에서 설명되는 걸로 보아,   
> 옵저버 패턴의 개념을 기반으로 lib가 개발된 것 같다.   
> <br>
> 옵저버 패턴이 정말 와닿지 않았는데, 2020년에 WebUI로 Angular9과 typescript를 만지다보니   
> 자연스레 이해가 되었다. 하도 Observable객체를 만들어 써제낀덕분에   
    
```javascript
// Angular9에서 rxjs를 import하고, typescript로 작성할 시.

// getInfo from apiCaller
public getInfo() : void {
    this.ui.block.show();
    resultCall : Observable<any> = this.callApi();

    // resultCall 1회차
    resultCall.subscribe(
        (res:any)=>{
            this.ui.block.hide();
            console.log('call success',res);
        },
        (error:any)=>{
            this.ui.block.hide();
            console.error('call fail',error);
        }
    );

    // resultCall 2회차
    resultCall.subscribe(
        (res.any)=>{
            console.log('2nd call success',res);
        },
        (error:any)=>{
            console.error('2nd call fail',res);
        }
    );
}

// defined apiCall 
private callApi() : Observable<any> {
    return Observable<any>(observer=>{
       this.api.call().then((res)=>{
           observer.next(res);
       }).catch((error)=>{
           observer.error(error);
       });
    });
}

```

> 자바스크립트로 rxjs없이 작성한다면, addEventLisner란 Method를 통해   
> 작성할 수 있다.   
> Object Class를 하나 정의하고, prototype으로 regist/unRegit 두 Method를 생성한다.   
> Observer를 여러개 등록하고, 등록된 Observer들에게 변경사항이 존재시, notify를 한다.   

```javascript

// 이것은 prototype Pattern에 대한 이해가 없으면 
// 이해에 어려움을 겪을 수 있다.

function Subject(){
    this.arr=[];
}

// prototype에 등록/해제/알림 표준정의
Subject.prototype = {
    unReg : function(ob){
        // filter에 arrowFunc을 사용해. redOb!==ob인것만 추려서 다시 
        // arr를 설정
        this.arr = this.arr.filter(redOb=>redOb!==ob);
    },
    reg : function(ob){
        this.arr.push(ob);
    },

    notifyToOb : function(data){
        this.arr.forEach(ob=>ob.notify(data));
    }
}

const sub = new Subject();
const obs = {
    ob1 : {
        notify : data=> console.log('ob1',data)
    },
    ob2 : {
        notify : data=> console.log('ob2',data)
    }
};

sub.reg(obs.ob1);
sub.reg(obs.ob2);
sub.notifyToOb('okay!');


```

<br>



**Singleton**   
> singleton을 주로 Java에서만 사용했는데,   
> 개념만 알고 있으면 js말고도 여러곳에서 사용할 수 있다.   
> 결국은 사용할 객체를 1개만 만드는 거니까   
<br>

```javascript

// let instObj; // chromeDev에서 테스트시 한번만 declare할 것.

instObj = (function(){
    let inst;
    let id;

    function init(){
        return {
            getId : function(){ return this.id; },
            setId : function(sId){ this.id = sId;}
        }
    }

    return {
        getInstance : function(){
            if(!inst){
                inst = init();
            }
            return inst;
        }
    }
})();

var inst1 = instObj.getInstance();
var inst2 = instObj.getInstance();

console.log('equal?',inst1===inst2);
console.log('setId',instObj.getInstance().setId('1234'));
console.log('getId',instObj.getInstance().getId());

```

> java든, javascript간에 호출 타이밍으로 인해 instance가 두개생성되는 건 걱정해야한다.   
> java야 JVM에게 떠넘기면 그만이지만, javascript는...   


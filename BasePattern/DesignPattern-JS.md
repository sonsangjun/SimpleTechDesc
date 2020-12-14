# Javascript패턴
> 참고 : https://itstory.tk/entry/%EA%BC%AD-%EC%95%8C%EC%95%84%EC%95%BC%ED%95%98%EB%8A%94-Javascript-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-4%EA%B0%80%EC%A7%80 <br>
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
inst.callId();
inst.echoId();


```

> prototype정의후, 상속

```javascript

```

<br>



**Observer**   
<br>



**Singleton**   
<br>




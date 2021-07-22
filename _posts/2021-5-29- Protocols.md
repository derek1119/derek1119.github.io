---
layout : single
title : Protocols
---
# Protocols

* 프로토콜은 API를 더욱 간결하게 표한하는 방법이다.

  프로토콜은 또 다른 타입 중 하나(class, enum, struct)이다. 프로토콜이 하는 일은 개발자가 API를 갖도록 해준다. 클래스와 구조체를 사용할 목적에 따라 완전하게 구현하는 대신에 클래스와 구조체가 무엇에 대한 것인지만 밝혀두는 것이다. 프로토콜은 선언된 메소드와 프로퍼티들로 된 집합체일 뿐이다. 그냥 메소드 묶음 같은 것이다. 

* 프로토콜은 타입이다.
  타입을 사용하는 곳이라면 어디서든 프로토콜을 사용할 수 있다.  

* 프로토콜의 메소드와 프로퍼티의 실행

  프로토콜은 프로토콜을 실행하는 class, struct, enum 안에서 실행된다. 프로토콜 자체는 프로퍼티와 메소드를 선언해 놓은 것일 뿐이지 프로토콜 자신은 프로토콜을 실행하지 않는다. 그렇지만 프로토콜의 일부나 전부를 실행하기 위해서 익스텐션을 사용하는 것도 가능하다. 그저 extension protocol 이라고 선언하면 된다. 프로토콜도 익스텐션처럼 저장 공간을 가지고 있지 않는다. 저장 공간이 있는 프로토콜을 실행할 수 있는 방법은 없다. var 변수를 가질 수는 있지만 계산 변수만 가능하다. 

* 프로코톨을 사용하기 위한 4단계 절차
  1. 프로토콜 선언하기 
     프로토콜에 들어갈 메소드와 프로퍼티를 선언하는 작업이다.
  2. class, struct, enum이 이 프로토콜을 실행하겠다는 선언
     class, struct, enum 중 하나가 메소드와 프로퍼티를 실행하기만 하는 것으로는 부족하다. 선언을 먼저 해줘야 프로퍼티와 메소드들을 실행할 수 있다.
  3. 프로토콜을 실행하는 코드 
     보통 프로토콜을 실행한다고 선언한 class, struct, enum에 있다. 익스텐션에도 있을 수도 있다. 
  4. 프로토콜 안에 있는 optional 메소드 
     스위프트에 있는 프로토콜과 모든 프로퍼티와 모든 메소드는 반드시 있어야 한다. 프로토콜을 실행한다고 하면 프로토콜 전부를 실행해야 한다. 선택적으로 실행할 수는 없다. 

* 프로토콜의 선언 

  ``` swift
  protocol SomeProtocol : InheritedProtocol1, IngeritedProtocol2 {
    var someProperty : Int { get set }
    func aMethod(arg1: Double, anotherArgument: String) -> SomeType
    mutating func changeIt()
    init(arg: Type)
  }
  ```
  
  프로토콜은 다중 상속을 받을 수 있다. 스위프트에서 클래스는 단일 상속만 가능하지만 프로토콜에서는 상속을 여러개 동시에 받는 것이 가능하다. 프로토콜이 다른 프로토콜로부터 상속받는다는 것이 무슨 뜻일까? SomeProtocol을 실행하고 싶은 사람은 여기 있는 모든 프로토콜도 함께 실행해야 한다는 의미이다. 프로토콜 안에 var가 있을 때 get only인지 get and set인지 명시를 해주어야한다. set만 가능한 프로퍼티는 없다. 바뀔 예정인 모든 함수들은 그 앞에 mutating 이라고 표시해야 한다. 왜냐하면 struct가 이 프로토콜을 실행할 수도 있기 때문이다. 스스로 변하는 메소드를 가진 struct는 mutating을 앞에 붙여 선언해야 한다. 프로토콜을 제한하는 것도 가능하다. 예를 들어 '오직 class만을 위한 프로토콜이다' 라는 식으로 말이다. 이때는 :class, 를 넣어주면 된다. 모든 상속 프로토콜 앞에 class만 넣으면 된다. 프로토콜 안에는 이니셜라이저(초기화 함수)를 넣울 수도 있다. 이 프로토콜을 실행하는 사람이라면 누구든 이렇게 생긴 이니셜라이져를 갖고 있어야 한다는 얘기이다. 

* 프로토콜 실행방법

  ```swift
  class SomeClass : SuperclassOfSomeClass, SomeProtocol, AnotherProtocol {
    //implementation of SomeClass here
    //which must include all the properties and methods in SomeProtocol & AnotherProtocol
  }
  ```

  여기서 프로토콜을 채택하고 실행해주지 않으면 컴파일 중에 에러메세지를 보낼 것이다.

  ```swift
  enum SomeEnum : SomeProtocol, AnotherProtocol {
    //implementation of SomeEnum here
    //which must include all the properties and methods in SomeProtocol & AnotherProtocol
  }
  ```

  위 코드에서 enum은 두 개의 프로토콜을 실행하겠다고 한다는 의미이다. 

  ```swift
  struct SomeStruct : SomeProtocol, AnotherProtocol {
    //implementation of SomeStruct here
    //which must include all the properties and methods in SomeProtocol & AnotherProtocol
  }
  ```

  구초체도 유사하며 만일 이 안에 mutating이 있으면 mutating을 넣어줄 것이다. 


  프로토콜의 갯수는 제한이 없다. 만일 프로토콜 안에 init이 있고 클래스가 그것을 실행한다면 그 init을 required로 만들어야 한다. 또한 extension을 통해서 프로토콜 적용을 추가할 수 있다. 스위프트에서 복잡하고 정교한 것을 만들고 싶다면 프로토콜을 잘 다루는 것은 핵심이다. 특히 여러 관계에서 API Contract가 무엇인지 보여주는 것에서 중요하다. 이는 상대가 실행하기를 개발자가 기대하는 함수와 메소드들이다. 프로토콜은 근본적으로 훌륭한 객체지향 프로그래밍방식이고 근본적으로 훌륭한 캡슐화이기 때문이다. 

* 프로토콜 예시

  ```swift
  protocol Moveable {
    mutating func moveTo(p : CGPoint)
  }
  //특정 지점으로 움직이는 moveTo라는 mutating func가 있다. 
  
  class Car : Moveable {
    func moveTo(p: CGPoint) { ... }
    func changeOil()
  }
  //클래스이기 때문에 mutating을 붙이지 않아도 된다. (왜?)
  
  struct Shape : Moveable {
    mutating func moveTo(p: CGPoint) { ... }
    func draw()
  }
  
  let prius : Car = Car()
  //prius는 차 모델명
  let square : Shape = Shape()
  
  var thingToMove : Moveable = prius
  //둘 다 Moveable타입을 따르기 때문에 위처럼 설정할 수 있다. (프로토콜은 타입이 될 수 있다.)
  thingToMove.MoveTo(...)
  //가능하다 
  thingToMove.changeOil()
  //불가능하다. 왜일까? prius는 Car였다. Car는 changeOil이 가능했다. 하지만 thingToMove는 Car가 아니라 Moveable이기 때문에 움직이는 방법만 알고있다. changeOil()과는 전혀 상관이 없다. 
  
  thingToMove = square
  //가능하다. 
  let thingsToMove : [Moveable] = [prius, square]
  //배열도 가능하다. prius와 square은 타입이 전혀 다르지만 둘 다 Moveable 프로토콜을 따르기 때문에 배열로 묶을 수 있다. 
  
  func slide(slider : Moveable) {
    let positionToSlideTo = ...
    slider.moveTo(positionToSlideTo)
  }
  slide(prius)
  slide(square)
  //함수의 인자 slider의 타입이 Moveable인 것이다. 위의 slide 함수도 문제 없이 작동한다. 
  ```

  


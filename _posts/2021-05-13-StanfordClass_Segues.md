---
layout : single
title : Segues
---

# Segues(세그웨이 : 넘어가다, 이어지다)

가장 많이 사용되는 segue는 show segue 이다.  
segue는 항상 가상으로 새 MVC를 만들어 MVC에 segue로 연결하면 MVC를 새로 하나 만든다.  
segue는 새 MVC를 만든다고 생각하면 된다.  
segue inspector 설정 창에서 세부 설정을 하면 되는데 가장 중요한 것은  
segue identifier(ID)이다. 모든 segue는 identifier가 필요하다.  
동사가 들어가는 무언가를 하는 형태로 id를 짜는 것이 좋다. 실제로 그러하니까  

그렇다면 코드 안에 어디에서 segue의 identifier를 사용하게 될까? 두 곳이 있는데 
한 곳은 자주 사용되지는 않지만 segue를 코드에서 연결할 때 쓰인다.  
``` swift
func performSegueWithIdentifier(identifier : String, sender: AnyObject?)
```
UIViewController에서 쓰이는 메소드이다. identifier로 segue를 실행하는 메소드인데, 이 메소드에  
identifier를 주면 sender라는 이름으로 원하는 AnyObject를 넘겨줄 수 있다. sender라는 것은 segue가 연결되게 만드는 객체이다.  
이때 identifier가 필요한 한 순간이다.  

identifier가 필요한 훨씬 더 중요한 순간은 segue가 연결될 때, segue로 연결하고 있는 MVC는 보통 화면에 나타나기 위한 준비가 필요하다.  
어떤 정보가 필요하다. 모델이 미리 설정될 필요가 있을 수도 있고 그림을 그려내는 속성들이 미리 설정될 필요가 있을 수도 있다.  
이것 들은 '준비'할 때 벌어질 것들이다. 이 '준비'는 메소드와 함께 발생하게 되는데 segue를 연결시키는 ViewController에게 보내지는 메소드이다.  
새로 만들어지고 있는 ViewController가 아니라 Segue를 연결시키는 ViewController를 말한다. (현재 화면의 viewController에 메소드를 호출해야
한다는 의미)
``` 
func prepareForSegue(segue : UIStoryboardSegue, sender: AnyObject?) {
    if let identifier = segue.identifier {
        switch identifier {
            case "Show Graph" : 
                if let vc = segue.destinationViewController as? GraphController {
                    vc.property1 = ...
                    vc.CallMethodToSetItUp(...)
                }
                default : break
            }
    }
}
```
위의 메세지의 의미는 segue에 의해 새롭게 만들어질 MVC를 준비하라는 것이다. 각각의 MVC는 저마다의 세계를 가지고 있다. 객체지향의 캡슐화 등등..

위 코드를 살펴보자. 첫번째 인자인 segue는 UIStoryboardSegue라는 클래스의 인스턴스이다. segue는 identifier의 정보를 가지고 있고 다음 화면으로  
연결되는 MVC의 controller정보(여기서 GraphController)도 있다. UIStoryboardSegue의 프로퍼티 중 하나가  destinationViewController이다.  
다음 sender라는 인자도 있다. 이건 누가 이 연결을 발생(트리거)시켰는가 하는 것이다. 보통 버튼인데 꼭 버튼이 아니어도 AnyObject가 될 수 있다.  
if let (옵셔널 바인딩)을 한 이유는 identifier라는 변수가 nil인지 아닌지 확인하기 위해서이다.  
여기서 switch문을 사용하는데 만일 identifier가 'Show Graph'라면 destinationViewController를 segue에 연결되도록 시도하고 준비할 것이다.  
그게 어떤 GraphController 중 하나라고 가정하면서 말이다.  
이 segue의 destinationViewController를 확인하고 그리고는 이게 Graph Controller인지 아닌지 확인해 볼 것이다. 이를 확인하기 위해서 'if let'과 'as'를 사용할 것이다. 왜냐하면 destinationViewController의 타입은 UIViewController이기 때문이다. 그래서 as로 이걸 캐스트(cast, 타입변환)해줄 것이다.  
가끔 as를 사용해서 AnyObject를 캐스트해준 것처럼 하는 것이다.  
여기서 가장 중요한 것은 destinationViewController를 준비할 때 지금 막 만들어진 것만 가능하다. 찰나의 시간 전에 만들어진 것을 의미한다. 
outlet이 아직 설정되지 않았다면 outlet이 설정되기 전에 준비하는 작업을 한다면 아주 짜증날 것이다. 왜냐하면 보통은 ViewController를 준비할 때 ViewController의 outlet 안에서 설정하고 싶을 것이다. 예를 들어 계산기에서 뭔가를 display에 설정하고 싶다던가.. 하지만 그렇게 할 수 없다.  
왜냐하면 outlet이 설정되기 이전에 해야 하는 일이기 때문이다. 그래서 데이터를 그냥 줘야 하고 그 데이터를 가지고 있다가 outlet이 설정되고 나면 그때 UI에 데이터를 전달하기 시작하는 것이다. 

이 외에도 shouldPerformSegueWithIdentifier라고 하는 것이 있다. (identifier로 segue를 실행해야 한다.) 이건 기본적으로 Bool을 반환하는데 그 segue가 발생되어야 하는지 아닌지를 말하는 것이다. 이걸로 화면이 넘어가지 않도록 예방해줄 수 있다. 

tabBarController에서 탭의 이름같은 것을 수정하고 싶을 때에는 tabBarController에서 수정할 수 없다. 이건 그저 tabBarController이기 때문이다. 탭의 이미지나 타이틀은 이 안에 있는 MVC가 결정한다. 

Tab Bar에 있는 것들은 UI가 서로 독립적인 경우가 많다. 서로 연결되어 있지 않는다. 서로한테 segue 연결을 해 줄 수도 없다. 


oulet은 준비하고 있을 땐 아직 set되지 않은 상태이다.   

---
layout : single
title : Delegation
---
# Delegation

프로토콜을 사용하는 중요한 예시로는 델리게이션이 있다. ![image-20210524220610836](/Users/shhong/Library/Application Support/typora-user-images/image-20210524220610836.png)

위 이미지처럼 뷰와 컨트롤러 사이에서 보이는 것 처럼 델리게이션은 눈에 보이지 않는 소통을 하는 방식을 말한다. 뷰는 가끔 "내가 이걸 해야할까(should)"나 "나는 이걸 할거야(will)"처럼 메세지를 보내고, 스크롤 뷰를 예로 들면 "이쪽으로 스크롤 했어(did)", "이만큼 줌 할거야(will)"아니면 테이블처럼 데이터가 있다면 "400개의 테이블 열을 갖고 있어(count)", "데이터를 7번째 열에 넣고 싶어(data)" "데이터를 12번째 열에서부터 20번째 열까지 넣고 싶어(data)" 등등 여기 있는 이런 의사소통들이 필요한데, 이젠 프로토콜이 뭔지 알고 있으니까 쉽게 해결할 수 있다. 프로토콜을 새로 하나 만들어서 모든 메소드를 넣어버리는 것이다. 보통은 컨트롤러에서 하겠지만 이젠 꼭 컨트롤러일 필요가 없다. 예를 들면, 컨트롤러는 이 메소드들을 실행하는 다른 오브젝트를 인스턴스화할 수 있는 것이다. 물론 여전히 컨트롤러 영역의 부분이겠지만 더이상 UIViewController의 서브클래스일 필요는 없는 것이다. 

* 실행방법
  1. 어느 뷰가 델리게이션 프로토콜을 선언한다. 
     뷰는 컨트롤러의 일반화되어 있는 미니언들이다. 뷰는 메소드가 있는 프로토콜을 선언한다. will, should, did나 data, count 같은 메소드같은 것들 말이다. 컨트롤러 같은 누군가가 실행해주길 바라는 것들이다. 
  2. 뷰의 API 어딘가에 public 프로퍼티가 하나 있는데 weak delegate이고 타입은 아까 만든 프로토콜이 될 것이다. 
     var 변수가 될 것이고 프로토콜을 실행하는 오브젝트가 되어야 할 것이다. weak인 이유는 뷰가 이렇게 말할 수도 있다. "아무도 will, did, should나 count, data를 하지 않으면 나는 그냥 비어 있는 채로 남겨져 있을텐데..내가 할 일을 아무것도 하지 않게 될거야. 아무에게도 will, should, did를 알려주지 않을거야, 누구에게도 should를 묻지 않을거야" 그래서 nil이 될 수도 있다. 그래서 delegate는 보통 weak이다. 뷰가 컨트롤러를 메모리에 저장하게 하고 싶진 않을테니 말이다. 
     컨트롤러가 뷰를 메모리에 저장할 수는 있지만 뷰가 컨트롤러를 메모리에 저장하게 두고 싶지는 않을 것이다. 
  3. 뷰가 메세지를 보내길 원한다면 언제든지 뷰가 이 델리게이트 프로퍼티를 사용할 것이다.
     should, will, did, count, data 등 이 델리게이트에게 보내는 것이다. 델리게이트가 프로토콜 메소드를 실행한다는 것을 알고 있기 때문이다. 
  4. 컨트롤러는 이 프로토콜을 실행한다고 선언해야 한다. 
     UIViewController, UITableViewControllerDelegate 등등 
  5. 컨트롤러는 자기자신(self)을 델리게이트라고 지정해야한다. 자기 자신을 이 프로퍼티로 설정하는 것이다.
     이렇게 하면, 뷰가 델리게이트에게 말을 하게 되면 컨트롤러에게 말을 하게 되는 것이다. 
  6. 컨트롤러는 이제 프로토콜을 실행한다. 
     iOS에서 보게되는 프로토콜, 델리게이션의 거의 모든 메소드들은 옵셔널이다. 그리고 이 프로토콜들은 Objective-C가 될 것이다.   



뷰는 이제 컨트롤러에 연결된 상태지만 뷰는 아직도 아무것도 모른다. 어떤 오브젝트가 will, should, did를 실행하고 있는지 말이다. 그 오브젝트가 구조체가 될 수도 있고 꼭 클래스가 아닐 수도 있기 때문에 뭐든 실행할 수 있다. 

iOS에서 어떤 것은 클로저로하고 어떤 건 델리게이트로 처리하는 것을 보게 될 것이다. 이 두 가지가 완전히 대체될 수 있는 것은 아니다. 프로토콜이 특히 유용할 때가 있는데 그 이유는 이 오브젝트가 무엇을 위임할 수 있는지를 매우 분명히 하기 때문이다. 클로저 같은 경우에는 error callback을 하거나 멀티 쓰레드(Multi-thread)환경에서 어떤 처리가 오래걸리다가 나중에 완료가 되면 끝났다고 알려줄 수 있다. 

* Example

  UIScrollView는 UIView의 서브클래스이고 delegate라는 이름의 weak 변수가 있다. 타입은 UIScrollViewDelegate옵셔널이다. 델리게이트를 반드시 설정할 필요는 없기 때문이다. 
  
  ```swift
  weak var delegate : UIScrollViewDelegate?
  ```
  
  UIScrollViewDelegate 프로토콜은 다음과 같이 생겼다. 
  
   ```swift
   @objc protocol UIScrollViewDelegate {
     optional func scrollViewDidScroll(ScrollView: UIScrollView)
     optional func viewForZoomingInScrollView(ScrollView: UIScrollView) -> UIView
     ...
   }
   ```
  
  컨트롤러는 스크롤뷰와 함께 작동하고 싶어하니까 MyViewController는 UIViewController의 서브클래스이고 그 다음에 UIScrollViewDelegate를 넣어서 실행할 것이다. 
  
  그 다음에 다음과 같이 viewDidLoad()에 코드를 넣는다.
  
  ```swift
  override func viewDidLoad() {
    super.viewDidLoad()
    
    scrollView.delegate = self
  }
  ```
  
  scrollView는 스크롤뷰를 가리키는 outlet이고 scrollView.delegate = self한 것은 뷰 컨트롤러가 이 프로토콜을 실행한다고 선언했기 때문에 self로 해도 괜찮다. scrollView는 뷰컨트롤러에게 말을 걸기 위해 이것을 사용할 것이다. 
  
  **스크롤뷰는 contentSize가 있어야한다.** 그렇지 않으면 작동하지 않는다. 
  
  zoomScale을 설정하면 줌이 가능한 크기를 설정할 수 있다. 
  
  델리게이트 메소드를 실행하지 않으면 줌이 작동하지 않을 것이다. 
  
  ```swift
  func viewForZoomingInScrollView(sender: UIScrollView) -> UIView
  ```
  
  여기서 주목할 점은 첫번째 인자는 항상 우리에게 델리게이트 메소드를 보내는 그것이 된다는 것이다. sender라는 이름의 어느 스크롤뷰인지 말해준다. 이 메소드는 두 손가락으로 확대하면 transform이 바뀌게 될 것이다. 
  
  


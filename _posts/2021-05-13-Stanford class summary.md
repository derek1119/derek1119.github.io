---
layout : single
title : MVC
---

## MVC란 무엇인가
Model : application이 무엇을 하는지 뜻함, 화면에 어떻게 그려지는 것과는 관계 없음 (계산기에서는 계산을 하는 부분) What

Controller : 모델이 스크린에 어떻게 표현될 것인가. How (계산기에서 숫자를 누르면 입력을 모델에 전달하고 뷰에는 어떻게 표시 될 것인지 정함)

View : 컨트롤러의 미니언(하위종속자). (계산기의 화면)
  
노란색 선은 건널 수 없다. 연속된 선은 정말 주의하지 않으면 건너서는 안되고 흰색 점선은 자유롭게 다닐 수 있다. 

컨트롤러는 모델의 모든 것을 알고 있고 보내고 싶은 메세지를 보낼 수 있다. 
모델의 모든 것을 통제한다. 사용자에게 모델에 있는 것을 표현하거나 사용자에게 정보를 받아 모델을 업데이트하는 것이다. 

컨트롤러는 자신의 미니언(View)을 통해서 표현해야하는데 보통 아울렛으로 연결된다.

뷰와 모델은 절대로 이어질 수 없다. 왜냐하면 모델은 UI와 독립적이다. 

뷰와 컨트롤러의 관계를 살펴보자. 뷰는 컨트롤러에게 메세지를 보낼 수 있을까?  
그럴 수도 있고 그러지 않을 수도 있다. 그 이유는 뷰는 계산기에 대해서도 모르고 이게 계산인지도 모른다. 뷰와 컨트롤러간의 소통은 제한적이다. 물론 뷰가 컨트롤러에게 메세지를 보내야 하는 일은 분명히 일어난다. 구조화된 방식으로 소통해야하는데 그 한가지는 타겟-액션이다. (@IBAction) 그 외에서 스크롤링, 줌 스케일 등등이 있다. 스크롤뷰와 같은 뷰는 무언가를 해도 괜찮은지 확신이 필요하다. “사용자가 지금 수직으로 스크롤 하는 것을 허용해야 할까?” 같은 것 말이다. 이를 컨트롤러에게 물어봐야한다. 이 과정에서 should, will, did 와 같은 말이 포함될 것이다. 이 질문들은 Delegate라고 불리는 것을 통해서 이루어진다. 이때 delegate라는 단어는 적당하다. 왜냐하면 뷰의 미니언은 컨트롤러에게 어떠한 책임을 위임하고 있으니까. delegate는 뷰 안에 있는 프로퍼티이다. 뷰는 뷰컨트롤러에 대해 아무 것도 알지 못하기때문에 프로퍼티의 클래스가 되지 않고 protocol로 불리는 것이 된다. 프로토콜이란 다른 녀석이 대신 실행하기로 약속된 메소드들을 모아놓은 것이다.   

뷰는 자신이 보여주고 있는 데이터를 소유할 수 없다. 

뷰는 매번 컨트롤러에게 매번 물어보면서 컨트롤러는 모델에서 데이터를 가져온다.  이건 다른 종류의 프로토콜이다. 이것도 delegation의 과정으로 일어나는데 이 delegate를 dataSource라고 부른다.

컨트롤러는 UI로직을 다루고 모델은 UI와 별개이다. 그러므로 모델은 컨트롤러에게 메세지를 보낼 수 없다. 하지만 UI와는 상관없는 모델이 값이 바뀌는 데이터가 있다면 어떻게 컨트롤러에게 알려줄까? 이는 radio-station 모델을 이용해서 해결한다. 라디오 방송국은 자신의 방송국을 세우고 뭔가 흥미로운 일이 생기면 방송을 한다. 그 다음 컨트롤러는 그 방송국에 주파수를 맞추고 있어 모델이 컨트롤러에게 직접알려주는 것이 아니라 알고 싶어하는 사람이라면 그들에게 방송으로 말을 해준다. 모델에 의해 이뤄지는 방송국 안의 모든 소통은 UI와 관련이 없고 모델 안에 있는 데이터와 관련된 것이다. 













# API(Application Programming Interface)
## API란 무엇인가
API란 위 그림에서 점원과 같은 역할을 한다. 손님(프로그램)이 주문할 수 있게 메뉴(명령 목록)을 정리하고, 주문(명령)을 받으면 요리사(응용프로그램)와 상호작용하여 요청된 메뉴(명령에 대한 값)를 전달한다. 쉽게 말해, API는 프로그램들이 서로 상호작용하는 것을 도와주는 매개체로 볼 수 있다. 

### API의 역할은?  
API는 서버와 데이터베이스에 대한 출입구 역할을 한다. 
데이터 베이스에는 소중한 정보들이 저장이되는데 모든 사람들이 데이터베이스에 접근할 수 있으면 안된다. API는 이를 방지하기 위해서 서버와 데이터베이스의 출입문 역할을 하며 허용된 사람에게만 접근성을 부여한다. 

API는 애플리케이션과 기기가 원활하게 통신할 수 있도록 한다.

API는 모든 접속을 표준화 한다. 
API는 모든 접속을 표준화하기 때문에 기계/운영체제 등과 상관없이 누구나 동일한 엑세스를 얻을 수 있다. 쉽게 말해, API는 범용 플러그처럼 작동한다고 볼 수 있다. 

- API의 유형  
Private API  
Public API  
Partner API 

### View에 관하여 

버튼, label, 등등 뷰의 subView(자식뷰)이다. UIView를 상속받았다. 
 Core Graphics Concept이란?


## UIBezierPath
```swift
let green = UIColor.greenColor()
```
여기서 greenColor()는 타입메소드이다. 왜냐하면 UIColor라고 하는 타입에 보내고 있으니까   
UIColor는 클래스이고 여기에 보내고 있다.  
즉, UIColor 클래스 안에 static Class func greenColor()라고 정의 되어 있을 것이다.  
UIColor는 투명도도 조절할 수 있다. colorWithAlphaComponent()
alpha는 투명도를 가르킨다. 0은 완전투명 1은 완전 불투명

hidden이라는 인스턴스를 이용하여 뷰를 숨길 수 있는데 이때는 어떠한 이벤트도 받지 않는다. 
텍스트는 폰트로 표현되는 UIBeizerPath의 모음일 뿐이다. 

text를 Label이 아닌 view에 직접 올리고 싶다면 NSAttributedString, NSMutableAttributedString(이것은 swift string도 오브젝트-C의 string도 아니다.) 

 좋은 앱을 만들고 싶다면 폰트를 신경써야한다. 

Var contentMode: UIviewContentMode로 컨텐트를 이동시킬 수 있다. (이미지가 landscape로 바뀌면 비율이 늘어나는 것을 방지)


## Rect과 Frame과 Bounds의 차이점 

####  Frame과 Bounds의 차이
먼저 frame과 bounds는 UIView의 instance property이다. 

둘의 타입은 CGRect이다. 

CGRect 은 CGpoint와 CGSize를 모두 포함한 구조체이다. 

단 frame은 superview의 좌표시스템 안에서 view의 위치와 크기를 나타낸다
bounds는 자신만의 좌표시스템 안에서 view의 위치와 크기를 나타낸다. 

- Mapping(맵핑) : 데이터간의 대응관계를 설정하는 것 

### Gestures
오직 View만이 gesture recognizer 을 인식할 수 있고 Controller는 불가능하다.   
1단계는 원하는 gesture recognizer을 생성하고 원하는대로 설정한 뒤에 어떤 View에게 지금부터 제스처를 인식해달라고 부탁을 한다.  
2단계는 그 recognizer가 제스처를 인식했다면 어떻게든 그걸 처리해야하고 처리하는건 gestureHandler가 처리를 한다. 그래서 gesture recognizer와 gestureHandler가 있는 것이다. gestureHandler는 gestureRecognizer가 해당 제스처를 인식하는 ‘상태 기계(State Machine)’로 갈 때 호출된다.  

GestureRecognizer를 생성하고 어떤 View에 추가하는 것은 일반적으로 Controller가 처리한다. 이는 좋은 방법이다. view는 어차피 controller의 미니언(부하)이니까 controller는 자신들의 미니언들을 관리하고 싶어한다. Controller는 제스처를 인식할 수 없다. Controller는  gestureRecognizer를 View에 추가할 수만 있다. 소속된 View에 gestureRecognizer를 생성해서 추가하는 것이다. 

GestureHandler는 controller가 해도되고 view가 처리해도 된다.  
만일 제스쳐가 view가 어떻게 보이는지 바꾸는 정도라면 view가 처리할 것이다. 
하지만 안약에 제스처가 모델을 직접적으로 변경한다면 당연히 controller가 제스처를 처리하게 될 것이다. 

UIView 에서 제스처를 가장 넣지 좋은 곳은 outlet의 didSet 구문 안이다. 
제스처는 두개의 인자를 받는데 첫번째 인자는 누가 제스처 인식을 처리할 것인지에 대해 묻는 것이다. 만일 self라면 Controller가 이를 처리한다는 의미이다. 이 아울렛은 controller 안에 있는 것이기 때문이다. 두 번째 인자는 제스처가 인식되면 self 안에 어떤 메소드를 호출할 건지이다. 

UIPanGesture의 추상화된 슈퍼클래스는 state라는 중요한 변수를 갖고 있다. gestureRecognizer는 ‘상태 기계’를 거치는데 핸들러에서 제스처가 어떤 상태에 있는지 확인할 수 있다. 모든 gestureRecognizer는 .Possible 상태에서 시작하여 gestureRecognizer.Possible이다. 스와이프같은 끊어지는 제스처에는 일단 스와이프를 감지하면 .Recognized로 바뀐다. 그다음 핸들러가 불려진다. 


## MVC 여러 개를 연결하여 더 큰 앱 만들기
### MVC
한 MVC의 View가 다른 MVC의 일부가 되는 형태를 만드는 것이다.  
Tap bar controller , SplitViewController, NavigationController
각 탭을 누를 때마다 화면에 다른 MVC가 나타난다. 
 
NavigationController는 카드를 쌓는 것과 같다. 각각의 카드가 서로 다른 MVC가 되는 것이다. 
뒤로 돌아가는 것은 해당 view가 사라진 것이다. 힙 메모리에서도 사라진다. 
rootViewController : 가장 첫 페이지, 가장 아래에 깔린 MVC를 rootViewController 라고 부른다. 항상 가장 아래에 위치한다. 


### MVC를 더 잘 사용하려면
어떻게 하나의 MVC에서 다른 MVC들에 접근 할 수 있을까?  
이는 변수를 통해서 갈 수 있다.  
```
var viewController : [UIViewController]? { get set }
```  
Tap bar 는 왼쪽에서 오른쪽으로 가는 array이다. 
splitView는 [0]은 master, [1] detail 이다. 
Navigation controller은 [0]이 root이고 나머지는 그 순서에 따라 stack으로 쌓인다. 
값을 set(할당) 할 수 있지만 값을 가져오기(get)만 할 것이다. 

그렇다면 만약 SplitViewController 내부에 있다면 들어와 있는 SplitViewController에 어떻게 접근할까?  
그 답은 UIViewController는 중요한 세가지 프로퍼티가 존재하는데 이는 tapBarController, splitViewController, navigationController이다. 이 3가지는 모두 옵셔널이다.  
  
두 가지의 viewController에 들어가 있는 것도 가능하다. 

### MVC들 연결하기 

SplitViewController의 경우 SplitViewController를 불러와서 master와 detail뷰로 control을 누른 채 드래그 하면 연결이 된다.   


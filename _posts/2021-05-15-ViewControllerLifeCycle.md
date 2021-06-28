---
layout : single
title : View Controller Life Cycle
---
# 뷰컨트롤러 생명주기(Life Cycle)
뷰 컨트롤러 생명주기는 단지 메소드를 모아놓은 세트이다. 뷰 컨트롤러가 life cycle을 거치는 동안 뷰 컨트롤러에 보내지는 메소드이다. 
life cycle이라는 것은 만들어지고 자기 일이 사라지는 과정을 말한다.  

생명 주기의 시작은 분명히 MVC를 생성하면서 부터이다. MVC는 거의 대부분 스토리보드로 만들어지는데 그 다음에는 다음 단계들을 거치게 된다.  
1. Preparation if being segued to. (준비하기) : 연결되고 준비하는 과정이기 때문에 제일 먼저 일어난다.  
2. Outler Setting(아울렛 설정) : 이 과정에서 앱이 충돌할 수 있다. 준비를 마친 후에 outlet을 설정을 해야하기 때문이다.  
3. Appearing and disappearing(나타나거나 사라지거나) : 이것은 반복적으로 나타날 수 있다.  
4. Geometry changes(기하학적 변화) : 나타나거나 사라지는 과정에서 또는 화면을 돌릴 때 기하학적인 변화가 일어난다.  
5. Low-memory situations(메모리 공간 부족 상황) : 뷰 컨트롤러에게 메모리를 더 할당해달라고 요청, 드문 경우이다. 

## 생명주기에 대한 메소드
### View
* viewDidLoad( )

```swift
override func viewDidLoad() {  
	super.viewDidLoad()  
	// always let super have a chance in lifecycle methods  
	// do some setup of my MVC  
}
```

인스턴스 생성과 준비 그리고 Outlet 설정이 끝나면 viewDidLoad( )라는 매우 중요한 메소드를 얻게 될 것이다. 'Load'라는건 Outlet을 
불러온다는 것을 말한다. viewDidLoad() 내부는 UIViewController에서 초기화 코드를 넣기에 훌륭한 장소이다. 그래서 ViewController의 init() 메소드를 절대로 override하지 않아도 된다. viewcontroller에는 정말 이상한 init메소드가 있다.  
prepare도 되어있고 outlet도 설정되었으니까 좋은 위치이다. 
여기에서 해주기 정말 좋은 것은 UI를 업데이트 하는 것이다. viewDidLoad()에서는 모든게 업데이트 될 준비가 되어있다.  
viewDidLoad()에서 조심해야할 한가지는 geometry(기하)이다. 뷰의 geometry는 스토리보드에서 여전히 정사각형 형태일 것이다.  
따라서 viewDidLoad() 안에서 geometry와 관련된 계산을 하는 것은 모두 쓸모가 없다.  
또 viewDidLoad()에서 하지 않는 건 뭔가 복잡한 작업을 시작하는 것들이다. 왜냐하면 ViewController가 생성한 viewDidLoad()는 그 안에 있는 것들이 화면에 나올지. 100% 보장할 수 없기 때문이다. 사용자가 어떤 UI를 터치하는지에 따라 나타날 수도 없을 수도 있다. 그래서 화면이 확실히 나온다는 것을 알기 전까지는 복잡한 계산을 피하는 것이다. 

* viewWillAppear( )  
```swift
func viewWillAppear(animated : Bool)
```
viewWillAppear( )는 화면을 올리기 직전에 호출되어 이게 호출되면 화면에 나올거라는 걸 확신할 수 있다. 뭔가 복잡한 걸 시작하기 좋은 곳이다. 
multi-threaded 라는 건 앱에서 동시에 여러가지를 실행하는 것을 말한다. 프로세서를 공유하는데 기본적으로 시간 공유 프로세서 개념이다. UI는 최우선 순위로 한개의 thread(쓰레드)에서 작동할 것이다. 그 밖에 네트워크에 접속하거나 막히는 것들을 처리하고 시간이 오래 걸리는 것들은 다른 thread에서 작동할 것이다. 이것이 동시에 작동하여 UI에 막힘이 생기는 건 원하지 않기 때문이다. UI를 터치했는데 아무 일도 일어나지 않는건 바라지 않는다. viewWillAppear( )는 다른 thread에서 하게 하는데 활용할 수 있다. 그럼 가져오려고 했던 데이터나 복잡한 것들이 없는 체로 View는 즉시 화면에 나타날 것이다. 

* viewDidAppear( )  
```swift
func viewDidAppear(animated : Bool) 
```
viewDidAppear( )는 화면이 나타난 직후에 호출되는 메소드이다. 여기서는 animation을 시작하기에 좋은 곳이다. 이젠 이미 화면이 나타난 상태이다. 

* viewWillDisappear( )  
```swift
override func viewWillDisappear(animated : Bool) {
		super.viewWillDisappear(animated)
}
```
viewDidAppear( )와 비슷한 viewWillDisappear( )라는 것도 있다. viewDidappear( )에서 했던 것을 undo(되돌리기)하게 될 것이다. 여기서 애니메이션을 멈추거나 Gyro(회전이나 평형상태 측정)를 보는 것을 멈춰야 할 것이다. 화면이 곧 사라질 예정이기 때문에 Gyro같은 것을 보는 것은 의미가 없다. 

* viewDidDisappear( )  
```swift
func viewDidDisappear(animated : Bool) 
```
여기선 viewWillAppear()안에서 네트워크로부터 가져온 데이터를 풀어줄 수 있다. 
위 내용을 살펴보면 서로 대칭이라는 것을 알 수 있다. Will/DidAppear과 Will/DidDisappear 이렇게 대칭이 된다. 이 메소드들이 불규칙적으로 호출되면서 do와 undo가 되는 것이다. 이런 appear과 disappear가 이루어질 때 viewDidLoad()는 오직 한번만 호출된다. appear과 disappear는 화면이 나타나고 사라질 때마다 반복적으로 호출될 수 있다. 

### Geometry의 변화   
어디서 해야할까? geometry를 위한 특별한 생명주기 메소드들이 있다. viewWillLayoutSubViews( )와 viewDidLayoutSubviews( )  
bounds가 변할때 마다 혹은 때로는 변하지 않을 때도 포함되니 조심해야한다. 이 두 메세지를 받을 것이다. viewWillLayoutSubViews( )와 viewDidLayoutSubviews( ) (현재 상황에 따라서 bounds가 변할 수 있다. tab bar, splitview, rotation이 생기거나 등등) viewWillLayoutSubViews( )와 viewDidLayoutSubviews( ) 사이에서 모든 Autolayout이 동작할 것이다. 여기가 Geometry 처리를 할 수 있는 유일한 곳이다. 여기서 알아둬야할 것은 bounds가 변할 때만 이 메소스들이 호출된다고 생각해서는 안된다. 시스템은 필요할 때 언제든 이 메소드들을 호출할 수 있다. 

* Autorotation  
디바이스를 가로로 긴 화면에서 세로로 긴 화면으로 돌리면 자동으로 bound를 바꾼다. 그래서 viewDidLayoutSubviews( )가 다루거나 아니면 constrains를 갖게 되는 경우가 더 많다. 그렇지만 Autorotation을 다른 것과 연관시키는 것은 가능하다. 특히 애니메이션에서 그렇다. 이 rotation을 잘 살펴보면 이들이 animation으로 움직인다는 것을 확인할 수있다. 따라서 개발자가 원하면 회전하는 화면을 직접 조종할 수도 있다.   
```swift
func viewWillTransitionToSize (size : CGSize, withTransitionCoordinator : UIViewControllerTransitionCoordinator)
```
viewWillTransitionToSize()라는 생명주기 메소드를 사용하면 된다. 이 메소드가 기본적으로 말하는 것은 화면을 전환하게 될 것이고 TransitionCoordinator(전환 조정자)라는 것이 있다. TransitionCoordinator(전환 조정자)에서 중요한 것은 개발자가 제공할 수 있는 Closure(클로저)이다. 클로저는 화면이 회전하면서 애니메이션으로 움직이는 것과 함께 실행될 것이다. 

### low-memory Situations

메모리가 부족할 땐 didReceiveMemoryWarning이 불린다. 이 메소드는 앱이 메모리를 많이 쓸 때만 일어나는 상황이다. 이 메소드가 호출되면 힙(Heap memory)에서 많은 메모리를 먹고 있는 나중에 다시 만들 수 있는 포인터들을 날려버려야 한다.  그렇게 되면 웹이나 파일시스템에서 다시 다운로드할 수 있다. 이 메소드가 호출되면 그런 것들을 버려야 한다. 

### awakeFromNib
생명 주기 맨 처음으로 되돌아가서 awakeFromNib( )이라는 메소드가 있다. 이 메소드는 매우 초반에 보내지는데 prepare 전, Outlet 설정하기 전, 그 모든 것들의 이전이다. 하지만 오직 스토리보드로 부터 나오는 경우에만 해당된다. 정말 초기에 호출되는 메소드 이며 스토리보드에 나오는 모든 오브젝트에 보내지는 메소드이다. 매우 이례적인 상황에서 쓰이기 때문에 viewDidLoad 나 viewWillAppear에 사용하는 것이 낫다. 

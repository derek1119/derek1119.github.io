# UITableView

 ![image-20210529101508236](/Users/shhong/Library/Application Support/typora-user-images/image-20210529101508236.png)

plain 스타일은 그냥 항목들을 쭉 나열한 것이고 섹션으로 묶을 수도 있지만 보통은 하나의 긴 목록이다. 오른쪽은 grouped 스타일인데 TableView 안의 섹션이 더욱 분명하게 구분되어 있는 형태이다. 섹션별로 다른 데이터가 들어가는 형태이다. 일반적으로 plain 스타일에는 Dynamic(동적) 데이터(아이템 수가 몇 개가 될지 모르는 데이터들)를 사용하며 반면에 Grouped 스타일은 Static(정적) 데이터를 사용한다. 몇 개의 목록일지 알고 있고 각 줄에 어떤 데이터가 들어갈지 확실히 알고 있는 경우이다. 

UITableView도 View의 일종이고 UIView의 하위뷰이다. 정확하게는 UIScrollView의 하위뷰이다.  

### 테이블뷰의 용어들 

<img src="/Users/shhong/Library/Application Support/typora-user-images/image-20210529105544979.png" alt="image-20210529105544979" style="zoom:50%;" />

* Table Header
  TableView의 맨 위에 있는 영역이다. 검색 창 같은 것을 넣어줄 수 있다. 

* Table Footer
  TableView의 맨 아래 부분에 들어가는 UIView이다. 

* Section 
  <img src="/Users/shhong/Desktop/스크린샷 2021-05-29 오전 11.01.05.png" alt="스크린샷 2021-05-29 오전 11.01.05" style="zoom:50%;" /> 
  이 사이에 Section이 있다. Section은 Section의 Header와 Footer라는 것으로 둘러 쌓여 있는데 이 둘은 보통 String 타입이지만 UIView가 될 수도 있다. 여기에 몇 개든 올 수 있는 Row가 들어온다. 

* Table Cell

  <img src="/Users/shhong/Library/Application Support/typora-user-images/image-20210529110809045.png" alt="image-20210529110809045" style="zoom:50%;" />
  Cell도 UIView이다. 정확히는 UITableViewCell이라고 부르는게 정확하다. UIView의 서브클래스이다. 특정한 Row에 데이터를 가져와서 보여주는 역할이다. 

* Sections of Not
  TableView는 Section이 있을 수도 있고 없을 수도 있다. 즉, Section은 선택 사항이다. 

* Cell Type
  미리 설정된 타입 4가지와 커스텀한 타입으로 되어있다. 

  * Subtitle(UITableViewCellStyle.subtitle)
    <img src="/Users/shhong/Library/Application Support/typora-user-images/image-20210529162013251.png" alt="image-20210529162013251" style="zoom:33%;" />
  * Basic(.default)
    <img src="/Users/shhong/Library/Application Support/typora-user-images/image-20210529162037948.png" alt="image-20210529162037948" style="zoom:33%;" />
  * Right Detail(.value1)
    <img src="/Users/shhong/Library/Application Support/typora-user-images/image-20210529162100340.png" alt="image-20210529162100340" style="zoom:33%;" />
  * Left Detail(.value2)
    <img src="/Users/shhong/Library/Application Support/typora-user-images/image-20210529162117798.png" alt="image-20210529162117798" style="zoom:33%;" />
  * Custom

  

# UITableView Protocols

* #### 모든 일을 코드로 연결하는 방법

  UITableView는 말그래도 아무것도 할 수 없는 클래스이다. 단, DataSource와 Delegate가 있다면 가능하다. 여기서는 두 프로토콜이 필수적이며 특히 DataSource는 반드시 있어야한다. 두 프로토콜이 없이 테이블뷰를 작동시키는 유일한 방법은 완전히 정적인 TableView를 만드는 것이다. 그런 것이 아니라면 데이터를 가져오기 위해서는 DataSource를 반드시 활용해야 한다. 

  UITableViewController는 자동으로 자기 자신을 Delegate와 DataSource로 설정하기 때문에 Delegate 메소드에 대한 코드를 UITableViewController의 서브클래스에 직접 넣어줄 수 있다. 또한 UITableViewController에는 tableView라는 멋진 변수가 있다. 이 변수는 기본적으로 self.view를 반환하는데 UITableView로 반환할 것이다. 

* #### DataSource 사용 시기 

  정적이 아닌 동적 데이터가 있을 때는 항상 사용해야 한다. 이 프로토콜에는 정말 중요한 3개의 메소드가 있다. 

  1. How many sections in the table? (몇 개의 섹션을 넣을 것이냐?)

     ```swift
     func numberOfSections(in tv: UITableView) -> Int
     ```

     default값은 1이다. 

  2. How many rows in each section? (각 섹션에 몇 개의 row가 있을 것이냐?)

     ```swift
     func tableView(_ tv: UITableView, numberOfRowsInSection: Int) -> Int
     ```

     default값이 없다. 

  3. Give me a view to use to draw each cell at a given row in a given section. (하나의 row를 채우기 위해 UITableViewCell 중 하나를 주어라.) 

     특정 row를 그려야하는 시점에 UITableViewCell을 UITableView에 다시 돌려주는 방법은 메소드가 호출되어야 한다. 여기서 만약에 Label과 ImageView가 들어간 거대한 UI라면 이 row가 10만개라면 끔찍한 작업이 될 것이다. 10만개의 view를 만들어야 하기 때문이다. 하지만 모든 row를 그려내는 UIView, UITableViewCell은 전부 재사용(reused)되기 때문이다. 현재 화면에 보이는 것만 UITableViewCell이 있는 것이다. 스크롤 하다가 셀이 맨 위로 올라오면 그걸 가져다가 한바퀴 감아서 맨 아래에 다시 쓰는 것이다. 새로운 데이터가 재사용되는 셀에 공급되는 것 뿐이다.(Cell은 있고 데이터만 패치) 

     ```swift
     func tableView(_: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
       let data = myInternalDataStructure[indexPath.section][indexPath.row]
       
       let cell = ... //create a UITableViewCell and load it up with data
       
       return cell
     }
     ```

     위 코드는 UITableView가 UITableViewCell을 달라고 DataSource에게 보내는 메소드이다. indexPath는 Section과 Row 정보가 담긴 컨테이너이다. 그리고 UITableViewCell을 반환하여 Row를 그려낸다. 그 다음 이 메소드 안에서 해주어야 할 일은, 우선 Row에 보여주려는 데이터를 모델에서 가져와야 한다. 그 다음에 데이터와 함께 그 셀을 로드하는 것이다. 

* #### 섹션 Header와 Footer Title에 대한 메소드 

  ```swift
  func tableView(UITableView, titleFor{Header, Footer}InSection: Int) -> String
  ```

  

# Table View Segue

다른 것처럼 row에서 밖으로 segue를 연결시켜줄 때도 컨트롤 + 드래그를 이용해서 연결하는데 이때 다른 점이 있다. 나타나는 검은 화면이 Selection Segue(선택 세그웨이), Accessory Action(보조 액션)으로 나누어져 있을 것이다. Selection Segue는 row를 선택했을 때이고, Accessory Action은 작은 Accessory 버튼을 클릭할 때이다.  

```swift
func prepare(for segue: UIStoryboardSegue, sender: Any?) {
  if let identifier = segue.identifier {
    switch identifier {
      case "xyzSegue": // handle xyzSegue here
      case "abcSegue":
      	if let cell = sender as? MyTableViewCell, 
						let indexPath = tableView.indexPath(for: cell),
      			let seguedToMVC = segue.destination as? MyVC { 
          seguedToMVC.publicAPI = data[indexPath.section][indexPath.row]
        }
      default: break
    }
  }
}
```

여기에 prepareForSegue가 있는데 sender가 Any타입인 것을 주의해라. Any는 UITableViewCell이 될 것이다. Row를 포함하고 있는 View말이다. UITableViewCell이 sender가 될 것이다. 첫번째로 해야하는 건 sender를 UITableViewCell로 바꿔주는 것이다. 

그리고 나면 그 셀의 indexPath를 가져오고 싶을 것이다. 어떤 row에서 Segue 연결이 넘어온 것인지 알기 위해서 말이다. 왜냐하면 모든 row가 클릭하면 segue로 연결되기 때문이다. indexPath(for: cell)라는 매우 중요한 메소드로 해당 row를 알아낼 수 있다. 그 다음에 원래 하던대로 MVC에 Segue를 연결할 것이다. 

그 다음, segue가 Public API에 연결되도록 준비를 시킬 것이다. 클릭된 Section과 Row을 기반으로 하는 모델에 있는 데이터를 사용할 것이다. 



# UITableViewDelegate

#### The delegate controls how the UITableView is displayed

일반적으로 델리게이트는 TableView를 어떻게 보여줄지에 관한 것이다. 이에 반해 Data Source는 Table안의 실질적인 데이터를 말한다. 

델리게이트와 데이터소스는 보통 같은 객체가 된다. TableViewDelegate 역시 will, did, should 같은 것이 있다. TableView에 무슨 일이 일어나는지 볼 수 있다. 예를 들면, "어느 한 row가 선택되었다(didSelected)" 등등.

```swift
func tableView(UITableView, didSelectedRowAt indexPath: IndexPath) {
  //go do something based on information about my Model
  //corresponding to indexPath.row in indexPath.section
  //maybe directly update the Detail if I'm the Master in a split view?
}
```

UITableView를 선택했을 때 target-action은 할 수 없다. 하지만 tableView didSelectedRowAt indexPath: 같은 Delegate 메소드는 실행할 수 있다. 특정 row를 터치했을 때 이 메소드가 호출될 것이다. 이와 똑같은 과정을 Detail Disclosure 버튼에서도 할 수 있는데, 이 버튼을 클릭하면 Delegate에 있는 아래 메소드가 불릴 것이다.  

```swift
func tableView(UITableView, accessoryButtonTappedForRowWith indexPath: IndexPath) 
```

이것 말고도 will과 did로 시작하는 메소드가 많다. 

### 나의 모델이 바뀌면 무슨 일이 일어날까?

Model이 더 커질수도 있고 더 작아질 수도 있다. 

할 수 있는 한가지는 망치로 내려치듯이 데이터를 다시 불러오는 것이다.

```swift
func reloadData()
```

TableView에서 reloadData()를 하면 모든 DataSource메소드를 다시 호출할 것이다. section은 몇 개이며 section에 들어있는 row는 몇 개인지 현재 보이는 모든 row에 대한 cell을 달라고 하는 것이다. 모든 것들을 전부 다시 하게 되는 것이다. 

이 과정은 망치로 전체를 내려치는 행동같은 것인데 왜냐하면 section 하나만 바꼈다는 걸 알고 있다면

```swift
func reloadRows(at indexPaths: [IndexPath], with animation: UITableViewRowAnimation)
```

위 reloadRows(at indexPaths: 같은 메소드를 호출하면 되기 때문이다. 다시 말하면, Model이 변경될 때마다 반드시 TableView에게 알려주어야 한다. Model이 바뀌면 어떤 식으로든 section과 row의 숫자가 변경될 것이다. 

"Model이 바꼈으면 바로 TableView에게 즉시 말해준다." Model을 먼저 바꾸고 그 다음에 TableView에게 말하는 것이다. 

#### Row의 높이 조절하기 

 개별 row의 높이는 보통 스토리보드에서 설정하는데 Delegate에게 요청하는 것도 가능하다. Delegate 메소드를 실행할 수 있는 것이다. row의 높이가 얼마가 될 것인지 말이다. 하지만 이것은 row의 높이가 매순간 계산되어 바뀌는 경우에 쓰는 방법이다. 

```swift
var rowHeight: CGFloat
```

혹은 

```swift
rowHeight = UITableViewAutomaticDimension
```

Autolayout을 적용해서 Automatic Dimension으로 높이를 설정할 수도 있다. 다시 말해 Autolayout을 통해 적절한 높이를 찾아달라고 한다는 뜻이다. 이 방법을 쓴다면 estimatedRowHeight 값도 설정해야될 것이다. 이는 수많은 cell들을 Autolayout하지 말고 estimatedRowHeight로 하돼, 화면에 cell이 올라오기 시작하면 그 때 Autolayout을 적용하게 해달라는 것이다. 
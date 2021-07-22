---
layout : single
title : Core Data and UITableView
---

# Core Data and UITableView

### NSFetchedResultsController

UITableView는 임의의 커다란 데이터 세트를 보여주가 딱 좋다. 애플에서 만든 아주 유용한 클래스가 있는데 그것이 NSFetchedResultsController이다. 이건 ViewController가 아니다. 코어 데이터와 UITableView가 서로 의사소통할 수 있도록 조정하는 Controller이다. NSFetchedResultsController는 생성한 NSFetchRequest를 UITableView와 연결하는 역할을 한다. 데이터 베이스에서 뭔가 작은 변화라도 있으면 NSFetchRequest가 다른 결과를 가져올 것이고 TableView에 자동으로 업데이트 될 것이다. 결론적으로 이건 실시간으로 활성화된 연결이다. 심지어 어떤 다른 context가 데이터를 바꾸더라도 신경쓰지 않아도 된다. UITableView는 항상 최신 데이터로 유지될 것이다. 

### NSFetchedResultsController의 작동방법 

1. Delegate
   델리게이트를 UITableView로 설정하고 NSFetchedResultsController가 데이터 베이스에서 FetchRequest에 영향을 주는 일이 생기는 것을 볼 때마다 NSFetchRequest는 UITableViewController에게 UITableView 원본에서 테이블뷰를 업데이트하라고 말해줄 것이다. 

2. 모든 UITableViewDataSource 메소드에 대한 실행을 제공
   테이블에 표시될 내용은 데이터 베이스로 부터 나온다. NSFetchedResultsController는 모든 것을 제공해줄 것이다. 다음 예시 실행 코드를 확인해보자.

   ```swift
   var fetchedResultsController = NSFetchedResultsController ...
   func numberOfSectionInTableView(sender: UITableView) -> Int {
     return fetchedResultsController?.section?.count ?? 1
   }
   func tableView(sender: UITableView, numberOfRowsInSection section: Int) -> Int {
     if let sections = fetchedResultsController?.sections, sections.count > 0 {
       return sections[section].numberOfObjects
     } else {
       return 0
     }
   }
   ```

   제일 먼저 할 일은 NSFetchedResultsController를 생성해야 한다. fetchedResultsController라는 변수가 항상 필요할 것이다. 그 다음 numberOfSectionInTableView같은 함수를 호출할 수 있고 기본값으로 section의 개수를 1로 두는 것이다. 

   ```swift
   func tableView(_ tableView: UITableView, cellForRowAt indextPath: NSIndexPath) -> UITableViewCell {
     let cell = tableView.dequeue...
     if let obj = fetchedResultsController.object(at: indexPath) {
       
     }
     return cell
   }
   ```



#### fetchResultsController 생성법

```swift
let request = NSFetchRequest<Tweet> = Tweet.fetchRequest()
request.sortDescriptors = [NSSortDescriptor(key: "created" ...)]
request.predicate = NSPredicate(format: "tweeter.name = %@", theName)

let fetchResultsController = NSFetchedResultsController<Tweet> (
  fetchRequest: request,
  managedObjectContext: context,
  sectionNameKeyPath: keyThatSaysWhichAttributeIsTheSectionName,
  //sectionNameKeyPath는 Entity 안에 있는 var 변수를 말한다. 그 값이 String타입이면서 그 안에 있는 타이틀을 가리킨다. 
  cacheName: "MyTwitterQueryCache")
) //NSFetchedResultsController의 이니셜 라이저이다. 
```

cacheName은 영구적인 결과를 캐싱(일지 정지)한다. 다시 말하면, 데이터 베이스에서 결과를 가져오면 내부 포맷으로 디스크에 저장을 하는 것이다. 이 저장 절차는 앱을 나갔다가 다시 돌아와도 여전히 그 캐시를 사용할 수 있게 해준다.  꽤 효율적이지만 조심해야할 부분이 있다. request에 대해선 어떤 것도 바꿀 수 없다는 점이다. request에서 무엇 하나라도 바꾸면 그럼 이제 캐시는 더이상 유효하지 않기 때문에 없던 것으로 만들어주어야 한다. 만일 이런 것이 번거로워서 캐싱을 쓰고 싶지 않다면 저기에 nil을 넘겨주면 된다. 

sectionKey에서도 조심해야할 부분이 있다. sortDescriptor가 row를 어떻게 분류할지 지정했든 간에 section이  분류된 순서와 똑같은 순서로 배열될 것이다. 테이블 전체가 section의 순서대로 분류 되었는지 잘 확인해야 한다. 

#### NSFetchedResultsController에서 무엇인가 변할 때의 메소드

```swift
func controller(NSFetchedResultsController,
                didChange: Any,
                atIndexPath: NSIndexPath?,
                forChangeType: NSFetchedResultsChangeType,
                newIndexPath: NSIndexPath?) {
  //row 업데이트하는 UITableView 메소드 호출
}
```

changeType은 Deleted, Inserted, Modified 등과 같은 것들이 올 것이다. 



위의 과정을들 하고 나면 TableViewController에 있는 fetchResultsController를 사용할 수 있다. 가장 먼저 해야하는 것은 fetchResultsController를 performFetch(Fetch 수행하기)하는 것이다. 그리고 tableView.reloadData()로 데이터를 다시 불러올 것이다. 왜냐하면 UITableView의 DataSource 메소드가 전부 다시 호출되길 원하기 때문이다. 

또 한가지 꼭 기억해야할 것은 fetchResultsController의 delegate를 self로 지정하는 것이다. 

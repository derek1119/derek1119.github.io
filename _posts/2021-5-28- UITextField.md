---
layout : single
title : UITextField
---
# UITextField

* UILabel 같지만 편집이 가능함

* UITextField가 "first responder(첫번째 응답자)"가 되면 키보드가 나타난다.
  UITextField를 first responder로 만들기 위해서는 becomeFirstResponder라는 함수를 호출하면 된다. 키보드를 내리기위해 첫번째 응답자로 하는 걸 멈출 수도 있다. 그때는 resignFirstResponder를 호출하면 된다. 

* 델리게이션은 Return(Mac의 Enter)키와 함께 사용된다. 
  return을 클릭하면 UITextField 델리게이트가 메시지를 보낸다. 

  ```swift
  func textFieldShouldReturn(sender: UITextField) -> Bool
  ```

  textFieldShouldReturn()이라는 델리게이트 메소드이다. shouldReturn(반환해야 한다.)이라고 한 건 타겟-액션에 대해 Bool을 반환하도록 했기 때문이다.  텍스트필드에 글자를 입력하고 Return을 누르면 타겟-액션을 하게 될 것이다. textFieldShouldReturn()는 true를 반환해야 한다. 그렇지 않으면 델리게이트에 의해 작동되지 않을 것이다. 이 메소드는 자주 호출하는데 여기서 resignFirstResponder를 해주게 될 것이다. return을 누르면 키보드가 내려가길 사용자가 바라기 때문이다. 

* 텍스트필드의 편집이 끝났을 때를 알아내는 것도 가능하다.
  보통은 사용자가 다른 텍스트필드를 클릭한 경우에 first responder의 역할이 끝나게 된다. 첫 응답자에서 벗어나게 되면 다음 메소드가 호출될 것이다. 

  ```swift
  func textFieldDidEditing(sender: UITextField)
  ```

* UITextField는 UIControl이다.
  그렇기 때문에 target/action을 해줄 수 있다.(control + drag 연결)

* 키보드의 설정은 UITextField를 통해서 할 수 있다. 
  iOS에는 UIKeyboard라는 별도의 오브젝트가 존재하지 않기 때문이다. 

* 키보드는 다른 View를 덮어버린다. 
  이때 NSNotificationCenter를 이용하는데 어떤 일이 생겼을 때 알림을 주는 방법 중 하나이다. 

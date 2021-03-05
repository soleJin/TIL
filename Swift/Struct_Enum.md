## struct-static let VS enum-case rawValue  <br>

<br>

문자열을 따로 관리할 때 여러가지 방법이 있다.
```swift
struct Constants {
  static let title = "타이틀"
}
```
```swift
enum Constants: String {
  case title = "타이틀"
}
```
아니면 `fileprivate으로 extension String` 으로 쓰는 방식도 있다. 어떻게 다를까?🤨<br>
개인적으로 enum에선 rawValue를 호출해 줘야해서 전 프로젝트 때 
```swift
enum Grade: String, CaseIterable {
  case VVIP = "VVIP"
  case VIP = "VIP"
  case 일반 = "일반"
  
  var priority: Int {
      switch self {
      case .VVIP:
        return 0
      case .VIP:
        return 1
      case .일반:
        return 2
      }
   }
}
```
```swift
class BankManager {
  //...
  var waitingList = Heap<Customer>(comparator: { $0.grade.priority < $1.grade.priority })
  let randomCustomerGrade = Grade.allCases.randomElement()
  //...
}
```
이렇게 구현했던 기억이 있다<br>
생각해보면? 
||struct|enum|
|:--:|:--:|:--:|
|depth|2단계<br>struct.perperty|3단계<br>enum.case.rawValue|
|instance|생성할 수 있음|못함(강제적임)|

이런 차이점을 제외하면 enum은 유한의 조건들을 갖을 때, struct는 enum보다는 유연함이 필요될 때를 고려해서 사용했던 것 간다. <br>
(사실 rawValue부르는거 자꾸 거슬려서 잘 안쓰게 되는건 안비밀 허허)<br>
그런데....? 😈<br>
요새 올라오는 다른캠퍼들의 PR에 달린 리뷰어들의 코멘트를 보다 야곰이 링크를 걸어줘서 확인해 봤더니 헐? (나와 짝꿍이었던 엘림의 질문이었....?) <br>
야곰과 흰이 알려준 [참조](https://github.com/raywenderlich/swift-style-guide#constants)를 보면 <br><br>
**Preferred:**

```swift
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}
let hypotenuse = side * Math.root2
```

이렇게 떡하니 `enum에 static 좋아함` 세상에😱 난 이걸 이제 알았...........와우... <br>
이래서 다른 캠퍼들의 리뷰도 많이 보고 듣고하는게 뼈와 살이 되는 것 같다. (하지만 내꺼하기도 너무빡센 현실... 오늘부터 잘 보면 되지 뭐...😭)<br>

결론은,<br>
난 앞으로 문자열 관리할 때는 기분 좋게 **enum**을 애용하게 될 것 같다! 오홍홍<br>
> 왜? 인스턴스 생성안하고, 확장유무 관계없이 모두 사용가능하니까
```swift
enum Family {
  case father
  case mother
  case me
  
  static let name: String
  static let address: String
}
```

[스위프트 스타일 가이드](https://github.com/raywenderlich/swift-style-guide#constants)를 틈틈히 보자!<br>
(요새 이상한거😰 공부해서 이런거 정리하면 행복할 듯😯)<br>
??... <br>
아무튼 화이팅!

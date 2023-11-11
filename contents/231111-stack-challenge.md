# Stack Challenge

## Challenge 1

스택의 주요 사용 사례 중 하나는 역추적을 용이하게 하는 것입니다.

일련의 값을 스택에 밀어 넣으면 스택을 순차적으로 popping하면 역순으로 값을 확인할 수 있습니다.

```swift
let array: [Int] = [1, 2, 3, 4, 5]

// Your code here
public struct Stack<Element> {
    private var storage: [Element] = []
    
    public mutating func push(_ element: Element) {
        storage.append(element)
    }
    
    @discardableResult
    public mutating func pop() -> Element? {
        storage.popLast()
    }
    
    public func peek() -> Element? {
        storage.last
    }
    
    public var isEmpty: Bool {
        peek() == nil
    }
    
    public init(_ elements: [Element]) {
        storage = elements
    }
}

func printInReverse<T>(_ array: [T]) {
    var stack = Stack<T>([])
    
    for value in array {
        stack.push(value)
    }
    
    while let value = stack.pop() {
        print(value)
    }
}

 printInReverse(array)

// prints
// 5
// 4
// 3
// 2
// 1
```

노드를 스택에 푸시하는 데 걸리는 시간 복잡도는 **O(n)** 입니다.

값을 print하기 위해 스택을 pop하는 시간 복잡도도 **O(n)** 입니다.

전체적으로 이 알고리즘의 시간 복잡도는 **O(n)** 입니다.

함수 내부에 컨테이너(the stack)를 할당하므로 **O(n)** 공간 복잡도 비용도 발생합니다.

> NOTE
> 
> 프로덕션 코드에서 배열을 반전시키는 방법은 표준 라이브러리가 제공하는 `reversed()` 메서드를 호출하는 것입니다.
> 
> 배열의 경우 이 방법은 시간과 공간에서 **O(1)** 입니다.
> 
> 이는 **lazy**하고 원본 컬렉션에 대한 반전된 보기만 생성하기 때문입니다.
> 
> 항목을 순회하고 모든 요소를 print하면 예상대로 시간상 **O(n)** 이 되고 공간상으로는 **O(1)** 이 유지됩니다.

## Challenge 2

문자열에 균형 잡힌 괄호가 있는지 확인하려면 문자열의 각 문자를 살펴봐야 합니다.

여는 괄호를 만나면 이를 스택에 push합니다.

반대로 닫는 괄호가 나타나면 스택을 pop헤야 합니다.

코드는 아래와 같습니다.

```swift
var testString1 = "h((e))llo(world)()"

// your code here
struct Stack<Element> {
    private var storage: [Element] = []
    
    public mutating func push(_ element: Element) {
        storage.append(element)
    }
    
    public mutating func pop() -> Element? {
        storage.popLast()
    }
    
    public func peek() -> Element? {
        storage.last
    }
    
    public var isEmpty: Bool {
        peek() == nil
    }
    
    public init(_ element: [Element]) {
        storage = element
    }
}

func checkParentheses(_ string: String) -> Bool {
    var stack = Stack<Character>([])
    
    for character in string {
        if character == "(" {
            stack.push(character)
        } else if character == ")" {
            if stack.isEmpty {
                return false
            } else {
                stack.pop()
            }
        }
    }
    return stack.isEmpty
}
 checkParentheses(testString1) // should be true
```

이 알고리즘의 시간 복잡도는 **O(n)** 입니다.

여기서 `n`은 문자열의 문자 수 입니다.

이 알고리즘은 `Stack` 데이터 구조를 사용하기 때문에 **O(n)** 공간 복잡도 비용도 발생합니다.

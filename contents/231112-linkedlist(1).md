# Linked Lists(1)

linked list는 선형적인 단방향 순서로 배열된 값의 모음입니다.

linked list는 Swift 배열과 같은 연속형 스토리지 옵션에 비해 몇 가지 이론적 이점이 있습니다.

- 목록 앞에서 지속적으로 삽입 및 제거됩니다.
- 안정적인 성능 특성.

```swift
[12] -> [1] -> [3] ->
// A linked list
```

위에서 간략히 나타낸 것을 보면 알 수 있듯 이 linked list는 `노드` 체인입니다.

노드에는 두 가지 책입이 있습니다.

1. 값을 유지합니다.
2. 다음 노드에 대한 참조를 유지합니다. `nil` 값은 목록의 끝을 나타냅니다.

<img src = "https://github.com/devKobe24/images/blob/main/kodeco-linkedlist1.png?raw=true"></br>

이번 글에서는 linked list을 구현하고 이와 관련된 일반적인 작업에 대해 알아봅니다.

각 작업의 시간 복잡성에 대해 배우고 copy-on-write라는 깔끔하고 작은 Swift 기능을 구현하게 됩니다.

## Node

아래의 코드를 봐봅시다.

```swift
public class Node<Value> {
    
    public var value: Value
    public var next: Node?
    
    public init(value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extenstion Node: CustomStringConvertible {
    
    public var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> " + String(describing: next) + " "
    }
}
```

다음 코드를 이어서 봐봅시다.

```swift
example(of: "creating and linking nodes") {
    let node1 = Node(value: 1)
    let node2 = Node(value: 2)
    let node3 = Node(value: 3)
    
    node1.next = node2
    node2.next = node3
    
    print(node1)
}
```

방금 3개의 노드를 생성하고 연결했습니다.

<img src = "https://github.com/devKobe24/images/blob/main/kodeco-linkedlist2.png?raw=true"></br>

콘솔에는 다음 출력이 표시됩니다.

```swift
---Example of creating and linking nodes---
1 -> 2 -> 3
```

실용성에 관한 한, 목록을 작성하는 현재 방법에는 부족한 점이 많습니다.

이런 방식으로 긴 목록을 작성하는 것이 비현실적이라는 것을 쉽게 알 수 있습니다.

이 문제를 완화하는 일반적인 방법은 `Node` 객체를 관리하는 `LinkedList`를 작성하는 것 입니다.

## LinkedList

아래 코드를 봐봅시다.

```swift
public struct LinkedList<Value> {
    
    public var head: Node<Value>?
    public var tail: Node<Value>?
    
    public init() {}
    
    public var isEmpty: Bool {
        head == nil
    }
}

extension LinkedList: CustomStringConvertible {
    
    public var description: String {
        guard let head = head else {
            return "Empty list"
        }
        return String(describing: head)
    }
}
```

linked list에는 각각 목록의 첫 번째 노드와 마지막 노드를 참조하는 **head**와 **tail**의 개념이 있습니다.

<img src = "https://github.com/devKobe24/images/blob/main/kodeco-linkedlist3.png?raw=true"></br>

## Adding values to the list

앞서 언급했듯이 `Node` 객체를 관리하기 위한 인터페이스를 제공할 것입니다.

먼저 값을 추가하는 작업을 처리합니다.

linked list에 값을 추가하는 방법에는 세 가지가 있으며 각 방법에는 고유한 성능 특성이 있습니다.

1. `push` : 목록 앞에 값을 추가합니다.
2. `append` : 목록 끝에 값을 추가합니다.
3. `insert(after:)` : 특정 목록 노드 뒤에 값을 추가합니다.

### push operations

목록 앞에 값을 추가하는 것을 `push` 작업이라고 합니다.

이는 **head-first insertion**이라고도 합니다.

코드는 매우 간단합니다.

아래 코드를 봐봅시다.

```swift
public mutating func push(_ value: Value) {
    head = Node(value: value, next: head)
    if tail == nil {
        tail = head
    }
}
```

빈 목록에 push하는 경우 새 노드는 목록의 `head`와 `tail`이 됩니다.

아래의 코드를 봐봅시다.

```swift
example(of: "push") {
    var list = LinkedList<Int>()
    list.push(3)
    list.push(2)
    list.push(1)
    
    print(list)
}
```

콘솔 출력에 다음이 표시될 것입니다.

```swift
---Example of push---
1 -> 2 -> 3
```

### append operation

다음으로 살펴볼 작업은 `append` 입니다.

이렇게 하면 목록 끝에 값이 추가되는데, 이를 **tail-end insertion** 이라고 합니다.

아래의 코드를 봐봅시다.

```swift
public mutating func append(_ value: Value) {
    // 1
    guard !isEmpty else {
        push(value)
        return
    }
    
    // 2
    tail!.next =Node(value: value)
    
    // 3
    tail = tail!.next
}
```

이 코드는 비교적 간단합니다.

1. 이전과 마찬가지로 목록이 비어있으면 `head`와 `tail`를 모두 새 노드로 업데이트 해야 합니다. 빈 목록에 `append`하는 것은 기능적으로 `push`와 동일하므로 `push`를 호출하여 작업을 수행합니다.

2. 다른 모든 경우에는 `tail` 노드 뒤에 새 노드를 만듭니다. 강제 언래핑은 위의 `guard` 문과 함께 `isEmpty` 케이스에 `push` 하므로 성공이 보장됩니다.

3. 이것은 끝 부분 삽입이므로 새 노드도 목록의 끝 부분입니다.

아래 코드를 봐봅시다.

```swift
example(of: "append") {
    var list = LinkedList<Int>()
    list.append(1)
    list.append(2)
    list.append(3)
    
    print(list)
}
```

콘솔에 다음과 같은 출력이 표시됩니다.

```swift
---Example of append---
1 -> 2 -> 3
```

### insert(after:) operations

값을 추가하는 세 번째이자 마지막 연산은 `insert(after:)`입니다.

이 연선은 목록의 특정 위치에 값을 삽입하며 두 단계가 필요합니다.

1. 목록에서 특정 노드 찾기.

2. 새 노드 삽입하기.

먼저 값을 삽입할 노드를 찾는 코드를 구현합니다.

아래의 코드를 봐봅시다.

```swift
public func node(at index: Int) -> Node<Value>? {
    // 1
    var currentNode = head
    var currentIndex = 0
    
    // 2
    while currentNode != nil && currentIndex < index {
        currentNode = currentNode!.next
        currentIndex += 1
    }
    
    return currentNode
}
```

`node(at:)`는 주어진 인덱스에 따라 목록에서 노드를 검색하려고 시도합니다.

헤드 노드에서만 목록의 노드에 접근할 수 있으므로 반복적인 탐색을 수행해야 합니다.

실황은 다음과 같습니다.

1. `head`에 대한 새 참조를 생성하고 현재 순회 수를 추적합니다.

2. `while` 루프를 사용하여 원하는 인덱스에 도달할 때까지 참조를 목록 아래로 이동합니다. 빈 목록이나 범위를 벗어난 인덱스는 `nil` 반환 값을 반환합니다.

이제 새 노드를 삽입해야 합니다.

아래 코드를 봐봅시다.

```swift
// 1
@discardableResult
public mutating func insert(_ value: Value,
                            after node: Node<Value>)
                            -> Node<Value> {
    // 2
    guard tail !== node else {
        append(value)
        return tail
    }
    // 3
    node.next = Node(value: value, next: node.next)
    return node.next!
}
```

위 코드가 수행한 작업은 다음과 같습니다.

1. `@discardableResult`를 사용하면 컴파일러가 위아래로 경고를 표시하지 않고 호출자가 이 메서드의 반환값을 무시할 수 있습니다.

2. 이 메서드가 `tail` 노드와 함께 호출되는 경우 기능적으로 동일한 `append` 메서드를 호출하게 됩니다. 그러면 `tail` 업데이트가 처리 됩니다.

3. 그렇지 않으면 새 노드를 목록의 나머지 부분과 연결하고 새 노드를 반환하기만 하면 됩니다.

테스트를 위하여 아래 코드를 봐봅시다.

```swift
example(of: "inserting at a particular index") {
    var list = LinkedList<Int>()
    list.push(3)
    list.push(2)
    list.push(1)
    
    print("Before inserting: \(list)")
    var middleNode = list.node(at: 1)!
    for _ in 1...4 {
        middleNode = list.insert(-1, after: middleNode)
    }
    print("After inserting: \(list)")
}
```

다음 출력이 표시됩니다.

```swift
---Example of inserting at a particular index---
Before inserting: 1 -> 2 -> 3
After inserting: 1 -> 2 -> -1 -> -1 -> -1 -> -1 -> 3
```

### Performance analysis

지금까지 좋은 진전을 이루었습니다.

요약하면 linked list에 값을 추가하는 세 가지 작업과 특정 인덱스에서 노드를 찾는 방법을 구현했습니다.

<img src = "https://github.com/devKobe24/images/blob/main/kodeco-linkedlist4.png?raw=true"></br>

다음으로 반대 작업인 제거 작업에 중점을 둡니다.

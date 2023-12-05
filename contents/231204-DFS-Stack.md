# 깊이 우선 탐색(DFS, Depth-First Search) - Stack편 🧩

## ✅🤔깊이 우선 탐색(DFS, Depth-First Search)란?

DFS, 즉 깊이 우선 탐색(Deep First Search) 알고리즘은 그래프 또는 트리 구조에서 모든 노드를 방문하기 위한 알고리즘 중 하나입니다. 

이 방법은 백트래킹 기법을 사용하여 깊이 우선으로 탐색을 진행합니다.

### ✅🧩 깊이 우선 탐색(DFS, Depth-First Search) 주요 특징과 작동 방식

1. **시작 노드 선택**: 탐색을 시작할 노드를 선택합니다.

2. **인접 노드 탐색**: 현재 노드에서 갈 수 있는 인접 노드 중 아직 방문하지 않은 노드를 선택합니다.

3. **깊이 우선 탐색**: 선택된 인접 노드를 새로운 현재 노드로 하여, 다시 인접 노드를 탐색합니다. 이 과정을 반복하면서 깊이 우선으로 탐색을 진행합니다.

4. **백트래킹**: 더 이상 탐색할 인접 노드가 없으면, 이전 노드로 돌아가(백트래킹) 다른 경로를 탐색합니다.

5. **모든 노드 방문**: 모든 노드를 방문할 때까지 이 과정을 반복합니다.

DFS는 특히 복잡한 구조에서 특정 경로나 사이클을 찾을 때 유용하며, 미로 탐색, 퍼즐 게임, 네트워크 분석 등 다양한 분야에서 활용됩니다.

DFS는 간단하면서도 강력한 탐색 알고리즘이지만, 탐색 과정에서 메모리를 많이 사용할 수 있으며, 가장 깊은 노드를 찾는 데 시간이 오래 걸릴 수 있습니다.

## ✅1️⃣ 깊이 우선 탐색(DFS, Depth-First Search) 알고리즘에 사용되는 자료구조.

### 🧩 1. 스택(Stack)
DFS의 핵심 자료구조로, 노드를 방문할 때마다 해당 노드를 스택에 넣고,
더 이상 탐색할 자식 노드가 없을 때 스택에서 노드를 하나씩 꺼내며 탐색을 진행합니다.
스택은 LIFO(Last In, First Out) 방식으로 동작하기 때문에 DFS에서 최근에 방문한 노드부터 탐색을 계속하는 데 적합합니다.

## ✅2️⃣ DFS에서의 Stack

### 🤔 DFS 알고리즘에서 Stack 자료구조가 핵심 자료구조인 이유는 무엇일까요?

DFS(Depth-First Search) 알고리즘에서 스택(Stack)이 핵심 자료구조로 사용되는 이유는 그 구조와 탐색 방식 때문입니다.

DFS의 목적은 가능한 깊게 한 경로를 탐색한 뒤, 더 이상 갈 돗이 없으면 이전 분기점으로 돌아가 다른 경로를 탐색하는 것입니다.
이러한 탐색 방식이 스택 LIFO(Last In, First Out) 특성과 잘 맞습니다.

### 🤔 Stack의 LIFO 특성과 DFS는 왜 잘 맞을까?

**1. 깊이 우선 탐색.**

DFS는 현재 노드에서 인접한 노드로 계속해서 깊이 들어가면서 탐색합니다.
스택에 노드를 계속 쌓아가면서 깊이 들어갈 수 있습니다.

**2. 백트래킹(Backtracking) 지원.**

더 이상 탐색할 노드가 없거나, 특정 조건에 도달했을 때, 스택을 사용하면 마지막 방문한 노드로 쉽게 되돌아 갈 수 있습니다.
이는 스택에서 가장 최근에 추가된 요소가 가장 먼저 제거되는 LIFO의 원리 때문입니다.

**3. 경로의 기록 및 복원.**

스택에 경로를 저장함으로써 현재까지의 탐색 경로를 기록하고, 필요에 따라 이전 상태로 쉽게 되돌릴 수 있습니다.

**4. 재귀 호출의 대안.**

실제로, 재귀 함수를 사용한 DFS 구현은 시스템 스택을 사용합니다.
그러나 재귀 호출에는 스택 오버플로우의 위험이 있기 때문에, 명시적인 스택을 사용한 반복적 구현이 이러한 위험을 줄일 수 있습니다.

**5. 요약.**

스택은 DFS의 깊이 우선 탐색과 백트래킹을 효과적으로 지원하는 자료구조입니다.
이를 통해 탐색 중인 경로를 기록하고, 필요한 경우 이전 상태로 쉽게 되돌아갈 수 있어 알고리즘의 효율성과 간결성을 높입니다.

## ✅3️⃣ Stack 해부하기 1.

```swift
struct Stack<Element> {
    private var elements = [Element]()
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var count: Int {
        return elements.count
    }
}
```

스택은 기본적으로 배열을 기반으로하는 자료구조이며,
주로 **`push`, `pop`, `peek`, `isEmpty`** 와 같은 기본적인 연산을 제공합니다.

위 스택 구현은 제네릭을 사용하여 다양한 유형의 요소를 저장할 수 있도록 설계되었습니다.
- **`push`** 메서드는 스택의 맨 위에 새 요소를 추가합니다.
- **`pop`** 메서드는 스택의 맨 위의 요소를 제거하고 반환합니다.
- **`peek`** 메서드는 스택의 맨 위 요소를 확인할 수 있지만 제거하지는 않습니다.
- **`isEmpty`** 는 스택이 비어있는지 확인하는 데 사용됩니다.
- **`count`** 속성은 스택에 저장된 요소의 수를 확인할 수 있습니다.

## ✅4️⃣ Stack 해부하기 2.

스택은 기본적으로 배열을 기반으로하는 자료구조라고 설명했습니다.

Swift 표준 라이브러리에서 배열이 어떻게 구성되어 있는지 확인해봅시다.

[Swift 표준 라이브러리 - 배열 코드](https://github.com/apple/swift/blob/main/stdlib/public/core/Array.swift)입니다.

직접 들어가서 보시면 정말 방대합니다 헤헿

### ✅ 4.1 `append(_:)` 메서드.

```swift
@inlinable
@_semantics("array.append_element")
@_effects(notEscaping self.value**)
public mutating func append(_ newElement: __owned Element) {
    // Separating uniqueness check and capacity check allows hoisting the
    // uniqueness check out of a loop.
    _makeUniqueAndReserveCapacityIfNotUnique()
    let oldCount = _buffer.mutableCount
    _reserveCapacityAssumingUniqueBuffer(oldCount: oldCount)
    _appendElementAssumeUniqueAndCapacity(oldCount, newElement: newElement)
    _endMutation()
}
```

**`append(_:)`** 메서드는 배열의 끝에 새로운 요소를 추가합니다. 이는 스택에서 **`push`** 연산과 동일합니다.

**이 메서드는 다음과 같은 작업을 수행합니다**

**1. `_makeUniqueAndReserveCapacityIfNotUnique()`**
배열의 내용이 고유하게 보장되고 필요한 경우 용량을 확보합니다.

**2. `let oldCount = _buffer.mutableCount`**
현재 배열의 요소 수를 저장합니다.

**3. `_reserveCapacityAssumingUniqueBuffer(oldCount: oldCount)`**
새 요소를 추가할 수 있을 만큼의 용량을 확보합니다.

**4. `_appendElementAssumeUniqueAndCapacity(oldCount, newElement: newElement)`**
실제로 새 요소를 배열의 끝에 추가합니다.

**5. `_endMutation()`**
변경 사항을 완료합니다.

**이 메서드는 Swift의 표준 라이브러리에서 `Array` 타입에 새로운 요소를 추가하는 핵심적인 기능을 제공합니다.**

### ✅ 4.2 `removeLast()` 메서드.

```swift
// MARK: - Array.swift
@inlinable
@_semantics("array.mutate_unknown")
@_effects(notEscaping self.value**)
@_effects(escaping self.value**.class*.value** -> return.value**)
public mutating func _customRemoveLast() -> Element? {
    _makeMutableAndUnique()
    let newCount = _buffer.mutableCount - 1
    _precondition(newCount >= 0, "Can't removeLast from an empty Array")
    let pointer = (_buffer.mutableFirstElementAddress + newCount)
    let element = pointer.move()
    _buffer.mutableCount = newCount
    _endMutation()
    return element
}

// MARK: - RangeReplaceableCollection.swift
@inlinable
@discardableResult
public mutating func removeLast() -> Element {
    _precondition(!isEmpty, "Can't remove last element from an empty collection")
    
    if let result = _customRemoveLast() { return result }
    return remove(at: index(before: endIndex))
}
```
**`removeLast()`** 메서드는 배열의 마지막 요소를 제거하고 반환합니다. 이는 스택에서 **`pop`** 연산과 동일합니다.

1️⃣ **`Array.swift`** 에 명시된 **`_customRemoveLast()`** 메서드는 다음과 같은 작업을 수행합니다.

**1. `_makeMutableAndUnique()`**
배열을 수정 가능하고 고유한 상태로 만듭니다.

**2. `let newCount = _buffer.mutableCount - 1`**
새 요소 수를 계산합니다.

**3. `_precondition(newCount >= 0, "Can't removeLast from an empty Array")`**
배열이 비어있지 않은지 확인합니다.

**4. `let pointer = (_buffer.mutableFirstElementAddress + newCount)`**
제거될 마지막 요소의 주소를 계산합니다.

**5. `let element = pointer.move()`**
해당 요소를 제거하고 그 값을 반환합니다.

**6. `_buffer.mutableCount = newCount`**
배열의 새 크기를 업데이트합니다.

**7. `_endMutation()`**
변경 사항을 완료합니다.

2️⃣ **`RangeReplaceableCollection.swift`** 에 명시된 **`removeLast()`** 메서드는 다음과 같은 작업을 수행합니다.

**1. `_precondition(!isEmpty, "Can't remove last element from an empty collection")`**
배열이 비어 있지 않은지 확인합니다. 배열이 비어 있으면 런타임 오류가 발생합니다.

**2. `if let result = _customRemoveLast() { return result }`**
_customRemoveLast() 메서드를 호출하여 마지막 요소를 제거하고 결과를 반환합니다. 
이 메서드는 내부 최적화나 특별한 경우를 처리할 수 있습니다.

**3. `return remove(at: index(before: endIndex))`**
만약 _customRemoveLast()가 사용할 수 없는 경우, remove(at:) 메서드를 사용하여 마지막 요소를 제거합니다. 
여기서 index(before: endIndex)는 배열의 마지막 요소의 인덱스를 계산합니다.

### ✅ 4.3 `isEmpty` 속성.

```swift
// MARK: - Collection.swift
public protocol Collection<Element>: Sequence {  
  /// A Boolean value indicating whether the collection is empty.
  ///
  /// When you need to check whether your collection is empty, use the
  /// `isEmpty` property instead of checking that the `count` property is
  /// equal to zero. For collections that don't conform to
  /// `RandomAccessCollection`, accessing the `count` property iterates
  /// through the elements of the collection.
  ///
  ///     let horseName = "Silver"
  ///     if horseName.isEmpty {
  ///         print("My horse has no name.")
  ///     } else {
  ///         print("Hi ho, \(horseName)!")
  ///     }
  ///     // Prints "Hi ho, Silver!"
  ///
  /// - Complexity: O(1)
  var isEmpty: Bool { get }
}
```

위의 **`isEmpty`** 속성은 Swift의 **`Collection`** 프로토콜의 일부로 정의된 것입니다.
이 **`isEmpty`** 속성은 **`Collection`** 을 구현하는 모든 타입.
즉, **`Array, Set, Dictionary, String`** 등의 컬렉션 타입에 대해 사용할 수 있습니다.

**`Collection`** 프로토콜의 **`isEmpty`** 속성은 컬렉션에 비어 있는지 여부를 나타내며, 이는 **`count == 0`** 과 동등한 조건을 확인합니다.
이는 **`Array`** 에서 사용하는 **`isEmpty`** 와 동일한 개념입니다.

**`Array`** 는 **`Collection`** 프로토콜을 준수하기 때문에, **`Collection`** 에 정의된 **`isEmpty`** 속성을 상속받아 사용합니다.

---

# Depth-First Search (DFS) - Stack Edition 🧩

## ✅🤔 What is Depth-First Search (DFS)?

DFS, or Depth-First Search, is one of the algorithms used to visit all nodes in a graph or tree structure.

This method employs backtracking to search in a depth-first manner.

### ✅🧩 Key Features and Working Mechanism of Depth-First Search (DFS)

1. **Selecting a Starting Node**: Choose the node where you want to start the search.

2. **Exploring Adjacent Nodes**: From the current node, select any adjacent node that has not yet been visited.

3. **Depth-First Exploration**: Use the chosen adjacent node as the new current node and repeat the process. Continue this depth-first exploration.

4. **Backtracking**: If there are no more adjacent nodes to explore, return to the previous node (backtrack) and search different paths.

5. **Visiting All Nodes**: Repeat this process until all nodes have been visited.

DFS is particularly useful for finding specific paths or cycles in complex structures and is used in various fields such as maze exploration, puzzle games, and network analysis.

While DFS is a simple yet powerful search algorithm, it can use a significant amount of memory during the search process and can take a long time to find the deepest node.

## ✅1️⃣ Data Structures Used in Depth-First Search (DFS).

### 🧩 1. Stack
The core data structure of DFS, where nodes are pushed onto the stack as they are visited,
and nodes are popped off the stack for further exploration when there are no more children nodes to explore.
The stack operates on a LIFO (Last In, First Out) principle, making it suitable for continuing the search from the most recently visited node in DFS.

## ✅2️⃣ Stack in DFS

### 🤔 Why is the Stack data structure central in DFS?

The reason why the Stack is a core data structure in DFS (Depth-First Search) lies in its structure and method of exploration.

The purpose of DFS is to explore as deeply as possible along one path and then, when no further paths are available, to backtrack to a previous junction to explore different paths.
This method of exploration aligns well with the LIFO (Last In, First Out) characteristic of a stack.

### 🤔 Why do the LIFO feature of Stack and DFS match well?

**1. Depth-First Exploration.**

DFS continuously goes deeper into adjacent nodes from the current node.
Stacks allow this depth to be achieved by stacking nodes continually.

**2. Support for Backtracking.**

When no further nodes to explore are available, or a specific condition is met, stacks facilitate easy backtracking to the last visited node.
This is due to the principle of LIFO, where the most recently added element is removed first.

**3. Recording and Restoring Paths.**

Storing paths in the stack helps in recording the current exploration path and, if needed, easily reverting to a previous state.

**4. An Alternative to Recursive Calls.**

In practice, recursive implementations of DFS use the system stack.
However, recursive calls can risk stack overflow, so iterative implementations using an explicit stack can mitigate this risk.

**5. Summary.**

Stacks effectively support the depth-first exploration and backtracking of DFS.
They allow recording the path being explored and easily reverting to a previous state, enhancing the algorithm's efficiency and simplicity.

## ✅3️⃣ Dissecting Stack 1.

```swift
struct Stack<Element> {
    private var elements = [Element]()
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var count: Int {
        return elements.count
    }
}
```

A stack is essentially a data structure based on an array,
mainly providing basic operations like **`push`, `pop`, `peek`, `isEmpty`**.

The above stack implementation uses generics to store elements of various types.
- **`push`** method adds a new element to the top of the stack.
- **`pop`** method removes and returns the element from the top of the stack.
- **`peek`** method allows viewing the top element without removing it.
- **`isEmpty`** checks whether the stack is empty.
- **`count`** property lets you see the number of elements stored in the stack.

## ✅4️⃣ Dissecting Stack 2.

Stacks are fundamentally array-based data structures, as explained.

Let’s take a look at how arrays are structured in the Swift standard library.

Here’s the [Swift standard library - Array code](https://github.com/apple/swift/blob/main

/stdlib/public/core/Array.swift).

It's really vast if you go in and see, hehe.

### ✅ 4.1 `append(_:)` Method.

```swift
@inlinable
@_semantics("array.append_element")
@_effects(notEscaping self.value**)
public mutating func append(_ newElement: __owned Element) {
    // Separating uniqueness check and capacity check allows hoisting the
    // uniqueness check out of a loop.
    _makeUniqueAndReserveCapacityIfNotUnique()
    let oldCount = _buffer.mutableCount
    _reserveCapacityAssumingUniqueBuffer(oldCount: oldCount)
    _appendElementAssumeUniqueAndCapacity(oldCount, newElement: newElement)
    _endMutation()
}
```

**`append(_:)`** method adds a new element to the end of an array. It's equivalent to **`push`** operation in a stack.

**This method performs the following operations**

**1. `_makeUniqueAndReserveCapacityIfNotUnique()`**
Ensures that the array content is unique and reserves capacity if needed.

**2. `let oldCount = _buffer.mutableCount`**
Stores the current number of elements in the array.

**3. `_reserveCapacityAssumingUniqueBuffer(oldCount: oldCount)`**
Reserves enough capacity for the new element.

**4. `_appendElementAssumeUniqueAndCapacity(oldCount, newElement: newElement)`**
Actually adds the new element to the end of the array.

**5. `_endMutation()`**
Completes the modifications.

**This method provides a core function in Swift’s standard library for adding new elements to an `Array` type.**

### ✅ 4.2 `removeLast()` Method.

```swift
// MARK: - Array.swift
@inlinable
@_semantics("array.mutate_unknown")
@_effects(notEscaping self.value**)
@_effects(escaping self.value**.class*.value** -> return.value**)
public mutating func _customRemoveLast() -> Element? {
    _makeMutableAndUnique()
    let newCount = _buffer.mutableCount - 1
    _precondition(newCount >= 0, "Can't removeLast from an empty Array")
    let pointer = (_buffer.mutableFirstElementAddress + newCount)
    let element = pointer.move()
    _buffer.mutableCount = newCount
    _endMutation()
    return element
}

// MARK: - RangeReplaceableCollection.swift
@inlinable
@discardableResult
public mutating func removeLast() -> Element {
    _precondition(!isEmpty, "Can't remove last element from an empty collection")
    
    if let result = _customRemoveLast() { return result }
    return remove(at: index(before: endIndex))
}
```
**`removeLast()`** method removes and returns the last element of an array. It's equivalent to **`pop`** operation in a stack.

1️⃣ In **`Array.swift`**, the **`_customRemoveLast()`** method performs the following operations.

**1. `_makeMutableAndUnique()`**
Makes the array mutable and unique.

**2. `let newCount = _buffer.mutableCount - 1`**
Calculates the new number of elements.

**3. `_precondition(newCount >= 0, "Can't removeLast from an empty Array")`**
Ensures that the array is not empty.

**4. `let pointer = (_buffer.mutableFirstElementAddress + newCount)`**
Calculates the address of the last element to be removed.

**5. `let element = pointer.move()`**
Removes and returns the value of that element.

**6. `_buffer.mutableCount = newCount`**
Updates the new size of the array.

**7. `_endMutation()`**
Completes the modifications.

2️⃣ In **`RangeReplaceableCollection.swift`**, the **`removeLast()`** method performs the following operations.

**1. `_precondition(!isEmpty, "Can't remove last element from an empty collection")`**
Ensures the array is not empty. An empty array will cause a runtime error.

**2. `if let result = _customRemoveLast() { return result }`**
Calls the _customRemoveLast() method to remove the last element and return the result. 
This method can handle internal optimizations or special cases.

**3. `return remove(at: index(before: endIndex))`**
If _customRemoveLast() is not available, uses the remove(at:) method to remove the last element. 
Here, index(before: endIndex) calculates the index of the last element of the array.

### ✅ 4.3 `isEmpty` Property

```swift
// MARK: - Collection.swift
public protocol Collection<Element>: Sequence {  
  /// A Boolean value indicating whether the collection is empty.
  ///
  /// When you need to check whether your collection is empty, use the
  /// `isEmpty` property instead of checking that the `count` property is
  /// equal to zero. For collections that don't conform to
  /// `RandomAccessCollection`, accessing the `count` property iterates
  /// through the elements of the collection.
  ///
  ///     let horseName = "Silver"
  ///     if horseName.isEmpty {
  ///         print("My horse has no name.")
  ///     } else {
  ///         print("Hi ho, \(horseName)!")
  ///     }
  ///     // Prints "Hi ho, Silver!"
  ///
  /// - Complexity: O(1)
  var isEmpty: Bool { get }
}
```

The above `isEmpty` property is defined as part of Swift's `Collection` protocol. This `isEmpty` property is applicable to all types that implement the `Collection` protocol, such as `Array, Set, Dictionary, String`, and other collection types.

The `isEmpty` property of the `Collection` protocol indicates whether the collection is empty, checking a condition equivalent to `count == 0`. This is the same concept as the `isEmpty` used in `Array`.

Since `Array` conforms to the `Collection` protocol, it inherits and utilizes the `isEmpty` property defined in `Collection`.

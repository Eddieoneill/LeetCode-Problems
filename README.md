# LeetCode-Problems

## 876. Middle of the Linked List
```swift
class Solution {
    func middleNode(_ head: ListNode?) -> ListNode? {
        if head?.next == nil {
            return head    
        }
        
        var fastNode = head
        var slowNode = head
        
        while fastNode?.next != nil {
            fastNode = fastNode?.next?.next
            slowNode = slowNode?.next
        }
        return slowNode
    }
}
```

## 206. Reverse Linked List

```swift

//  iterative solution
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        var current = head
        var next: ListNode? = nil
        var previous: ListNode? = nil
        
        while current != nil {
            next = current?.next
            current?.next = previous
            previous = current
            current = next
        }
        return previous
    }
}

// recursive solution
class Solution {
    func reverse(_ node: Node<Int>) -> Node<Int>? {
        guard node.next != nil else { return node }

        let head = reverse(node.next ?? Node(0))

        node.next?.next = node
        node.next = nil

        print(head ?? Node(0))
        return head
    }
}
```

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
## 21. Merge Two Sorted Lists

```swift
class Solution {
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var l1 = l1
        var l2 = l2
        let dummyNode = ListNode(0)
        var current = dummyNode
        dummyNode.next = current.next
        
        if l1 == nil && l2 == nil { return nil }
        
        while l1 != nil && l2 != nil {
            if l1?.val ?? 0 < l2?.val ?? 0{
                current.next = l1
                l1 = l1?.next
            } else {
                current.next = l2
                l2 = l2?.next
                
            }
            current = current.next!
        }
        
        if l1 == nil {
            current.next = l2
        } else {
            current.next = l1
        }
        return dummyNode.next
    }
}

```
## 	217	Contains Duplicate    

```swift
    func containsDuplicate(_ nums: [Int]) -> Bool {
        guard nums.count != nil else { return false }
        guard nums.count > 1 else { return false }
        guard nums.count > 2 else { return nums[0] == nums[1] }
        
        var uniqueNum = Set(nums)
        
        if uniqueNum.count == nums.count {
            return false
        } else {
            return true
        }
    }
```

## 832	Flipping an Image

```swift
    func flipAndInvertImage(_ A: [[Int]]) -> [[Int]] {
        var flippedImage = [[Int]]()
        var reverseCounter = A.count - 1
        
        for (index, a) in A.enumerated() {
            flippedImage.append([])
            for _ in 0..<a.count {
                flippedImage[index].append(a[reverseCounter])
                reverseCounter -= 1
            }
            reverseCounter = A.count - 1
        }
        
        for (index, arr) in flippedImage.enumerated() {
            for (index2, num) in arr.enumerated() {
                if num == 1 {
                    flippedImage[index][index2] = 0
                } else if num == 0 {
                    flippedImage[index][index2] = 1
                }
            }
        }
        return flippedImage
    }
```
## 268. Missing Number

```swift
    func missingNumber(_ nums: [Int]) -> Int {
        guard nums.count > 0 else { return 0 }
        guard nums.count > 1 else { return returnNumberPlusOne(nums[0]) }
        
        var sum = 0
        for i in 1...nums.count {
            sum += i
        }
        
        for num in nums {
            sum -= num
        }
        
        return sum
    }
    
    func returnNumberPlusOne(_ num: Int) -> Int {
        if num == 0 {
            return 1
        } else {
            return 0
        }
    }
```

## 1266. Minimum Time Visiting All Points

```swift
func minTimeToVisitAllPoints(_ points: [[Int]]) -> Int {
        var counter = 0
        var currentLocation = points[0]
        var vP = 0

        while currentLocation != points[points.count - 1] {

            if currentLocation == points[vP + 1] {vP += 1 }

            if currentLocation[0] != points[vP + 1][0] && currentLocation[1] != points[vP + 1][1] {
                if currentLocation[0] < points[vP + 1][0] {
                    currentLocation[0] += 1
                } else {
                    currentLocation[0] -= 1
                }

                if currentLocation[1] < points[vP + 1][1] {
                    currentLocation[1] += 1
                } else {
                    currentLocation[1] -= 1
                }

                counter += 1

            } else if currentLocation[1] == points[vP + 1][1] && currentLocation[0] != points[vP + 1][0] {
                if currentLocation[0] < points[vP + 1][0] {
                    currentLocation[0] += 1
                    counter += 1
                } else {
                    currentLocation[0] -= 1
                    counter += 1
                }
            } else if currentLocation[0] == points[vP + 1][0] && currentLocation[1] != points[vP + 1][1] {
                if currentLocation[1] < points[vP + 1][1] {
                    currentLocation[1] += 1
                    counter += 1
                } else {
                    currentLocation[1] -= 1
                    counter += 1
                }
            }
        }
        return counter
    }
```
better solution

```swift
    func minTimeToVisitAllPoints(_ points: [[Int]]) -> Int {
        var time = 0
        for count in 0..<points.count - 1 {
            let num = max(abs(points[count][0] - points[count + 1][0]), abs(points[count][1] - points[count + 1][1]))
            time += num
        }
        return time
    }
```

## 1252. Cells with Odd Values in a Matrix

```swift
func oddCells(_ n: Int, _ m: Int, _ indices: [[Int]]) -> Int {
        var matrix = createMatrix(n, m)
        var result = 0
        
        for num in 0..<indices.count {
            matrix = incrementArr(matrix, indices[num])
        }

        for arr in matrix {
            for num in arr where num % 2 == 1 {
                result += 1
            }
        }
        return result
    }

    func createMatrix(_ column: Int, _ row: Int) -> [[Int]] {
        var result =  [[Int]]()
        var numOfRow = [Int]()

        for _ in 0..<row {
            numOfRow.append(0)
        }

        for _ in 0..<column {
            result.append(numOfRow)
        }

        return result
    }

    func incrementArr(_ mtx: [[Int]], _ arr: [Int]) -> [[Int]] {
        var result = mtx

        for num1 in 0..<mtx[0].count {
            result[arr[0]][num1] += 1
            for num2 in 0..<mtx.count where num1 == arr[1] {
                result[num2][num1] += 1
            }
        }

        return result
    }
```
## 1295. Find Numbers with Even Number of Digits

```swift
    func findNumbers(_ nums: [Int]) -> Int {
        var result: Int = 0

        for num in nums where String(num).count % 2 == 0 {
            result += 1
        }

        return result
    }
```

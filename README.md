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
## 1108. Defanging an IP Address

```swift
    func defangIPaddr(_ address: String) -> String {
        var result = ""
        
        for char in address {
            if char == "." {
                result += "[.]"
            } else {
                result += String(char)
            }
        }
        return result
    }
 ```
## 771. Jewels and Stones

```swift
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        var checker: Set<Character> = []
        var result = 0
        J.forEach { checker.insert($0) }
        
        for stone in S where checker.contains(stone) {
            result += 1
        }
        
        return result
    }
```
## 1281. Subtract the Product and Sum of Digits of an Integer

```swift
    func subtractProductAndSum(_ n: Int) -> Int {
        var product = 1
        var sum = 0
        
        for num in String(n) {
            product *= Int(String(num))!
            sum += Int(String(num))!
        }
        
        return product - sum
    }
```
## 1290. Convert Binary Number in a Linked List to Integer

```swift
    func getDecimalValue(_ head: ListNode?) -> Int {
        var current = head
        var strNum = ""
        var result = 0
        var count = -1
        
        while current != nil {
            count += 1
            strNum += "\(current!.val)"
            current = current?.next
        }
        
        for num in strNum {
            var number = 2
            if count == 2 {
                number = 2 * 2
            } else if count <= 0 {
                number = 1
            } else {
                for _ in 1..<count {
                    number *= 2
                }
            }
            result += (number * Int(String(num))!)
            count -= 1
        }
        return result
    }
```
## 1221. Split a String in Balanced Strings

```swift
    func balancedStringSplit(_ s: String) -> Int {
        var r = 0
        var l = 0
        var result = 0
        
        for char in s {               
            if char == "R" {
                r += 1
            } else {
                l += 1
            }       
            if r == l {
                r = 0
                l = 0
                result += 1
            }  
        }
        return result
    }
```
## 709. To Lower Case

```swift
    func toLowerCase(_ str: String) -> String {
        return str.lowercased
    }
```
## 1021. Remove Outermost Parentheses

```swift
    func removeOuterParentheses(_ S: String) -> String {
        var openCount = 0
        var result = ""
        
        for char in S {
            if char == ")"{
                openCount -= 1   
            }
            if openCount >= 1 {
                result += String(char)
            }
            if char == "(" {
                openCount += 1   
            } 
        }
        return result
    }
```

## 804. Unique Morse Code Words

```swift
func uniqueMorseRepresentations(_ words: [String]) -> Int {
        let alphToMorse: [Character: String] = ["a": ".-", "b":"-...", "c":"-.-.", "d":"-..", "e":".", "f":"..-.", "g":"--.", "h":"....", "i":"..", "j":".---", "k":"-.-", "l":".-..", "m":"--", "n":"-.", "o":"---", "p":".--.", "q":"--.-", "r":".-.", "s":"...", "t":"-", "u":"..-", "v":"...-", "w":".--", "x":"-..-", "y":"-.--", "z":"--.."]
        var currentWord = ""
        var result: Set<String> = []
        
        for word in words {
            for char in word {
                currentWord += alphToMorse[char] ?? "a"
            }
            result.insert(currentWord)
            currentWord = ""
        }
        return result.count
    }
```
## 136. Single Number

```swift
    func singleNumber(_ nums: [Int]) -> Int {
        var result = nums[0]
        
        for num in 1..<nums.count {
            result ^= nums[num]
        }
        return result
    }
```
## 1. Two Sum

```swift
func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var dict: [Int: Int] = [:]
        
        for (index, num) in nums.enumerated() {
            let complete = target - num
            
            if dict[complete] != nil {
                return [dict[complete]!, index]
            } else {
                dict[num] = index
            }
        }
        return []
    }
```
## 1046. Last Stone Weight

```swift
    func lastStoneWeight(_ stones: [Int]) -> Int {
        var arr = stones
        while arr.count > 1 {
            arr = arr.sorted()
            let x = arr.popLast()!
            let y = arr.popLast()!
            arr.append(x - y)
        }
        return arr[0]
    }
```

## 292. Nim Game

```swift
    func canWinNim(_ n: Int) -> Bool {
        return n % 4 != 0
    }
```

## 167. Two Sum II - Input array is sorted

```swift
    func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
        var left = 0
        var right = numbers.count - 1
        
        while left < right {
            let sum = numbers[left] + numbers[right]
            if sum == target {
                return [left+1, right+1]
            } 
            
            if sum < target {
                left += 1
            } else {
                right -= 1
            }
        }
        
        return []
    
    }
```
## 153. Find Minimum in Rotated Sorted Array

```swift
    func findMin(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        
        var low = 0, high = nums.count-1
        while low < high {
            let mid = low + (high - low)/2
            if nums[low] < nums[high] {
                return nums[low]
            } else if nums[low] <= nums[mid] {
                low = mid + 1
            } else {
                high = mid
            }
        }
        
        return nums[low]
    }
```
## 1313. Decompress Run-Length Encoded List

```swift
    func decompressRLElist(_ nums: [Int]) -> [Int] {
        var result = [Int]()
        
        for index in stride(from: 0, to: nums.count, by: 2) {
            let count = nums[index]
            let number = nums[index+1]
            
            guard count > 0 else { continue }
            
            result += Array(repeating: number, count: count)
        }
        
        return result
    }
```


```swift
    func rangeSumBST(_ root: TreeNode?, _ L: Int, _ R: Int) -> Int {
        guard let current = root else { return 0 }
        var sum = 0
    
        if current.val >= L && current.val <= R {
             sum = current.val
        }
        if current.val > L {
         sum += rangeSumBST(current.left, L, R)
        }
    
        if current.val < R {
            sum += rangeSumBST(current.right, L, R)
        }
        
        return sum
    }
```
## 20. Valid Parentheses

```swift
    func isValid(_ s: String) -> Bool {
        guard s.count != 0 else { return true }
        guard s.count > 1 else { return false }
        
        var dict:[Character: Character] = ["(": ")", "{": "}", "[": "]"]
        var result: [Character] = []
        
        for char in s {
            if let next = dict[char] {
                result.append(next)
            } else if result.popLast() == char {
                continue
            } else { return false }
        }
        
        return result.isEmpty
    }
```
## 53. Maximum Subarray

```swift
    func maxSubArray(_ n: [Int]) -> Int {
        var best = n[0];
        var current = n[0];
        
        for i in 1..<n.count {
            current = max(current + n[i], n[i]);
            best = max(current, best);
        }
    
        return best;
    }
```
## 202. Happy Number

```swift
    func isHappy(_ n: Int) -> Bool {
        guard n > 1 else { return true }
        var seen: Set<Int> = []
        var digitList = [Int]()
        let strNum = String(n)
        seen.insert(n)
        
        
        for num in strNum {
            digitList.append(Int(String(num))!)
        }
        
        return square(digitList, seen)
    }
    
    func square(_ list: [Int], _ seen: Set<Int>) -> Bool {
        var sum = 0
        
        for num in list {
            let power = num * num
            sum += power
        }
        
        if seen.contains(sum) {
            return false
        } else {
            return happy(sum, seen)
        }
    }
    
    func happy(_ n: Int, _ seen: Set<Int>) -> Bool {
        guard n > 1 else { return true }
        var list = seen
        var digitList = [Int]()
        let strNum = String(n)
        list.insert(n)
        
        for num in strNum {
            digitList.append(Int(String(num))!)
        }
        
        return square(digitList, list)
    }
```

## 202. Happy Number - better solution

```swift
    func isHappy(_ n: Int) -> Bool {
        var happyNumber = n
        var seenSet = Set<Int>()
        while happyNumber != 1 && !seenSet.contains(happyNumber) {
            seenSet.insert(happyNumber)
            happyNumber = getNextNumber(&happyNumber)
        }
        return happyNumber == 1
    }

    func getNextNumber(_ happyNumber: inout Int) -> Int {
        var sum = 0
        while happyNumber > 0 {
            var remainder = happyNumber % 10
            happyNumber /= 10
            sum += remainder * remainder
        }
        return sum
    }
```
## 665. Non-decreasing Array

```swift
    func checkPossibility(_ nums: [Int]) -> Bool {
        var count = 0
        var prev = nums[0]
        for index in 1..<nums.count {
            if prev > nums[index] {
                count += 1
                if count > 1 {return false}
                if index != 1 && nums[index - 2] > nums[index] {
                    continue
                }
            }
            prev = nums[index]
        }
        return true
    }
```
## 7. Reverse Integer

```swift
    func reverse(_ x: Int) -> Int {
        var x = String(x)
        var negative = false
        var result = ""
        
        for num in x {
            if num == "-" {
                negative = true
            } else {
                result = String(num) + result
            }
        }
        
        if Int(result)! > Int32.max || Int(result)! < Int32.min {
            return 0
        } else if negative {
            return -Int(result)!
        } else {
          return Int(result)!
        }
    }
```
## 1207. Unique Number of Occurrences

```swift
    func uniqueOccurrences(_ arr: [Int]) -> Bool {
        var dict = [Int: Int]()
        
        for num in arr {
            dict[num, default: 0] += 1
        }
        
        return Set(Array(dict.values)).count == dict.keys.count
    }
```
## 859. Buddy Strings

```swift
    func buddyStrings(_ A: String, _ B: String) -> Bool {
        guard A.count == B.count else { return false }
        if A == B {
            return Set(A).count < A.count
        } else {
            var diffs = [Int]()
            let A = Array(A), B = Array(B)
            for i in 0..<A.count {
                if A[i] != B[i] {
                    diffs.append(i)
                }
            }
            guard diffs.count == 2 else { return false }
            let i = diffs[0], j = diffs[1]
            return A[i] == B[j] && A[j] == B[i]
        }
    }    
```
## 506. Relative Ranks

```swift
    func findRelativeRanks(_ nums: [Int]) -> [String] {
        var sortedNums = nums.sorted { $0 > $1 }
        var dict: [Int: String] = [:]
        var count = 4
        var result = [String]()
        
        for (index, num) in sortedNums.enumerated() {
            switch index {
            case 0:
                dict[num] = "Gold Medal"
            case 1: 
                dict[num] = "Silver Medal"
            case 2: 
                dict[num] = "Bronze Medal"
            default:
                dict[num] = "\(count)"
                count += 1
            }
        }
        
        for num in nums {
            result.append(dict[num]!)
        }
        return result
    }
```
## 1137. N-th Tribonacci Number

```swift
    func tribonacci(_ n: Int) -> Int {
        guard n != 0 else { return 0 }
        guard n > 2 else { return 1 }

        return nTribonacci(0, 1, 1, n, 3)
    }

    func nTribonacci(_ t1: Int, _ t2: Int, _ t3: Int, _ target: Int, _ current: Int) -> Int {
        guard current != target else { return t1 + t2 + t3 }
        return nTribonacci(t2, t3, t1 + t2 + t3, target, current + 1)
    }
```
## 2. Add Two Numbers

```swift
func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var carry = 0
        var cur1 = l1
        var cur2 = l2
        var result: ListNode?
        var curRes: ListNode?
        while cur1 != nil || cur2 != nil {
            var newVal = (cur1?.val ?? 0) + (cur2?.val ?? 0) + carry
            carry = newVal / 10
            newVal = newVal % 10
            let newNode = ListNode(newVal)
            if result == nil {
                result = newNode
                curRes = result
            } else {
                curRes!.next = newNode
                curRes = curRes?.next
            }
            cur1 = cur1?.next
            cur2 = cur2?.next
        }
        
        if carry != 0 {
            curRes?.next = ListNode(carry)
        }
        
        return result ?? ListNode(0)
    }
```
## 88. Merge Sorted Array

```swift
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        var k = m+n-1
        var i = m-1
        var j = n-1
        while k >= 0 && j>=0 {
            if i>=0 && nums1[i] > nums2[j] {
                nums1[k] = nums1[i]
                i-=1
            } else {
                nums1[k] = nums2[j]
                j-=1
            }
            k-=1
        }
    }
```

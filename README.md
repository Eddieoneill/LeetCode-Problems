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
## 347. Top K Frequent Elements

```swift
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
      var dict: [Int: Int] = [:]
	
		// O(n)
		for element in nums {
			dict[element, default: 0] += 1
		}
		
		var countArray: [(element: Int, count: Int)] = []
		// O(n)
		dict.forEach { (element, count) in
			countArray.append((element, count))
		}
		
		countArray.sort { (e1, e2) -> Bool in
			return e1.count > e2.count
		}
		
		var answer: [Int] = []
		
		for i in 0..<k {
			answer.append(countArray[i].element)
		}
		
		return answer
    }
```
## 1119. Remove Vowels from a String

```swift
    func removeVowels(_ S: String) -> String {
        var vowels: Set<Character> = ["a", "e", "i", "o", "u"]
        var result = ""
        
        for char in S where !vowels.contains(char) {
            result += String(char)
        }
        
        return result
    }
```
## 1165. Single-Row Keyboard

```swift
    func calculateTime(_ keyboard: String, _ word: String) -> Int {
        var dict: [Character: Int] = [:]
        var count = 0
        var result = 0
        var currentKey: Character = "a"
        
        for (index, char) in keyboard.enumerated() {
            if index == 0 { currentKey = char }
            dict[char] = count
            count += 1
        }
        
        for char in word {
            result += abs(dict[char]! - dict[currentKey]!)
            currentKey = char
        }
        
        return result
    }
```
## 905. Sort Array By Parity

```swift
    func sortArrayByParity(_ A: [Int]) -> [Int] {
        var counting = [Int]()
        var even = [Int]()
        var odd = [Int]()
        
        if A.count < 500 {
            counting = A.sorted { $0 < $1 }
            for num in counting {
                if num % 2 == 0 {
                    even.append(num)
                } else {
                    odd.append(num)
                }
            }
        } else {
            var point = 0
            counting = Array(repeating: 0, count: 5001)
            
            for num in A {
                counting[num] += 1
            }
        
            while point <= 5000 {
                if counting[point] > 1 && point % 2 == 0 {
                    counting[point] -= 1
                    even.append(point)
                } else if counting[point] == 1 && point % 2 == 0 {
                    counting[point] -= 1
                    even.append(point)
                    point += 1
                } else if counting[point] > 1 && point % 2 == 1 {
                    counting[point] -= 1
                    odd.append(point)
                } else if counting[point] == 1 && point % 2 == 1 {
                    counting[point] -= 1
                    odd.append(point)
                    point += 1
                } else {
                    point += 1
                }
            }
        }
        
        return even + odd
    }
```
## 1302. Deepest Leaves Sum

```swift
    func deepestLeavesSum(_ root: TreeNode?) -> Int {
        var sum = 0
        var maxLevel = 0
        checkSum(root, sum: &sum, level: 0, maxLevel: &maxLevel)
        return sum
    }


    func checkSum(_ root: TreeNode?, sum: inout Int, level: Int, maxLevel: inout Int) {
        guard let root = root else { return }
        guard root.left != nil || root.right != nil else {
            if level > maxLevel {
                maxLevel = level
                sum = root.val
            } else if maxLevel == level {
                sum += root.val
            }
            return
        }
        checkSum(root.left, sum: &sum, level: level + 1, maxLevel: &maxLevel)
        checkSum(root.right, sum: &sum, level: level + 1, maxLevel: &maxLevel)
    }
```
## 1086. High Five

```swift
    func highFive(_ items: [[Int]]) -> [[Int]] {
        var idToAllScores: [Int: [Int]] = [:]
        var finalArray: [[Int]] = []
        
        for item in items {
            let id = item[0]
            let score = item[1]
            
            if let currentArray = idToAllScores[id] {
                idToAllScores[id] = currentArray + [score]
            } else {
                idToAllScores[id] = [score]
            }
        }
        
        for (id, scores) in idToAllScores {
            finalArray.append([id, getAverageOfTopFiveScores(scores)])
        }
        
        return finalArray.sorted { $0[0] < $1[0] }
    }
    
    func getAverageOfTopFiveScores(_ allScores: [Int]) -> Int {
        var newScores = allScores
        var total = 0
        newScores.sort { $0 > $1 }
        let indexAmount = min(5, newScores.count)
        for index in 0..<indexAmount {
            total += newScores[index]
        }
        
        total = total / indexAmount
        
        return total
    }
```
## 128. Longest Consecutive Sequence

```swift
    func longestConsecutive(_ nums: [Int]) -> Int {
        guard nums.count > 0 else { return 0 }
        var seenNum: Set<Int> = []
        var setNums = Set(nums)
        var maxCount = 0
        
        for num in nums {
            guard !seenNum.contains(num) else { continue }
            var count = 0
            
            checkNums(&seenNum, &setNums, num, &count)      
            maxCount = max(count, maxCount)
        }
        
        return maxCount
    }
    
    func checkNums(_ seen: inout Set<Int>, _ setNums: inout Set<Int>, _ num: Int, _ count: inout Int) {
        seen.insert(num)
        count += 1
        if setNums.contains(num + 1) && !seen.contains(num + 1) {
            checkNums(&seen, &setNums, num + 1, &count) 
        }
        if setNums.contains(num - 1) && !seen.contains(num - 1) {
            checkNums(&seen, &setNums, num - 1, &count) 
        }
    }
```
## 973. K Closest Points to Origin

```swift
    func kClosest(_ points: [[Int]], _ K: Int) -> [[Int]] {
        var p = points
        var result = [[Int]]()
        p.sort(by: sort)

        for num in 0..<K {
            result.append(p[num])
        }

        return result
    }

    func sort(_ p1: [Int], p2: [Int]) -> Bool {
        return ((p1[0] * p1[0]) + (p1[1] * p1[1])) < (p2[0] * p2[0]) + (p2[1] * p2[1])
    }
```
## 1365. How Many Numbers Are Smaller Than the Current Number

```swift
    func smallerNumbersThanCurrent(_ nums: [Int]) -> [Int] {
        var count = [Int](repeating: 0, count: 101)
        var result = [Int](repeating: 0, count: nums.count)
        
        for i in 0..<nums.count {
            count[nums[i]] += 1
        }
        
        print(count)
        for i in 1...100 {
            count[i] += count[i-1]
        }
        print(count)
        
        for i in 0..<nums.count {
            result[i] = (nums[i] == 0) ? 0 : count[nums[i] - 1]
        }
        return result
    }
```

## 581. Shortest Unsorted Continuous Subarray

```swift
    func findUnsortedSubarray(_ nums: [Int]) -> Int {
        var sortedNums = nums.sorted(by: < )
        var start = 0
        var end = 0
        for i in 0 ..< nums.count {
            if nums[i] != sortedNums[i]  {
                start = i
                break
            }
        }
        for j in stride(from: nums.count-1, to: 0, by: -1) {
            if nums[j] != sortedNums[j] {
                end = j
                break
            }
        }
        if start == end {
            return 0
        } else {
            return end - start + 1
        }
    }
```
## 912. Sort an Array

```swift
class Solution {
    func sortArray(_ nums: [Int]) -> [Int] {
        guard nums.count > 1 else { return nums } 
        var array = nums
        
        QuickSort.sort(array: &array)
        
        return array
    }
}

class QuickSort {
    
    static func sort(array: inout [Int]) {
        quickSort(array: &array, l: 0, h: array.count - 1)
    }
    
    static func quickSort(array: inout [Int], l: Int, h: Int) {
        guard l < h else { return }
        
        let pivot =  partition(array: &array, l: l, h: h)
        
        quickSort(array: &array, l: l, h: pivot - 1)
        quickSort(array: &array, l: pivot + 1, h: h)
        
    }
    
    static func partition(array: inout [Int], l: Int, h: Int) -> Int {
        let pivot = array[h]  
        var i = l-1
        
        for index in l..<h {
            if array[index] < pivot {
                i += 1
                swap(array: &array, f: index, s: i)
            }
        }
        swap(array: &array, f: h, s: i + 1)
        return i+1; 
    }
    
    static func swap(array: inout [Int], f: Int, s: Int) {
        let tmp = array[f]
        
        array[f] = array[s]
        array[s] = tmp
    }
    
}
```
## 704. Binary Search

```swift
    func search(_ nums: [Int], _ target: Int) -> Int {
        var left = 0
        var right = nums.count - 1

        while left <= right {

            let middle = Int(floor(Double(left + right) / 2.0))

            if nums[middle] < target {
                left = middle + 1
            } else if nums[middle] > target {
                right = middle - 1
            } else {
                return middle
            }
        }

        return -1
    }
```

## 104. Maximum Depth of Binary Tree

```swift
class Solution {
    func maxDepth(_ root: TreeNode?) -> Int {
        guard root != nil else { return 0 }
        var maxHeight = 0
        height(max: &maxHeight, currentNode: root, currentHeight: 1)
        return maxHeight
    }

    func height(max: inout Int, currentNode: TreeNode?, currentHeight: Int) {
        guard currentNode?.left != nil || currentNode?.right != nil else {
            if currentHeight > max {
                max = currentHeight
            }
            return 
        }
        if currentNode?.left != nil {
            height(max: &max, currentNode: currentNode?.left, currentHeight: currentHeight + 1)
        }
        if currentNode?.right != nil {
            height(max: &max, currentNode: currentNode?.right, currentHeight: currentHeight + 1)
        }
    }
}
```
## 383. Ransom Note

```swift
    func canConstruct(_ ransomNote: String, _ magazine: String) -> Bool {
	guard ransomNote.count <= magazine.count else { return false }
	
        var availableChar: [Character: Int] = [:]
        
        for char in magazine {
            if let count = availableChar[char] {
                availableChar[char] = count + 1
            } else {
                availableChar[char] = 1
            }
        }
        
        for char in ransomNote {
            if let count = availableChar[char], count > 0 {
                availableChar[char] = count - 1
            } else {
                return false
            }
        }
        
        return true
    }
```
## 1436. Destination City

```swift
    func destCity(_ paths: [[String]]) -> String {
        var seenCity: Set<String> = []
        var result = paths[0][1]
        
        for path in paths {
            seenCity.insert(path[0])
        }
        
        for path in paths {
            if !seenCity.contains(path[1]) {
                result = path[1]
                print(path[1])
            }
        }
        
        return result
    }
```
## 1431. Kids With the Greatest Number of Candies

```swift
func kidsWithCandies(_ candies: [Int], _ extraCandies: Int) -> [Bool] {
        var max = 0
        var result: [Bool] = []
        
        for candy in candies {
            if candy > max {
                max = candy
            }
        }
        
        for candy in candies {
            if candy + extraCandies >= max {
                result.append(true)
            } else {
                result.append(false)
            }
        }
        
        return result
    }
```
## 1342. Number of Steps to Reduce a Number to Zero

```swift
    func numberOfSteps (_ num: Int) -> Int {
        var num = num
        var count = 0
        
        while num > 0 {
            if num % 2 == 0 {
                num = num / 2
            } else {
                num = num - 1
            }
            count += 1
        }
        
        return count
    }
```
## 1389. Create Target Array in the Given Order

```swift
    func createTargetArray(_ nums: [Int], _ index: [Int]) -> [Int] {
        var result: [Int] = []
        
        for (i, num) in nums.enumerated() {
            if index[i] <= i {
                result.insert(num, at: index[i])
            } else {
                result.append(num)
            }
        }
        
        return result
    }
```
## 1323. Maximum 69 Number

```swift
    func maximum69Number (_ num: Int) -> Int {
        var arrNum = Array(String(num))
        
        for (index, num) in arrNum.enumerated() {
            if num == "6" {
                arrNum[index] = "9"
                break
            }
        }
        
        return Int(arrNum.reduce("", { String($0) + String($1) })) ?? 0
    }
```
## 1351. Count Negative Numbers in a Sorted Matrix

```swift
    func countNegatives(_ grid: [[Int]]) -> Int {
        var count = 0
        
        for nums in grid {
            count += checkNegatives(nums)
        }
        
        return count
    }
    
    func checkNegatives(_ row: [Int]) -> Int {
        var right = row.count - 1 
        var result = 0
        
        for _ in 0..<row.count {
            if row[right] < 0 {
                result += 1
                right -= 1
            } else {
                break
            }
        }
        
        return result
    }
```
## 1299. Replace Elements with Greatest Element on Right Side

```swift
    func replaceElements(_ arr: [Int]) -> [Int] {
        let n = arr.count
        var a = Array(repeating: -1, count: n)
        var last = a[n-1]
        
        for i in (0..<arr.count-1).reversed() {
            last = max(last, arr[i + 1])
            a[i] = last
        }
        return a
    }
```
## 383. Ransom Note

```swift
func canConstruct(_ ransomNote: String, _ magazine: String) -> Bool {
        guard ransomNote.count <= magazine.count else { return false }
        var availableChar: [Character: Int] = [:]
        
        for char in magazine {
            if let count = availableChar[char] {
                availableChar[char] = count + 1
            } else {
                availableChar[char] = 1
            }
        }
        
        for char in ransomNote {
            if let count = availableChar[char], count > 0 {
                availableChar[char] = count - 1
            } else {
                return false
            }
        }
        
        return true
    }
```
## 1309. Decrypt String from Alphabet to Integer Mapping

```swift
    func freqAlphabets(_ s: String) -> String {
        let dict = ["4": "d", "23": "w", "6": "f", "20": "t", "21": "u", "13": "m", "25": "y", "19": "s", "2": "b", "7": "g", "8": "h", "1": "a", "10": "j", "15": "o", "22": "v", "18": "r", "14": "n", "17": "q", "16": "p", "12": "l", "9": "i", "11": "k", "24": "x", "3": "c", "26": "z", "5": "e"]
        var result = ""
        let arrStr = Array(s)
        var holder = ""
        var isTwoDigit = false
        
        for index in (0..<arrStr.count).reversed() {
            if holder.count == 2 {
                result = "\(dict[holder] ?? "")\(result)"
                holder = ""
                if arrStr[index] != "#" {
                    result = "\(dict[String(arrStr[index])] ?? "")\(result)"
                    isTwoDigit = false
                }
            } else if isTwoDigit {
                holder = "\(String(arrStr[index]))\(holder)"
            } else if arrStr[index] == "#" {
                isTwoDigit = true
            } else {
                result = "\(dict[String(arrStr[index])] ?? "")\(result)"
            }
        }
        
        if !holder.isEmpty {
            result = "\(dict[holder] ?? "")\(result)"
        }

        return result
    }
```
## 1304. Find N Unique Integers Sum up to Zero

```swift
    func sumZero(_ n: Int) -> [Int] {
        var result: [Int] = []
        var used: Set<Int> = []
        var isEven = n % 2 == 0
        var loopCount = n/2
        
        if !isEven {
            result.append(0)
        }
        
        for num in 0..<loopCount {
            result.append(-(num + 1))
            result.append((num + 1))
        }
        
        return result
    }
```
## 189. Rotate Array

```swift
    func rotate(_ nums: inout [Int], _ k: Int) {
        var result: [Int] = []
        var container: [Int] = []
        var current = 1
        var k = k
        
        if k > nums.count {
            k = k - nums.count
        }
        
        for num in nums {
            if nums.count - current >= k {
                result.append(num)
            } else {
                container.append(num)
            }
            current += 1
        }
        nums = container + result
    }
```
## 1374. Generate a String With Characters That Have Odd Counts

```swift
    func generateTheString(_ n: Int) -> String {
        var result = ""
        if n % 2 == 0 {
            for _ in 1..<n {
                result += "a"
            }
            result += "b"
        } else {
            for _ in 0..<n {
                result += "a"
            }
        }
        return result
    }
```
## 122. Best Time to Buy and Sell Stock II

```swift
    func maxProfit(_ prices: [Int]) -> Int {
        guard !prices.isEmpty else { return 0 }
        
        var result = 0
        
        for i in 1..<prices.count {
            if prices[i-1] < prices[i] {
                result += prices[i] - prices[i-1]
            }
        }
        return result
    }
```
## 760

```swift
func anagramMappings(A: [Int], B: [Int]) -> [Int] {
    var dict: [Int: Int] = [:]
    var result = [Int]()
    
    for (index, num) in B.enumerated() {
        dict[num] = index
    }
    
    for num in A {
        let b = dict[num] ?? 0
        result.append(b)
    }
    
    return result
}
```
## 1290. Convert Binary Number in a Linked List to Integer

```swift
    func getDecimalValue(_ head: ListNode?) -> Int {
        var current = head
        var result = 0
        
        while current != nil {
            if let val = current?.val {
                result *= 2
                result += val
                current = current?.next   
            }
        }
        
        return result
    }
```
## 1266. Minimum Time Visiting All Points

```swift
    func minTimeToVisitAllPoints(_ points: [[Int]]) -> Int {
        var time = 0
        
        for count in 0..<points.count - 1 {
            let x  = abs(points[count][0] - points[count + 1][0])
            let y = abs(points[count][1] - points[count + 1][1])
            let num = max(x, y)
            
            time += num
        }
        return time
    }
```
## 1252. Cells with Odd Values in a Matrix

```swift
    func oddCells(_ n: Int, _ m: Int, _ indices: [[Int]]) -> Int {
        var rows = Array(repeating: 0, count: n)
        var columns = Array(repeating: 0, count: m)
        var result = 0
        
        for index in indices {
            rows[index[0]] += 1
            columns[index[1]] += 1
        }
        
        for row in 0..<n {
            for column in 0..<m {
                if isOdd(rows[row], columns[column]) {
                    result += 1
                }
            }
        }
        
        return result
    }
    
    func isOdd(_ row: Int, _ column: Int) -> Bool {
        return (row + column) % 2 != 0 
    }
```
## 1370. Increasing Decreasing String

```swift
    func sortString(_ s: String) -> String {
        var buckets = Array(repeating: 0, count: 26)
        var result: [String] = []
        var count = 0
        
        for char in s {
            let ascii = Int(char.asciiValue!) - Int(Character("a").asciiValue!) 
            buckets[ascii] += 1
        }
        
        while !bucketIsEmpty(buckets) {
            if count % 2 == 0 {
                result = leastToBiggiest(&buckets, result)
            } else {
                result = biggestToLeast(&buckets, result)   
            }
            count += 1
        }
        

        return result.joined(separator: "")

    }
    
    func bucketIsEmpty(_ arr: [Int]) -> Bool {
        return arr.reduce(0, +) == 0
    }
    
    func leastToBiggiest(_ arr:inout [Int], _ result: [String]) -> [String] {
        var result = result
        
        for (index, num) in arr.enumerated() where num != 0 {
            let a = Int(Character("a").asciiValue!)
            let char = String(Character(UnicodeScalar(a + index)!))
            result.append(char)
            arr[index] -= 1
        }

        
        return result
    } 
    
    func biggestToLeast(_ arr:inout [Int], _ result: [String]) -> [String] {
        var result = result
        
        for (index, num) in arr.enumerated().reversed() where num != 0 {
            let a = Int(Character("a").asciiValue!)
            let char = String(Character(UnicodeScalar(a + index)!))
            result.append(char)
            arr[index] -= 1
            
        }
        
        return result
    }     
```
## 1295. Find Numbers with Even Number of Digits

```swift
    func findNumbers(_ nums: [Int]) -> Int {
        var result: Int = 0

        for num in nums where Int(log10(CGFloat(num)) + 1) % 2 == 0 {
            result += 1
        }

        return result
    }
```
## 905. Sort Array By Parity

```swift
func sortArrayByParity(_ A: [Int]) -> [Int] {
        var left = 0
        var right = A.count - 1
        var A = A
        
        while left < right {
            print(left)
            print(right)
            while left < right && A[left] % 2 == 0 {
                left += 1
            }
            
            while left < right && A[right] % 2 != 0 {
                right -= 1
            }
            
            let temp = A[left]
            A[left] = A[right]
            A[right] = temp
            
            left += 1
            right -= 1
        }
        
        return A
    }
```
## 771. Jewels and Stones

```swift
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        var jewls: Set<String> = []
        var result = 0
        
        for j in J {
            jewls.insert(String(j))
        }
        
        for stone in S where jewls.contains(String(stone)) {
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
        var num = n
        
        while num > 0 {
            var digit = num % 10
            product *= digit
            sum += digit
            num = num / 10
        }
        
        return product - sum
    }
```
## 617. Merge Two Binary Trees

```swift
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        return dfs(t1, t2)
    }

    func dfs(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        guard t1 != nil || t2 != nil else { return nil }
        
        var node: TreeNode? = nil
        
        if t2 == nil {
            node = TreeNode(t1!.val)
            node?.left = dfs(t1?.left, nil)
            node?.right = dfs(t1?.right, nil)
        } else if t1 == nil {
            node = TreeNode(t2!.val)
            node?.left = dfs(nil, t2?.left)
            node?.right = dfs(nil, t2?.right)
        } else {
            let sum = t1!.val + t2!.val
            node = TreeNode(sum)
            node?.left = dfs(t1?.left, t2?.left)
            node?.right = dfs(t1?.right, t2?.right)
        }
        
        return node
    }
```
## 961. N-Repeated Element in Size 2N Array

```swift
    func repeatedNTimes(_ A: [Int]) -> Int {
        var set: Set<Int> = []
        
        for num in A {
            if set.contains(num) {
                return num
            } else {
                set.insert(num)
            }
        }
        
        return 0
    }
```
## 657. Robot Return to Origin

```swift
    func judgeCircle(_ moves: String) -> Bool {
        var position = (0, 0)
        
        for move in moves {
            if move == "U" {
                position.0 += 1
            } else if move == "D" {
                position.0 -= 1
            } else if move == "R" {
                position.1 += 1
            } else {
                position.1 -= 1
            }
        }
        
        return position == (0, 0)
    }
```
## 977. Squares of a Sorted Array

```swift
    func sortedSquares(_ A: [Int]) -> [Int] {
        var startingPoint = 0
        var result: [Int] = [] 
        var left = 0
        var right = 0
        
        for (index, num) in A.enumerated() {
            if index < A.count - 1, abs(num) < abs(A[index + 1]) {
                startingPoint = index
                if result.isEmpty {
                    result.append(num * num)    
                } else {
                    result[0] = num * num
                }
                break
            }
            startingPoint = index
            if result.isEmpty {
                result.append(num * num)    
            } else {
                result[0] = num * num
            }
        }
        print(result)
        
        left = startingPoint - 1
        right = startingPoint + 1
        
        while result.count < A.count {
            if left >= 0 && right < A.count {
                let leftNum = A[left] * A[left]
                let rightNum = A[right] * A[right]
            
                if leftNum < rightNum {
                    result.append(leftNum)
                    left -= 1
                } else {
                    result.append(rightNum)
                    right += 1
                }
            } else if left >= 0 {
                let leftNum = A[left] * A[left]
                result.append(leftNum)
                left -= 1
            } else if right < A.count {
                let rightNum = A[right] * A[right]
                result.append(rightNum)
                right += 1
            }

        }
        
        return result
    }
```
## 942. DI String Match

```swift
    func diStringMatch(_ S: String) -> [Int] {
        var result: [Int] = Array(repeating: 0, count: S.count + 1)
        var left = 0
        var right = S.count 
        var count = 0
        
        for char in S {
            if char == "I" {
                result[count] = left 
                left += 1
            } else {
                result[count] = right
                right -= 1
            }
            
            count += 1
        }

        if left < right {
            result[count] = right
        } else {
            result[count] = left 
        }
        
        return result
    }
```
## 461. Hamming Distance

```swift
    func hammingDistance(_ x: Int, _ y: Int) -> Int {
        var mismatch = x ^ y
        var count = 0
        
        while mismatch > 0 {
            count += 1
            mismatch &= (mismatch - 1)
        }
        
        return count
    }
```
## 561. Array Partition I

```swift
    func arrayPairSum(_ nums: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 20_001) 
        var sorted: [Int] = []
        var sum = 0
        
        for num in nums {
            buckets[num + 10_000] += 1
        }
        
        for (index, bucket) in buckets.enumerated() where bucket > 0 {
            var count = bucket
            
            while count > 0 {
                sorted.append(index - 10_000)
                count -= 1
            }
        }
        
        var count = 0
        
        while count < sorted.count {
            sum += min(sorted[count], sorted[count + 1]) 
            count += 2
        }
        
        return sum
    }
```
## 852. Peak Index in a Mountain Array

```swift
    func peakIndexInMountainArray(_ A: [Int]) -> Int {
        var left = 0
        var right = A.count - 1
        
        while left < right {
            let mid = ((right - left) / 2) + left
            
            if mid + 1 < A.count, A[mid] > A[mid + 1] {
                right = mid
            } else {
                left = mid + 1
            }
        }
        
        return left
    }
```
## 1403. Minimum Subsequence in Non-Increasing Order

```swift
    func minSubsequence(_ nums: [Int]) -> [Int] {
        var buckets = Array(repeating: 0, count: 101)
        var totalSum = 0
        var sum = 0
        var result: [Int] = []
        
        for num in nums {
            buckets[num] += 1
            totalSum += num
        }
        
        for index in (0..<buckets.count).reversed() {
            var count = buckets[index]
            
            while sum <= totalSum && count > 0 {
                sum += index
                totalSum -= index
                result.append(index)
                count -= 1
            }
            
            if sum > totalSum {
                return result
            }
        }
        
        return result
    }
```
## 589. N-ary Tree Preorder Traversal

```swift
    func preorder(_ root: Node?) -> [Int] {
        var result: [Int] = []
        func dfs(_ node: Node?) {
            guard let node = node else { return }
            result.append(node.val)
            
            for child in node.children {
                dfs(child)
            }
        }
        dfs(root)
        return result
    }
```
## 590. N-ary Tree Postorder Traversal

```swift
    func postorder(_ root: Node?) -> [Int] {
        var result: [Int] = []
        func dfs(_ node: Node?) {
            guard let node = node else { return }
            
            for child in node.children {
                dfs(child)
            }
            result.append(node.val)
        }
        dfs(root)
        return result
    }
```

```swift
    func postorder(_ root: Node?) -> [Int] {
        guard let root = root else { return [] }
        var result: [Int] = []
        var stack = [root]
        
        while !stack.isEmpty {
            let top = stack.removeLast()
            result.append(top.val)
            
            
            for child in top.children {
                stack.append(child)
            }
        }
        return result.reversed()
    }
```
## 1207. Unique Number of Occurrences

```swift
    func uniqueOccurrences(_ arr: [Int]) -> Bool {
        var dict = [Int: Int]()
        
        for num in arr {
            dict[num, default: 0] += 1
        }
        
        return Set(dict.values).count == dict.keys.count
    }
```
## 700. Search in a Binary Search Tree

```swift
    func searchBST(_ root: TreeNode?, _ val: Int) -> TreeNode? {
        guard let node = root else { return nil }
        
        if val == node.val {
            return root
        }
        
        if node.val > val {
            return searchBST(root?.left, val)
        } else {
            return searchBST(root?.right, val)
        }
    }
```
## 1051. Height Checker

```swift
    func heightChecker(_ heights: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 101)
        var sorted: [Int] = []
        var diff = 0
        
        for height in heights {
            buckets[height] += 1
        }
        
        for (index, bucket) in buckets.enumerated() where bucket > 0 {
            var count = bucket
            
            while count > 0 {
                sorted.append(index)
                count -= 1
            }
        }
        
        for (index, height) in heights.enumerated() {
            if height != sorted[index] {
                diff += 1
            }
        }
        
        return diff
    }
```
## 944. Delete Columns to Make Sorted

```swift
    func minDeletionSize(_ A: [String]) -> Int {
        var d = 0
        for column in 0..<A[0].count {
            let columnIndex = A[0].utf16.index(A[0].utf16.startIndex, offsetBy: column)
            for row in 1..<A.count {
                if A[row][columnIndex] < A[row - 1][columnIndex] {
                    d += 1
                    break
                }
            }
        }
        return d
    }
```
## 346. Moving Average from Data Stream

```swift
class MovingAverage {

    var window: [Int]
    var total: Int = 0
    var currentLast = 0
    var max: Int
    var count = 0
    
    init(_ size: Int) {
        max = size
        window = Array(repeating: 0, count: size)
    }
    
    func next(_ val: Int) -> Double {
        if count < max { 
            window[count] = val
            total += val 
            count += 1
            return Double(total) / Double(count)
        } else {
            total -= window[currentLast]
            total += val
            window[currentLast] = val
            if currentLast == window.count - 1 {
                currentLast = 0
            } else {
                currentLast += 1
            }
            return Double(total) / Double(window.count)
        }
    }
}
```
## 359. Logger Rate Limiter

```swift
class Logger {

    var dict: [String: Int] = [:]
    
    init() {}
    
    func shouldPrintMessage(_ timestamp: Int, _ message: String) -> Bool {
        if let preTime = dict[message] {
            if timestamp >= preTime + 10 {
                dict[message] = timestamp
                return true
            }
            return false
        }
        
        dict[message] = timestamp
        return true
    }
}
```
## 1441. Build an Array With Stack Operations

```swift
    func buildArray(_ target: [Int], _ n: Int) -> [String] {
        var result: [String] = []
        var current = 0 
        
        for num in 1...n {
            if current < target.count {
                if num == target[current] {
                    result.append("Push")
                    current +=  1
                } else {
                    result.append("Push")
                    result.append("Pop")
                }
            } else {
                break
            }
        }
        
        return result
    }
```
## 811. Subdomain Visit Count

```swift
func subdomainVisits(_ cpdomains: [String]) -> [String] {
        var result = [String]()
        
        guard !cpdomains.isEmpty else {
            return result
        }
        
        var hashMap = [String: Int]()
        
        for i in 0..<cpdomains.count {
            let spaceSplit = cpdomains[i].split(separator: " ")
            
            guard spaceSplit.count == 2 else {
                continue
            }
            
            let visits = Int(String(spaceSplit[0])) ?? 0
            let domain = spaceSplit[1]
            
            let domainSplit = domain.split(separator:".") //Substring
            var current = ""
            for j in stride(from: domainSplit.count-1, through: 0, by: -1) {
                let domainString = String(domainSplit[j])
                current =  (j == domainSplit.count - 1) ?  "\(domainString)" : "\(domainString).\(current)"                
                hashMap[current, default: 0] +=  visits
            }
        }
        
        result = hashMap.map { "\($1) \($0)" }
        
        return result
    }
```
## 897. Increasing Order Search Tree

```swift
    func increasingBST(_ root: TreeNode?) -> TreeNode? {
        var newTree: TreeNode? = nil
        var current: TreeNode? = nil
        
        func dfs(_ node: TreeNode?) {
            guard let curr = node else { return }
            
            dfs(node?.left)
            
            if newTree == nil {
                newTree = TreeNode(curr.val)
                current = newTree
            } else {
                current?.right = TreeNode(curr.val)
                current = current?.right
            }
            
            dfs(node?.right)
        }
        
        dfs(root)
        
        return newTree
    }
```
## 1356. Sort Integers by The Number of 1 Bits

```swift
class Solution {
    
    var memo: [Int: Int] = [:]
    
    func sortByBits(_ arr: [Int]) -> [Int] {
        var result = arr
        result.sort { (a, b) -> Bool in
                     var countA = countOfOnes(a)
                     var countB = countOfOnes(b) 
                     if countA == countB {
                         return a < b
                     } else {
                         return countA < countB
                     }
                 }
        return result
    }
    
    func countOfOnes(_ num: Int) -> Int {
        if let result = memo[num] {
            return result
        }
        
        var n = num
        var count = 0            
        
        while n > 0 {
            count += 1      
            n = n & (n - 1)
        }
        
        memo[num] = count
        
        return count
    }
                 
}
```
## 922. Sort Array By Parity II

```swift
    func sortArrayByParityII(_ A: [Int]) -> [Int] {
        var A = A
        var j = 1
        
        for i in stride(from:0, to:A.count, by: 2) {
            if A[i] % 2 == 1 {
                
                while j < A.count && A[j] % 2 == 1 {
                    j = j + 2
                }
                
                var temp = A[i]
                A[i] = A[j]
                A[j] = temp
            }
        }
          return A
    }
```
## 1196. How Many Apples Can You Put into the Basket

```swift
    func maxNumberOfApples(_ arr: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 1001)
        var total = 0
        var count = 0
        
        for num in arr {
            buckets[num] += 1
        }
        
        for (index, bucket) in buckets.enumerated() where bucket > 0 {
            var bucket = bucket
            
            while bucket > 0 {
                if index + total > 5000 {
                    return count
                } else {
                    bucket -= 1
                    count += 1
                    total += index
                }
            }
        }
        
        return count
    }
```
## 1337. The K Weakest Rows in a Matrix

```swift
    func kWeakestRows(_ mat: [[Int]], _ k: Int) -> [Int] {
        let counts = getCounts(mat)   
        var result: [Int] = []
        var buckets = Array(repeating: (0, [0]), count: 101)
        
        for (index, count) in counts {
            if buckets[count].0 == 0 {
                buckets[count].0 += 1
                buckets[count].1[0] = index
            } else {
                buckets[count].0 += 1
                buckets[count].1.append(index)
            }
        }
        
        for bucket in buckets where bucket.0 > 0 {
            var count = bucket.0
            var index = 0
            
            while count > 0 && result.count < k {
                result.append(bucket.1[index])
                index += 1
                count -= 1
            }
        }
        
        return result
    }
    
    func getCounts(_ mat: [[Int]]) -> [(Int, Int)] {
        var counts: [(Int, Int)] = []
        for (index, row) in mat.enumerated() {
            let count = binarySerch(row)
            counts.append((index, count) )
            print(count)
        }
        return counts
    }
    
    func binarySerch(_ row: [Int]) -> Int {
        var left = 0
        var right = row.count - 1
        
        while left < right {
            let mid = (right - left) / 2 + left
            
            if row[mid] == 0 {
                right = mid - 1
            } else if row[mid] == 1 && row[mid + 1] == 0 {
                left = mid
                break
            } else {
                left = mid + 1
            }
        }
        
        if row[left] == 0 {
            return 0
        } else {
            return left + 1
        }
    }
```
## 876. Middle of the Linked List


```swift
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
```
## 559. Maximum Depth of N-ary Tree

```swift
    func maxDepth(_ root: Node?) -> Int {
        var maxDepth = 0
        
        func dfs(_ root: Node?, _ depth: Int) {
            guard let node = root else { return }
            
            if depth > maxDepth {
                maxDepth = depth
            }
            
            for child in node.children {
                dfs(child, depth + 1)
            }
        }  
        
        dfs(root, 1)
        
        return maxDepth
    }
```
## 929. Unique Email Addresses

```swift
    func numUniqueEmails(_ emails: [String]) -> Int {
        var uniqueEmail : Set<String> = []
        
        for email in emails {
            
            var validEmail: String = ""
            var index = email.startIndex
            
            while index < email.endIndex {
                let letter = email[index]
                if letter == "+" {
                    index = email.index(after: index)
                    while email[index] != "@" {
                        index = email.index(after: index)
                    }
                } else if letter == "." {
                    index = email.index(after: index)
                } else if letter == "@" {
                    validEmail.append(String(email[index...]))
                    break
                } else {
                    validEmail.append(letter)
                    index = email.index(after: index)
                }
            }
            uniqueEmail.insert(validEmail)
        }
        
        return uniqueEmail.count
    }
```
## 1122. Relative Sort Array

```swift
    func relativeSortArray(_ arr1: [Int], _ arr2: [Int]) -> [Int] {
        var buckets = Array(repeating: 0, count: 1001)
        var seen: Set<Int> = []
        var result: [Int] = []
        
        for num in arr1 {
            buckets[num] += 1
        }
        
        for num in arr2 {
            var count = buckets[num]
            seen.insert(num)
            while count > 0 {
                buckets[num] -= 1
                result.append(num)
                count -= 1
            }
        }
        
        for (index, num) in buckets.enumerated() where num != 0 {
            var count = num
            
            while count > 0 {
                result.append(index)
                count -= 1
            }
        }
        
        return result
    }
```
## 1047. Remove All Adjacent Duplicates In String

```swift
    func removeDuplicates(_ S: String) -> String {
        var stack: [String] = []
        
        for (index, char) in S.enumerated() {
            if stack.isEmpty {
                stack.append(String(char))
            } else if stack.last! == String(char) {
                stack.removeLast()
            } else {
                stack.append(String(char))
            }
        }
        
        return stack.joined()
    }
```
## 965. Univalued Binary Tree

```swift
    func isUnivalTree(_ root: TreeNode?) -> Bool {
        guard let seen = root?.val else { return false }
        var isSame = true
        
        func dfs(_ root: TreeNode?) {
            guard let val = root?.val else { return }
            
            if val != seen {
                isSame = false
                return
            }
            
            dfs(root?.left)
            dfs(root?.right)
        }
        
        dfs(root)
        return isSame
        
    }
```
## 1064. Fixed Point

```swift
    func fixedPoint(_ A: [Int]) -> Int {
        var left = 0
        var right =  A.count - 1
        
        while left + 1 < right {
            var mid = left + (right - left) / 2
            
            if mid <= A[mid] {
                right = mid 
            } else {
                left = mid + 1
            }
        }
        
        if A[left] == left {
            return left
        } else if A[right] == right {
            return right
        } else {
            return -1
        }
    }
```
## 1160. Find Words That Can Be Formed by Characters

```swift
func countCharacters(_ words: [String], _ chars: String) -> Int {
        var count = Array(chars).reduce(into: [:]) { count, char in             
            count[char, default: 0] += 1
        }
        var res = 0
        for word in words {
            var copyCount = count
            var build = true
            for char in word {
                if copyCount[char] == nil {
                    build = false
                    break
                } else {
                    copyCount[char]! -= 1
                    if copyCount[char]! < 0 {
                        build = false
                        break
                    }
                }
            }
            
            if build {
                res += word.count
            }
        }
        
        return res
    }
```
## 883. Projection Area of 3D Shapes

```swift
    func projectionArea(_ grid: [[Int]]) -> Int {
        var frontArea = 0
        var sideArea = 0
        var topArea = 0
        
        for i in 0..<grid.count {
            var rowMax = 0
            var columnMax = 0
            for j in 0..<grid[0].count {
                rowMax = grid[i][j] > rowMax ? grid[i][j] : rowMax
                columnMax = grid[j][i] > columnMax ? grid[j][i] : columnMax
                topArea += grid[i][j] > 0 ? 1 : 0
            }
            sideArea += rowMax
            frontArea += columnMax
        }
        return topArea + sideArea + frontArea
    }
```
## 1002. Find Common Characters

```swift
    func commonChars(_ A: [String]) -> [String] {
        var chars = Array(repeating: Int.max, count: 26)
        var ret: [String] = []
        
        for word in A {
            var _chars = Array(repeating: 0, count: 26)
            for c in word.unicodeScalars {
                _chars[Int(c.value) - 97] += 1
            }
            for i in 0..<26 {
                chars[i] = min(chars[i], _chars[i])
            }
        }
        
        for i in 0..<26 {
            for _ in 0..<chars[i] {
                ret.append(String(Character(Unicode.Scalar(i + 97)!)))
            }
        }
        
        return ret
    }
```
## 1133. Largest Unique Number

```swift
    func largestUniqueNumber(_ A: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 1001)
        
        for num in A {
            buckets[num] += 1
        }
        
        for (index, num) in buckets.enumerated().reversed() {
            if num == 1 {
                return index
            }
        }
        
        return -1
    }
```
## 509. Fibonacci Number


```swift
class Solution {
    var memo: [Int: Int] = [:]
    
    func fib(_ N: Int) -> Int {
        if let num = memo[N] { return num }
        if N == 0 { return 0 }
        if N == 1 { return 1 }
        
        var result = fib(N - 1) + fib(N - 2)
        
        memo[N] = result
        
        return result
    }
}
```
```swift
class Solution {
    
    func fib(_ N: Int) -> Int {
        if N == 0 { return 0 }
        if N == 1 { return 1 }
        
        var dp1 = 0
        var dp2 = 1
        
        for num in 0..<N {
            let result = dp1 + dp2
            dp1 = dp2
            dp2 = result
        }
        
        return dp1
    }
}
```
## 344. Reverse String


```swift
    func reverseString(_ s: inout [Character]) {
        var left = 0
        var right = s.count - 1
        
        while left < right {
            var temp = s[left]
            s[left] = s[right]
            s[right] = temp
            left += 1
            right -= 1
        }
    }
```
## Shortest Distance to a Character

```swift
func shortestToChar(_ S: String, _ C: Character) -> [Int] {
        var indices: [Int] = []
        var result: [Int] = []
        var first = 0
        var second = 1
        
        for (index, char) in S.enumerated() where char == C {
            indices.append(index)
        }
        
        for (index, char) in S.enumerated() {
            let f = abs(indices[first] - index)
            
            if second < indices.count {
                let s = abs(indices[second] - index)
                
                if s < f {
                    result.append(s)
                    first += 1
                    second += 1
                    continue   
                }
            } 
            result.append(f)
        }
        
        return result
    }
```
## 1394. Find Lucky Integer in an Array


```swift
   func findLucky(_ arr: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 501)
        
        for num in arr {
            buckets[num] += 1
        }
        
        for (num, count) in buckets.enumerated().reversed() where count > 0 {
            if count == num {
                return num
            }
        }
        return -1
    }
```
## 1022. Sum of Root To Leaf Binary Numbers

```swift
    func sumRootToLeaf(_ root: TreeNode?) -> Int {
        var sum = 0
        func dfs(_ root: TreeNode?, _ num: Int) {
            guard let node = root else { return }
            
            var num = num
            
            num *= 2
            num += node.val
            
            if node.left == nil && node.right == nil {
                sum += num
                return
            }
            
            dfs(root?.left, num)
            dfs(root?.right, num)
        }
        
        dfs(root, 0)
        
        return sum
    }
```
## 893. Groups of Special-Equivalent Strings


```swift
    func numSpecialEquivGroups(_ A: [String]) -> Int {
        var words: Set<String> = Set()
        for item in A{
            let chars = Array(item)
            var arr:[Character] = Array(repeating: "0", count: 52)
            for i in 0..<chars.count{
                if i % 2 == 0{
                    let val = Int(String(arr[Int(chars[i].asciiValue!) - 97]))! + 1
                    arr[Int(chars[i].asciiValue!) - 97] = Character(val.description)
                }else{
                    let val = Int(String(arr[Int(chars[i].asciiValue!) - 97 + 26]))! + 1
                    arr[Int(chars[i].asciiValue!) - 97 + 26] = Character(val.description)                
                }
            }
            words.insert(String(arr))
        }
        return words.count
    }
```
## 1200. Minimum Absolute Difference

```swift
    func minimumAbsDifference(_ arr: [Int]) -> [[Int]] {
        var arr = arr.sorted { $0 < $1 }
        var result: [[Int]] = []
        var minDiff = Int.max
        
        for (index, num) in arr.enumerated() where index < arr.count - 1 {
            let next = arr[index + 1]
            
            if abs(num - next) < minDiff {
                minDiff = abs(num - arr[index + 1])
                result = []
                result.append([num, next])
            } else if abs(num - next) == minDiff {
                result.append([num, next])
            }
        }
        
        return result
    }
```
## 1413. Minimum Value to Get Positive Step by Step Sum


```swift
    func minStartValue(_ nums: [Int]) -> Int {
        var minVal = 0
        var curr = 0
        
        for num in nums {
            curr += num
            if curr < minVal {
                minVal = curr
            }
        }
        
        return abs(minVal) + 1
    }
```
## 908. Smallest Range I


```swift
    func smallestRangeI(_ A: [Int], _ K: Int) -> Int {
        var minNum = Int.max
        var maxNum = Int.min
        
        for num in A {
            minNum = min(num, minNum)
            maxNum = max(num, maxNum)
        }
        
        let minMax = minNum + K
        let maxMin = maxNum - K
        
        return max(maxMin - minMax, 0)
    }
```
## 1030. Matrix Cells in Distance Order


```swift
    func allCellsDistOrder(_ R: Int, _ C: Int, _ r0: Int, _ c0: Int) -> [[Int]] {
        var buckets = Array(repeating: [[Int]](), count: R + C + 1)
        var result: [[Int]] = []
        
        for row in 0..<R {
            for col in 0..<C {
                let dist = manhattan(r0, c0, row, col)
                buckets[dist].append([row, col])
            } 
        }
        
        for bucket in buckets where bucket != [] {
            for point in bucket {
                result.append(point)
            }
        }
        
        return result
    }
    
    func manhattan(_ r1: Int, _ c1: Int, _ r2: Int, _ c2: Int) -> Int  {
        return abs((r1 - r2) ) + abs(c1 - c2) 
    }
```
```swift
    func allCellsDistOrder(_ R: Int, _ C: Int, _ r0: Int, _ c0: Int) -> [[Int]] {
        var result: [[Int]] = []
        let dirs = [[1, 0], [-1, 0], [0, 1], [0, -1]]
        var queue = [[r0,c0]]
        var visited: Set<String> = []
        visited.insert("\(r0)-\(c0)")
        
        while !queue.isEmpty {
            let vertex = queue.removeFirst()
            result.append(vertex)
            
            for dir in dirs {
                let r1 = vertex[0] + dir[0]
                let c1 = vertex[1] + dir[1]
                if r1 < 0 || r1 >= R || c1 < 0 || c1 >= C { continue }
                if visited.contains("\(r1)-\(c1)") { continue }
                visited.insert("\(r1)-\(c1)")
                queue.append([r1, c1])
            }
        }
        
        return result
    }
```
## 104. Maximum Depth of Binary Tree

```swift
    func maxDepth(_ root: TreeNode?) -> Int {
        var max = 0
        func dfs(_ root: TreeNode?, _ height: Int) {
            guard root != nil else { return }
            
            if height > max {
                max = height
            }
            
            dfs(root?.left, height + 1)
            dfs(root?.right, height + 1)
        }
        dfs(root, 1)
        
        return max
    }
```
```swift
    func maxDepth(_ root: TreeNode?) -> Int {
        guard root != nil else { return 0 }
        return max(maxDepth(root?.left), maxDepth(root?.right)) + 1
    }
```
## 136. Single Number


```swift
    func singleNumber(_ nums: [Int]) -> Int {
        var result = 0
        
        for num in 0..<nums.count {
            result ^= nums[num]
        }
        return result
    }
    
    // ^= is 
    
    // Bit Manipulation
    // [5,10,6,5,6]
    // ^
    // 0101
    // 1010
    // 1111
    // ^
    // 1111
    // 0110
    // 1001
    // ^
    // 1001
    // 0101
    // 1100
    // ^
    // 1100
    // 0110
    // 1010 = 10
    // N ^ N = 0
    // N ^ 0 = N
    // ^ XOR
    // 1 ^ 1 = 0
    // 1 ^ 0 = 1
    // 0 ^ 1 = 1
    // 0 ^ 0 = 0
```
## 872. Leaf-Similar Trees

```swift
    func leafSimilar(_ root1: TreeNode?, _ root2: TreeNode?) -> Bool {
        var list: [Int] = []
        var firstLoop = true
        var count = 0
        var result = true
        
        func getLeafs(_ root: TreeNode?) {
            guard let node = root else { return }
            guard root?.left != nil || root?.right != nil else {
                if firstLoop {
                    list.append(node.val)
                    return 
                } else if list.count - 1 >= count, list[count] != node.val {
                    result = false
                } else if list.count - 1 < count {
                    result = false
                }
                count += 1
                return
            }
            
            getLeafs(root?.left)
            getLeafs(root?.right)
        }
        
        getLeafs(root1)
        firstLoop = false
        getLeafs(root2)
        
        return result
    }
```
## 476. Number Complement

```swift
    func findComplement(_ num: Int) -> Int {
        var result = 0
        var i = 0
        var num = num
        
        while num > 0 {
            var bit = num & 1
            if bit == 1 {
                bit = 0
            } else {
                bit = 1
            }
            bit <<= i
            result |= bit
            num >>= 1
            i += 1
        }
        
        return result
    }
```
## 1078. Occurrences After Bigram


```swift
    func findOcurrences(_ text: String, _ first: String, _ second: String) -> [String] {
        var strArr = text.split(separator: " ")
        var result: [String] = []
        var i = 0
        var j = 1
        
        
        while j < strArr.count - 1 {
            if strArr[i] == first, strArr[j] == second {
                result.append(String(strArr[j + 1]))
            }
            
            i += 1
            j += 1
        }
        
        return result
    }
```
## 1185. Day of the Week


```swift
    func dayOfTheWeek(_ day: Int, _ month: Int, _ year: Int) -> String {
        let daysOfTheWeek = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
        var days = day

        func isLeapYear(_ year: Int) -> Bool {
            return year % 4 == 0 ? (year % 100 == 0 ? (year % 400 == 0 ? true : false) : true) : false
        }

        func daysInYear(_ year: Int) -> Int {
            return isLeapYear(year) ? 366 : 365
        }

        func daysInMonth(_ year: Int, _ month: Int) -> Int {
            switch month {
                case 1, 3, 5, 7, 8, 10, 12: return 31
                case 4, 6, 9, 11: return 30
                case 2: return isLeapYear(year) ? 29 : 28
                default: return 0
            }
        }

        for i in 1..<month {
            days += daysInMonth(year, i)
        }
        for i in 1971..<year {
            days += daysInYear(i)
        }
        return daysOfTheWeek[(4 + days) % 7]
    }
```
## 806. Number of Lines To Write String

```swift
    func numberOfLines(_ widths: [Int], _ S: String) -> [Int] {
        var lines = 1
        var lastLineWidth = 0
        var aAscii = Int(Character("a").asciiValue!)
        
        for char in S {
            let ascii = Int(char.asciiValue!)
            let currIndex = ascii - aAscii
            let curr = widths[currIndex]
            
            if curr + lastLineWidth > 100 {
                lines += 1
                lastLineWidth = curr
            } else {
                lastLineWidth += curr
            }
        }
        
        
        return [lines, lastLineWidth]
    }
```
## 1399. Count Largest Group

```swift
class Solution {
    var memo: [Int: Int] = [:]
    func countLargestGroup(_ n: Int) -> Int {
        var dict: [Int: [Int]] = [:]
        var max = 0
        var count = 0
        
        for num in 1...n {
            let sum = getSum(num)
            
            dict[sum, default: []].append(num)
            guard let val = dict[sum] else { continue }
            
            if max < val.count {
                max = val.count
                count = 1
            } else if val.count == max {
                count += 1
            }
        }
        
        return count
    }
    
    func getSum(_ num: Int) -> Int {
        if let result = memo[num] {
            return result
        }
        
        var sum = 0
        var num = num
        
        while num > 0 {
            let digit = num % 10
            sum += digit
            num = num / 10
        }
        
        memo[num] = sum
        
        return sum
    }
}
```
## 766. Toeplitz Matrix

```swift
    func isToeplitzMatrix(_ matrix: [[Int]]) -> Bool {
        let m = matrix.count
        let n = matrix[0].count
        var preRow: ArraySlice<Int> = matrix[0][0..<n - 1]  
        
        for row in 1..<m {
            if preRow == matrix[row][1..<n] {
                preRow = matrix[row][0..<n - 1]
            } else {
                return false
            }
        }
        
        return true
    }
```
```swift
    func isToeplitzMatrix(_ matrix: [[Int]]) -> Bool {
        for col in 1..<matrix[0].count {
            for row in 0..<matrix.count - 1 {
                if matrix[row][col - 1] != matrix[row + 1][col] { return false }
            }
        }
        return true
    }
```
```swift
    func isToeplitzMatrix(_ matrix: [[Int]]) -> Bool {
        let m = matrix.count
        let n = matrix[0].count
        
        for col in 0..<n {
            if !getVal(col, 0, m, n, matrix) {
                return false
            }
        }
        
        for row in 0..<m {            
            if !getVal(0, row, m, n, matrix) {
                return false
            }
        }
        
        return true
    }
    
    func getVal(_ currCol: Int, _ currRow: Int, _ m: Int, _ n: Int, _ matrix: [[Int]]) -> Bool {
        var preVal: Int? = nil
        var currCol = currCol
        var currRow = currRow
        
        while currRow < m && currCol < n {
            if preVal == nil {
                preVal = matrix[currRow][currCol]
            } else {
                if let pre = preVal, pre != matrix[currRow][currCol] { return false }
            }
            currRow += 1            
            currCol += 1
        }
        
        return true
    }
```
## 500. Keyboard Row

```swift
    func findWords(_ words: [String]) -> [String] {
        var keyboard = [
            Set(Array("qwertyuiop")),
            Set(Array("asdfghjkl")),
            Set(Array("zxcvbnm"))  ]
        var result: [String] = []
        
        for row in keyboard {
            for word in words where row.contains(String(word).lowercased().first!) {
                for (index, char) in word.lowercased().enumerated() {
                    if index == 0 {
                        if word.count == 1 && row.contains(char) {
                            result.append(word)    
                        }
                    } else if index == word.count - 1 && row.contains(char) {
                        result.append(word)
                    } else if !row.contains(char) {
                        break
                    }
                }
            }
        }
        
        return result
    }
```
## 1217. Play with Chips

```swift
    func minCostToMoveChips(_ chips: [Int]) -> Int {
        var even = 0
        var odd = 0
        
        for num in chips {
            if num % 2 == 0 {
                even += 1
            } else {
                odd += 1
            }
        } 
        
        return min(even, odd)
    }
```
## 463. Island Perimeter

```swift
    func islandPerimeter(_ grid: [[Int]]) -> Int {
        var perimeter = 0
        for (row, rows) in grid.enumerated() {
            for (cell, cellVal) in rows.enumerated() where cellVal == 1 {
                
                if row == 0 || grid[row - 1][cell] == 0 { perimeter += 1 }

                if row == grid.count - 1 || grid[row + 1][cell] == 0 { perimeter += 1 }

                if cell == 0 || grid[row][cell - 1] == 0 { perimeter += 1 }

                if cell == rows.count - 1 || grid[row][cell + 1] == 0 { perimeter += 1 }
            }
        }
        return perimeter
    }
```
## 784. Letter Case Permutation

```swift
    func letterCasePermutation(_ S: String) -> [String] {
        var result: [String] = []
        var arrStr = Array(S)
        
        func dfs(_ curr: [String], _ i: Int) {
            guard i < arrStr.count else {
                result.append(curr.joined())
                return
            }
            var curr = curr
            let str = String(arrStr[i])
            
            if Int(str) == nil {
                curr.append(str.lowercased())
                dfs(curr, i + 1)
                curr.removeLast()
                
                curr.append(str.uppercased())
                dfs(curr, i + 1)
                curr.removeLast()
            } else {
                curr.append(str)
                dfs(curr, i + 1)
                curr.removeLast()
            }
        }
        dfs([], 0)
        return result
    }
```
## 867. Transpose Matrix

```swift
    func transpose(_ A: [[Int]]) -> [[Int]] {
        var result: [[Int]] = Array(repeating: Array(repeating: 0, count: A.count), count: A[0].count)
        
        for (row, rows) in A.enumerated() {
            for (col, num) in rows.enumerated() {
                result[col][row] = num
            }
        }
        
        return result
    }
```
## 682. Baseball Game

```swift
    func calPoints(_ ops: [String]) -> Int {
        var stack: [Int] = [0, 0]
        var sum = 0
        
        for action in ops {
            if action == "C" {
                let val = stack.removeLast()
                sum -= val
            } else if action == "D" {
                let val = stack[stack.count - 1] * 2
                stack.append(val)
                sum += val
            } else if let val = Int(action) {
                stack.append(val)
                sum += val
            } else if action == "+" {
                let first = stack[stack.count - 1]
                let second = stack[stack.count - 2]
                let val = first + second
                stack.append(val)
                sum += val
            }
        }
        return sum
    }
```
## 496. Next Greater Element I

```swift
    func nextGreaterElement(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var dict: [Int: Int] = [:]
        var stack: [Int] = []
        var result = Array(repeating: 0, count: nums1.count)
        
        for num in nums2 {
            while !stack.isEmpty && stack[stack.count - 1] < num {
                let els = stack.removeLast()
                dict[els] = num
            }
            
            stack.append(num)
        }
        
        for (index, num) in nums1.enumerated() {
            result[index] = dict[num] ?? -1
        }
        
        return result
    }
```
## 226. Invert Binary Tree

```swift
     func invertTree(_ root: TreeNode?) -> TreeNode? {
        func dfs(_ root: TreeNode?) -> TreeNode? {
            guard let node = root else { return nil }
            
            let leftVal = root?.left
            root?.left = root?.right
            root?.right = leftVal
            
            dfs(root?.left)
            dfs(root?.right)
            
            return root
        }
        
        return dfs(root)
    }
```
```swift
    func invertTree(_ root: TreeNode?) -> TreeNode? {
        var stack = [root]
        
        while !stack.isEmpty {
            let node = stack.removeLast()
            
            if node == nil { continue }
            
            let temp = node?.left
            node?.left = node?.right
            node?.right = temp
            
            stack.append(node?.left)
            stack.append(node?.right)
        }
        
        return root
    }
```
## 824. Goat Latin

```swift
    func toGoatLatin(_ S: String) -> String {
        var words = S.split(separator: " ")
        var result: [String] = []
        var vowel: Set<String> = ["a", "e", "i", "o", "u"]
        
        for (index, word) in words.enumerated() {
            var charArr = word.map { String($0) }
            
            if !vowel.contains(charArr[0].lowercased()) {
                charArr.append(charArr[0])
                charArr[0] = ""
            }
            
            charArr.append("ma")
            charArr.append(String(repeating: "a", count: index + 1))
            result.append(charArr.joined())
        }
        
        return result.joined(separator: " ")
    }
```
## 884. Uncommon Words from Two Sentences

```swift
    func uncommonFromSentences(_ A: String, _ B: String) -> [String] {
        var arrA = A.split(separator: " ").map { String($0) }
        var arrB = B.split(separator: " ").map { String($0) }
        var dict: [String: Int] = [:]
        var result: [String] = []
        
        for word in arrA {
            dict[word, default: 0] += 1
        }
        
        for word in arrB {
            dict[word, default: 0] += 1
        }
        
        for (key, val) in dict where val < 2{
            result.append(key)
        }
        
        return result
    }
```
## 669. Trim a Binary Search Tree

```swift
    func trimBST(_ root: TreeNode?, _ L: Int, _ R: Int) -> TreeNode? {
        guard let node = root else { return nil }
        if node.val > R { return trimBST(node.left, L, R) }
        if node.val < L { return trimBST(node.right, L, R) }
        
        node.left = trimBST(node.left, L, R)
        node.right = trimBST(node.right, L, R)
        return node
    }
```
## 762. Prime Number of Set Bits in Binary Representation

```swift
    func countPrimeSetBits(_ L: Int, _ R: Int) -> Int {
        var count = 0
        
        for num in L...R {
            let countOfSetBits = getCount(num)
            
            if isPrime(countOfSetBits) {
                count += 1
            }
        }
        
        return count
    }
    
    func getCount(_ num: Int) -> Int {
        var count = 0
        var num = num
        
        while num > 0 {
            num &= num - 1
            count += 1
        }
        
        return count
    }
    
    func isPrime(_ num: Int) -> Bool {
        if num == 1 { return false }
        var sq = Int(sqrt(Double(num)))
        for n in 1...sq where num % n == 0 && n > 1 {
            return false
        }
        
        return true
    }
```
## 1046. Last Stone Weight

```swift
class Solution {
    func lastStoneWeight(_ stones: [Int]) -> Int {
        let heap = Heap(stones)
        
        while heap.count > 1 {
            let stone1 = heap.remove()! 
            let stone2 = heap.remove()!
            
            if stone1 != stone2 {
                heap.insert(stone1 - stone2)
            }
        }
        
        if let result = heap.remove() {
            return result
        } else {
            return 0
        }
    }
}

class Heap<T: Comparable> {
    var elements: [T]
    var count: Int {
      return self.elements.count
    }
    
    init(_ elements: [T]) {
        self.elements = elements
        self.heapify()
    }
}

extension Heap {
    func heapify() {
        for i in (0...(self.elements.count - 1) / 2 + 1).reversed() {
            siftDown(i)
        }
    }
    
    func insert(_ element: T) {
        self.elements.append(element)
        siftUp()
    }
    
    func remove() -> T? {
        if self.elements.isEmpty { return nil }
        
        let temp = elements[0]
        self.elements[0] = self.elements[self.elements.count - 1]
        self.elements[self.elements.count - 1] = temp
        let result = elements.removeLast()
        siftDown(0)
        
        return result
    }
    
    func siftUp() {
        var child = self.elements.count - 1
        print(child)
        var parent = parentIndex(child)
        while child > 0 && self.elements[child] > self.elements[parent] {
            let temp = self.elements[child]
            self.elements[child] = self.elements[parent]
            self.elements[parent] = temp
            
            child = parent
            parent = self.parentIndex(child)
        }
    }
    
    func siftDown(_ index: Int) {
        var parent = index
        
        while true {
            let left = leftChildIndex(parent)
            let right = rightChildIndex(parent)
            var candidate = parent
        
            if self.elements.count > left, self.elements[candidate] < self.elements[left] { candidate = left }
        
            if self.elements.count > right, self.elements[candidate] < self.elements[right] { candidate = right }
            if candidate == parent { return }
            let temp = self.elements[candidate]
            self.elements[candidate] = self.elements[parent]
            self.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    func size() -> Int {
        return self.elements.count
    }
    
    func leftChildIndex(_ index: Int) -> Int {
        return index * 2 + 1
    }
    
    func rightChildIndex(_ index: Int) -> Int {
        return index * 2 + 2
    }
    
    func parentIndex(_ index: Int) -> Int {
        return (index + 1) / 2
    }
}
```
## 637. Average of Levels in Binary Tree

```swift
    func averageOfLevels(_ root: TreeNode?) -> [Double] {
        var queue = [root]
        var result: [Double] = []
        
        while !queue.isEmpty {
            let size = Double(queue.count)
            var sum: Double = 0
            
            for num in 0..<Int(size) {
                let node = queue.removeFirst()
                sum += Double(node!.val)
                
                if node?.left != nil { queue.append(node?.left) }
                if node?.right != nil { queue.append(node?.right) }
            }
            result.append(sum / size)
        }
        
        return result
    }
```
## 412. Fizz Buzz

```swift
    func fizzBuzz(_ n: Int) -> [String] {
        var result: [String] = []
        
        for num in 1...n {
            if num % 15 == 0 {
                result.append("FizzBuzz")        
            } else if num % 3 == 0 {
                result.append("Fizz")
            } else if num % 5 == 0 {
                result.append("Buzz")
            } else {
                result.append("\(num)")
            }
        }
        
        return result
    }
```
## 985. Sum of Even Numbers After Queries

```swift
    func sumEvenAfterQueries(_ A: [Int], _ queries: [[Int]]) -> [Int] {
        var A = A
        var evenSum = 0
        var result = [Int]()
        
        for num in A{
            if num % 2 == 0 {
                evenSum += num
            }
        }
        
        for query in queries {
            let val = query[0]
            let index = query[1]
            
            if A[index] % 2 == 0 {
                evenSum -= A[index]
            }
            
            A[index] += val
            
            if A[index] % 2 == 0 {
                evenSum += A[index]
            }
            
            result.append(evenSum)
        }
        
        return result
    }
```
## 266. Palindrome Permutation


```swift
    func canPermutePalindrome(_ s: String) -> Bool {
        var dict: [Character: Int] = [:]
        var diff = 0
        
        for char in s {
            dict[char, default: 0] += 1
        }
        
        for (_, val) in dict {
            if val % 2 != 0 {
                diff += 1
                if diff > 1 {
                    return false
                }
            }
        }
        
        return true
    }
```
## 575. Distribute Candies

```swift
    func distributeCandies(_ candies: [Int]) -> Int {
        return min(candies.count / 2, Set(candies).count)
    }
```
## 206. Reverse Linked List

```swift
//     func reverseList(_ head: ListNode?) -> ListNode? {
//         var current = head
//         var next: ListNode? = nil
//         var previous: ListNode? = nil
        
//         while current != nil {
//             next = current?.next
//             current?.next = previous
//             previous = current
//             current = next
//         }
//         return previous
//     }
    
    func reverseList(_ head: ListNode?) -> ListNode? {
        guard head != nil else { return nil }
        guard head?.next != nil else { return head }

        let node = reverseList(head?.next)

        head?.next?.next = head
        head?.next = nil

        return node
    }
```
## 800. Similar RGB Color


```swift
    func similarRGB(_ color: String) -> String {
        let options = ["00", "11", "22", "33", "44", "55", "66", "77", "88", "99", "aa", "bb", "cc", "dd", "ee", "ff"];
        var result = "#"
        let arr = Array(color)
        
        for count in stride(from: 1, to: arr.count, by: 2) { 
            let hex = String(arr[count..<count + 2])
            var num = Int.max
            var rgb = ""
            
            for combo in options {
                let val = abs(Int(hex, radix: 16)! - Int(combo, radix: 16)!)
                if val < num {
                    num = val
                    rgb = combo
                }
            }
            
            result += rgb
        }
        
        return result
    }
```
## 349. Intersection of Two Arrays


```swift
    func intersection(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var set1 = Set(nums1)
        var set2 = Set(nums2)
        
        return Array(set1.intersection(set2))
    }
```
## 1260. Shift 2D Grid


```swift
    func shiftGrid(_ grid: [[Int]], _ k: Int) -> [[Int]] {
        var rows = grid.count
        var cols = grid[0].count
        var result = Array(repeating: Array(repeating: 0, count: grid[0].count), count: grid.count)
        
        for row in 0..<rows {
            for col in 0..<cols {
                let shiftRow = (row + ((col + k) / cols)) % rows
                let shiftCol = (col + k) % cols
                
                result[shiftRow][shiftCol] = grid[row][col]
            }
        }
        
        return result
    }
```
## 1189. Maximum Number of Balloons

```swift
    func maxNumberOfBalloons(_ text: String) -> Int {
        var dict: [Character: Int] = [:]
        var min = Int.max
        
        for char in text {
            dict[char, default: 0] += 1
        }
        
        for char in "balon" {
            guard let val = dict[char] else { return 0 }
            
            if (char == "l" || char == "o") &&  (val / 2) < min {
                min = val / 2
            } else if val < min {
                min = val
            }
        }
        
        return min
    }
```
## 1099. Two Sum Less Than K


```swfit
    func twoSumLessThanK(_ A: [Int], _ K: Int) -> Int {
        var buckets = Array(repeating: 0, count: 1001)
        var sortedA: [Int] = []
        var maxNum = -1
        var left = 0
        var right = A.count - 1
        
        for num in A {
            buckets[num] += 1
        }
        
        for (num, count) in buckets.enumerated() {
            var count = count
            
            while count > 0 {
                sortedA.append(num)
                count -= 1
            }
        }
        
        while left < right {
            let sum = sortedA[left] + sortedA[right]
            
            if sum < K {
                maxNum = max(maxNum, sum)
                left += 1
            } else {
                right -= 1
            }
        }
        
        return maxNum
    }
```
## 243. Shortest Word Distance

```swift
    func shortestDistance(_ words: [String], _ word1: String, _ word2: String) -> Int {
        var w1Index = -1
        var w2Index = -1
        var minNum = Int.max
        
        for (index, word) in words.enumerated() {
            if word == word1 {
                w1Index = index
            } else if word == word2 {
                w2Index = index
            }
            
            if w1Index != -1 && w2Index != -1 {
                minNum = min(minNum, abs(w1Index - w2Index))
            }
        }
        
        return minNum
    }
```
## 293. Flip Game

```swift
    func generatePossibleNextMoves(_ s: String) -> [String] {
        guard !s.isEmpty else { return []}
        var charArr = s.map { String($0) }
        var result: [String] = [] 
        
        for num in 1..<s.count {
            if charArr[num - 1] == "+" && charArr[num] == "+" {
                charArr[num - 1] = "-"
                charArr[num] = "-"
                result.append(charArr.joined())
                charArr[num - 1] = "+"
                charArr[num] = "+"
            }
        }
        
        return result
    }
```
## 1103. Distribute Candies to People

```swift
    func distributeCandies(_ candies: Int, _ num_people: Int) -> [Int] {
        var result = Array(repeating: 0, count: num_people)
        var candies = candies
        var n = 1
        var i = 0
        
        while candies > 0 {
            result[i % result.count] += min(n, candies)
            candies -= n
            n += 1
            i += 1
        }
        
        return result
    }
```
## 1408. String Matching in an Array

```swift
    func stringMatching(_ words: [String]) -> [String] {
        var output = Set<String>()
        
        for i in 0 ..< words.count {
            let word = words[i]
            for j in i ..< words.count - 1 {
                let str = words[j + 1]                
                if word.contains(str) {
                    output.insert(str)
                } else if str.contains(word) {
                    output.insert(word)
                }
            }
        }
        
        return Array(output)
    }
```
## 566. Reshape the Matrix

```swift
    func matrixReshape(_ nums: [[Int]], _ r: Int, _ c: Int) -> [[Int]] {
        var m = nums.count
        var n  = nums[0].count
        
        if m * n != c * r { return nums }
        
        var matrix = Array(repeating: Array(repeating: 0, count: c), count: r)
        var currRow = 0
        var currCol = 0
        
        for row in 0..<m {
            for col in 0..<n {
                matrix[currRow][currCol] = nums[row][col]
                currCol += 1
                
                if currCol == c {
                    currCol = 0
                    currRow += 1
                }
            }
        }
        
        return matrix
    }
```
## 868. Binary Gap

```swift
    func binaryGap(_ N: Int) -> Int {
        var num = N
        var lastDist = -1
        var maxDist = 0
        var i = 0
        
        while num > 0 {
            if num & 1 == 1 {
                if lastDist != -1 {
                    maxDist = max(maxDist, abs(lastDist - i))   
                }
                lastDist = i
            }
            i += 1
            num >>= 1
        }
        
        return maxDist
    }
```
## 1150. Check If a Number Is Majority Element in a Sorted Array


```swift
    func isMajorityElement(_ nums: [Int], _ target: Int) -> Bool {
        var starting = binarySearch(nums, target)
        
        if starting == -1 { return false }
        
        var ending = starting + nums.count / 2
        
        if ending >= nums.count { return false }
        
        return nums[starting] == nums[ending]
    }
    
    func binarySearch(_ nums: [Int], _ target: Int) -> Int {
        var left = 0
        var right = nums.count - 1
        
        while left < right {
            let mid = ((right - left) / 2) + left
            
            if nums[mid] < target {
                left = mid + 1
            } else {
                right = mid
            }
        }
        
        return nums[left] == target ? left : -1
    }
```
## 1287. Element Appearing More Than 25% In Sorted Array

```swift
    func findSpecialInteger(_ arr: [Int]) -> Int {
        let offset = arr.count / 4
        
        var i = 0
        
        while i < arr.count {
            if arr[i] == arr[i + offset] {
                return arr[i]
            }
            i += 1
            while arr[i] == arr[i - 1] {
                i += 1
            }
        }
        
        return -1
    }
```
## 237. Delete Node in a Linked List

```swift
    func deleteNode(_ node: ListNode?) {
        guard let node = node else { return }
        guard let next = node.next else { return }
        
        node.val = next.val
        node.next = next.next
    }
```
## 693. Binary Number with Alternating Bits

```swift
    func hasAlternatingBits(_ n: Int) -> Bool {
        var pre: Int? = nil
        var n = n
        
        while n > 0 {
            if pre == n & 1 {
                return false
            }
            
            pre = n & 1
            n >>= 1
        }
        
        return true
    }
```
## 1170. Compare Strings by Frequency of the Smallest Character

```swift
    func numSmallerByFrequency(_ queries: [String], _ words: [String]) -> [Int] {
        var buckets = Array(repeating: 0, count: 11)
        var pre = 0
        
        for word in words {
            buckets[f(word)] += 1
        }
        
        for i in (0..<buckets.count).reversed() {
            let temp = buckets[i]
            buckets[i] = pre
            pre += temp
        }
        
        return queries.map { buckets[f($0)] }
    }
    
    func f(_ str: String) -> Int {
        var smallestChar: Character = "z"
        var smallestCharCount = 0
        
        for char in str {
            if char < smallestChar {
                smallestChar = char
                smallestCharCount = 0
            }
            if char == smallestChar {
                smallestCharCount += 1
            }
        }
        
        
        return smallestCharCount
    }
```
## 1426. Counting Elements

```swift
    func countElements(_ arr: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 1001) 
        var count = 0
        
        for a in arr {
            buckets[a] += 1
        }
        
        for i in 0..<buckets.count - 1 {
            if buckets[i + 1] != 0 {
                count += buckets[i]
            }
        }
        
        return count
    }
```
## 1331. Rank Transform of an Array

```swift
    func arrayRankTransform(_ arr: [Int]) -> [Int] {
        let sortedArray = arr.sorted { $0 < $1 }
        var rankMap: [Int: Int] = [:]
        var rank = 1
        
        for a in sortedArray {
            if let map = rankMap[a] { continue }
            
            rankMap[a] = rank
            rank += 1
        }
        
        return arr.map { rankMap[$0, default: 0] }
    }
```
## 1009. Complement of Base 10 Integer

```swift
    func bitwiseComplement(_ N: Int) -> Int {
        guard N != 0 else { return 1 }

        var bits = N 
        var complement = 0
        var shiftIndex = 0

        while bits > 0 {
            let maskedBits = bits & 1

            if maskedBits == 0 { 
                complement += 1 << shiftIndex
            }
            
            shiftIndex += 1
            bits = bits >> 1
        }

        return complement
    }
```
## 892. Surface Area of 3D Shapes

```swift
    func surfaceArea(_ grid: [[Int]]) -> Int {
        var area = 0
        
        for row in 0..<grid.count {
            for col in 0..<grid[0].count {
                if grid[row][col] == 0 { continue }
                
                area += grid[row][col] * 4 + 2
                
                if col - 1 >= 0 {
                    let boarder = min(grid[row][col], grid[row][col - 1])
                    area -= (boarder * 2)
                }
                
                if row - 1 >= 0 {
                    let boarder = min(grid[row][col], grid[row - 1][col])
                    area -= (boarder * 2)
                }
            }
        }
        
        return area
    }
```
## 1332. Remove Palindromic Subsequences

```swift
    func removePalindromeSub(_ s: String) -> Int {
        if s.isEmpty { return 0 }
        return isPalindrome(s) ? 1 : 2
    }
    
    func isPalindrome(_ s:String) ->Bool {
        var i = 0;
        var j = s.count-1
        var arr = Array(s)
        
        while (i < j) {
            if arr[i] != arr[j]{ return false }
            i += 1;
            j -= 1;
        }
        return true;
    }
```
## 812. Largest Triangle Area


```swift
    func largestTriangleArea(_ points: [[Int]]) -> Double {
        var maxSize = 0.0
        for i in 0..<(points.count - 2) {
            for j in (i+1)..<(points.count - 1) {
                for k in (j+1)..<(points.count) {
                    let curSize = getSize(points[i], points[j], points[k])
                    if maxSize < curSize {
                        maxSize = curSize
                    }
                }
            }
        }

        return maxSize
    }

    func getSize(_ p1: [Int], _ p2: [Int],_ p3: [Int]) -> Double {
        let a = Double(p1[0])
        let b = Double(p1[1])
        let c = Double(p2[0])
        let d = Double(p2[1])
        let e = Double(p3[0])
        let f = Double(p3[1])
        let result = Double((a * d + c * f + e * b) - (c * b + e * d + a * f)) / 2.0
        return result >= 0 ? result : -result
    }
```
## 169. Majority Element

```swift
    func majorityElement(_ nums: [Int]) -> Int {
        var major = nums[0]
        var count = 1
        
        for (index, num) in nums.enumerated() where index != 0 {
            if num == major {
                count += 1
            } else {
                count -= 1
            }
            
            if count == 0 {
                major = num
                count += 1
            }
        }
        
        return major
    }
```
## 917. Reverse Only Letters

```swift
    func reverseOnlyLetters(_ S: String) -> String {
        var arr = S.map { String($0) }
        var left = 0
        var right = arr.count - 1
        
        while left < right {
            if isChar(arr[left]) && isChar(arr[right]) {
                let temp = arr[left]
                arr[left] = arr[right]
                arr[right] = temp
                left += 1
                right -= 1
            } else if !isChar(arr[left]) {
                left += 1
            } else {
                right -= 1
            }
        }
        
        return arr.joined()
    }
    
    func isChar(_ char: String) -> Bool {
        if char.lowercased() >= "a" && char.lowercased() <= "z" {
            return true
        }
        return false
    }
```
## 888. Fair Candy Swap

```swift
    func fairCandySwap(_ A: [Int], _ B: [Int]) -> [Int] {
        let sumA = A.reduce(0, +)
        let sumB = B.reduce(0, +)
        let setB = Set<Int>(B)
        let target = (sumA + sumB) / 2
        let delta = target - sumA
        var rlt: [Int] = [0,0]
        for candy in A {
            let b = candy + delta
            if setB.contains(b) {
                rlt[0] = candy
                rlt[1] = b
                return rlt
            }
        }
        return rlt
    }
```
## 976. Largest Perimeter Triangle

```swift
    func largestPerimeter(_ A: [Int]) -> Int {
    
        guard A.count > 2 else { return 0 }
        
        var sortA = A;
        sortA.sort(by: >);
        var triangle = sortA.prefix(3);
        
        if (triangle[1] + triangle[2] > triangle[0]) &&  (abs(triangle[1]-triangle[2]) < triangle[0]){
            return triangle[0] + triangle[1] + triangle[2];
        }
        
        sortA.removeFirst();
        return largestPerimeter(sortA);
    }
```
## 283. Move Zeroes

```swift
    func moveZeroes(_ nums: inout [Int]) {
        var i = 0
        
        
        for j in 1..<nums.count {
            if nums[i] == 0 {
                if nums[j] != 0 {
                    let temp = nums[i]
                    nums[i] = nums[j]
                    nums[j] = temp      
                    i += 1
                }
                continue
            }
            i += 1
        }
    }
```
## 521. Longest Uncommon Subsequence I


```swift
    func findLUSlength(_ a: String, _ b: String) -> Int {
        if a == b { return -1 }
        
        return max(a.count, b.count)
    }
```


```swift
    func isMonotonic(_ A: [Int]) -> Bool {
        guard A.count > 0 else { return false }
        
        return increasing(A) || decreasing(A)
    }
    
    private func increasing(_ nums: [Int]) -> Bool {
        for i in 0..<nums.count - 1 {
            if nums[i] > nums[i + 1] { return false}
        }
        
        return true
    }
    
    private func decreasing(_ nums: [Int]) -> Bool {
        for i in 0..<nums.count - 1 {
            if nums[i] < nums[i + 1] { return false}
        }
        
        return true
    }
```
## 1118. Number of Days in a Month

```swift
    func numberOfDays(_ Y: Int, _ M: Int) -> Int {
        let daysOfMonth = [31, 28, 31, 30,31, 30, 31, 31, 30,31, 30 ,31]
        
        if M == 2 && isLeepYear(Y) {
            return 29
        }
        
        return daysOfMonth[M - 1]
    }
    
    func isLeepYear(_ year: Int) -> Bool {
        if year % 400 == 0 { return true }
        
        if year % 100 != 0 && year % 4 == 0 { return true }
        
        return false
    }
```
## 788. Rotated Digits

```swift
    func rotatedDigits(_ N: Int) -> Int {
        var array = [1, 1, 2, 0, 0, 2, 2, 0, 1, 2] // refers to 0 ... 9
        var result = 0
        for i in 1 ... N {
            var p = i
            var s = 1
            while p != 0 {
                s *= array[p % 10]
                p /= 10
            }
            if s >= 2 {
                result += 1
            }
        }
        return result
    }
```
## 1317. Convert Integer to the Sum of Two No-Zero Integers

```swift
    func getNoZeroIntegers(_ n: Int) -> [Int] {
        for i in 1 ... 999 {
            let pairInt = n - i
            if checkNonZero(pairInt) && checkNonZero(i) {
                return [i, pairInt]
            }
        }
        
        return []
    }
    
    func checkNonZero(_ value: Int) -> Bool {
        var value = value
        
        while value != 0 {
            if value % 10 == 0 {
                return false
            }
            value /= 10
        }
        return true
    }
```
## 690. Employee Importance

```swift
    func getImportance(_ employees: [Employee], _ id: Int) -> Int {
        var dict = buildGraph(employees)
        var sum = 0
        
        func dfs(_ id: Int) {
            guard let emp = dict[id] else { return }
            
            sum += emp.0
            
            for peer in emp.1 {
                dfs(peer)
            }
        }
        
        dfs(id)
        
        return sum
    }
    
    func buildGraph(_ employees: [Employee]) -> [Int: (Int, [Int])] {
        var graph: [Int: (Int, [Int])] = [:]
        
        for employee in employees {
            if graph[employee.id] == nil { graph[employee.id] = (employee.importance, employee.subordinates) }
        }
        
        return graph
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
## 1029. Two City Scheduling

```swift
    func twoCitySchedCost(_ costs: [[Int]]) -> Int {
        let costs = costs.sorted { $0[0] - $0[1] < $1[0] - $1[1] }
        let count = costs.count
        var sum = 0
        
        for i in 0..<count / 2 {
            sum += costs[i][0] + costs[i + count / 2][1]
        }
        return sum
    }
```
## 1029. Two City Scheduling

```swift
    func shortestCompletingWord(_ licensePlate: String, _ words: [String]) -> String {
        var charMap: [String: Int] = [:]
        var result: String? = nil
        
        for char in licensePlate {
            if char.lowercased() >= "a" && char.lowercased() <= "z" {
                charMap[char.lowercased(), default: 0] += 1
            }
        }
        
        
        outer: for word in words {
            var currMap = charMap
            
            for char in word {
                if char.lowercased() >= "a" && char.lowercased() <= "z" {
                    if let val = currMap[char.lowercased()] {
                        currMap[char.lowercased()] = val - 1   
                    }
                }
            }
            
            for (_ , count) in currMap {
                if count > 0 {
                    continue outer
                }
            }
            
            if result == nil {
                result = word
            } else if let preWord = result, word.count < preWord.count {
                result = word
            }
        }
        
        return result ?? ""
    }
```
## 292. Nim Game

```swift
    func canWinNim(_ n: Int) -> Bool {
        return n % 4 != 0
    }
```
## 1089. Duplicate Zeros

```swift
    func duplicateZeros(_ arr: inout [Int]) {
        
        var i = -1
        var j = arr.count - 1
        
        for num in arr {
            if num == 0 { i += 2; continue }
            i += 1
        }
        
        
        while i > j {
            if arr[j] == 0 {
                if i < arr.count { arr[i] = 0 }
                i -= 1
                
                if i < arr.count { arr[i] = 0 }
                i -= 1
                j -= 1
            } else {
                if i < arr.count { arr[i] = arr[j] }
                i -= 1
                j -= 1
            }
        }
    }
```
## 258. Add Digits

```swift
    func addDigits(_ num: Int) -> Int {
        return (num - 1) % 9 + 1
    }
```
## 242. Valid Anagram

```swift
    func isAnagram(_ s: String, _ t: String) -> Bool {
        var dict: [Character: Int] = [:]
        
        for char in s {
            dict[char, default: 0] += 1
        }
        
        for char in t {
            dict[char, default: 0] -= 1
        }
        
        for (_, val) in dict {
            if val != 0 {
                return false
            }
        }
        
        return true
    }
```
## 122. Best Time to Buy and Sell Stock II


```swift
    func maxProfit(_ prices: [Int]) -> Int {
        guard !prices.isEmpty else { return 0 }
        
        var result = 0
        
        for i in 1..<prices.count {
            if prices[i-1] < prices[i] {
                result += prices[i] - prices[i-1]
            }
        }
        return result
    }
```
## 1417. Reformat The String

```swift
    func reformat(_ s: String) -> String {
        var numArr: [String] = []
        var charArr: [String] = []
        var large: [String] = []
        var small: [String] = []
        var result: [String] = []
        var count1 = 0
        var count2 = 0
        
        for char in s {
            var str = String(char)
            if Int(str) != nil {
                numArr.append(str)
            } else {
                charArr.append(str)
            }
        }
        
        if abs(charArr.count - numArr.count) > 1 {
            return ""
        } else if charArr.count > numArr.count {
            large = charArr
            small = numArr
        } else {
            large = numArr
            small = charArr
        }
        
        for i in 0..<s.count {
            if i % 2 == 0 {
                result.append(large[count1])
                count1 += 1
            } else {
                result.append(small[count2])
                count2 += 1
            }
        }
        
        return result.joined()
    }
```
## Is binary search tree

```swift
class Node {
    var val: Int
    var left: Node?
    var right: Node?
    
    init(_ val: Int, _ left: Node? = nil, _ right: Node? = nil) {
        self.val = val
        self.left = left
        self.right = right
    }
}


func isBST(_ root: Node?) -> Bool {
    var result = true
    
    func dfs(_ root: Node?, _ maximum: Int = Int.max, _ minimum: Int = Int.min) {
        guard let node = root else { return }
        
        if !result { return }
        if let left = node.left, (left.val <= minimum || left.val >= node.val) { result = false }
        if let right = node.right, (right.val > maximum || right.val < node.val) { print(node.val, maximum); result = false }
        
        dfs(root?.left, min(node.val, maximum), minimum)
        dfs(root?.right, maximum, max(node.val, minimum))
    }
    
    
    dfs(root)
    return result
}
```
## 485. Max Consecutive Ones

```swift
    func findMaxConsecutiveOnes(_ nums: [Int]) -> Int {
        var count = 0
        var maximum = 0
        
        for num in nums {
            if num == 0 {
                maximum = max(maximum, count)
                count = 0
            } else {
                count += 1
            }
        }
        
        maximum = max(maximum, count)
        
        return maximum
    }
```
## 448. Find All Numbers Disappeared in an Array

```swift
    func findDisappearedNumbers(_ nums: [Int]) -> [Int] {
        var nums = nums
        var result: [Int] = []
        
        for num in nums {
            nums[num - 1] = -1
        }
        
        for i in 0..<nums.count {
            if nums[i] != -1 {
                result.append(i + 1)
            }
        }
        
        return result
    }
```
## 217. Contains Duplicate

```swift
    func containsDuplicate(_ nums: [Int]) -> Bool {
        guard nums.count != nil else { return false }
        guard nums.count > 1 else { return false }
        guard nums.count > 2 else { return nums[0] == nums[1] }
        
        var uniqueNum = Set(nums)
        
        return !(uniqueNum.count == nums.count)
    }
```
## 696. Count Binary Substrings

```swift
    func countBinarySubstrings(_ s: String) -> Int {
        guard s.count > 1 else { return 0 }
        
        let arr = Array(s)
        var prev = 0
        var curr = 1
        var output = 0
        
        for i in 1...arr.count {
            if i < arr.count && arr[i] == arr[i - 1] {
                curr += 1
                continue
            }
            
            output += min(prev, curr)
            prev = curr
            curr = 1
        }
        
        return output
    }
```
## 13. Roman to Integer

```swift
    func romanToInt(_ s: String) -> Int {
        var dict = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]
        var arr = s.map { String($0) }
        var result = 0
        var i = 0
        
        while i < arr.count {
            if i < arr.count - 1, dict[arr[i], default: 0] < dict[arr[i + 1], default: 0] {
                result += dict[arr[i + 1], default: 0] - dict[arr[i], default: 0]
                i += 2
            } else {
                result += dict[arr[i], default: 0]
                i += 1
            }
        }
        
        return result
    }
```
## 953. Verifying an Alien Dictionary

```swift
    func isAlienSorted(_ words: [String], _ order: String) -> Bool {
        var dictionary: [Character: Int] = [:]
        for i in 0..<order.count {
            let char = order[order.index(order.startIndex, offsetBy: i)]
            dictionary[char] = i
        }
        for i in 0..<words.count - 1 {
            let wordA = words[i]
            let wordB = words[i + 1]
            var compaired = false
            for w in 0..<min(wordA.count, wordB.count) {
                let charA = wordA[wordA.index(wordA.startIndex, offsetBy: w)]
                let charB = wordB[wordB.index(wordB.startIndex, offsetBy: w)]
                let mapA = dictionary[charA] ?? -1
                let mapB = dictionary[charB] ?? -1
                if mapA > mapB {
                    return false
                } else if mapA < mapB {
                    compaired = true
                    break
                }
            }
            if !compaired {
                if wordA.count > wordB.count {
                    return false
                }
            }
        }
        return true
    }
```
```swift
    func isAlienSorted(_ words: [String], _ order: String) -> Bool {
        let orderArray = Array(order)
        var orderDict = [Character: Int]()
        
        for i in 0..<order.count {
            orderDict[orderArray[i]] = i
        }
        
        for i in 1..<words.count {
            if compare(orderDict, words[i-1], words[i]) > 0 { return false }
        }
        
        return true
    }
    
    func compare(_ orderDict: [Character: Int],
                _ s1: String, 
                _ s2: String) -> Int {
        let s1Array = Array(s1)
        let s2Array = Array(s2)
        
        var i = 0
        var compare = 0
        
        while i < s1Array.count && i < s2Array.count && compare == 0 {
            compare = orderDict[s1Array[i]]! - orderDict[s2Array[i]]!
            i += 1
        }
        
        if compare == 0 {
            return s1.count - s2.count
        } else { 
            return compare 
        }
    }
```
## 653. Two Sum IV - Input is a BST


```swift
    func findTarget(_ root: TreeNode?, _ k: Int) -> Bool {
        var seen: Set<Int> = []
        
        func dfs(_ root: TreeNode?) -> Bool {
            guard let node = root else { return false }
            
            if seen.contains(k - node.val) {
                return true
            }
            
            seen.insert(node.val)
            return dfs(root?.left) || dfs(root?.right)
        }
        
        return dfs(root)
    }
```
## 1184. Distance Between Bus Stops

```swift
    func distanceBetweenBusStops(_ distance: [Int], _ start: Int, _ destination: Int) -> Int {
        var dist1 = 0
        var dist2 = 0
        
        let stop1 = min(start, destination)
        let stop2 = max(start, destination)
        
        for i in 0..<distance.count {
            var val = distance[i] 
            if i >= stop1 && i < stop2 {
                dist1 += distance[i]   
            } else {
                dist2 += distance[i]    
            }
        }
        
        return min(dist1, dist2)
    }
```
## 733. Flood Fill

```swift
    func floodFill(_ image: [[Int]], _ sr: Int, _ sc: Int, _ newColor: Int) -> [[Int]] {
        var oldColor = image[sr][sc]
        if oldColor == newColor { return image }
        
        var image = image
        var queue = [[sr, sc]]
        var dirs = [[-1, 0], [1, 0], [0, 1], [0, -1]]
        
        
        while !queue.isEmpty {
            let curr = queue.removeFirst()
            var cr = curr[0]
            var cc = curr[1]
            image[cr][cc] = newColor
            
            for arr in dirs {
                var dr = arr[0]
                var dc = arr[1]
                let nr = cr + dr
                let nc = cc + dc
                
                if nr < 0 || nr >= image.count || nc < 0 || nc >= image[0].count || image[nr][nc] != oldColor { continue }
                
                queue.append([nr, nc])
            }
        }
        
        return image
    }
```
## 389. Find the Difference

```swift
    func findTheDifference(_ s: String, _ t: String) -> Character {
        var have: [Character: Int] = [:]
        
        for char in t {
            have[char, default: 0] += 1
        }
        
        for char in s {
            have[char, default: 0] -= 1
        }
        
        for (key, val) in have where val == 1 {
            return key
        }
        
        return Character("")
    }
```
## 1013. Partition Array Into Three Parts With Equal Sum


```swift
    func canThreePartsEqualSum(_ A: [Int]) -> Bool {
        let result = A.reduce(0, +)
        guard result % 3 == 0 else { return false }
        
        let ave = result / 3
        var count = 0
        var sum = 0
        
        for n in A {
            sum += n
            if sum == ave {
                count += 1
                sum = 0
                if count == 3 {
                    return true
                }
            }
        }
        
        return false
    }
```
## 538. Convert BST to Greater Tree

```swift
    func convertBST(_ root: TreeNode?) -> TreeNode? {
        var sum = 0
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            
            dfs(root?.right)
            sum += node.val
            root?.val = sum
            
            
            dfs(root?.left)
        }
        
        dfs(root)
        
        return root
    }
```

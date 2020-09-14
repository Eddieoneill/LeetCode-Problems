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
## 1422. Maximum Score After Splitting a String

```swift
    func maxScore(_ s: String) -> Int {
        let chars = Array(s)
        var result = 0
        var left = 0
        var right = chars.filter { $0 == "1" }.count

        for i in 0..<chars.count - 1 {
            if chars[i] == "0" {
                left += 1
            } else {
                right -= 1
            }
            result = max(result, left + right)
        }
        
        return result
    }
```
## 252. Meeting Rooms

```swift
    func canAttendMeetings(_ intervals: [[Int]]) -> Bool {
        guard !intervals.isEmpty else { return true }
        var sortedList = intervals.sorted { $0[0] < $1[0] }
        let smallest = sortedList[0]
        var start = smallest[0]
        var end = smallest[1]
        
        for (i, meeting) in sortedList.enumerated() where i != 0 {
            let currStart = meeting[0]
            let currEnd = meeting[1]
            if (currStart >= start && currStart < end) { return false }
            if (currEnd > start && currEnd <= end) { return false }
            
            start = currStart
            end = currEnd
        }
        
        return true
    }
```
## 937. Reorder Data in Log Files
### not working
```swift
    func reorderLogFiles(_ logs: [String]) -> [String] {
        var result = logs.sorted { (a, b) -> Bool in
                     let aLog = isDigitLog(a)
                     let bLog = isDigitLog(b)
                     
                     if aLog && bLog {
                         return false
                     } else if !aLog && !bLog {
                         let aWord = makeWord(a)
                         let bWord = makeWord(b)
                         return aWord < bWord 
                     } else if aLog {
                         return false
                     } else {
                         return true
                     }
        }
        
        return result
        
    }
    
    func makeWord(_ str: String) -> String {
        var words = str.split(separator: " ")
        return String(words[1]) + String(words[0])
    }
    
    func isDigitLog(_ log: String) -> Bool {
        var seenSpace = false
        for char in log {
            if seenSpace && Int(String(char)) != nil {
                return true
            } else if String(char) == " " {
                seenSpace = true
            }
        }
        return false
    }
```
### working
```swift
    func reorderLogFiles(_ logs: [String]) -> [String] { 
        guard logs.count != 0 else {
            return []
        }
        var digLogs: [String] = []
        var letterCountLogs: [(identifier: String, value: String)] = []
        for log in logs {
            if log[log.index(before: log.endIndex)].isNumber {
                digLogs.append(log)
            } else {
                let firstSpaceIndex = log.firstIndex(of: " ")!
                let identifier = String(log[..<firstSpaceIndex])
                let str = String(log[log.index(after: firstSpaceIndex )...])
                letterCountLogs.append((identifier: identifier, value: str))
            }
        }
        let letterLogs = letterCountLogs.sorted(by: {
            if $0.value == $1.value {
                return $0.identifier < $1.identifier
            } else {
                return $0.value < $1.value
            }
        }).map({ "\($0.identifier) \($0.value)"})
        return letterLogs + digLogs
    }
```
## 171. Excel Sheet Column Number


```swift
    func titleToNumber(_ s: String) -> Int {
       var columnNumber = 0
       let offset = 64   

       for letter in s.utf8 { 
           columnNumber = columnNumber * 26 + Int(letter) - offset
       }   

       return columnNumber
    } 
```
## 1176. Diet Plan Performance

```swift
    func dietPlanPerformance(_ calories: [Int], _ k: Int, _ lower: Int, _ upper: Int) -> Int {
        var currentCalorieCount = 0
        var points = 0
        
        for i in 0..<calories.count {
            currentCalorieCount = currentCalorieCount + calories[i]
            if i >= k - 1 {
                if currentCalorieCount < lower {
                    points = points - 1
                }
                if currentCalorieCount > upper {
                    points = points + 1
                }
                currentCalorieCount = currentCalorieCount - calories[i - (k - 1)]
            }
        }
        return points
    }
```
## 1071. Greatest Common Divisor of Strings

```swift
    func gcdOfStrings(_ str1: String, _ str2: String) -> String {        
        if str1 == str2 { return str1 }
        let maxStr = str1.count > str2.count ? str1 : str2
        let minStr = str1.count > str2.count ? str2 : str1
        if String(maxStr.prefix(minStr.count)) != minStr { return "" }
        return gcdOfStrings(String(maxStr.suffix(maxStr.count-minStr.count)), minStr)
    }
```
## 1228. Missing Number In Arithmetic Progression


```swift
    func missingNumber(_ arr: [Int]) -> Int {
        var min = Int.max
        var result = 0
        
        for (i, num) in arr.enumerated() where i != arr.count - 1 {
            if arr[i + 1] == num { return num }
            
            var curr = abs(num - arr[i + 1])
            if curr < min {
                min = curr
            }
        }
        
        for (i, num) in arr.enumerated() where i != arr.count - 1 {
            if arr[i + 1] < num {
                let next = num - min    
                if arr[i + 1] != next {
                    result = next
                    return next
                }
            } else {
                let next = num + min    
                if arr[i + 1] != next {
                    result = next
                    return result
                }
            }
        }
        
        return result
    }
```
## 28. Implement strStr()

```swift
    func strStr(_ haystack: String, _ needle: String) -> Int {
        if haystack.count == needle.count {
            return haystack != needle ? -1 : 0
        } else if let i = haystack.range(of: needle) {
            return haystack.distance(from: haystack.startIndex, to: i.lowerBound)
        } else {
            return needle.isEmpty ? 0 : -1
        }
    }
```
## 520. Detect Capital

```swift
    func detectCapitalUse(_ word: String) -> Bool {
        var upper = 0
        
        for char in word {
            if String(char).uppercased() == String(char) { upper += 1 }
        }
        
        if upper == word.count || upper == 0 || (upper == 1 && isUpper(word)) {
            return true
        } else {
            return false
        } 
    }
    
    func isUpper(_ str: String) -> Bool {
        for char in str {
            if String(char).uppercased() == String(char) {
                return true
            } else {
                return false
            }
        }
        
        return false
    }
```
## 387. First Unique Character in a String


```swift
    func firstUniqChar(_ s: String) -> Int {
        var dict: [Character: Int] = [:]
        
        for char in s {
            dict[char, default: 0] += 1
        }
        
        for (i, char) in s.enumerated() {
            if let val = dict[char], val == 1 {
                return i
            }
        }
        
        return -1
    }
```
530. Minimum Absolute Difference in BST

```swift
    func getMinimumDifference(_ root: TreeNode?) -> Int {
        var minimum = Int.max
        var arr: [Int] = []
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            
            dfs(root?.left)
            arr.append(node.val)
            dfs(root?.right)
        }
        
        dfs(root)
        
        for i in 0..<arr.count where i != arr.count - 1 {
            let curr = arr[i]
            let next = arr[i + 1]
            
            if minimum > abs(curr - next) {
                minimum = abs(curr - next)
            }
        }
        
        return minimum
    }
```
## 1427. Perform String Shifts

```swift
    func stringShift(_ s: String, _ shift: [[Int]]) -> String {
        guard !shift.isEmpty else { return s }
        
        var totalShift = 0
        for sh in shift {
            if sh[0] == 0 {
                totalShift -= sh[1]
            } else {
                totalShift += sh[1]
            }
        }
        
        if totalShift == 0 {
            return s
        }
        
        if abs(totalShift) >= s.count {
            totalShift =  totalShift % s.count
        }
        if totalShift < 0 {
            let stratingIndex = s.index(s.startIndex, offsetBy: abs(totalShift))
            var prefix = s.prefix(upTo: stratingIndex)
            var suffix = s.suffix(from: stratingIndex)
            return String(suffix + prefix)
        }
        
        let stratingIndex = s.index(s.startIndex, offsetBy: s.count - abs(totalShift))
        var prefix = s.prefix(upTo: stratingIndex)
        var suffix = s.suffix(from: stratingIndex)
        return String(suffix + prefix)
    }
```
## 21. Merge Two Sorted Lists

```swift
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var l1 = l1
        var l2 = l2
        let dummyNode = ListNode(0)
        var current = dummyNode
        dummyNode.next = current.next
        
        if l1 == nil && l2 == nil { return nil }
        
        while l1 != nil && l2 != nil {
            if l1?.val ?? 0 < l2?.val ?? 0 {
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
```
## 100. Same Tree

```swift
    func isSameTree(_ p: TreeNode?, _ q: TreeNode?) -> Bool {
        if p == nil && q == nil { return true }
        if p == nil || q == nil { return false }
        if p!.val != q!.val { return false }
        
        let left = isSameTree(p?.left, q?.left)
        let right = isSameTree(p?.right, q?.right)
        
        return left && right
    }
```
## 993. Cousins in Binary Tree

```swift
    func isCousins(_ root: TreeNode?, _ x: Int, _ y: Int) -> Bool {
        var xParent: TreeNode? = nil
        var xDepth = -1
        var yParent: TreeNode? = nil
        var yDepth = -1
        
        func dfs(_ root: TreeNode?, _ height: Int, _ parent: TreeNode?) {
            guard let node = root else { return }
            
            if node.val == x {
                xParent = parent
                xDepth = height
                return
            }
            
            if node.val == y {
                yParent = parent
                yDepth = height
                return
            }
            
            dfs(root?.left, height + 1, root)
            dfs(root?.right, height + 1, root)
        }
        
        dfs(root, 0, nil)
        
        if let xP = xParent, let yP = yParent {
            return xP.val != yP.val && xDepth == yDepth   
        } else {
            return false
        }
    }
```
## 256. Paint House

```swift
	func minCost(_ costs: [[Int]]) -> Int {
        if costs.isEmpty { return 0 }
        var lastR = costs[0][0]
        var lastB = costs[0][1]
        var lastG = costs[0][2]
    
        for i in 1..<costs.count{
            let currR = min(lastB, lastG) + costs[i][0]
            let currB = min(lastR, lastG) + costs[i][1]
            let currG = min(lastB, lastR) + costs[i][2]
            
            lastR = currR
            lastB = currB
            lastG = currG
        }
        
        return min(min(lastR, lastB),lastG)
    }
```
## 447. Number of Boomerangs

```swift
    func dist(_ a: (Int, Int), _ b: (Int, Int)) -> Int {
        let dx = a.0 - b.0
        let dy = a.1 - b.1
        return dx * dx + dy * dy 
    }
    
    func numberOfBoomerangs(_ points: [[Int]]) -> Int {
        let ps = points.map { ($0[0], $0[1]) } 
        var ans = 0
        for i in 0..<ps.count {
            let curr = ps[i]
            var dict = [Int: Int]()
            for j in 0..<ps.count {
                if i == j { continue }
                let d: Int = dist(curr, ps[j])
                dict[d] = 1 + (dict[d] ?? 0)
            }
            ans += dict.reduce(0) { res, t in 
                return  res + t.1 * (t.1 - 1)
            }
        }
        return ans 
    }
```
## 1271. Hexspeak


```swift
    func toHexspeak(_ num: String) -> String {
        var hex = String(Int(num) ?? 0, radix: 16, uppercase: true).map { String($0) }
        var set: Set<String> = ["A", "B", "C", "D", "E", "F", "I", "O"]
    
        for (i, char) in hex.enumerated() {
            if char == "1" {
                hex[i] = "I"     
            } else if char == "0" {
                hex[i] = "O"
            }
        }
        
        for char in hex {
            if !set.contains(char) {
                return "ERROR"
            }
        }
        return hex.joined()
    } 
```
## 606. Construct String from Binary Tree

```swift
    func tree2str(_ t: TreeNode?) -> String {
        var result: [String] = []
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            
            result.append("\(node.val)")

            if node.left != nil || node.right != nil {
                result.append("(")
                dfs(root?.left)
                result.append(")")
            }

            if node.right != nil {
                result.append("(")
                dfs(root?.right)
                result.append(")")
            }
        }
        
        dfs(t)
        
        return result.joined()
    }
```
## 697. Degree of an Array

```swift
    func findShortestSubArray(_ nums: [Int]) -> Int {
        var dict: [Int: [Int]] = [:]
        var degree = 0
        var length = Int.max
        
        for (i, num) in nums.enumerated() {
            if dict[num] == nil {
                dict[num] = [0, i, 0]
            }
            
            dict[num, default: [0, 0, 0]][0] += 1
            dict[num, default: [0, 0, 0]][2] = i
                 
            degree = max(degree, dict[num, default: [0, 0, 0]][0])
        }
        
        for (_, val) in dict {
            var count =  val[0]
            var start = val[1]
            var end = val[2]
            
            if count == degree {
                length = min(length, end - start + 1)
            }
        }
        
        return length
    }
```
## 383. Ransom Note

```swift
    func canConstruct(_ ransomNote: String, _ magazine: String) -> Bool {
        var dict: [String: Int] = [:]
        
        for char in magazine {
            let str = String(char)
            dict[str, default: 0] += 1
        }
        
        for char in ransomNote {
            let str = String(char)
            
            dict[str, default: 0] -= 1
            
            if dict[str, default: 0] < 0 {
                return false
            }
            
        }
        
        return true
    }
```
## 860. Lemonade Change

```swift
    func lemonadeChange(_ bills: [Int]) -> Bool {
        var five = 0
        var ten = 0
        
        for bill in bills {
            if bill == 5 {
                five += 1
            } else if bill == 10 {
                five -= 1
                ten += 1
            } else if bill == 20 && ten > 0 {
                ten -= 1
                five -= 1
            } else {
                five -= 3
            }
            
            if five < 0 { return false }
        }
        return true
    }
```
## 704. Binary Search


```swift
    func search(_ nums: [Int], _ target: Int) -> Int {
        var left = 0
        var right = nums.count - 1

        while left <= right {

            let middle = ((right - left) / 2) + left

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
    
    func search(_ nums: [Int], _ target: Int) -> Int {
        func bst(_ left: Int, _ right: Int) -> Int {
            if left > right { return -1}
            
            let middle = ((right - left) / 2) + left
            
            if nums[middle] == target {
                return middle
            } else if nums[middle] < target {
                return bst(middle + 1, right)
            } else {
                return bst(left, middle - 1)
            }
            
        } 
        return bst(0, nums.count - 1)
    }
```
## 118. Pascal's Triangle

```swift
    func generate(_ numRows: Int) -> [[Int]] {
        guard numRows > 0 else { return []}
        guard numRows > 1 else { return [[1]] }
        guard numRows > 2 else { return [[1], [1, 1]] }
        
        var result = [[1], [1, 1]]
        
        for row in 2..<numRows {
            var nextRow: [Int] = [1]
            var preRow = result[row - 1]
            
            for i in 0..<preRow.count - 1 {
                let val = preRow[i] + preRow[i + 1]
                nextRow.append(val)
            }
            
            nextRow.append(1)
            result.append(nextRow)
        }
        
        return result
    }
```
## 1005. Maximize Sum Of Array After K Negations


```swift
    func largestSumAfterKNegations(_ A: [Int], _ K: Int) -> Int {
        var buckets = Array(repeating: 0, count: 201)
        var sortedArr: [Int] = []
        var i = 0
        var k = K
        
        for num in A {
            buckets[num + 100] += 1
        }
        
        for (num, count) in buckets.enumerated() where count > 0 {
            var count = count 
            
            while count > 0 {
                sortedArr.append(num - 100)
                count -= 1
            }
        }
        
        while sortedArr[i] < 0 && k > 0 {
            sortedArr[i] = abs(sortedArr[i])
            k -= 1
            i += 1
        }
        
        print(sortedArr, k)
        if k > 0 && k % 2 != 0 {
            var pre = sortedArr[0]
            for (index, num) in sortedArr.enumerated() where index != 0 {
                if num > pre {
                    sortedArr[index - 1] = -pre
                    break
                }
                pre = num
            }
        }
        
        return sortedArr.reduce(0, +)
    }
```
## 661. Image Smoother

```swift
    func imageSmoother(_ M: [[Int]]) -> [[Int]] {
        var M = M
        var newM = M
        var m = M.count
        var n = M[0].count
        
        if m == 0 && n == 0 {
            return [[]]
        }
        
        var calculators = [[0,1],[0,-1],[1,0],[-1,0],[-1,-1],[1,1],[-1,1],[1,-1]] 
        
        for i in 0 ..< m {
            for j in 0 ..< n {
                var sum = M[i][j]
                var count = 1
                for k in 0 ..< calculators.count {
                    var x = i + calculators[k][0]
                    var y = j + calculators[k][1]
                    if x < 0 || x > m - 1 || y < 0 || y > n - 1 {
                        continue
                    }
                    sum += M[x][y]
                    count += 1
                }
                newM[i][j] = sum / count
            }
        }
        return newM
    }
```
## 268. Missing Number

```swift
    func missingNumber(_ nums: [Int]) -> Int {
        guard nums.count > 0 else { return 0 }
        
        var sum = 0
        
        for i in 1...nums.count { sum += i }
        
        for num in nums { sum -= num }
        
        return sum
    }
```
## 107. Binary Tree Level Order Traversal II

```swift
    func levelOrderBottom(_ root: TreeNode?) -> [[Int]] {
        guard root != nil else { return [] }
        var result: [[Int]] = []
        
        func dfs(_ root: TreeNode?, _ height: Int) {
            guard let node = root else { return }
            
            dfs(root?.left, height + 1)
            dfs(root?.right, height + 1)
            
            if height >= result.count { 
                for _ in 0..<height - result.count + 1 {
                    result.append([])
                } 
            }
            result[height].append(node.val)    
            
        }
        
        dfs(root, 0)
        return result.reversed()
    }
```
## 350. Intersection of Two Arrays II

```swift
    func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var nums1 = nums1.sorted{ $0 < $1 }
        var nums2 = nums2.sorted{ $0 < $1 }
        var i = 0
        var j = 0
        var result: [Int] = []
        
        while i < nums1.count && j < nums2.count {
            if nums1[i] == nums2[j] { 
                result.append(nums1[i]) 
                i += 1
                j += 1
            } else if nums1[i] > nums2[j] {
                j += 1
            } else {
                i += 1
            }
        }
        return result  
    }
```

```swift
    func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var dict: [Int : Int] = [:]
        var sm: [Int] = []
        var bg: [Int] = []
        var result: [Int] = []
        
        if nums1.count > nums2.count {
            bg = nums1
            sm = nums2
        } else {
            bg = nums2
            sm = nums1
        }
        
        for num in bg {
            dict[num, default: 0] += 1
        }
        
        for num in sm {
            if let val = dict[num], val > 0 {
                result.append(num)
                dict[num, default: 0] -= 1
            }
        }
        
        return result
    }
```
## 404. Sum of Left Leaves

```swift
    func sumOfLeftLeaves(_ root: TreeNode?, _ isLeft: Bool = false) -> Int {
        guard let node = root else { return 0 }
            
        if root?.left == nil && root?.right == nil && isLeft { return node.val }
            
        return sumOfLeftLeaves(root?.left, true) + sumOfLeftLeaves(root?.right, false)
    }
```
## 997. Find the Town Judge

```swift
    func findJudge(_ N: Int, _ trust: [[Int]]) -> Int {
        if N == 1 && trust.isEmpty { return 1 }
        var dict: [Int: (Int, Int)] = [:]
        var result = -1
        
        for val in trust {
            let me = val[0]
            let out = val[1]
            
            dict[me, default: (0, 0)].0 += 1
            dict[out, default: (0, 0)].1 += 1
        }
        
        for (key, val) in dict {
            if val.0 == 0 && val.1 == N  - 1 {
                result = key
                break
            }
        }
        
        return result
    }
```
## 1175. Prime Arrangements

```swift
class Solution {
    let modulos = 1_000_000_007
    func numPrimeArrangements(_ n: Int) -> Int {
        let primesCount = getPrime(n)
        
        return factorial(primesCount) * factorial(n - primesCount) % modulos
    }
    
    func factorial(_ n: Int) -> Int {
        if n < 1 {
            return 1
        }
        var result: Int = 1
        for i in 1...n {
            result = (result * i) % modulos
        }
        return result
    }
    
    func getPrime(_ n: Int) -> Int {
        var primes: [Int] = [2]
        var i: Int = 3
        ext: while i <= n {
            for x in primes {
                if x * x > i {
                    break
                }
                if i % x == 0 {
                    i += 2
                    continue ext
                }
            }
            primes.append(i)
            i += 2
        }
        return n < 2 ? 0 : primes.count
    }
}
```
## 599. Minimum Index Sum of Two Lists

```swift
    func findRestaurant(_ list1: [String], _ list2: [String]) -> [String] {
        var dict: [String: Int] = [:]
        var mini = Int.max
        var result: [String] = []
        
        for (i, str) in list1.enumerated() {
            dict[str] = i
        }
        
        for (i, str) in list2.enumerated() {
            if let val = dict[str] {
                if i + val == mini {
                    result.append(list2[i])
                }
                
                if i + val < mini {
                    mini = i + val
                    result = [list2[i]]
                }
            }
        }
        
        return result
    }
```
## 1056. Confusing Number

```swift
    func confusingNumber(_ N: Int) -> Bool {
        var valid: [Int: Int] = [0: 0, 1: 1, 6: 9, 8: 8, 9: 6]
        var num = N
        var result = 0
        
        while num > 0 {
            let digit = num % 10
            
            if let val = valid[digit] {
                result *= 10
                result += val
                num /= 10
            } else {
                return false
            }
        }
        
        return result != N
    }
```
## 506. Relative Ranks

```swift
    func findRelativeRanks(_ nums: [Int]) -> [String] {
        var sortedNums = nums.sorted { $0 > $1 }
        var dict: [Int: String] = [:]
        var result = [String]()
        
        for (i, num) in sortedNums.enumerated() {
            if i == 0 {
                dict[num] = "Gold Medal"
            } else if i == 1 {
                dict[num] = "Silver Medal"
            } else if i == 2 {
                dict[num] = "Bronze Medal"
            } else {
                dict[num] = "\(i + 1)"
            }
        }
        
        for num in nums {
            result.append(dict[num]!)
        }
        return result
    }
```
## 202. Happy Number

```swift
class Solution {
    var memo: [Int: Bool] = [:]
    func isHappy(_ n: Int) -> Bool {
        var seen: Set<Int> = []
        func _isHappy(_ n: Int) -> Bool {
            if n == 1 { return true }
            if seen.contains(n) { return false }
            if let val = memo[n] { return val }
            
            var num = n
            var nextNum = 0
            
            seen.insert(n)
            
            while num > 0 {
                let digit = num % 10
                nextNum += digit * digit
                num = num / 10
            }
            
            memo[n] = _isHappy(nextNum)
            return memo[n, default: false]
        }
        
        return _isHappy(n)
    }
}
```
## 257. Binary Tree Paths

```swift
    func binaryTreePaths(_ root: TreeNode?) -> [String] {
        var result: [String] = []
        
        func dfs(_ root: TreeNode?, _ path: [String]) {
            var path = path
            guard let node = root else { return }
            
            path.append("\(node.val)")
            
            if root?.left == nil && root?.right == nil {
                result.append(path.joined(separator: "->"))
            }
            
            dfs(root?.left, path)
            dfs(root?.right, path)
        }
        dfs(root, [])
        print(result)
        
        return result
    }
```
## 409. Longest Palindrome

```swift
    func longestPalindrome(_ s: String) -> Int {
        var dict: [String: Int] = [:]
        var count = 0
        var hasOne = false
        
        for char in s {
            dict[String(char), default: 0] += 1
        }
        
        for (key, val) in dict {
            count += (val / 2) * 2
            if hasOne == false && val % 2 != 0 {
                hasOne = true
                count += 1
            }
        }
        
        return count
    }
```
## 453. Minimum Moves to Equal Array Elements

```swift
    func minMoves(_ nums: [Int]) -> Int {
        let minNum = nums.min()!
        var result = nums.map { $0 - minNum; }
        return result.reduce(0, +)
    }
```
## 796. Rotate String

```swift
    func rotateString(_ A: String, _ B: String) -> Bool {
        if A.count != B.count { return false }
        if A.isEmpty && B.isEmpty { return true }
        var con = B + B
        
        return con.contains(A)
    }
```
## 746. Min Cost Climbing Stairs


```swift
    func minCostClimbingStairs(_ cost: [Int]) -> Int {
        var path1 = cost[0]
        var path2 = cost[1]
        
        for i in 2..<cost.count {
            let curr = min(path1, path2) + cost[i]
            path1 = path2
            path2 = curr
        }
        
        return min(path1, path2)
    }
```
## 455. Assign Cookies

```swift
    func findContentChildren(_ g: [Int], _ s: [Int]) -> Int {
        var g = g.sorted { $0 < $1 }
        var s = s.sorted { $0 < $1 }
        var j = 0
        var count = 0
        print(g, s)
        for i in 0..<g.count where j < s.count {
            let greed = g[i]
            let size = s[j]
            
            while s[j] < greed { 
                j += 1 
                if j >= s.count { return count }
            }
            
            count += 1
            j += 1
        }
        
        return count
    }
```
## 1512. Number of Good Pairs

```swift
    func numIdenticalPairs(_ nums: [Int]) -> Int {
        var buckets = Array(repeating: 0, count: 101)
        var result = 0
        
        for num in nums {
            buckets[num] += 1
        }
        
        for (num, count) in buckets.enumerated() where count > 1 {
            result += (count * (count - 1)) / 2
        }
        
        return result
    }
```
## 1507. Reformat Date

```swift
    func reformatDate(_ date: String) -> String {
        var date = date.split(separator: " ")
        var day: [String] = []
        var month = ["Jan": "01", "Feb": "02", "Mar": "03", "Apr": "04", "May": "05", "Jun": "06", "Jul": "07", "Aug": "08", "Sep": "09", "Oct": "10", "Nov": "11", "Dec": "12"]
        
        for char in date[0] {
            let str = String(char)
            if let _ = Int(str) {
                day.append(str)
            }
        }
        
        if day.count == 1 {
            day = ["0", day[0]]
        }
        
        return "\(date[2])-\(month[String(date[1]), default: "01"])-\(day.joined())"
    }
```
## 598. Range Addition II


```swift
    func maxCount(_ m: Int, _ n: Int, _ ops: [[Int]]) -> Int {
        var minRow = m
        var minCol = n
        
        for arr in ops {
            minRow = min(minRow, arr[0])
            minCol = min(minCol, arr[1])
        }
        
        return minRow * minCol
    }
```
## 492. Construct the Rectangle

```swift
    func constructRectangle(_ area: Int) -> [Int] {
        let sqrt = Int(Float(area).squareRoot())
        
        for i in (1...sqrt).reversed() {
            if area % i == 0 { return [area / i, i] }
        }
        
        return []
    }
```
## 1360. Number of Days Between Two Dates

```swift
    func daysBetweenDates(_ date1: String, _ date2: String) -> Int {
        let date1 = daysSince1970(date1)
        let date2 = daysSince1970(date2)
        return abs(date1 - date2)
    }
    
    func daysSince1970(_ date: String) -> Int {
        let date = date.split(separator: "-")
        guard let year = Int(date[0]), let month = Int(date[1]), let day = Int(date[2]) else { return 0}
        var countYears = 0
        var countMonths = 0
        
        for index in 1970..<year {
            if index % 4 == 0 {
                countYears += 366
            } else {
                countYears += 365
            }
        }
        
        for index in 1..<month {
            countMonths += daysInMonth(index, year: year)
        }
        
        return countYears + countMonths + day
    }
    
    func daysInMonth(_ month: Int, year: Int) -> Int {
        switch month {
            case 1, 3, 5, 7, 8, 10, 12:
                return 31
            case 4, 6, 9, 11:
                return 30
            case 2:
                if isLeepYear(year) {
                    return 29
                }
                return 28
            default:
                return 0
        }
    }
    
    func isLeepYear(_ year: Int) -> Bool {
        return (year % 4 == 0 && year % 100 != 0) || year % 400 == 0 
    }
```
## 717. 1-bit and 2-bit Characters

```swift
    func isOneBitCharacter(_ bits: [Int]) -> Bool {
        if bits.count == 1 { return true }
        var i = 0
        
        while i < bits.count {
            if bits[i] == 1 {
                i += 2
                continue
            } else if i == bits.count - 1 {
                return true
            } else {
                i += 1
            }
        }
        
        return false
    }
```
## 830. Positions of Large Groups

```swift
    func largeGroupPositions(_ S: String) -> [[Int]] {
        var start = 0
        var i = 1
        var s = S.map { String($0) }
        var result: [[Int]] = []
        
        while i < s.count{
            if s[i - 1] == s[i] { 
                i += 1
                continue
            }
            
            if i - start >= 3 {
                result.append([start, i - 1])
            }
            
            start = i
            i += 1
        }
        
        if i - start >= 3 {
            result.append([start, i - 1])
        }
        
        return result
    }
```
## 836. Rectangle Overlap

```swift
    func isRectangleOverlap(_ rec1: [Int], _ rec2: [Int]) -> Bool {
        return !(rec1[2] <= rec2[0] || rec1[3] <= rec2[1] || rec1[0] >= rec2[2] || rec1[1] >= rec2[3])
    }
```
## 235. Lowest Common Ancestor of a Binary Search Tree

```swift
    func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
        var result: TreeNode? = nil
        let small = min(p!.val, q!.val)
        let big = max(p!.val, q!.val) 
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            guard result == nil else { return }
            
            if small <= node.val && big >= node.val { result = node; return }
            
            if small < node.val && big < node.val { dfs(root?.left)}
            
            if small >= node.val && big >= node.val { dfs(root?.right)}
        }
        
        dfs(root)
        
        return result
    }
```
## 191. Number of 1 Bits

```swift
    func hammingWeight(_ n: Int) -> Int {
        var count = 0
        var n = n
        
        while n > 0 {
            count += 1
            n &= (n - 1)
        }
        
        return count
    }
```
## 232. Implement Queue using Stacks

```swift
class MyQueue {
    var stack1: [Int]
    var stack2: [Int]
    
    init() {
        stack1 = []
        stack2 = []
    }
    
    func push(_ x: Int) {
        stack1.append(x)
    }
    
    func pop() -> Int {
        if stack2.isEmpty {
            while !stack1.isEmpty {
                stack2.append(stack1.removeLast())
            }
        }
        
        return stack2.removeLast()
    }
    
    func peek() -> Int {
        if stack2.isEmpty {
            return stack1[0]
        } else {
            return stack2[stack2.count - 1]
        }
    }
    
    func empty() -> Bool {
        return stack1.isEmpty && stack2.isEmpty
    }
}
```
## 1042. Flower Planting With No Adjacent


```swift
    func gardenNoAdj(_ N: Int, _ paths: [[Int]]) -> [Int] {
        if paths.count == 0 { return Array(repeating:1, count: N) }
        var graph = [Int: Set<Int>]()
        var result = Array(repeating: 0, count: N)
        
        for path in paths {
            graph[path[0], default: []].insert(path[1])
            graph[path[1], default: []].insert(path[0])
        }
        
        for i in 0..<N {
            var takenColor = Array(repeating: 0, count: 5)
            var neighbors = Set<Int>()
            
            if graph[i+1] != nil { neighbors = graph[i+1 ,default: []] }
            
            for neighbor in neighbors {
                takenColor[result[neighbor-1]] = 1
            }
            
            for j in 1...4 {
                if takenColor[j] == 0 { result[i] = j }
            }
            
        }
        
        return result
    }
```
## 1154. Day of the Year

```swift
    func dayOfYear(_ date: String) -> Int {
        var day = 0
        var arrDate: [String] = []
        var word = ""
        
        for char in date {
            if char == "-" {
                arrDate.append(word)
                word = ""
            } else {
                word += String(char)
            }
        }
        
        arrDate.append(word)
        
        let dateMonth = Int(arrDate[1]) ?? 1
        let dateDay = Int(arrDate[2]) ?? 1
        let dateYear = Int(arrDate[0]) ?? 1
        let months = [31,28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
        
        for month in 1..<dateMonth {
            
            if month == 2 && isLeapYear(dateYear) {
                day += months[month - 1] + 1
            } else {
                day += months[month - 1]
            }
        }
        
        return day + dateDay
    }
    
    func isLeapYear(_ year: Int) -> Bool {
        return (year % 4 == 0 && year % 100 != 0) || year % 400 == 0
    }
```
## 1232. Check If It Is a Straight Line

```swift
    func checkStraightLine(_ coordinates: [[Int]]) -> Bool {
        if coordinates.count <= 2 { return true }
        
        for i in 1 ..<coordinates.count - 1 {
            let x1 = coordinates[i - 1][0]
            let y1 = coordinates[i - 1][1]
            let x2 = coordinates[i][0]
            let y2 = coordinates[i][1]
            let x3 = coordinates[i + 1][0]
            let y3 = coordinates[i + 1][1]
            if (y2 - y1) * (x3 - x2) != (y3 - y2) * (x2 - x1) { return false }
        }
        
        return true
    }
```
## 392. Is Subsequence

```swift
    func isSubsequence(_ s: String, _ t: String) -> Bool {
        guard !s.isEmpty else { return true }
        let s = s.map { String($0) }
        var i = 0
        
        for char in t {
            let str = String(char)
            if str == s[i] { i += 1 }
            if i >= s.count { return true }
        }
        
        return false
    }
```
## 392. Is Subsequence

```swift
 class Solution {
    var sourceMax = 0
    var targetMax = 0
    var s: [Character] = []
    var t: [Character] = []
    func isSubsequence(_ s: String, _ t: String) -> Bool {
        return _isSubsequence(s, t)
        if s.isEmpty { return true }
        if t.isEmpty { return false }
        
        self.s = s.map { $0 }
        self.t = t.map { $0 }
        self.sourceMax = s.count
        self.targetMax = t.count
        return isS(l: 0, r: 0)
    }
    
    func isS(l: Int, r: Int) -> Bool {
        if l == sourceMax { return true } 
        if r == targetMax { return false }
        
        var l = l
        var r = r
        if s[l] == t[r] { l += 1 }
        r += 1
        return isS(l: l, r: r)
    }
}

    func _isSubsequence(_ s: String, _ t: String) -> Bool {
        guard !s.isEmpty else { return true }
        let s = s.map { $0 }
        var i = 0
        
        for char in t {
            if i == s.count { break }
            if char == s[i] { i += 1 }
        }

        return i == s.count
    }
```
## 543. Diameter of Binary Tree

```swift
    func diameterOfBinaryTree(_ root: TreeNode?) -> Int {
        var maxVal = 0
        
        func dfs(_ root: TreeNode?) -> Int {
            guard root != nil else { return 0 }
            
            let left = dfs(root?.left)
            let right = dfs(root?.right)
            
            maxVal = max(left + right, maxVal)
            return 1 + max(left, right)
            
            
        }
        
        dfs(root)
        
        return maxVal
    }
```
## 563. Binary Tree Tilt


```swift
    func findTilt(_ root: TreeNode?) -> Int {
        var tilt = 0
        
        func dfs(_ root: TreeNode?) -> Int {
            guard let node = root else { return 0 }
            
            let left = dfs(root?.left)
            let right = dfs(root?.right)
            
            tilt += abs(left - right)
            
            return left + right + node.val
        }
        
        dfs(root)
        
        return tilt
    }
```
## 541. Reverse String II


```swift
    func reverseStr(_ s: String, _ k: Int) -> String {
        var charArr = s.map { String($0) }
        var i = 0
        
        while i < s.count {
            reverse(&charArr, i, min(s.count - 1, i + k - 1))
            i += 2 * k
        }
        
        return charArr.joined()
    }
    
    func reverse(_ arr: inout [String], _ left: Int, _ right: Int) {
        var left = left
        var right = right 
        
        while left < right {
            let temp = arr[left]
            arr[left] = arr[right]
            arr[right] = temp
            
            left += 1
            right -= 1
        }
    }
```
## 1128. Number of Equivalent Domino Pairs

```swift
    func numEquivDominoPairs(_ dominoes: [[Int]]) -> Int {
        var seen: [String: [Int]] = [:]
        var result = 0
        
        for i in 0..<dominoes.count {
            let a = dominoes[i][0]
            let b = dominoes[i][1]
            let key = "\(min(a, b))-\(max(a, b))"
            seen[key, default: []].append(i)
        }
        
        for (_ , arr) in seen {
            if arr.count > 1 {
                result += arr.count * (arr.count - 1) / 2
            }
        }
        
        return result
    }
```
## 27. Remove Element

```swift
    func removeElement(_ nums: inout [Int], _ val: Int) -> Int {
        guard !nums.isEmpty else { return 0 }
        guard nums.count > 1 else { return nums[0] == val ? 0: 1 }
        var i = 0
        var j = nums.count - 1
        
        while i < j {
            if nums[j] == val { j -= 1; continue }
            if nums[i] == val {
                let temp = nums[i]
                nums[i] = nums[j]
                nums[j] = temp
                i += 1
                j -= 1
                continue
            }
            
            i += 1
        }
        
        if nums[i] == val {
            return i
        }
        return i + 1
    }
```
## 720. Longest Word in Dictionary


```swift
class Solution {
    func longestWord(_ words: [String]) -> String {
        let trie = Trie()
        
        for word in words {
            trie.insert(word)
        }
        
        return trie.longestWord()
    }
}

class Trie {
    let root: TrieNode
    init() {
        self.root = TrieNode()
    }
    
    func insert(_ word: String) {
        var curr = self.root
        for char in word {
            let index = char.asciiValue! - Character("a").asciiValue!
            if curr.children[Int(index)] == nil {
                curr.children[Int(index)] = TrieNode(String(char))
            }
            
            curr = curr.children[Int(index)]!
            
        }
        
        curr.isEnd = true
    }
    
    func longestWord() -> String {
        var maxWord = ""
        func dfs(_ node: TrieNode?, _ currWord: [String]) {
            var currWord = currWord
            guard let node = node else { return }
            if node.key != "" && !node.isEnd { return }
            
            if currWord.count > maxWord.count { maxWord = currWord.joined() }
            
            for i in 0..<26 {
                let child = node.children[i]
                if child == nil { continue }
                currWord.append(String(child!.key))
                dfs(child, currWord)
                currWord.removeLast()
            }
        } 
        
        dfs(self.root, [])
        return maxWord
    }
}

class TrieNode {
    let key: String
    var children: [TrieNode?]
    var isEnd: Bool
    
    init(_ key: String = "") {
        self.key = key
        self.children = Array(repeating: nil, count: 26)
        self.isEnd = false
    }
}
```
## 119. Pascal's Triangle II

```swift
    func getRow(_ rowIndex: Int) -> [Int] {
        guard rowIndex > 0 else { return [1] }
        guard rowIndex > 1 else { return [1, 1] }
        
        var result = [1, 1]
        
        for row in 2...rowIndex {
            var nextRow: [Int] = [1]
            
            for i in 0..<result.count - 1 {
                let val = result[i] + result[i + 1]
                nextRow.append(val)
            }
            
            nextRow.append(1)
            result = nextRow
        }
        
        return result
    }
```
## 9. Palindrome Number


```swift
    func isPalindrome(_ x: Int) -> Bool {
        guard x >= 0 else { return false }
        guard x >= 10 else { return true } 
        var result = 0
        var n = x
        
        while n > 0 {
            result *= 10
            result += n % 10
            n /= 10
        }
        
        return result == x
    }
```
## 1018. Binary Prefix Divisible By 5


```swift
    func prefixesDivBy5(_ A: [Int]) -> [Bool] {
        var current: Int = 0 
        var result: [Bool] = Array(repeating: false, count: A.count)

        for i in 0..<A.count {
            current <<= 1
            print(A[i], current + A[i])
            current += A[i]

            if current % 5 == 0 {
                result[i] = true
            }
            
            current %= 5
        }

        return result
    }
```
## 270. Closest Binary Search Tree Value

```swift
    func closestValue(_ root: TreeNode?, _ target: Double) -> Int {
        let target = Int(round(target))
        var diff = Int.max
        var result = 0
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            let curr = abs(node.val - target)
            
            if curr < diff {
                diff = curr
                result = node.val
            }
            
            if target < node.val {
                dfs(root?.left)   
            } else {
                dfs(root?.right)   
            }
        }
        
        dfs(root)
        
        return result
    }
```
## 401. Binary Watch

```swift
    func readBinaryWatch(_ num: Int) -> [String] {
        var times: [String] = []
        
        func _readBinaryWatch(_ n: Int, _ hourIndex: Int, _ minIndex: Int, _ hours: Int,  _ min: Int) {
            guard n > 0 else { 
                if hours <= 11 && min <= 59  {
                    var time = "\(hours):\(min)"
                    if min < 10 {
                        time = "\(hours):0\(min)"
                    }
                    times.append(time)
                }
                return 
            }
            
            for i in hourIndex..<4 {
                _readBinaryWatch(n - 1, i + 1, minIndex, hours | 1 << i, min)
            }           
            
            for i in minIndex..<6 {
                _readBinaryWatch(n - 1, 4, i + 1, hours, min | 1 << i)
            }
        }
        
        _readBinaryWatch(num, 0, 0, 0, 0)
        
        
        return times
    }
```
## 628. Maximum Product of Three Numbers


```swift
    func maximumProduct(_ nums: [Int]) -> Int {
        guard nums.count > 3 else { return nums[0] * nums[1] * nums[2] }
        var maxN1 = Int.max
        var maxN2 = Int.max
        var maxP1 = Int.min
        var maxP2 = Int.min
        var maxP3 = Int.min
        
        
        for num in nums {
            if num < maxN1 {
                maxN2 = maxN1
                maxN1 = num
            } else if num < maxN2 {
                maxN2 = num
            }
            
            if num > maxP1 {
                maxP3 = maxP2
                maxP2 = maxP1
                maxP1 = num
            } else if num > maxP2 {
                maxP3 = maxP2
                maxP2 = num
            } else if num > maxP3 {
                maxP3 = num
            }
        }
        
        return max(maxP1 * maxP2 * maxP3, maxN1 * maxN2 * maxP1)
    }
```
## 628. Maximum Product of Three Numbers

```swift
    func maximumProduct(_ nums: [Int]) -> Int {
        guard nums.count > 3 else { return nums[0] * nums[1] * nums[2] }
        var buckets = Array(repeating: 0, count: 2001)
        var result: [Int] = []
        
        for num in nums {
            buckets[num + 1000] += 1
        }
        
        for (num, count) in buckets.enumerated() where count > 0 {
            let num = num - 1000
            var count = count
            
            while count > 0 {
                result.append(num)
                count -= 1
            }
        }
        
        return max(result[result.count - 1] * result[result.count - 2] * result[result.count - 3], result[result.count - 1] * result[0] * result[1])
    }
```
## 415. Add Strings

```swift
    func addStrings(_ num1: String, _ num2: String) -> String {
        var result: [Int] = []
        var num1 = num1.map { String($0) }
        var num2 = num2.map { String($0) }
        var i = num1.count - 1
        var j = num2.count - 1
        var carry = 0
        
        while i >= 0 || j >= 0 {
            var digit1 = 0
            var digit2 = 0
            
            if i >= 0, let val = Int(num1[i]) {
                digit1 = val
            }
            
            if j >= 0, let val = Int(num2[j]) {
                digit2 = val
            }
            let sum = digit1 + digit2 + carry
            result.append(sum % 10)
            carry = sum / 10
            
            i -= 1
            j -= 1
        }
        
        if carry > 0 {
            result.append(carry)
        }
        
        
        return result.reversed().map { String($0) }.joined(separator: "")
    }
```
## 70. Climbing Stairs

```swift
    func climbStairs(_ n: Int) -> Int {
        guard n > 0 else { return 0 }
        guard n > 1 else { return 1 }
        guard n > 2 else { return 2 }
        
        var num1 = 1
        var num2 = 2
        var curr = 3
        
        while curr < n {
            let temp = num1
            num1 = num2
            num2 = temp + num2
            curr += 1
        }
        
        return num1 + num2
    }
```
## 437. Path Sum III

```swift
    func pathSum(_ root: TreeNode?, _ sum: Int) -> Int {
        var count = 0
        var dict: [Int: Int] = [:]
        
        func dfs(_ root: TreeNode?, _ currSum: Int) {
            guard let node = root else { return }
            var currSum = currSum
            currSum += node.val
            if currSum == sum { count += 1 }
            
            count += dict[currSum - sum] ?? 0
            dict[currSum, default: 0] += 1
            
            
            dfs(root?.left, currSum)
            dfs(root?.right, currSum)
            
            dict[currSum, default: 0] -= 1
        }
        
        dfs(root, 0)
        
        return count
    }
```
## 1. Two Sum


```swift
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var seen: [Int: Int] = [:]
        
        for (i, num) in nums.enumerated() {
            if let index = seen[target - num] { return [i, index] }
            seen[num] = i
        }
        
        return []
    }
```
## 744. Find Smallest Letter Greater Than Target


```swift
    func nextGreatestLetter(_ letters: [Character], _ target: Character) -> Character {
        var left = 0
        var right = letters.count
        
        while left < right {
            let mid = ((right - left) / 2) + left
            
            if letters[mid] > target {
                right = mid
            } else {
                left = mid + 1
            }
        }
        
        if left >= letters.count {
            return letters[0]
        } else {
            return letters[left]
        }
    }
```
## 246. Strobogrammatic Number


```swift
    func isStrobogrammatic(_ num: String) -> Bool {
        var dict = ["1": "1", "6": "9", "8": "8", "9": "6", "0": "0"]
        var num = num.map { String($0) }
        var i = 0
        var j = num.count - 1
        
        while i <= j {
            if let val1 = dict[num[j]] {
                if val1 != num[i] { return false }
            } else {
                return false
            }
            
            i += 1
            j -= 1
        }
        
        return true
    }
```
## 26. Remove Duplicates from Sorted Array


```swift
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        guard nums.count > 0 else { return 0 }
        var i = 0
        
        for j in 1..<nums.count {
            if nums[i] != nums[j] {
                i += 1
                nums[i] = nums[j]
            }
        }
        
        return i + 1
    }
```
## 989. Add to Array-Form of Integer


```swift
    func addToArrayForm(_ A: [Int], _ K: Int) -> [Int] {
        var k = K
        var result: [Int] = []
        var carry = 0
        var i = A.count - 1
        
        while i >= 0 {
            let val = k % 10
            let sum = A[i] + val + carry
            k /= 10
            
            result.append(sum % 10)
            
            carry = sum / 10
            i -= 1
        }
        
        while k > 0 {
            let val = k % 10
            let sum = val + carry
            k /= 10
            
            result.append(sum % 10)
            
            carry = sum / 10
        }
        
        if carry > 0 {
            result.append(carry)
        }
        
        return result.reversed()
    }
```
## 819. Most Common Word


```swift
    func mostCommonWord(_ paragraph: String, _ banned: [String]) -> String {
        var dict: [String: Int] = [:]
        var banned = Set(banned)
        var words: [String] = []
        var currWord: [String] = []
        var result = ""
        var count = 0
        
        for char in paragraph {
            let str = String(char).lowercased()
            
            if str < "a" || str > "z" {
                if !currWord.isEmpty {
                    words.append(currWord.joined())   
                }
                currWord = []
            } else {
                currWord.append(str)
            }
        }
        
        if !currWord.isEmpty {
            words.append(currWord.joined())   
        }
        
        
        for word in words where !banned.contains(word) {
            dict[word, default: 0] += 1
        }
        
        for (key, val) in dict {
            if val > count {
                result = key
                count = val
            }
        }
        
        return result
    }
```
## 925. Long Pressed Name


```swift
    func isLongPressedName(_ name: String, _ typed: String) -> Bool {
        var name = name.map { String($0) }
        var typed = typed.map { String($0) }
        var i = 0
        
        if name[0] != typed[0] { return false }
        
        for j in 0..<typed.count {
            if i >= name.count { i -= 1 }
            
            if typed[j] == name[i] {
                i += 1
            } else if typed[j] != name[i - 1] {
                return false
            }
        }
        
        return i == name.count
    }
```
## 758. Bold Words in String

```swift
    func boldWords(_ words: [String], _ S: String) -> String {
        var s = S.map { String($0) }
        var marked = Array(repeating: false, count: S.count)
        var start = false
        var result = [String]()
        
        
        for word in words {
            mark(word, &marked, s)
        }
        
        print(marked)
        print(s)
        
        for (i, char) in s.enumerated() {
            if marked[i] == true {
                if !start {
                    result.append("<b>\(char)")   
                    start = true
                } else if i == s.count - 1 || marked[i + 1] != true {
                    result.append("\(char)</b>")
                    start = false
                } else {
                    result.append(char)    
                }
            } else if start {
                result.append("</b>\(char)")
                start = false
            } else {
                result.append(char)    
            }
        }
        
        if start {
            result.append("</b>")
        }
        
        
        
        return result.joined()
        
        
    }
    
    func mark(_ word: String, _ marked: inout [Bool], _ s: [String]) {
        var str = [String]()
        
        for (i, char) in s.enumerated() {
            if str.count < word.count { str.append(char) }
            
            if word == str.joined() {
                for j in i - word.count + 1...i {
                    marked[j] = true 
                }
                
            } 
            
            if str.count >= word.count { str.removeFirst() }
        }
    }
```
## 101. Symmetric Tree


```swift
    func isSymmetric(_ root: TreeNode?) -> Bool {
        
        func dfs(_ root1: TreeNode?, _ root2: TreeNode?) -> Bool {
            if root1 == nil && root2 == nil { return true }
            guard let node1 = root1, let node2 = root2 else { return false }
            
            return node1.val == node2.val && dfs(root1?.left, root2?.right) && dfs(root1?.right, root2?.left)
        }
        
        return dfs(root?.left, root?.right)
    }
```
## 594. Longest Harmonious Subsequence


```swift
    func findLHS(_ nums: [Int]) -> Int {
        var maxVal = 0
        var dict: [Int: Int] = [:]
        
        for num in nums {
            dict[num, default: 0] += 1
            
            if let curr = dict[num] {
                if let nei1 = dict[num + 1] {
                    maxVal = max(maxVal, curr + nei1)       
                }
                
                if let nei2 = dict[num - 1] {
                    maxVal = max(maxVal, curr + nei2)
                }
                
            }
        }
        
        return maxVal
    }
```
## 504. Base 7


```swift
    func convertToBase7(_ num: Int) -> String {
        guard num != 0 else { return "0" }
        var num = num
        var result: [String] = []
        var isNeg = false
        
        if num < 0 { isNeg = true; num = abs(num) }
        
        while num > 0 {
            let digit = num % 7
            result.append(String(digit))
            
            num /= 7
        }
        
        if isNeg {
            return "-\(result.reversed().joined())"
        }
        
        return result.reversed().joined()
    }
```
## 674. Longest Continuous Increasing Subsequence


```swift
    func findLengthOfLCIS(_ nums: [Int]) -> Int {
        var maxCount = 0
        var currCount = 0
        var pre = Int.min
        
        for num in nums {
            if num > pre {
                currCount += 1
            } else {
                maxCount = max(maxCount, currCount)
                currCount = 1
            }
            pre = num
        }
        
        maxCount = max(maxCount, currCount)
        
        return maxCount
    }
```
## 572. Subtree of Another Tree

```swift
    func isSubtree(_ s: TreeNode?, _ t: TreeNode?) -> Bool {
        var strArr1: [String] = []
        var strArr2: [String] = []
        func convert(_ root: TreeNode?, _ result: inout [String]) {
            guard let node = root else { result.append("*"); return }
            
            result.append("-\(String(node.val))")
            
            convert(root?.left, &result)
            convert(root?.right, &result)
        }
        
        convert(s, &strArr1)
        convert(t, &strArr2)
        
        return strArr1.joined().contains(strArr2.joined())
    }
```
# MerkleTree Solution
```swift
class Solution {
    func isSubtree(_ s: TreeNode?, _ t: TreeNode?) -> Bool {
        var sMerkle: MerkleNode? = nil
        var tMerkle: MerkleNode? = nil
        
        func convertToMerkle(_ root: TreeNode?, _ merkle: MerkleNode?) -> MerkleNode? {
            guard let node = root else { return nil }
            var merkle = merkle
            if merkle == nil { merkle = MerkleNode(node.val) }
            
            merkle?.left = converToMerkle(root?.left, merkle?.left)
            merkle?.right = converToMerkle(root?.right, merkle?.right)
            
            if let left = merkle?.left {
                merkle!.hasher.combine(left.val)
            }
            
            if let right = merkle?.right {
                merkle!.hasher.combine(right.val)
            }
            
            merkle!.hashVal = merkle!.hasher.finalize()
            
            return merkle
        }
        
        func dfs(_ root1: MerkleNode?, _ root2: MerkleNode?) -> Bool {
            if root1 == nil && root2 == nil { return true }
            guard let node1 = root1, let node2 = root2 else { return false }
            
            if node1.hashVal! == node1.hashVal! { 
                if isEqual(root1, root2) { return true }
            }
            
            return dfs(root1?.left, root2) || dfs(root1?.right, root2)
        }
        
        func isEqual(_ root1: MerkleNode?, _ root2: MerkleNode?) -> Bool {
            if root1 == nil && root2 == nil { return true }
            guard let node1 = root1, let node2 = root2 else { return false }
            
            return node1.val == node2.val && isEqual(root1?.left, root2?.left) && isEqual(root1?.right, root2?.right)
        }
        
        sMerkle = convertToMerkle(s, sMerkle)
        tMerkle = convertToMerkle(t, tMerkle)
        
        return dfs(sMerkle, tMerkle)
    }
}

class MerkleNode {
    var left: MerkleNode?
    var right: MerkleNode?
    var val: Int
    var hasher = Hasher()
    var hashVal: Int? = nil
    
    init(_ val: Int, _ left: MerkleNode? = nil, _ right: MerkleNode? = nil) {
        self.val = val
        self.left = left
        self.right = right
        hasher.combine(val)
    }
}
```
## 38. Count and Say


```swift
    func countAndSay(_ n: Int) -> String {
        guard n > 1 else { return "1" }
        
        var result = ["1", "1"]
        
        for _ in 2..<n {
            var count = 0
            var char = ""
            var nextResult: [String] = []
            for currChar in result {
                if char == "" { 
                    char = currChar
                    count = 1
                    continue
                }
                
                if currChar == char { 
                    count += 1
                    continue
                }
                
                nextResult.append("\(count)")
                nextResult.append(char)
                count = 1
                char = currChar
            }
            nextResult.append("\(count)")
            nextResult.append(char)
            result = nextResult
        }
        return result.joined()
    }
```
## 844. Backspace String Compare


```swift
    func backspaceCompare(_ S: String, _ T: String) -> Bool {
        return backspace(S) == backspace(T)
    }
    
    func backspace(_ s: String) -> String {
        var s = s.map { String($0) }
        var i = s.count - 1
        var result: [String] = []
        var count = 0
        
        while i >= 0 {
            if s[i] == "#" {
                count += 1
            } else if count > 0 {
                count -= 1
            } else { 
                result.append(s[i])                   
            }
            i -= 1
        }
          
          return result.reversed().joined()
      } 
```
## 53. Maximum Subarray


```swift
    func maxSubArray(_ n: [Int]) -> Int {
        var maxVal = n[0]
        var curr = n[0]
        
        for i in 1..<n.count {
            curr = max(curr + n[i], n[i])
            maxVal = max(curr, maxVal)
        }
    
        return maxVal
    }
```
## 225. Implement Stack using Queues


```swift
class MyStack {
    var queue1: [Int] = []
    /** Initialize your data structure here. */
    init() {}
    
    /** Push element x onto stack. */
    func push(_ x: Int) {
        queue1.append(x)
        var count = queue1.count
        while count > 1 {
            queue1.append(queue1.removeFirst())
            count -= 1
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    func pop() -> Int {
        return queue1.removeFirst()
    }
    
    /** Get the top element. */
    func top() -> Int {
        return queue1[0]
    }
    
    /** Returns whether the stack is empty. */
    func empty() -> Bool {
        return queue1.isEmpty
    }
}
```
## 345. Reverse Vowels of a String

```swift
    func reverseVowels(_ s: String) -> String {
        var vowels: Set<String> = ["a", "e", "i", "o", "u"]
        var s = s.map { String($0) }
        var j = s.count - 1
        var i = 0
        
        while i < j {
            let str1 = s[i].lowercased()
            let str2 = s[j].lowercased()
            if vowels.contains(str1) && vowels.contains(str2) {
                let temp = s[i]
                s[i] = s[j]
                s[j] = temp
                j -= 1
                i += 1
            } else if vowels.contains(str1) {
                j -= 1
            } else {
                i += 1
            }
        }
        
        return s.joined()
    }
```
## 155. Min Stack


```swift
class MinStack {
    var arr: [Int] = []
    var mins: [Int] = []
    /** initialize your data structure here. */
    init() {}
    
    func push(_ x: Int) {
        arr.append(x)
        if mins.isEmpty {
            mins.append(x)
        } else if mins[mins.count - 1] >= x {
            mins.append(x)
        }
    }
    
    func pop() {
        let val = arr.removeLast()
        if !mins.isEmpty {
            if val == mins[mins.count - 1] {
               mins.removeLast()
            }   
        }
    }
    
    func top() -> Int {
        return arr[arr.count - 1]
    }
    
    func getMin() -> Int {
        return mins[mins.count - 1]
    }
}
```
## 724. Find Pivot Index


```swift
    func pivotIndex(_ nums: [Int]) -> Int {
        var sum = nums.reduce(0, +)
        var curr = 0
        
        for (i, num) in nums.enumerated() {
            if curr == sum - num - curr { return i }
            
            curr += num
        }
        
        return -1
    }
```
## 405. Convert a Number to Hexadecimal


```swift
    func toHex(_ num: Int) -> String {
        guard num != 0 else { return "0" }
        var hexMap = "0123456789abcdef".map { String($0) }
        var result: [String] = []
        var num = num
        if num < 0 { num += 4294967296 }
        
         while num > 0 {
            let digit = hexMap[num % 16]
            result.append(digit)
            num /= 16
        }
        
        return result.reversed().joined()
    }
```
## 67. Add Binary


```swift
    func addBinary(_ a: String, _ b: String) -> String {
        var a = a.map { String($0) }
        var b = b.map { String($0) }
        var i = a.count - 1
        var j = b.count - 1
        var carry = 0
        var result: [String] = []
        
        while i >= 0 || j >= 0 {
            let bit1 = i >= 0 ? Int(a[i])! : 0
            let bit2 = j >= 0 ? Int(b[j])! : 0
            let sum = bit1 + bit2 + carry
            
            result.append("\(sum % 2)")
            carry = sum / 2
            i -= 1
            j -= 1
        }
        
        if carry > 0 {
            result.append("\(carry)")
        }
        
        return result.reversed().joined()
    }
```
## 303. Range Sum Query - Immutable


```swift
class NumArray {
    var prefixSum: [Int]

    init(_ nums: [Int]) {
        guard !nums.isEmpty else {
            prefixSum = [Int]()
            return
        }

        prefixSum = [Int](repeating: 0, count: nums.count + 1)
        
        for i in 0 ..< nums.count {
            prefixSum[i + 1] = prefixSum[i] + nums[i]
        }
    }

    func sumRange(_ i: Int, _ j: Int) -> Int {
        return prefixSum[j + 1] - prefixSum[i]
    }
}
```
## 231. Power of Two


```swift
    func isPowerOfTwo(_ n: Int) -> Bool {
        return n > 0 && n & (n - 1) == 0
    }
```
## 1380. Lucky Numbers in a Matrix


```swift
    func luckyNumbers (_ matrix: [[Int]]) -> [Int] {
        var minRow: [Int: Int] = [:]
        var maxCol: [Int: Int] = [:]
        var result: [Int] = []
        
        for row in 0..<matrix.count {
            for col in 0..<matrix[0].count {
                minRow[row] = min(minRow[row, default: Int.max], matrix[row][col])
                maxCol[col] = max(maxCol[col, default: Int.min], matrix[row][col])
            }
        }
        
        for row in 0..<matrix.count {
            for col in 0..<matrix[0].count {
                if minRow[row, default: Int.max]  == maxCol[col, default: Int.min] {
                    result.append(minRow[row, default: Int.max])
                }
            }
        }
        
        return result
    }
```
## 680. Valid Palindrome II


```swift
    func validPalindrome(_ s: String) -> Bool {
        guard s.count > 2 else { return true }
        var s = s.map { String($0) }
        var i = 0
        var j = s.count - 1
        
        
        
        while i <= j {
            if s[i] != s[j] {
                return isPalindrome(s, i + 1, j) || isPalindrome(s, i, j - 1)
            }
            
            i += 1
            j -= 1
        }
        
        
        return true
    }
    
    func isPalindrome(_ s: [String], _ i: Int, _ j: Int) -> Bool {
        var i = i
        var j = j
        
        while i <= j {
            if s[i] != s[j] { return false }
            
            i += 1
            j -= 1
        }
        return true
    }
```
## 482. License Key Formatting


```swift
    func licenseKeyFormatting(_ S: String, _ K: Int) -> String {
        let S = S.split(separator: "-")
        var s: [String] = []
        var result: [String] = []
        var curr: [String] = []
        var count = 0
        
        for arr in S {
            for str in arr {
                s.append(String(str))
            }
        }
        
        count = s.count % K
        
        
        for char in s {
            if curr.count >= K || (count > 0 && curr.count == count && result.isEmpty) {
                result.append("\(curr.joined())-")
                curr = []
            }
            curr.append(char.uppercased())
        }
        
        result.append(curr.joined())
        
        return result.joined()
    }
```

## 110. Balanced Binary Tree


```swift
    func isBalanced(_ root: TreeNode?) -> Bool {
        func dfs(_ root: TreeNode?) -> Int {
            guard root != nil else { return 0 }
            
            let leftHeight = dfs(root?.left)
            let rightHeight = dfs(root?.right)
            
            if leftHeight == -1 || rightHeight == -1 || abs(leftHeight - rightHeight) > 1 {
                return -1
            }
            
            return max(leftHeight, rightHeight) + 1
        }
        
        return dfs(root) != -1
    }
```
## 671. Second Minimum Node In a Binary Tree


```swift
    func findSecondMinimumValue(_ root1: TreeNode?) -> Int {
        func dfs(_ root2: TreeNode?) -> Int {
            guard let node = root2, (node.left != nil && node.right != nil) else { return -1 }
            
            var left = node.left!.val
            var right = node.right!.val
            
            if left == root1!.val { left = dfs(root2?.left) }
            if right == root1!.val { right = dfs(root2?.right) }
            
            if left != -1 && right != -1 { return min(left, right) }
            if left == -1 { return right }
            return left
        }
        
        return dfs(root1)
    }
```
## 1480. Running Sum of 1d Array


```swift
    func runningSum(_ nums: [Int]) -> [Int] {
        var result = nums
        
        for (i, num) in nums.enumerated() where i > 0 {
            result[i] += result[i - 1]
        }
        
        return result
    }
```
## 1470. Shuffle the Array


```swift
    func shuffle(_ nums: [Int], _ n: Int) -> [Int] {
        var result = Array(repeating: 0, count: n * 2)
        var i = 0
        var j = n
        var count = 0
        
        while j < nums.count && i < n {
            result[count] = nums[i]
            result[count + 1] = nums[j]
            i += 1
            j += 1
            count += 2
        }
        
        return result
    }
```
## 1469. Find All The Lonely Nodes



```swift
    func getLonelyNodes(_ root: TreeNode?) -> [Int] {
        guard root != nil else { return [] }
        var result: [Int] = []
        
        func dfs(_ root: TreeNode?, _ shouldAdd: Bool) {
            if shouldAdd { result.append(root!.val) }

            let hasToAdd = root?.left != nil && root?.right != nil

            if root?.left != nil {
                dfs(root?.left, !hasToAdd)
            }
            if root?.right != nil {
                dfs(root?.right, !hasToAdd)
            }
        }
        
        dfs(root, false)
        
        return result
    }
```
## 374. Guess Number Higher or Lower


```swift
    func guessNumber(_ n: Int) -> Int {
        var left = 1
        var right = n
        
        while left <= right {
            let mid = ((right - left) / 2) + left
            let result = guess(mid)
            
            if result == 0 {
                return mid
            } else if result == 1 {
                left = mid + 1
            } else {
                right = mid - 1
            }
        }
        
        return -1
    }
```
## 66. Plus One


```swift
    func plusOne(_ digits: [Int]) -> [Int] {
        var carry = 1
        var result: [Int] = []
        var i = digits.count - 1
        
        
        while i >= 0 {
            let sum = digits[i] + carry
            result.append(sum % 10)
            carry = sum / 10
            i -= 1
        }
        
        if carry > 0 {
            result.append(carry)
        }
        
        return result.reversed()
    }
```
## 326. Power of Three


```swift
    func isPowerOfThree(_ n: Int) -> Bool {
        var n = n
        var count = 0
        
        while n > 0 {
            let digit = n % 3
            count += digit
            n /= 3
        }
        
        return count == 1
    }
```
## 326. Power of Three

```swift
    func isPowerOfThree(_ n: Int) -> Bool {
        var n = n
        var count = 0
        
        while n > 0 {
            let digit = n % 3
            count += digit
            n /= 3
        }
        
        return count == 1
    }
```
```swift
    func isPowerOfThree(_ n: Int) -> Bool {
        guard n > 0 else { return false }
        let num = logs(Double(n), 3)
        let p = pow(3, Int(round(num)))
        return n == p
    }
    
    func logs(_ x: Double, _ y: Double) -> Double {
        return log(x)/log(y)
    }
    
    func pow(_ with: Int, _ of: Int) -> Int {
        var sum = 1
        
        for _ in 0..<of {
            sum *= with
        }
        
        return sum
    }
```
## 326. Power of Three


```swift
class MaxStack {
    var stack: [Int] = []
    var maxStack: [Int] = []
    
    init() {}
    
    func push(_ x: Int) {
        var maxVal = x
        
        if !maxStack.isEmpty, maxStack[maxStack.count - 1] > x {
            maxVal = maxStack[maxStack.count - 1]
        }
        
        maxStack.append(maxVal)
        stack.append(x)
    }
    
    func pop() -> Int {
        maxStack.removeLast()
        return stack.removeLast()
    }
    
    func top() -> Int {
        return stack[stack.count - 1]
    }
    
    func peekMax() -> Int {
        return maxStack[maxStack.count - 1]
    }
    
    func popMax() -> Int {
        let maxVal = maxStack[maxStack.count - 1]
        var temp: [Int] = []
        
        while top() != maxVal {
            temp.append(pop())
        }
        
        pop()
        
        while !temp.isEmpty {
            push(temp.removeLast())
        }
        
        return maxVal
    }
}
```
## 734. Sentence Similarity


```swift
    func areSentencesSimilar(_ words1: [String], _ words2: [String], _ pairs: [[String]]) -> Bool {
        guard words1.count == words2.count else { return false }
        var dict: [String: Set<String>] = [:]
        
        for pair in pairs {
            dict[pair[0], default: []].insert(pair[1])
            dict[pair[1], default: []].insert(pair[0])
        }
        
        for i in 0..<words2.count {
            if words1[i] == words2[i] { continue }
            
            if let set1 = dict[words1[i]], let set2 = dict[words2[i]], (set1.contains(words2[i]) && set2.contains(words1[i])) {
                continue
            } else {
                return false
            }
        }
        
        return true
    }
```
## 299. Bulls and Cows


```swift
    func getHint(_ secret: String, _ guess: String) -> String {
        var bulls = 0
        var cows = 0
        var secret = secret.map { String($0) }
        var guess = guess.map { String($0) }
        var correct: [String: Int] = [:]
        var cowGuess: [String: Int] = [:]
        
        for char in secret {
            correct[char, default: 0] += 1
        }
        
        for i in 0..<secret.count {
            if secret[i] == guess[i] {
                if cowGuess[guess[i], default: 0] > 0 && correct[guess[i], default: 0] <= 0 {
                    cows -= 1
                    cowGuess[guess[i], default: 0] -= 1
                }
                bulls += 1
                correct[guess[i], default: 0] -= 1
            } else if correct[guess[i], default: 0] > 0 {
                cows += 1
                correct[guess[i], default: 0] -= 1
                cowGuess[guess[i], default: 0] += 1
            }
        }
        
        return "\(bulls)A\(cows)B"
    }
```
## 645. Set Mismatch


```swift
    func findErrorNums(_ nums: [Int]) -> [Int] {
        var buckets = Array<Int>(repeating: 0, count: nums.count + 1)
        var dup = 0
        var missing = 0
        for num in nums {
            if buckets[num] == 1 {
                dup = num
            }
            buckets[num] = 1
        }
        
        for i in 1..<buckets.count {
            if buckets[i] == 0 {
                missing = i
                break
            }
        }
        return [dup, missing]
    }
```
## 35. Search Insert Position


```swift
    func searchInsert(_ nums: [Int], _ target: Int) -> Int {
        var left = 0
        var right = nums.count
        
        while left < right {
            let mid = ((right - left) / 2) + left
            
            if nums[mid] >= target {
                right = mid
            } else {
                left = mid + 1
            }
        }
        
        return left 
    }
```
## 501. Find Mode in Binary Search Tree


```swift
    func findMode(_ root: TreeNode?) -> [Int] {
        var maxCount = 0
        var result: [Int] = []
        var currVal = Int.min
        var currCount = 0
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            
            dfs(root?.left)
            if node.val != currVal {
                currVal = node.val
                currCount = 1
            } else {
                currCount += 1
            }
            
            if maxCount == currCount {
                result.append(node.val)
            } else if maxCount < currCount {
                result = [node.val]
                maxCount = currCount
            }
            
            dfs(root?.right)
        }
        
        dfs(root)
        
        return result
    }
```
## 198. House Robber


```swift
    func rob(_ nums: [Int]) -> Int {
        var curr = 0
        var pre = 0
        
        for num in nums {
            let temp = curr
            curr = max(curr, pre + num)
            pre = temp
        }
        
        return curr
    }
```
## 747. Largest Number At Least Twice of Others


```swift
    func dominantIndex(_ nums: [Int]) -> Int {
        var largest = 0
        var index = 0
        var second = 0
        
        for (i, num) in nums.enumerated() {
            if num > largest {
                second = largest
                largest = num
                index = i
            } else if num > second {
                second = num
            }
        }
        
        if largest >= second * 2 {
            return index
        } else {
            return -1
        }
    }
```
## 367. Valid Perfect Square


```swift
    func isPerfectSquare(_ num: Int) -> Bool {
        var left = 1
        var right = Int(round(Double(num) / 2))
        
        while left <= right {
            let mid = ((right - left) / 2) + left
            let sqrt = mid * mid
            
            if sqrt == num {
                return true
            } else if sqrt > num {
                right = mid - 1
            } else {
                left = mid + 1
            }
        }
        
        return false
    }
```
## 342. Power of Four


```swift
    func isPowerOfFour(_ num: Int) -> Bool {
        var count = 0
        var num = num
        
        while num > 0 {
            let digit = num % 4
            count += digit
            num /= 4
        }
        
        return count == 1
    }
```
## 263. Ugly Number


```swift
    func isUgly(_ num: Int) -> Bool {
        if num <= 0 { return false }
        var num = num
        
        while num % 2 == 0 { num /= 2 }
        while num % 3 == 0 { num /= 3 }
        while num % 5 == 0 { num /= 5 }
        
        return num == 1
    }
```
## 643. Maximum Average Subarray I


```swift
    func findMaxAverage(_ nums: [Int], _ k: Int) -> Double {
        guard !nums.isEmpty else { return 0 }
        guard nums.count > 1 else { return Double(nums[0]) }
        var maxAve: Double = Double(Int.min)
        var curr: Double = 0
        var j = 0
        
        for i in 0..<nums.count {
            let num = Double(nums[i])
            
            if i - j < k {
                curr += num
            } else {
                maxAve = max(curr / Double(k), maxAve)
                curr -= Double(nums[j])
                curr += num
                j += 1
            }
        }
        
        maxAve = max(curr / Double(k), maxAve)
        
        return maxAve
    }
```
## 1528. Shuffle String


```swift
    func restoreString(_ s: String, _ indices: [Int]) -> String {
        var result = Array(repeating: "", count: s.count)
        var s = s.map { String($0) }
        
        for (index, i) in indices.enumerated() {
            result[i] = s[index]
        }
        
        return result.joined()
    }
```
## 1033. Moving Stones Until Consecutive


```swift
    func numMovesStones(_ a: Int, _ b: Int, _ c: Int) -> [Int] {
        let nums = [a, b, c].sorted { $0 < $1 }
        var x = nums[0]
        var y = nums[1]
        var z = nums[2]
        var maxVal = (y - x) + (z - y) - 2
        var minVal = (y - x) > 2 && (z - y) > 2 ? 2 : (y - x) > 1 || (z - y) > 1 ? 1 : 0
        
        return [minVal, maxVal]
    }
```
## 112. Path Sum


```swift
    func hasPathSum(_ root: TreeNode?, _ sum: Int) -> Bool {
        func dfs(_ root: TreeNode?, _ currSum: Int) -> Bool {
            guard let node = root else { return false }
            var currSum = currSum + node.val
            
            if node.left == nil && node.right == nil { return currSum == sum }
            
            return dfs(node.left, currSum) || dfs(node.right, currSum)
        }
        
        return dfs(root, 0)
    }
```
## 443. String Compression


```swift
    func compress(_ chars: inout [Character]) -> Int {
        var i = 0
        var count = 1
        var j = 1
        
        while j < chars.count {
            if chars[j] == chars[j - 1] {
                count += 1
                j += 1
                continue
            }
            
            chars[i] = chars[j - 1]
            i += 1
            if count > 1 {
                var nums = "\(count)"
                for num in nums {
                    chars[i] = num
                    i += 1   
                }
            }
            count = 1
            j += 1
        }
        
        chars[i] = chars[j - 1]
        i += 1
        if count > 1 {  
            var nums = "\(count)"
            for num in nums {
                chars[i] = num
                i += 1   
            }
        }
        
        return i 
    }
```
## 141. Linked List Cycle


```swift
    func hasCycle(_ head: ListNode?) -> Bool {
        var f = head
        var s = head
        
        while f?.next != nil {
            f = f?.next?.next
            s = s?.next
            if f === s { return true }
        }
        
        return false 
    }
```
## 441. Arranging Coins

# Binary Search Solution
```swift
    func arrangeCoins(_ n: Int) -> Int {
        var left = 1
        var right = n
        
        while left < right {
            let mid = ((right - left) / 2) + left
            let curr = (mid * (mid + 1)) / 2
            
            if curr >= n {
                right = mid
            } else {
                left = mid + 1
            }
        }
        
        return (left * (left + 1)) / 2 == n ? left : left - 1
    }
```
# Math Solution
```swift
    func arrangeCoins(_ n: Int) -> Int {
        return  Int((-1 + (1 + 8 * Double(n)).squareRoot()) / 2)
    }
```
## 1486. XOR Operation in an Array


```swift
    func xorOperation(_ n: Int, _ start: Int) -> Int {
        var sum = 0
        var curr = 0
        
        for i in 0..<n {
            sum = sum ^ (start + 2 * i)
        }
        
        return sum
    }
```
## 760. Find Anagram Mappings


```swift
    func anagramMappings(_ A: [Int], _ B: [Int]) -> [Int] {
        var dict: [Int: Int] = [:]
        var result: [Int] = []
        
        for (i, num) in B.enumerated() {
            dict[num] = i
        }
        
        for num in A {
            result.append(dict[num, default: 0])
        }
        
        return result
    }
```
## 1213. Intersection of Three Sorted Arrays


```swift
    func arraysIntersection(_ arr1: [Int], _ arr2: [Int], _ arr3: [Int]) -> [Int] {
        var buckets = Array(repeating: 0, count: 2001)
        var result: [Int] = []
        var i = 0
        
        while i < arr1.count {
            buckets[arr1[i]] += 1
            buckets[arr2[i]] += 1
            buckets[arr3[i]] += 1
            i += 1
        }
        
        for (num, count) in buckets.enumerated() where count >= 3 {
            result.append(num)
        }
        
        
        return result
    }
```
## 849. Maximize Distance to Closest Person


```swift
    func maxDistToClosest(_ seats: [Int]) -> Int {
        var start = 0
        var end = 1
        var maxDis = -1
        
        while end < seats.count {
            if seats[end] != 1 { 
                end += 1
                continue 
            }
            
            if seats[start] != 1 { 
                maxDis = max(maxDis, end)
            } else {
                maxDis = max(maxDis, (end - start) / 2)
            }
            
            start = end
            end += 1
        }
        
        if maxDis == -1 {
            maxDis = seats.count - 1
        } else if seats[end - 1] == 0 {
            maxDis = max(maxDis, end - 1 - start)
        }
        
        return maxDis
    }
```
## 1134. Armstrong Number


```swift
    func isArmstrong(_ N: Int) -> Bool {
        var numDigits = 0
        var n = N
        var sum = 0
        
        while n > 0 {
            numDigits += 1
            n /= 10
        }
        
        n = N
        
        while n > 0 {
            let digit = n % 10
            n /= 10
            sum += Int(pow(Double(digit), Double(numDigits)))
            if sum > N {
                return false
            }
        }
        return sum == N
    }
```
## 970. Powerful Integers


```swift
    func powerfulIntegers(_ x: Int, _ y: Int, _ bound: Int) -> [Int] {
        let maxI = x == 1 ? 1 : Int(log(Double(bound))/log(Double(x)))
        let maxJ = y == 1 ? 1 : Int(log(Double(bound))/log(Double(y)))
        var result: Set<Int> = []
        
        for i in 0...maxI {
            for j in 0...maxJ {
                let power = Int(pow(Double(x), Double(i)) + pow(Double(y), Double(j)))
                
                if power <= bound { result.insert(power) }
            }
        }
        
        return Array(result)
    }
```
## 205. Isomorphic Strings


```swift
    func isIsomorphic(_ s: String, _ t: String) -> Bool {
        var s = s.map { String($0) }
        var t = t.map { String($0) }
        var dict1: [String: String] = [:]
        var dict2: [String: String] = [:]
        
        for (i, char) in s.enumerated() {
            
            if let val = dict1[t[i]], val != char {
                return false
            }
            
            if let val = dict2[char], val != t[i] {
                return false
            }
            
            dict1[t[i]] = char
            dict2[char] = t[i]
            
            
        }
        
        return true
    }
```
## 1346. Check If N and Its Double Exist


```swift
    func checkIfExist(_ arr: [Int]) -> Bool {
        var seen = Set(arr)
        var zeros = 0
        
        for num in arr {
            if zeros == 1 && num == 0 { return true }
            if seen.contains(num * 2) && num != 0 { return true } 
            if num == 0 { zeros += 1 }
        }
        
        return false
    }
```
## 160. Intersection of Two Linked Lists


```swift
    func getIntersectionNode(_ headA: ListNode?, _ headB: ListNode?) -> ListNode? {
        var aCount = getCount(headA)
        var bCount = getCount(headB)
        var diff = abs(aCount - bCount)
        var a = headA
        var b = headB
        
        if aCount > bCount {
            a = move(a, diff)
        } else if bCount > aCount {
            b = move(b, diff)
        }
        
        while a != nil {
            if a === b { return a }
            
            a = a?.next
            b = b?.next
        }
        
        return nil
        
    }
    
    func getCount(_ node: ListNode?) -> Int {
        var count = 0
        var curr: ListNode? = node
        
        while curr != nil {
            count += 1
            curr = curr?.next
        }
        
        return count
    }
    
    func move(_ node: ListNode?, _ count: Int) -> ListNode? {
        var count = count
        var node = node
        
        while count > 0 {
            node = node?.next
            count -= 1
        }
        
        return node
    }
```
## 88. Merge Sorted Array


```swift
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        var p = m + n - 1
        var i = m - 1
        var j = n - 1
        
        while p >= 0 && j >= 0 {
            if i >= 0, nums1[i] > nums2[j] {
                nums1[p] = nums1[i]
                i -= 1
            } else {
                nums1[p] = nums2[j]
                j -= 1
            }
            p -= 1
        }
    }
```
## 624. Maximum Distance in Arrays


```swift
    func maxDistance(_ arrays: [[Int]]) -> Int {
        var minVal = arrays[0][0]
        var maxVal = arrays[0][arrays[0].count - 1]
        var result = 0

        for (index, array) in arrays.enumerated() where index != 0 {
            result = max(max(result, abs(maxVal - array[0])), max(result, abs(minVal - array[array.count - 1])))
            minVal = min(minVal, array[0])
            maxVal = max(maxVal, array[array.count - 1])
        }

        return result   
    }
```
## 234. Palindrome Linked List


```swift
    func isPalindrome(_ head: ListNode?) -> Bool {
        var fast: ListNode? = head
        var slow: ListNode? = head
        
        while fast != nil && fast?.next != nil {
            fast = fast?.next?.next
            slow = slow?.next
        }
        
        if fast != nil { slow = slow?.next }
        
        slow = reverse(slow)
        fast = head
        
        while slow != nil {
            if fast?.val != slow?.val { return false }
            fast = fast?.next
            slow = slow?.next
        }
        
        return true
    }
    
    func reverse(_ head: ListNode?) -> ListNode? {
        var head = head
        var pre: ListNode? = nil
    
        while head != nil {
            let next = head?.next
            head?.next = pre
            pre = head
            head = next
        }
        
        return pre
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
            } else if result.popLast() != char {
                return false
            }
        }
        
        return result.isEmpty
    }
```
## 434. Number of Segments in a String


```swift
    func countSegments(_ s: String) -> Int {
        var s = s.map { $0 }
        var count = 0
        
        for (i, char) in s.enumerated() {
            if (i == 0 || s[i - 1] == " ") && s[i] != " " {
                count += 1
            }
        }
        
        return count
    }
```
## 507. Perfect Number


```swift
    func checkPerfectNumber(_ num: Int) -> Bool {
        guard num > 1 else { return false }
        var sum = 0
        
        for i in 1...Int(Double(num).squareRoot()) {
            if num % i != 0 { continue }
            
            sum += i
            sum += num / i
        }
        
        sum -= num
        
        return sum == num
    }
```
## 14. Longest Common Prefix


```swift
    func longestCommonPrefix(_ strs: [String]) -> String {
        guard !strs.isEmpty else { return "" }
        var result = strs[0].map { String($0) }
        
        for (index, str) in strs.enumerated() where index > 0 {
            var temp: [String] = []
            for (i, char) in str.enumerated() where i < result.count {
                let char = String(char)
                if result[i] != char { break }
                temp.append(char)
            }
            result = temp
        }
        
        return result.joined()
    }
```
## 278. First Bad Version



```swift
    func firstBadVersion(_ n: Int) -> Int {
        var left = 1
        var right = n
        
        while left < right {
            let mid = ((right - left) / 2) + left
            
            if isBadVersion(mid) {
                right = mid
            } else {
                left = mid + 1
            }
        }
        
        return left
    }
```
## 874. Walking Robot Simulation


```swift
    func robotSim(_ commands: [Int], _ obstacles: [[Int]]) -> Int {
        var s = Set(obstacles)
        let step = [(1, 0), (0, -1), (-1, 0), (0, 1)]
        var dir = 3
        
        var currentX = 0
        var currentY = 0
        var dist = 0
        for command in commands {
            if command == -1 {
                dir = (dir + 1) % 4
            } else if command == -2 {
                dir = (dir - 1 + 4) % 4
            } else {
                for i in 1...command {
                    let tx = currentX + step[dir].0
                    let ty = currentY + step[dir].1
                    if s.contains([tx, ty]) { break }
                    
                    currentX = tx
                    currentY = ty
                    
                    dist = max(dist, currentX * currentX + currentY * currentY)
                }
            }
        }
        
        return dist
    }
```
## 914. X of a Kind in a Deck of Cards


```swift
    func hasGroupsSizeX(_ deck: [Int]) -> Bool {
        var dict:[Int: Int] = [:]
        var g = 0
        
        for num in deck {
            dict[num, default: 0] += 1
        }
        
        for count in dict.values {
            g = getGCD(g, count)
        }
        
        return g >= 2
    }
    
    func getGCD(_ a: Int, _ b: Int) -> Int {
        return a == 0 ? b : getGCD(b % a, a)
    }
```
## 914. X of a Kind in a Deck of Cards


```swift
    func hasGroupsSizeX(_ deck: [Int]) -> Bool {
        var dict:[Int: Int] = [:]
        var g = 0
        
        for num in deck {
            dict[num, default: 0] += 1
        }
        
        for count in dict.values {
            g = getGCD(g, count)
        }
        
        return g >= 2
    }
    
    func getGCD(_ a: Int, _ b: Int) -> Int {
        return a == 0 ? b : getGCD(b % a, a)
    }
```
## 1450. Number of Students Doing Homework at a Given Time


```swift
    func busyStudent(_ startTime: [Int], _ endTime: [Int], _ queryTime: Int) -> Int {
        var result = 0
        
        for i in 0..<startTime.count {
            if startTime[i] <= queryTime && endTime[i] >= queryTime {
                result += 1
            }
        }
        
        return result
    }
```
## 1464. Maximum Product of Two Elements in an Array


```swift
    func maxProduct(_ nums: [Int]) -> Int {
        guard nums.count > 2 else { return (nums[0] - 1) * (nums[1] - 1) }
        var n = max(nums[0], nums[1])
        var m = min(nums[0], nums[1])
        
        for i in 2..<nums.count {
            if nums[i] > n {
                m = n
                n = nums[i]
            } else if nums[i] > m {
                m = nums[i]
            }
        }
        
        return (n - 1) * (m - 1)
    }
```
## 1180. Count Substrings with Only One Distinct Letter


```swift
    func countLetters(_ S: String) -> Int {
        var count = 0
        var n = 1
        var pre = S.first!
        var used = true
        
        for (i, char) in S.enumerated() where i > 0 {
            if pre == char {
                n += 1
                used = false
            } else {
                count += (n * (n + 1)) / 2
                used = true
                n = 1
            }
            
            pre = char
        }
        
        if !used {
            count += (n * (n + 1)) / 2
        } else {
            count += 1
        }
        
        return count
    }
```
## 203. Remove Linked List Elements


```swift
    func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
        var dummy: ListNode? = ListNode()
        dummy?.next = head
        var curr = dummy
        
        while curr?.next != nil {
            if curr!.next!.val == val {
                curr?.next = curr?.next?.next
            } else {
                curr = curr?.next
            }
        }
        
        return dummy?.next
    }
```
## 219. Contains Duplicate II


```swift
    func containsNearbyDuplicate(_ nums: [Int], _ k: Int) -> Bool {
        var seen: Set<Int> = []
        
        for i in 0..<nums.count {
            if i - k > 0 { seen.remove(nums[i - k - 1]) }
            if seen.contains(nums[i]) { return true }
            
            seen.insert(nums[i])
        }
        
        return false
    }
```
## 111. Minimum Depth of Binary Tree


```swift
    func minDepth(_ root: TreeNode?) -> Int {
        func dfs(_ root: TreeNode?, _ height: Int) -> Int {
            guard let node = root else { return Int.max }
            if node.left == nil && node.right == nil { return height }
            
            return min(dfs(node.left, height + 1), dfs(node.right, height + 1))
        }
        
        return root != nil ? dfs(root, 1) : 0
    }
```
## 604. Design Compressed String Iterator


```swift
class StringIterator {
    var str: [Character]
    var i = 0
    var currChar: Character = " "
    var currCount = 0
    var currIndex = 0
    
    init(_ compressedString: String) {
        str = compressedString.map { $0 }
        getNextChar()
        getNextCount()
    }
    
    func next() -> Character {
        if currCount <= 0 {
            getNextChar()
            getNextCount()
        }
        
        currCount -= 1
        return currChar
    }
    
    func hasNext() -> Bool {
        return currIndex < str.count || currCount > 0
    }
    
    func getNextChar() {
        if currIndex >= str.count {
            currChar = " "
        } else {
            currChar = str[currIndex]
            currIndex += 1
        }
    }
    
    func getNextCount() {
        currCount = 0
        
        while currIndex < str.count {
            if let count = Int(String(str[currIndex])) {
                currCount *= 10
                currCount += count
                currIndex += 1
            } else {
                break
            }
        }
    }
}
```
## 422. Valid Word Square

```swift
    func validWordSquare(_ words: [String]) -> Bool {
        var words = words.map { $0.map { String($0) } }
        for i in 0..<words.count {
            for j in 0..<words[i].count {
                if j >= words.count {
                    return false
                } else if words[j].count <= i {
                    return false
                } else if words[i][j] != words[j][i] {
                    return false
                }
            }
        }
        
        return true
    }
```
## 840. Magic Squares In Grid


```swift
    func numMagicSquaresInside(_ grid: [[Int]]) -> Int {
        var count = 0
        var x = grid.count
        var y = grid[0].count
        if x < 3 || y < 3 {
            return 0
        }
        for i in 0 ..< x - 2 {
            for j in 0 ..< y - 2 {
                if grid[i + 1][j + 1] == 5 && judge(grid, i, j) {
                    count += 1
                }
            }
        }
        return count
    }
    
    func judge(_ grid: [[Int]], _ i: Int, _ j: Int) -> Bool {
        if grid[i][j] == 5 { return false }
        
        for x in i ..< i + 3 {
            for y in j ..< j + 3 {
                if grid[x][y] > 9 {
                    return false
                }
            }
        }
        
        for x in i ..< i + 3 {
            if grid[x][j] + grid[x][j + 1] + grid[x][j + 2] != 15 { 
                return false
            }
        }
        
        for y in j ..< j + 3 {
            if grid[i][y] + grid[i + 1][y] + grid[i + 2][y] != 15 { 
                return false
            }
        }
        if grid[i][j] + grid[i + 1][j + 1] + grid[i + 2][j + 2] != 15 { 
            return false
        }
        if grid[i + 2][j] + grid[i + 1][j + 1] + grid[i][j + 2] != 15 {
            return false
        }
        return true
    }
```
## 290. Word Pattern


```swift
    func wordPattern(_ pattern: String, _ str: String) -> Bool {
        var m: [Character:Substring] = [:]
        let pat = Array(pattern)
        let str = str.split(separator: " ")
        if pat.count != str.count { return false }
        
        for i in 0...str.count-1 {
            if m[pat[i]] == nil && !m.values.contains(str[i]) { m[pat[i]] = str[i] }
            if m[pat[i]] != str[i] { return false }
        }
        
        return true
    }
```
## 949 Laargest Time for Given Digits

```swift
    func largestTimeFromDigits(_ A: [Int]) -> String {
        var time = ""
        
        for i in 0..<A.count {
            for j in 0..<A.count {
                if i == j { continue }
                for k in 0..<A.count {
                    if i == k || j == k { continue }
                    for l in 0..<A.count {
                        if i == l || j == l || k == l { continue }
                        let hours = "\(A[i])\(A[j])"
                        let mins = "\(A[k])\(A[l])"
                        
                        if hours >= "24" || mins >= "60" { continue }
                        
                        let currTime = hours + ":" + mins
                        
                        if time == "" || time < currTime { time = currTime }
                    }
                }
            }
        }
        
        return time
    }
```
## 190. Reverse Bits


```swift
    func reverseBits(_ n: Int) -> Int {
        var n = n
        var result = 0
        
        for i in 0..<32 {
            result += (n & 1)
            n >>= 1
            if i != 31 { result <<= 1 }
        }
        
        return result
    }
```
## 687. Longest Univalue Path


```swift
    func longestUnivaluePath(_ root: TreeNode?) -> Int {
        var maxVal = 0
        
        func dfs(_ root: TreeNode?, _ val: Int = 0) -> Int {
            guard let node = root else { return 0 }
            
            let left = dfs(node.left, node.val)
            let right = dfs(node.right, node.val)
            maxVal = max(maxVal, left + right)
            
            return node.val == val ? max(left, right) + 1 : 0
        }
        
        dfs(root)
        
        return maxVal
    }
```
## 434. Number of Segments in a String


```swift
    func countSegments(_ s: String) -> Int {
        var s = Array(s)
        var count = 0
        
        for (i, char) in s.enumerated() {
            if (i == 0 || s[i - 1] == " ") && s[i] != " " {
                count += 1
            }
        }
        
        return count
    }
```
## 125. Valid Palindrome


```swift
    func isPalindrome(_ s: String) -> Bool {
        var s = s.map { String($0) }
        var valid = Set("1234567890abcdefghijklmnopqrstuvwxyz".map { String($0) })
        var left = 0
        var right = s.count - 1
        
        while left < right {
            let leftChar = s[left].lowercased()
            let rightChar = s[right].lowercased()
            
            if !valid.contains(leftChar) {
                left += 1
                continue
            } else if !valid.contains(rightChar) {
                right -= 1
                continue
            } else if leftChar != rightChar {
                return false
            }
            
            left += 1
            right -= 1
        }
        
        return true
    }
```
## 941. Valid Mountain Array


```swift
    func validMountainArray(_ A: [Int]) -> Bool {
        guard A.count >= 3 else { return false }
        guard A[0] < A[1] && A[A.count - 2] > A[A.count - 1] else { return false }
        var mountain = true
        
        for (i, num) in A.enumerated() where i > 0 {
            if mountain && num > A[i - 1] { continue }
            
            if mountain && num < A[i - 1] {
                mountain = false
            } else if num == A[i - 1] || num > A[i - 1] { 
                return false
            }
        }
        
        return !mountain
    }
```
## 189. Rotate Array


```swift
    func rotate(_ nums: inout [Int], _ k: Int) {
        let k = k % nums.count
        reverse(&nums, 0, nums.count - 1)
        reverse(&nums, 0, k - 1)
        reverse(&nums, k, nums.count - 1)
    }
    
    func reverse(_ nums: inout [Int], _ left: Int, _ right: Int) {
        var left = left
        var right = right
        
        while left < right {
            let temp = nums[left]
            nums[left] = nums[right]
            nums[right] = temp
            
            left += 1
            right -= 1
        }
    }
```
## 69. Sqrt(x)

```swift
    func mySqrt(_ x: Int) -> Int {
        guard x > 1 else { return x }
        var left = 2
        var right = (x / 2)
        
        while left <= right {
            let mid = ((right - left) / 2) + left
            var r = mid * mid
            
            if r == x {
                return mid
            } else if r > x {
                right = mid - 1
            } else {
                left = mid + 1
            }
        }
        
        return right
    }
```
## 157. Read N Characters Given Read4


```swift
    func read(_ buf: inout [Character], _ n: Int) -> Int {
        var i = 0
        
        while true {
            var arr = Array(repeating: Character(" "), count: 4)
            let charCount = read4(&arr)
            if charCount == 0 { break }
            
            for (count, char) in arr.enumerated() where count < charCount{
                if i >= n { break }
                buf[i] = char
                i += 1
            }
        }
        
        return i
    }
```
## 170. Two Sum III - Data structure design


```swift
class TwoSum {
    var dict: [Int: Int] = [:]
    
    init() {}
    
    func add(_ number: Int) {
        dict[number, default: 0] += 1
    }
    
    func find(_ value: Int) -> Bool {
        for (num, count) in dict {
            if let res = dict[value - num] {
                if num == value - num {
                    if count > 1 { return true }
                } else {
                    return true
                }
            }
        }
        
        return false
    }
}
```
## 475. Heaters


```swift
    func findRadius(_ houses: [Int], _ heaters: [Int]) -> Int {
        var heaters = heaters.sorted()
        var houses = houses.sorted()
        var result = 0
        var i = 0
        
        for house in houses {
            while i < heaters.count - 1 && abs(house - heaters[i]) >= abs(heaters[i + 1] - house) {
                i += 1
            }
            
            result = max(result, abs(heaters[i] - house))
        }
        
        return result
    }
```
## 58. Length of Last Word


```swift
    func lengthOfLastWord(_ s: String) -> Int {
        var count = 0
        
        for char in s.reversed() {
            if count > 0 && char == " " { return count }
            if char != " " { count += 1 }
        }
        
        return count
    }
```
## 633. Sum of Square Numbers


```swift
    func judgeSquareSum(_ c: Int) -> Bool {
        var left = 0
        var right = Int(Double(c).squareRoot())
        
        while left <= right {
            let sum = (left * left) + (right * right)
            
            if sum == c {
                return true
            } else if sum < c {
                left += 1
            } else {
                right -= 1
            }
        }
        
        return false
    }
```
## 532. K-diff Pairs in an Array


```swift
    func findPairs(_ nums: [Int], _ k: Int) -> Int {
        guard k >= 0 else { return 0 }
        var seen: Set<Int> = []
        var pairs: Set<[Int]> = []
        
        for num in nums {
            if (seen.contains(num + k) && !pairs.contains([min(num, num + k), max(num, num + k)])) {
                pairs.insert([min(num, num + k), max(num, num + k)])
            }
            if (seen.contains(num - k) && !pairs.contains([min(num, num - k), max(num, num - k)])) {
                pairs.insert([min(num, num - k), max(num, num - k)])
            }
            
            seen.insert(num)
        }
        
        return pairs.count
    }
```
## 204. Count Primes


```swift
    func countPrimes(_ n: Int) -> Int {
        guard n > 1 else { return 0 }
        var sieve = Array(repeating: true, count: n)
        var count = 0
        sieve[0] = false
        sieve[1] = false
        
        for i in 2..<n where sieve[i] {
            if sieve[i] == true {
                var multiple = 2
                count += 1
                while i * multiple < n {
                    sieve[i * multiple] = false
                    multiple += 1
                }
            }
        }
        
        return count
    }
```
## 605. Can Place Flowers


```swift
    func canPlaceFlowers(_ flowerbed: [Int], _ n: Int) -> Bool {
        let count = flowerbed.count
        var preZero = flowerbed[0] == 0
        var canPlant = 0
        
        for i in 0..<count {
            if flowerbed[i] == 0 {
                if preZero && (i == count - 1 || flowerbed[i + 1] == 0) {
                    canPlant += 1
                    preZero = false
                } else {
                    preZero = true
                }
            } else {
                preZero = false
            }
        }
        
        return n <= canPlant
    }
```
## 581. Shortest Unsorted Continuous Subarray


```swift
    func findUnsortedSubarray(_ nums: [Int]) -> Int {
        guard nums.count > 1 else { return 0 }
        let n = nums.count - 1
        var mn = nums[n]
        var mx = nums[0]
        var start = Int.min + 1
        var end = Int.min
        
        for i in 1...n {
          mx = max(mx, nums[i])
          mn = min(mn, nums[n - i])
            
          if mx > nums[i] { end = i }
          if mn < nums[n - i] { start = n - i }
        }

        return end - start + 1
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
## 665. Non-decreasing Array


```swift
    func checkPossibility(_ nums: [Int]) -> Bool {
        var count = 0
        var prev = nums[0]
        
        for index in 1..<nums.count {
            if prev > nums[index] {
                count += 1
                if count > 1 { return false }
                if index != 1 && nums[index - 2] > nums[index] { continue }
            }
            prev = nums[index]
        }
        
        return true
    }
```
## 1446. Consecutive Characters


```swift
    func maxPower(_ s: String) -> Int {
        var power = 1
        var currPower = 1
        var curr: Character = " "
        
        for char in s {
            if char == curr {
                currPower += 1
            } else {
                curr = char
                power = max(power, currPower)
                currPower = 1
            }
        }
        
        power = max(power, currPower)
        
        return power
    }
```
## 1450. Number of Students Doing Homework at a Given Time


```swift
    func busyStudent(_ startTime: [Int], _ endTime: [Int], _ queryTime: Int) -> Int {
        var result = 0
        
        for i in 0..<startTime.count {
            if startTime[i] <= queryTime && endTime[i] >= queryTime {
                result += 1
            }
        }
        
        return result
    }
```
## 1086. High Five


```swift
    func highFive(_ items: [[Int]]) -> [[Int]] {
        var idToAllScores: [Int: [Int]] = [:]
        var result: [[Int]] = []
        
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
            result.append([id, getAverageOfTopFiveScores(scores)])
        }
        
        return result.sorted { $0[0] < $1[0] }
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
## 1455. Check If a Word Occurs As a Prefix of Any Word in a Sentence


```swift
    func isPrefixOfWord(_ sentence: String, _ searchWord: String) -> Int {
        var words = sentence.map { String($0) }
        var searchWord = searchWord.map { String($0) }
        var j = 0
        var i = 0
        var count = 1
        
        while i < words.count {
            if words[i] == searchWord[j] {
                j += 1
                i += 1
                if j >= searchWord.count { return count }
            } else if words[i] == " " {
                count += 1
                j = 0
                i += 1
            } else {
                while i < words.count, words[i] != " " {
                    i += 1
                }
            }
        }
        
        return -1
    }
```
## 1464. Maximum Product of Two Elements in an Array


```swift
    func maxProduct(_ nums: [Int]) -> Int {
        guard nums.count > 2 else { return (nums[0] - 1) * (nums[1] - 1) }
        var n = max(nums[0], nums[1])
        var m = min(nums[0], nums[1])
        
        for i in 2..<nums.count {
            if nums[i] > n {
                m = n
                n = nums[i]
            } else if nums[i] > m {
                m = nums[i]
            }
        }
        
        return (n - 1) * (m - 1)
    }
```
## 1460. Make Two Arrays Equal by Reversing Sub-arrays


```swift
    func canBeEqual(_ target: [Int], _ arr: [Int]) -> Bool {
        var buckets1 = Array(repeating: 0, count: 1001)
        var buckets2 = Array(repeating: 0, count: 1001)
        
        for i in 0..<arr.count {
            buckets1[target[i]] += 1
            buckets2[arr[i]] += 1
        }
        
        for i in 0..<buckets1.count {
            if buckets1[i] != buckets2[i] {
                return false
            }
        }
        
        return true
    }
```
## 1469. Find All The Lonely Nodes


```swift
    func getLonelyNodes(_ root: TreeNode?) -> [Int] {
        var result: [Int] = []
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            
            if node.left != nil && node.right == nil {
                result.append(node.left!.val)
            } else if node.right != nil && node.left == nil {
                result.append(node.right!.val)
            }
            
            dfs(node.left)
            dfs(node.right)
        }
        
        dfs(root)
        return result
    }
```
## 1470. Shuffle the Array


```swift
    func shuffle(_ nums: [Int], _ n: Int) -> [Int] {
        var i = n - 1
        var j = nums.count - 1
        var nums = nums
        
        while j >= n {
            nums[j] <<= 10
            nums[j] |= nums[i]
            i -= 1
            j -= 1
        }

        i = 0
        j = n
        
        while j < nums.count {
            let num1 = nums[j] & 1023
            let num2 = nums[j] >> 10
            nums[i] = num1
            nums[i + 1] = num2
            i += 2    
            j += 1
        }

        return nums
    }
```
## 1474. Delete N Nodes After M Nodes of a Linked List


```swift
    func deleteNodes(_ head: ListNode?, _ m: Int, _ n: Int) -> ListNode? {
        var curr = head
        var m1 = m
        var n1 = n
        
        while curr != nil {
            if m1 > 1 {
                curr = curr?.next
                m1 -= 1
            } else if n1 > 0 {
                curr?.next = curr?.next?.next
                n1 -= 1
            } else {
                curr = curr?.next
                m1 = m
                n1 = n
            }
        }
        
        return head
    }
```
## 1480. Running Sum of 1d Array


```swift
    func runningSum(_ nums: [Int]) -> [Int] {
        var nums = nums
        
        for i in 1..<nums.count {
            nums[i] += nums[i - 1]
        }
        
        return nums
    }
```
## 1512. Number of Good Pairs


```swift
    func numIdenticalPairs(_ nums: [Int]) -> Int {
        var dict: [Int: Int] = [:]
        var result = 0
        
        for num in nums {
            dict[num, default: 0] += 1
        }
        
        for (key, count) in dict where count > 1 {
            result += (count * (count - 1)) / 2
        }
        
        return result
    }
```
## 1502. Can Make Arithmetic Progression From Sequence


```swift
    func canMakeArithmeticProgression(_ arr: [Int]) -> Bool {
        var arr = arr.sorted()
        var diff = arr[0] - arr[1]
        
        for i in 1..<arr.count - 1 {
            if arr[i] - arr[i + 1] != diff { return false }
        }
        
        return true
    }
```
## 1475. Final Prices With a Special Discount in a Shop


```swift
    func finalPrices(_ prices: [Int]) -> [Int] {
        var result = [Int](repeating:0 ,count: prices.count);
        
        for i in 0..<prices.count {
            var flag = 1;
            for j in i+1..<prices.count{
                if j>i && prices[j] <= prices[i] {
                    result[i] = prices[i]-prices[j]
                    flag = 0;
                    break;
                }
            }
            if flag == 1 { result[i] = prices[i] }
        }
        return result;
    }
```
## 1491. Average Salary Excluding the Minimum and Maximum Salary


```swift
    func average(_ salary: [Int]) -> Double {
        var minVal = Int.max
        var maxVal = Int.min
        var sum = 0
        
        for num in salary {
            sum += num
            
            maxVal = max(maxVal, num)
            minVal = min(minVal, num)
        }
        
        sum -= maxVal
        sum -= minVal
        
        return Double(sum) / Double(salary.count - 2)
    }
```
## 1518. Water Bottles


```swift
    func numWaterBottles(_ numBottles: Int, _ numExchange: Int) -> Int {
        var b = 0
        var empty = 0
        var numBottles = numBottles
        
        while numBottles >= 1 {
            b +=  numBottles
            empty += numBottles
            numBottles = empty / numExchange
            empty %= numExchange
        }
        
        return b
    }
```
## 1507. Reformat Date


```swift
    func reformatDate(_ date: String) -> String {
        var date = date.split(separator: " ").map { String($0) }
        var months = ["Jan" : "01", "Feb": "02", "Mar": "03", "Apr": "04", "May" : "05", "Jun": "06", "Jul": "07", "Aug": "08", "Sep": "09", "Oct": "10", "Nov": "11", "Dec": "12"]
        var d: [String] = []
        var m = months[date[1], default: ""]
        let y = date[2]
        
        for char in date[0] {
            if let _ = Int(String(char)) {
                d.append(String(char))
            }
        }
        
        if d.count < 2 { return "\(y)-\(m)-0\(d.joined())" }
        
        return "\(y)-\(m)-\(d.joined())"
    }
```
## 1496. Path Crossing


```swift
    func isPathCrossing(_ path: String) -> Bool {
        var seen: Set<String> = ["0-0"]
        var curr = [0, 0]
        
        for dir in path {
            if dir == "N" {
                curr[0] -= 1
            } else if dir == "S" {
                curr[0] += 1
            } else if dir == "W" {
                curr[1] -= 1
            } else {
                curr[1] += 1
            }
            
            if seen.contains("\(curr[0])-\(curr[1])") { return true }
            seen.insert("\(curr[0])-\(curr[1])")
        }
        
        return false
    }
```
## 1523. Count Odd Numbers in an Interval Range


```swift
    func countOdds(_ low: Int, _ high: Int) -> Int {
        return low % 2 == 0 && high % 2 == 0 ? (high - low) / 2 : (high - low) / 2 + 1
    }
```
## 1544. Make The String Great


```swift
    func makeGood(_ s: String) -> String {
        var result: [String] = []
        var s = s.map { String($0) }
        
        for char in s {
            if result.isEmpty {
                result.append(char)
                continue
            }
            
            let top = result[result.count - 1]
            if top != char && top.lowercased() == char.lowercased() {
                result.removeLast()
                continue
            }
            
            result.append(char)
        }
        
        return result.joined()
    }
```
## 1534. Count Good Triplets


```swift
    func countGoodTriplets(_ arr: [Int], _ a: Int, _ b: Int, _ c: Int) -> Int {
        var count = 0
        
        for i in 0..<arr.count - 2 {
            for j in i + 1..<arr.count - 1 {
                for k in j + 1..<arr.count {
                    if abs(arr[i] - arr[j]) <= a && abs(arr[j] - arr[k]) <= b && abs(arr[i] - arr[k]) <= c {
                        count += 1
                    }
                }
            }
        }
        return count
    }
```
## 1539. Kth Missing Positive Number


```swift
    func findKthPositive(_ arr: [Int], _ k: Int) -> Int {
        var seen = Set(arr)
        var missing = 0
        
        for num in 1...2000 where !seen.contains(num) {
            missing += 1
            if missing == k {
                return num
            }
        }
        
        return missing
    }
```
## 1389. Create Target Array in the Given Order


```swift
    func createTargetArray(_ nums: [Int], _ index: [Int]) -> [Int] {
        var result: [Int] = []
        
        for i in 0..<nums.count {
            if result.isEmpty || i == index[i] {
                result.append(nums[i])
            } else {
                result.insert(nums[i], at: index[i])
            }
        }
        
        return result
    }
```
## 108. Convert Sorted Array to Binary Search Tree


```swift
    func sortedArrayToBST(_ nums: [Int]) -> TreeNode? {
        if nums.isEmpty { return nil }
        
        func placeNumber(_ low: Int, _ high: Int) -> TreeNode? {
            guard low <= high else { return nil }
            let mid = (high + low) / 2
            let node = TreeNode(nums[mid])
            
            node.left = placeNumber(low, mid - 1)
            node.right = placeNumber(mid + 1, high)
            
            return node
        }
        
        return placeNumber(0, nums.count - 1)
    }
```
## 168. Excel Sheet Column Title


```swift
    func convertToTitle(_ n: Int) -> String {
        var num = n
        var res: [String] = []

        while num != 0 {
            let uni = (num - 1) % 26
            num = (num - 1) / 26
            let cha = String(Character(UnicodeScalar(uni + 65)!))
            res.append(cha)
        }
        return res.reversed().joined()
    }
```
## 1502. Can Make Arithmetic Progression From Sequence


```swift
    func canMakeArithmeticProgression(_ arr: [Int]) -> Bool {
        guard arr.count > 2 else { return true }
        
        var seen: Set<Int> = []
        var maxVal = Int.min
        var minVal = Int.max
        
        for num in arr {
            maxVal = max(num, maxVal)
            minVal = min(num, minVal)
            seen.insert(num)
        }
        
        let d = (maxVal - minVal) / (arr.count - 1)
        
        for i in 1...arr.count where !seen.contains(minVal + (i - 1) * d) {
            return false
        }
        
        return true
    }
```
## 1475. Final Prices With a Special Discount in a Shop


```swift
    func finalPrices(_ prices: [Int]) -> [Int] {
        var result = prices
        var stack: [Int] = []
        
        for (i, price) in prices.enumerated() {
            while !stack.isEmpty, prices[stack[stack.count - 1]] >= price {
                result[stack.removeLast()] -= price
            }
            stack.append(i)
        }
        
        return result
    }
```
## 665. Non-decreasing Array


```swift
    func checkPossibility(_ nums: [Int]) -> Bool {
        var index: Int?
        
        for i in 0..<nums.count - 1 {
            if nums[i] > nums[i + 1] {
                if index != nil { return false }
                index = i
            }     
        }
        
        return (index == nil ||
                index! == 0 ||
                index! == nums.count - 2 ||
                nums[index! - 1] <= nums[index! + 1] ||
                nums[index!] <= nums[index! + 2])
    }
```
## 1550. Three Consecutive Odds


```swift
    func threeConsecutiveOdds(_ arr: [Int]) -> Bool {
        guard arr.count > 2 else { return false }
        
        for i in 0..<arr.count - 2 {
            if arr[i] % 2 == 1 && arr[i + 1] % 2 == 1 && arr[i + 2] % 2 == 1 {
                return true
            }
        }
        
        return false
    }
```
## 339. Nested List Weight Sum


```swift
    func depthSum(_ nestedList: [NestedInteger], _ depth: Int = 1) -> Int {
        var sum = 0
        
        for list in nestedList {
            if list.isInteger() {
                sum += (list.getInteger() * depth)
            } else {
                sum += depthSum(list.getList(), depth + 1)
            }
        }
        
        return sum
    }
```
## 728. Self Dividing Numbers


```swift
    func selfDividingNumbers(_ left: Int, _ right: Int) -> [Int] {
        var result: [Int] = []
        
        for num in left...right {
            var n = num
            
            while n > 0 {
                let digit = n % 10
                
                if digit != 0, num % digit == 0 { 
                    n /= 10
                } else {
                    break
                }
            }
            
            if n == 0 { result.append(num) }
        }
        
        return result
    }
```
## 1085. Sum of Digits in the Minimum Number


```swift
    func sumOfDigits(_ A: [Int]) -> Int {
        var num = A.min() ?? 0
        var sum = 0
        
        while num > 0 {
            sum += num % 10
            num /= 10
        }
        
        return sum % 2 == 0 ? 1 : 0
    }
```
## 1025. Divisor Game


```swift
    func divisorGame(_ N: Int) -> Bool {
        return N % 2 == 0
    }
```
## 276. Paint Fence


```swift
    func numWays(_ n: Int, _ k: Int) -> Int {
        guard n > 0 else { return 0 }
        guard n > 1 else { return k }
        
        var diffCount = k * (k - 1)
        var colorCount = k
        
        for i in 2..<n {
            let temp = diffCount
            diffCount = (diffCount + colorCount) * (k - 1)
            colorCount = temp
        }
        return diffCount + colorCount
    }
```
## 706. Design HashMap


```swift
class MyHashMap {
    var elements: [LinkedList?]
    var loadFactor = 0.75
    var count = 0
    
    init() {
        self.elements = Array(repeating: nil, count: 10)
    }
    
    func put(_ key: Int, _ value: Int) {
        let index = hash(key)
        if elements[index] == nil {
            elements[index] = LinkedList()
        }
        elements[index]!.insert(key, value)
        count += 1
        
        if shouldReHash() { rehash() }
    }
    
    func get(_ key: Int) -> Int {
        let index = hash(key)
        return elements[index] != nil ? elements[index]!.get(key) : -1
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    func remove(_ key: Int) {
        let index = hash(key)
        guard let list = elements[index] else { return }
        if list.remove(key) { count -= 1 }
        
    }
    
    func hash(_ key: Int) -> Int {
        return key % elements.count
    }
    
    func shouldReHash() -> Bool {
        return Double(count) / Double(elements.count) >= loadFactor
    }
    
    func rehash() {
        let old = elements
        elements = Array(repeating: nil, count: old.count * 2)
        count = 0
        for list in old {
            var head = list?.head
            
            while head != nil {
                put(head!.key, head!.val)
                head = head?.next
            }
            
        }   
    }
}

class LinkedList {
    var head: Node?
    
    init() {
        self.head = nil
    }
    
    func get(_ key: Int) -> Int {
        var curr = head
        
        while curr != nil {
            if curr!.key == key {
                return curr!.val
            }
            curr = curr?.next
        }
        
        return -1
    }
    
    func insert(_ key: Int, _ val: Int) {
        if get(key) != -1 { 
            var curr = head
            
            while curr != nil {
                if curr!.key == key {
                    curr?.val = val
                    return
                }
                curr = curr?.next
            }
        } else {
            let node = Node(key, val)
            node.next = head
            head = node
        }
    }
    
    func remove(_ key: Int) -> Bool {
        if head == nil { return false }
        if head!.key == key {
            head = head?.next
            return true
        }
        let dummy: Node? = Node(0,0)
        dummy?.next = head
        var curr = dummy
        
        while curr?.next != nil {
            if curr?.next!.key == key {
                curr?.next = curr?.next?.next
                return true
            }
            
            curr = curr?.next
        }
        
        return false
    }   
}

class Node {
    var key: Int
    var val: Int
    var next: Node? = nil
    
    init(_ key: Int, _ val: Int) {
        self.key = key
        self.val = val
    }
}
```
# AVLTree Solution
```swift
class MyHashMap {
    var elements: [AVLTree?]
    var loadFactor = 0.75
    var count = 0
    
    init() {
        self.elements = Array(repeating: nil, count: 10)
    }
    
    func put(_ key: Int, _ value: Int) {
        let index = hash(key)
        if elements[index] == nil {
            elements[index] = AVLTree()
        }
        elements[index]!.insert(key, value)
        count += 1
        
        if shouldReHash() { rehash() }
    }
    
    func get(_ key: Int) -> Int {
        let index = hash(key)
        guard let tree = elements[index] else { return -1 }
        guard let result = tree.get(key) else { return -1 }
        return result.val
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    func remove(_ key: Int) {
        let index = hash(key)
        guard var tree = elements[index] else { return }
        if tree.remove(key) { count -= 1 }
    }
    
    func hash(_ key: Int) -> Int {
        return key % elements.count
    }
    
    func shouldReHash() -> Bool {
        return Double(count) / Double(elements.count) >= loadFactor
    }
    
    func rehash() {
        let old = elements
        elements = Array(repeating: nil, count: old.count * 2)
        count = 0
        for tree in old where tree != nil {
            let nodes = tree!.getNodes()
            
            for node in nodes {
                put(node.key, node.val)
            }
        }   
    }
}

class AVLNode {
    var key: Int
    var val: Int
    var left: AVLNode?
    var right: AVLNode?
    var height = 0
    
    var balanceFactor: Int {
        return leftHeight - rightHeight
    }
    
    var leftHeight: Int {
        return left?.height ?? -1
    }
    
    var rightHeight: Int {
        return right?.height ?? -1
    }
    
    var min: AVLNode {
        return left?.min ?? self
    }
    
    init(_ key: Int, _ val: Int) {
        self.key = key
        self.val = val
    }
}

class AVLTree {
    var root: AVLNode? = nil
}

extension AVLTree {
    func leftRotate(_ node: AVLNode) -> AVLNode {
        let pivot = node.right!
        node.right = pivot.left
        pivot.left = node
        
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
        
        return pivot
    }
    
    func rightRotate(_ node: AVLNode) -> AVLNode {
        let pivot = node.left!
        node.left = pivot.right
        pivot.right = node
        
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
        
        return pivot
    }
    
    func leftRightRotate(_ node: AVLNode) -> AVLNode {
        guard let left = node.left else { return node }
        node.left = leftRotate(left)
        return rightRotate(node)
    }
    
    func rightLeftRotate(_ node: AVLNode) -> AVLNode {
        guard let right = node.right else { return node }
        node.right = rightRotate(right)
        return leftRotate(node)
    }
    
    func balance(_ node: AVLNode) -> AVLNode {
        switch node.balanceFactor {
        case 2:
            if let left = node.left, left.balanceFactor == -1 {
                return leftRightRotate(node)
            } else {
                return rightRotate(node)
            }
        case -2:
            if let right = node.right, right.balanceFactor == 1 {
                return rightLeftRotate(node)
            } else {
                return leftRotate(node)
            }
        default:
            return node
        }
    }
    
    func insert(_ key: Int, _ val: Int) {
        if let node = get(key) {
            node.val = val
        } else {
            root = _insert(from: root, key, val)   
        }
    }
    
    func _insert(from node: AVLNode?, _ key: Int, _ val: Int) -> AVLNode? {
        guard let node = node else { return AVLNode(key, val) }
        
        if key < node.key {
            node.left = _insert(from: node.left, key, val)
        } else {
            node.right = _insert(from: node.right, key, val)
        }
        
        let balancedNode = balance(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
    
    func remove(_ key: Int) -> Bool {
        guard get(key) != nil else { return false }
        root = _remove(from: root, key)
        return true
    }
    
    func _remove(from node: AVLNode?, _ key: Int) -> AVLNode? {
        guard let node = node else { return nil }
        
        if key == node.key {
            if node.left == nil && node.right == nil { 
                return nil 
            } else if node.left == nil {
                return node.right
            } else if node.right == nil {
                return node.left
            } else {
                let rightMin = node.right!.min
                node.key = rightMin.key
                node.val = rightMin.val
                node.right = _remove(from: node.right, node.key)
            }
        } else if key < node.key {
            node.left = _remove(from: node.left, key)
        } else {
            node.right = _remove(from: node.right, key)
        }
        
        let balancedNode = balance(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
    
    func get(_ key: Int) -> AVLNode? {
        var curr = root
        
        while let node = curr {
            if key == node.key {
                return node
            } else if key < node.key {
                curr = node.left
            } else {
                curr = node.right
            }
        }
        
        return nil
    }
    
    func getNodes() -> [AVLNode] {
        var result: [AVLNode] = []
        
        dfs(root, &result)
        
        return result
    }
    
    func dfs(_ root: AVLNode?, _ result: inout [AVLNode]) {
        guard let node = root else { return }
        
        dfs(node.left, &result)
        result.append(node)
        dfs(node.right, &result)
    }
} 
```
## 551. Student Attendance Record I


```swift
    func checkRecord(_ s: String) -> Bool {
        var aCount = 0
        var lCount = 0
        
        for (i, char) in s.enumerated() {
            if char == "L" {
                lCount += 1
                if lCount > 2 { return false  }
                continue
            }
            
            if char == "A" { 
                aCount += 1
                if aCount > 1 { return false }
            }
            
            lCount = 0
        }
        
        return true
    }
```
## 1556. Thousand Separator


```swift
    func thousandSeparator(_ n: Int) -> String {
        guard n > 0 else { return "0" }
        var count = 0
        var n = n
        var result: [String] = []
        
        while n > 0 {
            let digit = n % 10
            n /= 10
            if count == 3 {
                result.append(".")
                count = 0
            }
            result.append("\(digit)")
            count += 1
        }
        
        return result.reversed().joined()
    }
```
## 1037. Valid Boomerang


```swift
    func isBoomerang(_ points: [[Int]]) -> Bool {
        let a = points[0]
        let b = points[1]
        let c = points[2]
        
        return (b[1] - a[1]) * (c[0] - a[0]) != (b[0] - a[0]) * (c[1] - a[1])
    }
```
## 172. Factorial Trailing Zeroes


```swift
    func trailingZeroes(_ n: Int) -> Int {
        var n = n
        var count = 0
        
        while n >= 5 { 
            count += n / 5
            n /= 5
        }
        
        return count
    }
```
## 706. Design HashMap
# Qyadratic proving
```swift

class MyHashMap {
    var elements: [[Int]] = Array(repeating: [], count: 10)
    let loadFactor = 0.49
    var count = 0
    
    init() {}
    
    func put(_ key: Int, _ value: Int) {
        var index = hash(key)
        var power = 0
        
        while !elements[index].isEmpty {
            if elements[index][0] == key { break }
            index += power * power
            index %= elements.count
            power += 1
        }
        
        if elements[index].isEmpty { count += 1 }
        elements[index] = [key, value]
        
        if shouldRehash() { rehash() }
    }
    
    func get(_ key: Int) -> Int {
        var index = hash(key)
        var power = 0
        
        while !elements[index].isEmpty {
            if elements[index][0] == key { 
                return elements[index][1]
            }
            index += power * power
            index %= elements.count
            power += 1
        }
        
        return -1
    }
    
    func remove(_ key: Int) {
        var index = hash(key)
        var power = 0
        
        while !elements[index].isEmpty {
            if elements[index][0] == key { 
                elements[index] = [-1, -1]
                count -= 1
                return 
            }
            index += power * power
            index %= elements.count
            power += 1
        }
    }
    
    func hash(_ key: Int) -> Int {
        return key % elements.count
    }
    
    func shouldRehash() -> Bool {
        return Double(count) / Double(elements.count) >= loadFactor
    }
    
    func rehash() {
        var old = elements
        elements = Array(repeating: [], count: old.count * 2)
        
        for element in old where !element.isEmpty && element[0] != -1 {
            put(element[0], element[1])
        }
    }
}
```
## 705. Design HashSet


```swift

class MyHashSet {
    var elements: [Int?] = Array(repeating: nil, count: 10)
    let loadFactor = 0.49
    var count = 0
    
    init() {}
    
    func add(_ key: Int) {
        var index = hash(key)
        var power = 0
        
        while elements[index] != nil {
            if elements[index] == key { return }
            index += power * power
            index %= elements.count
            power += 1
        }
        
        count += 1 
        elements[index] = key
        
        if shouldRehash() { rehash() }
    }
    
    func remove(_ key: Int) {
        var index = hash(key)
        var power = 0
        
        while elements[index] != nil {
            if elements[index] == key { 
                elements[index] = -1
                count -= 1
                return 
            }
            index += power * power
            index %= elements.count
            power += 1
        }
    }
    
    func contains(_ key: Int) -> Bool {
        var index = hash(key)
        var power = 0
        
        while elements[index] != nil {
            if elements[index] == key { return true }
            index += power * power
            index %= elements.count
            power += 1
        }
        
        return false
    }
    
    func hash(_ key: Int) -> Int {
        return key % elements.count
    }
    
    func shouldRehash() -> Bool {
        return Double(count) / Double(elements.count) >= loadFactor
    }
    
    func rehash() {
        var old = elements
        elements = Array(repeating: nil, count: old.count * 2)
        
        for element in old where element != nil && element != -1 {
            add(element!)
        }
    }
}

```
## 1010. Pairs of Songs With Total Durations Divisible by 60


```swift
    func numPairsDivisibleBy60(_ times: [Int]) -> Int {
        var dict: [Int: Int] = [:]
        var count = 0
        
        for time in times {
            var mod = time % 60
            let diff = 60 - mod
            if let val = dict[diff] { count += val }
            mod = mod != 0 ? mod : 60
            dict[mod, default: 0] += 1
        }
        
        return count
    }
```
## 144. Binary Tree Preorder Traversal


```swift
    func preorderTraversal(_ root: TreeNode?) -> [Int] {
        var stack: [TreeNode?] = [root]
        var result:[Int] = []
        
        while !stack.isEmpty {
            guard let node = stack.removeLast() else { continue }
            
            result.append(node.val)
            
            stack.append(node.right)
            stack.append(node.left)
        }
        
        return result
    }
```
## 94. Binary Tree Inorder Traversal


```swift
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        var stack: [TreeNode?] = []
        var result: [Int] = []
        var curr = root
        
        while curr != nil || !stack.isEmpty {
            while curr != nil {
                stack.append(curr)
                curr = curr?.left
            }
            
            guard let node = stack.removeLast() else { continue }
            
            result.append(node.val)
            curr = node.right
        }
        
        return result
    }
```
## 145. Binary Tree Postorder Traversal


```swift
    func postorderTraversal(_ root: TreeNode?) -> [Int] {
        var curr = root
        var stack: [TreeNode?] = []
        var result: [Int] = []
        
        while curr != nil || !stack.isEmpty {
            while curr != nil {
                stack.append(curr)
                curr = curr?.left
            }
            
            var temp = stack[stack.count - 1]?.right
            if temp != nil {
                curr = temp
                continue
            }
            
            while !stack.isEmpty && stack[stack.count - 1]?.right === temp {
                temp = stack.removeLast()
                if let node = temp {
                    result.append(node.val)   
                }
            }
        }
        
        return result
    }
```
## 783. Minimum Distance Between BST Nodes


```swift
    func minDiffInBST(_ root: TreeNode?) -> Int {
        var diff = Int.max
        var pre: Int? = nil
        
        func dfs(_ root: TreeNode?) {
            guard let node = root else { return }
            
            dfs(node.left)
            if pre != nil {  diff = min(diff, node.val - pre!) }
            pre = node.val
            dfs(node.right)
        }
        
        dfs(root)
        
        return diff
    }
```
## 1534. Count Good Triplets


```swift
    func countGoodTriplets(_ arr: [Int], _ a: Int, _ b: Int, _ c: Int) -> Int {
        var count = 0
        
        for i in 0..<arr.count - 2 {
            for j in i + 1..<arr.count - 1 {
                if abs(arr[i] - arr[j]) > a { continue }
                for k in j + 1..<arr.count {
                    if abs(arr[j] - arr[k]) <= b &&
                       abs(arr[i] - arr[k]) <= c {
                       count += 1
                    }
                }
            }
        }
        
        return count
    }
```
## 674. Longest Continuous Increasing Subsequence


```swift
    func findLengthOfLCIS(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        var maxCount = 0
        var curr = 1
        
        for i in 1..<nums.count {
            if nums[i - 1] < nums[i] {
                curr += 1
            } else {
                maxCount = max(maxCount, curr)
                curr = 1
            }
        }
        
        maxCount = max(maxCount, curr)
        
        return maxCount
    }
```
## 849. Maximize Distance to Closest Person


```swift
    func maxDistToClosest(_ seats: [Int]) -> Int {
        var start = 0
        var maxDis = -1
        
        for end in 1..<seats.count {
            if seats[end] != 1 { continue }
            
            if seats[start] != 1 { 
                maxDis = end
            } else {
                maxDis = max(maxDis, (end - start) / 2)
            }
            
            start = end
        }
        
        if maxDis == -1 {
            maxDis = seats.count - 1
        } else if seats[seats.count - 1] == 0 {
            maxDis = max(maxDis, seats.count - 1 - start)
        }
        
        return maxDis
    }
```
## 1426. Counting Elements

```swift
    func countElements(_ arr: [Int]) -> Int {
        let set = Set(arr)
        var count = 0
        
        for num in arr {
            if set.contains(num + 1) { count += 1 }
        }
        
        return count
    }
```
## 1137. N-th Tribonacci Number


```swift
    func tribonacci(_ n: Int) -> Int {
        guard n != 0 else { return 0 }
        guard n > 2 else { return 1 }
        
        var t1 = 0
        var t2 = 1
        var t3 = 1
        
        for _ in 3..<n {
            let temp = t3
            t3 = t1 + t2 + t3
            t1 = t2
            t2 = temp
        }
        
        
        return t1 + t2 + t3
    }
```
## 1265. Print Immutable Linked List in Reverse


```swift
/**
 * Definition for ImmutableListNode.
 * public class ImmutableListNode {
 *     public func printValue() {}
 *     public func getNext() -> ImmutableListNode? {}
 * }
 */

class Solution {
    func printLinkedListInReverse(_ head: ImmutableListNode?) {
        let count = listCount(head)
        var stack: [ImmutableListNode?] = []
        var offset = Int(count.squareRoot())
        var curr = head
        
        while curr != nil {
            stack.append(curr)
            for _ in 0..<offset {
                curr = curr?.getNext()
            }
        }
        
        while !stack.isEmpty {
            printReverse(stack.removeLast(), offset)
        }
    }
    
    func printReverse(_ node: ImmutableListNode?, _ offset: Int) {
        var stack: [ImmutableListNode?] = []
        var offset = offset
        var curr = node
        
        while curr != nil && offset > 0 {
            stack.append(curr)
            curr = curr?.getNext()
            offset -= 1
        }
        
        while !stack.isEmpty {
            stack.removeLast()?.printValue()
        }
    }
    
    func listCount(_ node: ImmutableListNode?) -> Double {
        var count: Double = 0
        var curr = node
        
        while curr != nil {
            count += 1
            curr = curr?.getNext()
        }
        
        return count
    }
}
```
## 1287. Element Appearing More Than 25% In Sorted Array


```swift
    func findSpecialInteger(_ arr: [Int]) -> Int {
        var percentage = Int(Double(arr.count) * 0.25)
        var pre = -1
        
        for i in 0..<arr.count - percentage where arr[i] != pre {
            if arr[i] == arr[i + percentage] { return arr[i] }
            pre = arr[i]
        }
        
        return 0
    }
```
## 1317. Convert Integer to the Sum of Two No-Zero Integers


```swift
    func getNoZeroIntegers(_ n: Int) -> [Int] {
        for i in 1...n / 2 {
            let curr = n - i
            if hasZero(curr) && hasZero(i) { return [i, curr] }
        }
        
        return []
    }
    
    func hasZero(_ n: Int) -> Bool {
        var n = n
        
        while n != 0 {
            if n % 10 == 0 { return false }
            n /= 10
        }
        
        return true
    }
```
## 594. Longest Harmonious Subsequence


```swift
    func findLHS(_ nums: [Int]) -> Int {
        var maxCount = 0
        var dict: [Int: Int] = [:]
        
        for num in nums {
            dict[num, default: 0] += 1
        }
        
        for (num, count) in dict {
            var length = count
            
            length = max(count + dict[num - 1, default: 0], count + dict[num + 1, default: 0])
            
            if count != length {
                maxCount = max(maxCount, length)   
            }
        }
        
        return maxCount
    }
```
## 645. Set Mismatch


```swift
    func findErrorNums(_ nums: [Int]) -> [Int] {
        var nums = nums
        var missing = 0
        var dup = 0
        
        for num in nums {
            if nums[abs(num) - 1] < 1 {
                dup = abs(num)
            } else {
                nums[abs(num) - 1] *= -1   
            }
        }
        
        for i in 0..<nums.count {
            if nums[i] > 0 {
                missing = i + 1
            }
        }
        
        return [dup, missing]
    }
```
## 661. Image Smoother


```swift
    func imageSmoother(_ M: [[Int]]) -> [[Int]] {
        var result: [[Int]] = Array(repeating: Array(repeating: 0, count: M[0].count), count: M.count)
        
        for row in 0..<M.count {
            for col in 0..<M[0].count {
                var dirs = [[0,1],[0,-1],[1,0],[-1,0],[-1,-1],[1,1],[-1,1],[1,-1]]
                var sum = M[row][col]
                var count = 1
                
                for dir in dirs {
                    let r = row + dir[0]
                    let c = col + dir[1]
                    
                    if r >= 0 && c >= 0 && r < M.count && c < M[0].count{
                        sum += M[r][c]
                        count += 1
                    }
                }
                
                result[row][col] = sum / count
            }
        }
        
        return result
    }
```
## 830. Positions of Large Groups


```swift
    func largeGroupPositions(_ S: String) -> [[Int]] {
        var result: [[Int]] = []
        var pre = ""
        var count = 0
        var start = 0
        
        for (i, char) in S.enumerated() {
            let curr = String(char)
            
            if pre == curr {
                count += 1
            } else {
                if count > 2 { result.append([start, i - 1]) }
                count = 1
                pre = curr
                start = i
            }
        }
        
        if count > 2 { result.append([start, S.count - 1]) }
        
        return result
    }
```
## 1413. Minimum Value to Get Positive Step by Step Sum


```swift
        var minVal = 0
        var sum = 0
        for num in nums {
            sum += num
            minVal = min(sum, minVal)
        }
        
        return minVal < 0 ? abs(minVal) + 1 : 1
    }
```
## 1029. Two City Scheduling


```swift
    func twoCitySchedCost(_ costs: [[Int]]) -> Int {
        var costs = costs.sorted() { $0[0] - $0[1] < $1[0] - $1[1] }
        var buckets: [[[Int]]] = Array(repeating: [], count: 2001)
        var result = 0
        var count = costs.count / 2
        
        for cost in costs {
            let diff = cost[0] - cost[1]
            if diff < 0 {
                buckets[1000 + diff].append([cost[0], cost[1]]) 
            } else {
                buckets[diff + 1000].append([cost[0], cost[1]]) 
            }
        }
        
        for (diff, cost) in buckets.enumerated() where !cost.isEmpty {
            var cost = cost
            
            while !cost.isEmpty {
                if count > 0 {
                    result += cost.removeLast()[0]
                } else {
                    result += cost.removeLast()[1]
                }
                count -= 1
            }
        }
        
        return result
    }
```
## 1576. Replace All ?'s to Avoid Consecutive Repeating Characters


```swift
    func modifyString(_ s: String) -> String {
        guard s.count > 0 else { return s == "?" ? "a" : s }
        var s = s.map { String($0) }
        var chars = ["a", "b", "c"]
        
        for i in 0..<s.count {
            if s[i] == "?" {
                for char in chars {
                    if (i == 0 || s[i - 1] != char) && (i == s.count - 1 || s[i + 1] != char) {
                        s[i] = char   
                    }
                }
            }
        }
        
        return s.joined()
    }
```
## 1572. Matrix Diagonal Sum


```swift
    func diagonalSum(_ mat: [[Int]]) -> Int {
        var left = 0
        var right = mat.count - 1
        var row = 0
        var sum = 0
        
        while left < mat.count && right >= 0 {
            if left == right {
               sum += mat[row][left]
            } else {
                sum += mat[row][left]
                sum += mat[row][right]
            }
            row += 1
            left += 1
            right -= 1
        }
        
        return sum
    }
```
## 509. Fibonacci Number


```swift
    var memo: [Int: Int] = [:]
    
    func fib(_ N: Int) -> Int {
        guard N > 0 else { return 0 }
        guard N > 1 else { return 1 }
        
        if let val = memo[N] {
            return val
        } else {
            memo[N] = fib(N - 1) + fib(N - 2)
        }
        
        return memo[N, default: 0]
    }

    func fib(_ N: Int) -> Int {
        guard N > 0 else { return 0 }
        guard N > 1 else { return 1 }
        
        var dp = Array(repeating: 0, count: N + 1)
        
        dp[0] = 0
        dp[1] = 1
        
        for i in 2...N {
            dp[i] = dp[i - 1] + dp[i - 2]
        }
        
        return dp[N]
    }

    func fib(_ N: Int) -> Int {
        guard N > 0 else { return 0 }
        guard N > 1 else { return 1 }
        
        var f1 = 0
        var f2 = 1
        
        for _ in 2...N {
            let temp = f2
            f2 = f1 + f2
            f1 = temp
        }
        
        return f2
    }
```
## 268. Missing Number


```swift
    func missingNumber(_ nums: [Int]) -> Int {
        var sum = nums.reduce(0, +)
        var expectedSum = nums.c ount * (nums.count + 1) / 2
        
        return  expectedSum - sum
    }
    
    func missingNumber(_ nums: [Int]) -> Int {
        var missing = 0
        
        for num in nums {
            missing ^= num
        }
        
        for i in 0...nums.count {
            missing ^= i
        }
        
        return missing
    }
    
        func missingNumber(_ nums: [Int]) -> Int {
        var nums = nums
        
        for num in nums where num < nums.count {
            nums[num] = -1
        }
        
        for i in 0..<nums.count {
            if nums[i] != -1 { return i }
        }
        
        return nums.count
    }
```
## 482. License Key Formatting


```swift
    func licenseKeyFormatting(_ S: String, _ K: Int) -> String {
        var s = S.map { String($0) }
        var result: [String] = []
        var size = 0
        
        for char in s.reversed() where char != "-" {
            if size == K {
                size = 0
                result.append("-")
            }
            
            result.append(char.uppercased())
            size += 1
        }
        
        return result.reversed().joined()
    }
```
## 617. Merge Two Binary Trees


```swift
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        guard t2 != nil else { return t1 }
        guard t1 != nil else { return t2 }
        
        t1!.val += t2!.val
        t1?.left = mergeTrees(t1?.left, t2?.left)
        t1?.right = mergeTrees(t1?.right, t2?.right)
        
        return t1
    }
```
```swift
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        guard t1 != nil else { return t2 }
        var stack = [(t1, t2)]
        
        while !stack.isEmpty {
            let tuple = stack.removeLast()
            var node1 = tuple.0
            var node2 = tuple.1
            if node1 == nil || node2 == nil { continue }
            
            node1!.val += node2!.val
            
            if node1?.left == nil {
                node1?.left = node2?.left
            } else {
                stack.append((node1?.left, node2?.left))
            }
            
            if node1?.right == nil {
                node1?.right = node2?.right
            } else {
                stack.append((node1?.right, node2?.right))
            }
        }
        
        return t1
    }
```
## 690. Employee Importance


```swift
    func getImportance(_ employees: [Employee], _ id: Int) -> Int {
        var sum = 0
        var dict: [Int: Employee] = [:]
        var queue: [Employee] = []
        
        for employee in employees {
            dict[employee.id] = employee
        }
        
        queue.append(dict[id]!)
        
        while !queue.isEmpty {
            let employee = queue.removeLast()
            for subordinate in employee.subordinates {
                queue.append(dict[subordinate]!)
            }
            
            sum += employee.importance
        }
        
        return sum
    }
```
## 1556. Thousand Separator


```swift
    func thousandSeparator(_ n: Int) -> String {
        guard n >= 1000 else { return "\(n)" }
        var result: [String] = []
        var n = n
        var count = 0
        
        while n > 0 {
            count += 1
            let digit = n % 10
            n /= 10
            
            if count == 4 { 
                result.append(".")
                count = 1
            }
            result.append("\(digit)")
        }
        
        return result.reversed().joined()
    }
```
## 1282. Group the People Given the Group Size They Belong To


```swift
    func groupThePeople(_ groupSizes: [Int]) -> [[Int]] {
        var dict: [Int: [Int]] = [:]
        var result: [[Int]] = []
        
        for (i, size) in groupSizes.enumerated() {
            dict[size, default: []].append(i)
            if let group = dict[size], group.count == size {
                result.append(group)
                dict[size] = []
            }
        }
        
        return result
    }
```
## 1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree


```python3
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def dfs(node1: TreeNode, node2: TreeNode) -> TreeNode:
            if (not node1): return None
            
            if (node1 == target): return node2
            
            return dfs(node1.left, node2.left) or dfs(node1.right, node2.right)
            
        return dfs(original, cloned)
```
## 1302. Deepest Leaves Sum


```swift 
    func deepestLeavesSum(_ root: TreeNode?) -> Int {
        var sum = 0
        var maxHeight = 1
        
        func dfs(_ root: TreeNode?, _ height: Int) {
            guard let node = root else { return }
            
            if height > maxHeight { 
                maxHeight = height
                sum = 0
            }
            
            if height == maxHeight { sum += node.val }
            
            dfs(node.left, height + 1)
            dfs(node.right, height + 1)
        }
        
        dfs(root, 1)
        
        return sum
    }
```
## 807. Max Increase to Keep City Skyline


```swift
    func maxIncreaseKeepingSkyline(_ grid: [[Int]]) -> Int {
        var maxRow = Array(repeating: 0, count: grid.count)
        var maxCol = Array(repeating: 0, count: grid[0].count)
        var sum = 0
        
        for row in 0..<grid.count {
            for col in 0..<grid[0].count {
                maxRow[row] = max(maxRow[row], grid[row][col])
                maxCol[col] = max(maxCol[col], grid[row][col])
            }
        }
        
        for row in 0..<grid.count {
            for col in 0..<grid[0].count {
                sum += min(maxRow[row], maxCol[col]) - grid[row][col]
            }
        }
        
        return sum
    }
```
## 700. Search in a Binary Search Tree


```swift

    func searchBST(_ root: TreeNode?, _ val: Int) -> TreeNode? {
        guard let node = root else { return nil }
        
        if node.val == val { 
            return node
        } else if node.val < val {
            return searchBST(node.right, val)
        } else {
            return searchBST(node.left, val)
        }
    }

    func searchBST(_ root: TreeNode?, _ val: Int) -> TreeNode? {
        var curr = root
        
        while curr != nil {
            if curr!.val == val { 
                return curr
            } else if curr!.val < val {
                curr = curr?.right
            } else {
                curr = curr?.left
            }
        }
        
        return nil
    }
```
## 1490. Clone N-ary Tree


```swift
    func cloneTree(_ root: Node?) -> Node? {
        guard let node = root else { return nil }
        
        func dfs(_ node: Node) -> Node {
            let newNode = Node(node.val)
            
            for child in node.children {
                newNode.children.append(dfs(child))
            }
        
            return newNode
        }
        
        return dfs(node)
    }
```
## 160. Intersection of Two Linked Lists


```swift
    func getIntersectionNode(_ headA: ListNode?, _ headB: ListNode?) -> ListNode? {
        var count1 = 0
        var count2 = 0
        var node1 = headA
        var node2 = headB
        var bigger = true
        var diff = 0
        
        while node1 != nil || node2 != nil {
            if node1 != nil {
                count1 += 1
                node1 = node1?.next
            }
            
            if node2 != nil {
                count2 += 1
                node2 = node2?.next
            }
        }
        
        node1 = headA
        node2 = headB
        diff = abs(count1 - count2)
        bigger = count1 > count2
        
        while node1 != nil || node2 != nil {
            while diff > 0 {
                if bigger {
                    node1 = node1?.next
                } else {
                    node2 = node2?.next
                }
                diff -= 1
            }
            
            if node1 === node2 { return node1 }
            
            node1 = node1?.next
            node2 = node2?.next
        }
        
        return nil
    }
```
## 1315. Sum of Nodes with Even-Valued Grandparent


```swift
    func sumEvenGrandparent(_ root: TreeNode?) -> Int {
        var sum = 0
        
        func dfs(_ root: TreeNode?, _ parent: TreeNode?, _ gParent: TreeNode?) {
            guard let node = root else { return }
            
            if let gp = gParent, gp.val % 2 == 0 { sum += node.val }
            
            dfs(node.left, node, parent)
            dfs(node.right, node, parent)
        }
        
        dfs(root, nil, nil)
        
        return sum
    }
```
## 1395. Count Number of Teams


```swift
    func numTeams(_ rating: [Int]) -> Int {
        if rating.count <= 2 { return 0 }
        
        var res = 0
        
        for i in 1..<rating.count-1 {
            var leftLarger = 0
            var leftSmaller = 0
            var rightLarger = 0
            var rightSmaller = 0
            
            for j in 0..<i {
                if rating[j] < rating[i] {
                    leftSmaller += 1
                } else {
                    leftLarger += 1
                }
            }
            for j in i+1..<rating.count {
                if rating[j] < rating[i] {
                    rightSmaller += 1
                } else {
                    rightLarger += 1
                }
            }
            
            res += (leftLarger * rightSmaller) + (leftSmaller * rightLarger)
        }
        
        return res
    }
```
## 1385. Find the Distance Value Between Two Arrays


```swift
    func findTheDistanceValue(_ arr1: [Int], _ arr2: [Int], _ d: Int) -> Int {
        var arr1 = bucketSort(arr1)
        var arr2 = bucketSort(arr2)
        var count = 0
        var i = 0
        var j = 0
        
        while i < arr1.count && j < arr2.count {
            let diff = abs(arr1[i] - arr2[j])
            
            if diff > d {
                if arr1[i] > arr2[j] {
                    j += 1
                } else {
                    i += 1
                    count += 1
                }
            } else {
                i += 1
            }
        }
        
        count += arr1.count - i
        
        return count
    }
    
    func bucketSort(_ arr: [Int]) -> [Int] {
        var bucket = Array(repeating: 0, count: 2001)
        var result = arr
        var i = 0
        
        for num in arr {
            bucket[num + 1000] += 1
        }
        
        for (num, count) in bucket.enumerated() where count > 0 {
            var count = count
            
            while count > 0 {
                result[i] = num
                i += 1
                count -= 1
            }
        }
        
        return result
    }
```
## 1038. Binary Search Tree to Greater Sum Tree


```swift
    func bstToGst(_ root: TreeNode?) -> TreeNode? {
        func dfs(_ root: TreeNode?, _ sum: Int) -> Int {
            guard let node = root else { return sum }
            
            node.val += dfs(node.right, sum)
            
            return dfs(node.left, node.val)
        }
        
        dfs(root, 0)
        
        return root
    }
```
## 654. Maximum Binary Tree


```swift
    func constructMaximumBinaryTree(_ nums: [Int]) -> TreeNode? {
        var stack: [TreeNode] = []
        
        for i in 0..<nums.count {
            let node = TreeNode(nums[i])
            
            while !stack.isEmpty && stack[stack.count - 1].val < node.val {
                node.left = stack.removeLast()
            }
            
            if !stack.isEmpty && stack[stack.count - 1].val > node.val {
                stack[stack.count - 1].right = node
            }
            
            stack.append(node)
        }
        
        return stack.isEmpty ? nil : stack[0]
    }
```
## 535. Encode and Decode TinyURL


```swift
class Codec {
    var urls: [String: String] = [:]
    var codeCount = 6
    
    func encode(_ longUrl: String) -> String {
        var code = generateCode(codeCount)
        var count = 0
        
        while urls[code] != nil {
            if count > 10 {
                codeCount += 1
                count = 0
            }
            code = generateCode(codeCount)
            count += 1
        }
        
        urls[code] = longUrl
        
        return "http://tinyurl.com/\(code)"
    }
    
    func generateCode(_ length: Int) -> String {
        var ran = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
        var result: [String] = []
        
        for _ in 0..<length {
            result.append(String(ran.randomElement()!))
        }
        
        return result.joined()
    }
    
    // Decodes a shortened URL to its original URL.
    func decode(_ shortUrl: String) -> String {
        let code = shortUrl.split(separator: "/")
        
        return urls[String(code[code.count - 1]), default: ""]
    }
}
```

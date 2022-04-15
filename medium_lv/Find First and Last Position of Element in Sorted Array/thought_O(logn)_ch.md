## 想法
這題主要是要考慮到時間複雜度O(log n)的問題
關於這題解法, 因為有時間複雜度的限制, 必須透過二分搜尋法方式去找


## Code 
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        ans = [-1, -1]
        if len(nums) == 0:
            return ans
        
        numsLen = len(nums)
    
        if nums[0] == target:
            ans[0] = 0

        if nums[numsLen - 1] == target:
            ans[1] = numsLen - 1
    
        if ans[0] is not -1 and  ans[1] is not -1:
            return  ans

        leftIndex = 0
        rightIndex = numsLen
        middleIndex = int((leftIndex + rightIndex) / 2)

        while leftIndex <= rightIndex and middleIndex >= 0 and middleIndex < numsLen:
            # find middle index value        
            val = nums[middleIndex]

            # if middle value is less than target
            if val < target:
                # than search right side in the list
                leftIndex = middleIndex + 1
            else:
                # if middle value is bigget than target
                if val > target:

                    # than search left side in the list
                    rightIndex = middleIndex - 1
                else:
                    # if middle value is equal target

                    if middleIndex - 1 >= 0:
                        if nums[middleIndex - 1] < val:
                            ans[0] = middleIndex
                            break
                        else:
                            rightIndex = middleIndex - 1
                    
                    # if index is 0
                    else:
                        ans[0] = middleIndex
                        break  
            middleIndex = int((leftIndex + rightIndex) / 2)


        leftIndex = 0
        rightIndex = numsLen
        middleIndex = int((leftIndex + rightIndex) / 2)

        while leftIndex <= rightIndex and middleIndex >= 0 and middleIndex < numsLen:            
            val = nums[middleIndex]

            if val < target:
                leftIndex = middleIndex + 1
            else:
                if val > target:
                    rightIndex = middleIndex - 1
                else:
                    if middleIndex + 1 < numsLen:
                        if nums[middleIndex + 1] > val:
                            ans[1] = middleIndex
                            break
                        else:
                            leftIndex = middleIndex + 1
                    else:
                        ans[1] = middleIndex
                        break
            middleIndex = int((leftIndex + rightIndex) / 2)

        return ans
```

## 解法說明
### 解法觀念

```
1. 透過二分搜尋法先找出左邊第一個出現target
2. 再透過二分搜尋法先找出右邊第一個出現target
```

### 為何需要兩個while loop

```
1. 讓程式簡單化
    也許有高手可以只用一個while loop即可解出答案, 但是可能邏輯會更複雜, 也變得不容易懂
2. 當遇到middle value與target相同時, 處裡的細微邏輯不太一樣
    左邊的邏輯: 當遇到middle value與target相同且並非左邊第一個出現的話, 因為要找出左邊的第一個出現target的index, 接下來要往左側去執行二元搜尋
    右邊的邏輯: 當遇到middle value與target相同且並非右邊第一個出現的話, 因為要找出右邊的第一個出現target的index, 接下來要往右側去執行二元搜尋
```

### 參考範例

```
 [5,7,7,8,8,10], target = 8
```

### 透過二分搜尋法先找出左邊第一個出現target
```
# 1
# 起始

answer: [-1, -1]
leftIndex: 0
rightIndex: 5 → list的最後一個index
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 2
# while loop開始
# 先找出中間的middleIndex, (leftIndex + rightIndex ) / 2 = 3
# nums[3] 剛好等於 target

answer: [-1, -1]
leftIndex: 0
rightIndex: 5
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```

```
# 3
# 先確認此數值是否為左邊第一個出現的target數值
# nums[3] 剛好為左邊第一個出現的target數值,　所以找到左邊的答案
# 將左邊的答案從-1改為3
# 找到答案即跳出while loop

answer: [3, -1]
leftIndex: 0
rightIndex: 5
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```


### 再透過二分搜尋法找出右邊第一個出現target
```
# 4
# 起始

leftIndex: 0
rightIndex: 5 → list的最後一個index
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 5
# while loop開始
# 先找出中間的middleIndex, (leftIndex + rightIndex ) / 2 = 3
# nums[3] 剛好等於 target

leftIndex: 0
rightIndex: 5 → list的最後一個index
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```

```
# 6
# 先確認此數值是否為右邊第一個出現的target數值
# nums[3] 並非為右邊數來第一個出現的target數值,　所以繼續找尋下一個middleIndex
# 這時候因為是要找右邊第一個出現的target數值, 因此當target與middle value一樣時, 我們盡量讓middleIndex往右邊移動
# 因此leftIndex設置為middleIndex下一個index, 即為4

leftIndex: 4
rightIndex: 5 → list的最後一個index
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```

```
# 7
# 先確認此數值是否為右邊第一個出現的target數值
# nums[4] 剛好為右邊第一個出現的target數值,　所以找到右邊的答案
# 將右邊的答案從-1改為4
# 找到答案即跳出while loop

leftIndex: 4
rightIndex: 5 → list的最後一個index
answer: [3, 4]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
              ⬆
              i
```

```
# 8
# 程式結束, 回傳答案

answer: [3, 4]

```
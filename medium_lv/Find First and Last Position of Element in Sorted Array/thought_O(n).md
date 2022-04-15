## 想法
這題主要是要考慮到時間複雜度O(log n)的問題
關於這題解法, 如果沒有時間複雜度的限制, 透過for loop一路從頭找到尾, 即可找出答案
正常來說, 此題需透過二分搜尋法方式去找, 但是如果不要求一定要O(log n)的話, 突然想到一個比一般還要快速找到答案的解法


## Code 
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        ans = [-1, -1]
        if len(nums) == 0:
            return ans
        
        for index in range(len(nums)):
            if nums[index] < target:
                continue
            else:
                if nums[index] == target:
                    ans[0] = index
                break

        for index in range(len(nums)-1, -1, -1):
            if nums[index] > target:
                continue
            else:
                if nums[index] == target:
                    ans[1] = index
                break
        return ans
```

## 解法說明
### 解法觀念

```
1. 從左到右開始找出找到第一個出現的target
2. 再從又到左找出第一個出現的target
```



### 參考範例

```
 [5,7,7,8,8,10], target = 8
```

### 從左到右開始找出找到第一個出現的target
```
# 1
# 起始
answer: [-1, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 2
# for loop開始
# 5 比 target小, 所以繼續往前
answer: [-1, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
  ⬆
  i
```

```
# 3
# 7 比 target小, 所以繼續往前
answer: [-1, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
     ⬆
     i
```

```
# 4
# 7 比 target小, 所以繼續往前
answer: [-1, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
        ⬆
        i
```

```
# 5
# 8 為 target, 將此list的index記錄下來, 並離開loop
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```

### 再從又到左找出第一個出現的target
```
# 9
# 起始
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 10
# for loop開始
# 10 比 target大, 所以繼續往前
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
                 ⬆
                 i
```

```
# 11
# 8 為 target, 將此list的index記錄下來, 並離開loop
# 程式執行結束, 將答案回傳
answer: [3, 4]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
              ⬆
              i
```

## 解法的時間複雜度
### 最差為O(n)

```
假如target剛好位於最左邊或最右邊的時候
兩個for loop中, 一定會有一個是從頭跑到尾
```
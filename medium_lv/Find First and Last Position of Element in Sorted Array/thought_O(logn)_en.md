## Idea
According the description of question, we need to consider the time complexity before we start to coding. 
In order to finish the problem in O(log n), so we can not use easy way,  to find the answer.
In here we need to use binary search idea to solve our problem.


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

## Solution
### Idea

```
1. Find the starting postion of a given target value by binary search.
2. Then find the ending position of a given target value by binary search.
```

### Why use two while loop?

```
1. Make code easy to read
    I believe that there have some people can solve this problem only use one while loop.
    But it will make the coding logic become more complex, and make it hard to understand.
2. There have some different processing between find the starting postion and ending position.
    Find the starting postion one: if the value we found that is equal with target but not starting postion, we need to go on.
    In order the find the starting postion, so we need to make binary search to process the left side of the list.

    Find the ending postion one: if the value we found that is equal with target but not ending postion, we also need to go on.
    In order the find the ending postion, so we need to make binary search to process the right side of the list.
```

### Example

```
 [5,7,7,8,8,10], target = 8
```

### Find the starting postion of a given target value by binary search.
```
# 1
# init

answer: [-1, -1]
leftIndex: 0
rightIndex: 5 → the last index in the list
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 2
# while loop begin
# find the middle index in the list by binary search, (leftIndex + rightIndex ) / 2 = 3
# nums[3] is equal with target

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
# checking this is the starting position of target. 
# nums[3] is the starting position of target, so we found the starting position.
# change the starting position answer from -1 to 3.
# leave while loop because found the answer

answer: [3, -1]
leftIndex: 0
rightIndex: 5
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```


### Then find the ending position of a given target value by binary search.
```
# 4
# init

leftIndex: 0
rightIndex: 5 → the last index in the list
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 5
# while loop begin
# find the middle index in the list by binary search, (leftIndex + rightIndex ) / 2 = 3
# nums[3] is equal with target

leftIndex: 0
rightIndex: 5
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```

```
# 6
# checking this is the ending position of target. 
# nums[3] the ending position of target,　so binary search continue to find the next middle index.
# Because we want to find the ending position, so we need to make binary search going on right side in the list.
# So the next middle index will be 4. ((4 + 5) / 2 = 4)

leftIndex: 4
rightIndex: 5
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```

```
# 7
# checking this is the ending position of target. 
# nums[4] is the ending position of target, so we found the ending position.
# change the ending position answer from -1 to 4.
# leave while loop because found the answer

leftIndex: 4
rightIndex: 5
answer: [3, 4]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
              ⬆
              i
```

```
# 8
# processing end, then return answer

answer: [3, 4]

```
## アイデア
この問題を解決するため、一番考える必要な物は時間計算量です。
今回はO(log n)で問題を解決する必要ですので、普通の方法はいけないと思います。
O(log n)で解決するため、二分探索アルゴリズムでやる決まります。

## コード 
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

## 解法説明
### やり方アイデア

```
1. まず二分探索で左側一番目のターゲットのインデックスを探します。
2. 次に二分探索で右側最後のターゲットのインデックスを探します。
```

### なぜ二つwhileでやる理由

```
1. コードが分かりやすくため
    経験よくあるエンジニアさんから、多分一つwhileで問題が解決できると思います。
    ただそうすると、処理のロジックは複雑になる可能性があるかもしれません。

2. 二分探索で処理するときに、ターゲットと同じバリューを見つける場合に、一番目のターゲットのインデックス探すロジックと最後のターゲットのインデックス探すロジックは少し違う
    一番目のターゲット探すロジック: 
        ターゲットと同じバリューを見つけて、一番目のターゲットじゃない場合に、リストに左側一番目のターゲットを探すため、二分探索でリストの左側に引き続き処理する必要です。
    最後のターゲット探すロジック:
        ターゲットと同じバリューを見つけて、最後のターゲットじゃない場合に、リストに右側最後のターゲットを探すため、二分探索でリストの右側に引き続き処理する必要です。
```

### 例

```
 [5,7,7,8,8,10], target = 8
```

### まず二分探索で左側一番目のターゲットのインデックスを探します。
```
# 1
# 初期化

answer: [-1, -1]
leftIndex: 0
rightIndex: 5 → リストの最後インデックス
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 2
# while loop開始
まずリストの真ん中インデックスを確認する。(leftIndex + rightIndex ) / 2 = 3
# nums[3] バリューとターゲットは同じ

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
# このインデックスは左側一番目のターゲットのインデックスが確認する。
# nums[3] は左側一番目のターゲットですので、一番目のターゲットのインデックスが見つけた。
# 一番目のターゲットのインデックスは-1から3に変更する。
# 一番目のターゲットが見つけたので、while loopストップ

answer: [3, -1]
leftIndex: 0
rightIndex: 5
target: 8
[ 5, 7, 7, 8, 8, 10 ]
           ⬆
           i
```


### 次に二分探索で右側最後のターゲットのインデックスを探します。
```
# 4
# 初期化

leftIndex: 0
rightIndex: 5 → リストの最後インデックス
answer: [3, -1]
target: 8
[ 5, 7, 7, 8, 8, 10 ]
```

```
# 5
# while loop開始
まずリストの真ん中インデックスを確認する。(leftIndex + rightIndex ) / 2 = 3
# nums[3] バリューとターゲットは同じ

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
# このインデックスは右側最後のターゲットのインデックスが確認する。
# nums[3]は最後のターゲットじゃあないので、引き続き二分探索で次のインデックスを探す。
# 最後のターゲットを探すため、二分探索でリストの右側に引き続き処理する必要です。
# なので次のインデックスを4。(4+5)/2 = 4

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
# このインデックスは右側最後のターゲットのインデックスが確認する。
# nums[4] は右側最後のターゲットですので、右側最後のターゲットのインデックスが見つけた。
# 最後のターゲットのインデックスは-1から4に変更する。
# 最後のターゲットが見つけたので、while loopストップ

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
# プロセス終了

answer: [3, 4]

```
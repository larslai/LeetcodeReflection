## 想法
這題由於是node link list的關係, 無法用一般的for loop去跑迴圈
根據NodeList的結構, 要用node.next的方式去執行
由於回傳的答案也必須是link list, 所以要額外創建一個link list來儲存答案

## Code 
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
        ansNodePosition = None
        ansHead = None

        position = head.next
        
        currentRangeSum = 0
        
        while position!= None:
            if position.val == 0:
                node = ListNode(val=currentRangeSum)
                
                if ansHead is None:
                    ansNodePosition = node
                    ansHead = ansNodePosition
                else:
                    ansNodePosition.next = node
                    ansNodePosition = ansNodePosition.next

                currentRangeSum = 0
            else:
                currentRangeSum = currentRangeSum + position.val
            position = position.next
        
        return ansHead
```

## 解法時間複雜度
```
因為會將Node link list一路訪問到底
O(n)
```

## 解法空間複雜度
```
因為會依照Node link list長度不同,導致答案的node link list長度也會發生 
O(n)
```

## Link list觀念
```
這題主要考驗的是對於node link list是否熟悉
在程式碼中有一個參數為ansHead, 此參數會一直指著答案專用的link list的起點, 以便用來記錄整個答案專用的link list, 所以此參數不會對它做任何動作
而另一個參數:ansNodePosition, 則是用來連結下一個新的node用的, 所以會一直移動
為什麼需要一個永遠指著link list起點的參數呢?
以下是此題目提供的node結構:
    def __init__(self, val=0, next=None):
    #         self.val = val
    #         self.next = next

他只記錄了當前node的值, 以及下一個node要指向誰, 但是此node的結構並沒有紀錄上一個正在連接著的node的功能, 如下圖
Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
            ⬆
            這個node, 它只知道它連著的下一個node為0, 不知道上一個node是誰


如果是下列的node結構則不需要多一個參數來記錄答案專用的link list的起點
    def __init__(self, val=0, pervios=None ,next=None):
    #         self.val = val
    #         self.next = next
    #         self.pervious = pervios

因為這樣子node將會是雙向的, 如下圖

Node list image:
None  ⇋ 0  ⇋  3  ⇋  1  ⇋  0  ⇋  4  ⇋  5  ⇋  2  ⇋  0  ⇋  None
                    ⬆
                    這個node, 不僅知道它連著的下一個node為1, 還知道上一個node是3  


相對的, 如果是上述的node結構, 最後需要用一個while迴圈, 從最後一個Node一路往回找到源頭後再回傳答案
```

## 解法說明
### 解法觀念

```
1. 透過While去將Link list跑一遍
2. 每經過一個node, 就將node的value做加總
3. 當遇到0, 就將目前的總和儲存起來再初始化總和值
```

### 參考範例

```
[0,3,1,0,4,5,2,0]
```

```
# 1
# 起始

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None

Answer node list image:
None

currentRangeSum: 0
```

```
# 2
# while loop開始
# 由於第一位node一定為0, 所以直接從第二個node開始
# 此node非0, 先將目前總和做加總, currentRangeSum變為3

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
      ⬆
      position

Answer node list image:
None

currentRangeSum: 3
```

```
# 3
# 此node非0, 先將目前總和做加總, currentRangeSum變為4

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
            ⬆
            position

Answer node list image:
None

currentRangeSum: 4
```

```
# 4
# 此node為0, 目前總和儲存至answser node list, 並初始化currentRangeSum

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
                  ⬆
                  position

Answer node list image:
4 →  None
⬆ 
ansNodePosition
ansHead

currentRangeSum: 0
```

```
# 5
# 此node非0, 先將目前總和做加總, currentRangeSum變為4

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
                        ⬆
                        position

Answer node list image:
4 → None
⬆ 
ansNodePosition
ansHead

currentRangeSum: 4
```

```
# 6
# 此node非0, 先將目前總和做加總, currentRangeSum變為9

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
                              ⬆
                              position

Answer node list image:
4 → None
⬆ 
ansNodePosition
ansHead

currentRangeSum: 9
```

```
# 7
# 此node非0, 先將目前總和做加總, currentRangeSum變為11

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
                                    ⬆
                                    position

Answer node list image:
4 → None
⬆ 
ansNodePosition
ansHead

currentRangeSum: 11
```

```
# 8
# 此node為0, 目前總和儲存至answser node list, 並初始化currentRangeSum

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
                                          ⬆
                                          position

Answer node list image:
4 → 11 → None
⬆    ⬆
|    ansNodePosition
|
ansHead

currentRangeSum: 11
```


```
# 9
# 程式結束, 回傳ansHead

answer: [4, 11]

```
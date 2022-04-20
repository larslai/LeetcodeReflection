## アイデア
今回はnodeで処理必要なので、普通のfor loopで処理ができなさそうです。
提供したNodeListの構成によって、次のnodeを取得する為、node.nextメソッドでやる必要です。
すると、node link listでreturnする必要です。
つまり、アンサー用node link listを作る必要です。


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

## 時間計算量
```
while loopでnode一つずつを処理する必要ですから。
O(n)
```

## 空間計算量
```
インプットのlink listによって、作ったアンサー用node link listの長さも違いますから。
O(n)
```

## Link list説明
```
今回はnodeで処理必要なので、まずnodeの処理やり方は確認しないとコーディングできないです。

下記は今回nodeのclassの構成:
    def __init__(self, val=0, next=None):
    #         self.val = val
    #         self.next = next

すると、下記のnode link listのイメージ:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None

基本的に、nodeの間に、linkで連携している感じです。
nodeの構成によって、nextメソッドで次に連携しているnodeが取得できますが、前のnextが取得できないです。

0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
            ⬆
            このnodeにつて、次に連携しているnodeは0という情報が分かる。しかし誰がこのnodeを連携する情報が判断できない。

アンサーするときに、node link listで返事が必要ですので、上記の状況を考えると、必ず一個変数を作って、いつもアンサー用node link listの最初nodeを連携する必要です。
そうしないと、アンサー用node link listの全てnode link listが返事できないです。

もしnodeの構成は下記の感じなら、nextメソッドで次に連携しているnodeが取得できますし、前のnextも取得できます。
    def __init__(self, val=0, pervios=None ,next=None):
    #         self.val = val
    #         self.next = next
    #         self.pervious = pervios

node link listのイメージ:
None  ⇋ 0  ⇋  3  ⇋  1  ⇋  0  ⇋  4  ⇋  5  ⇋  2  ⇋  0  ⇋  None
                    ⬆
                    このnodeにつて、次に連携しているnodeは0という情報が分かるし、か誰がこのnodeを連携する情報も分かります。
```

## 解法説明
### やり方アイデア

```
1. while loopでnode一つずつを処理する
2. nodeのバリューは0じゃない場合に、合計する
3. nodeのバリューは0になる場合に、アンサー用node link listに今まで合計情報を追加して、合計情報を初期化する。
```

### 例

```
[0,3,1,0,4,5,2,0]
```

```
# 1
# init

Node list image:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None

Answer node list image:
None

currentRangeSum: 0
```

```
# 2
# while loop開始
# questionの説明によって、最初のnodeは必ず0ですので、次のnodeから処理します。
# 今のnodeバリューは0じゃないので、合計する。
# currentRangeSumは3に変更する。

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
# 今のnodeバリューは0じゃないので、合計する。
# currentRangeSumは4に変更する。

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
# 今のnodeバリューは0ですので、アンサー用node link listに今まで合計情報を追加して、合計情報を初期化する。

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
# 今のnodeバリューは0じゃないので、合計する。
# currentRangeSumは4に変更する。

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
# 今のnodeバリューは0じゃないので、合計する。
# currentRangeSumは9に変更する。

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
# 今のnodeバリューは0じゃないので、合計する。
# currentRangeSumは11に変更する。

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
# 今のnodeバリューは0ですので、アンサー用node link listに今まで合計情報を追加して、合計情報を初期化する。

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
# 処理完了, return ansHead

answer: [4, 11]

```
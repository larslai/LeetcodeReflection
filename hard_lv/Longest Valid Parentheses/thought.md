## 想法
原本以為可以透過類似[valid-parentheses](https://leetcode.com/problems/valid-parentheses/) 的方法去解題
結果發現不適合, 參考了網路大家的討論, 發現了一種非常聰明的解法

## Code 
```python
class Solution:
    def longestValidParentheses(self, s):
        """
        :type s: str
        :rtype: int
        """
        l,r = 0,0
        max_ = 0
        for i in range(len(s)):
            if s[i] == '(':
                l +=1
            else:
                r +=1
            if l == r:
                max_ = max(max_, 2 * r)
            else:
                if r >= l:
                    l = r = 0
        l,r = 0,0
        for i in range(len(s)-1,-1,-1):
            if s[i] == '(':
                l += 1
            else:
                r += 1
            if l == r:
                max_ = max(max_, 2 * l)
            else:
                if l >= r:
                    l = r = 0
        return max_
```

## 解法說明
### 解法觀念

```
1. 從左到右開始找出最長的valid parentheses
2. 再從又到左找出最長的valid parentheses
3. 兩者去比較並找出其中最長的valid parentheses即為答案
```

#### 找出valid parentheses的觀念
```
以下都是valid parentheses
()     → valid parentheses長度為2
((())) → valid parentheses長度為6
()()   → valid parentheses長度為4
(())() → valid parentheses長度為6

也就是說當左括號數量與右括號數量相等時, 就可以計算出最長的valid parentheses長度
```

#### 從左到右開始找出valid parentheses的觀念
```
從上面例子來看, 都是先畫左括號再畫右括號
也就是說在畫的時候, 左括號一定都會比右括號多或兩者的數量會相同

反過來說什麼時候是invalid parentheses?
就是右括號比左括號多的時候就是invalid parentheses, 即下面例子
()) → 右括號比左括號多
)() → 先畫了右括號

所以當右括號比左括號多的時候, 即代表出現了invalid parentheses
```

#### 從右到左開始找出valid parentheses的觀念
```
這時畫括號的順序會反過來, 都是先畫右括號再畫左括號
也就是說在畫的時候, 右括號一定都會比左括號多或兩者的數量會相同

反過來說什麼時候是invalid parentheses?
就是左括號比右括號多的時候就是invalid parentheses, 即下面例子 (記得要從最後面往前看)
(() → 左括號比右括號多
()( → 先畫了左括號

所以當左括號比右括號多的時候, 即代表出現了invalid parentheses
```

### 參考範例

```
"(()(()"
```

### 首先從左到右找出最長的parentheses
```
# 1
# 起始
l: 0 → 左括號數量
r: 0 → 右括號數量
max_: 0 → 目前最長的parentheses長度

( ( ) ) ( ( )

```

```
# 2
# for loop開始
# 遇到 ( , 因此左括號數量加1
l: 1
r: 0
max_: 0

( ( ) ) ( ( )
⬆
i
```

```
# 3
# 遇到 ( , 因此左括號數量加1
l: 2
r: 0
max_: 0

( ( ) ) ( ( )
  ⬆
  i
```

```
# 4
# 遇到 ) , 因此右括號數量加1
l: 2
r: 1
max_: 0

( ( ) ) ( ( )
    ⬆
    i
```

```
# 5
# 遇到 ) , 因此右括號數量加1
# 因為遇到一對valid parentheses, 所以開始計算最長的valid parentheses
# 目前最長的valid parentheses為4 (左右跨號總數), 所以max_ 設置為4
l: 2
r: 2
max_: 4

( ( ) ) ( ( )
      ⬆
      i
```

```
# 6
# 遇到 ( , 因此左括號數量加1
l: 3
r: 2
max_: 4

( ( ) ) ( ( )
        ⬆
        i
```

```
# 7
# 遇到 ( , 因此左括號數量加1
l: 4
r: 2
max_: 4

( ( ) ) ( ( )
          ⬆
          i
```

```
# 8
# 遇到 ) , 因此右括號數量加1
# for loop到底
l: 4
r: 3
max_: 4

( ( ) ) ( ( )
            ⬆
            i
```

### 接著從右到左找出最長的parentheses
```
# 9
# 起始
l: 0 → 左括號數量
r: 0 → 右括號數量
max_: 4 → 目前最長的parentheses長度

( ( ) ) ( ( )
```

```
# 10
# for loop開始
# 遇到 ) , 因此右括號數量加1
l: 0
r: 1
max_: 4

( ( ) ) ( ( )
            ⬆
            i
```

```
# 11
# 遇到 ( , 因此左括號數量加1
# 因為遇到一對valid parentheses, 所以開始計算最長的valid parentheses
# 這次計算出來的valid parentheses長度為2
# 目前最長的valid parentheses為4 (左右跨號總數), 所以max_ 不改變
l: 1
r: 1
max_: 4

( ( ) ) ( ( )
          ⬆
          i
```

```
# 12
# 遇到 ( , 因此左括號數量加1
# 因為左括號比右括號多, 代表遇到了invalid的parentheses
l: 2
r: 1
max_: 4

( ( ) ) ( ( )
        ⬆
        i
```

```
# 13
# 為了重新計算接下來最長valid parentheses長度, 必須初始左右括號數
l: 0
r: 0
max_: 4

( ( ) ) ( ( )
        ⬆
        i
```

```
# 14
# 遇到 ) , 因此右括號數量加1
l: 0
r: 1
max_: 4

( ( ) ) ( ( )
      ⬆
      i
```

```
# 15
# 遇到 ) , 因此右括號數量加1
l: 0
r: 2
max_: 4

( ( ) ) ( ( )
    ⬆
    i
```

```
# 16
# 遇到 ( , 因此左括號數量加1
l: 1
r: 2
max_: 4

( ( ) ) ( ( )
  ⬆
  i
```

```
# 17
# 遇到 ( , 因此左括號數量加1
# 因為遇到一對valid parentheses, 所以開始計算最長的valid parentheses
# 目前最長的valid parentheses也為4 (左右跨號總數), 所以max_ 不改變
# for loop到底
# 因為程式已經跑完, 所以回傳 max_, 即為4
l: 2
r: 2
max_: 4

( ( ) ) ( ( )
⬆
i
```

## 解法說明延伸
### 從左邊計算過來和從右邊計算過來, 最長的valid parentheses會有不同的時候嗎?

```
以下例來解釋, 就會導致兩邊計算出來的最長長度不同
(()

```

### 首先從左到右找出最長的parentheses
```
# 1
# 起始
l: 0 → 左括號數量
r: 0 → 右括號數量
max_: 0 → 目前最長的parentheses長度

( ( )

```

```
# 2
# for loop開始
# 遇到 ( , 因此左括號數量加1
l: 1
r: 0
max_: 0

( ( )
⬆
i
```

```
# 3
# 遇到 ( , 因此左括號數量加1
l: 2
r: 0
max_: 0

( ( )
  ⬆
  i
```

```
# 4
# 遇到 ) , 因此右括號數量加1
# for loop結束, 因為左括號和右括號數量不對等, 所以max_沒有被計算到
l: 2
r: 1
max_: 0

( ( )
    ⬆
    i
```

### 接著從右到左找出最長的parentheses
```
# 5
# 起始
l: 0 → 左括號數量
r: 0 → 右括號數量
max_: 0 → 目前最長的parentheses長度

( ( )
```

```
# 6
# for loop開始
# 遇到 ) , 因此右括號數量加1
l: 0
r: 1
max_: 0
( ( )
    ⬆
    i
```

```
# 7
# 遇到 ( , 因此左括號數量加1
# 因為遇到一對valid parentheses, 所以開始計算最長的valid parentheses
# 目前最長的valid parentheses為2 (左右跨號總數), 所以max_ 設定為2
l: 1
r: 1
max_: 2
( ( )
  ⬆
  i
```

```
# 8
# 遇到 ( , 因此左括號數量加1
# for loop到底
# 因為程式已經跑完, 所以回傳 max_, 即為2
l: 2
r: 1
max_: 2

( ( )
⬆
i
```
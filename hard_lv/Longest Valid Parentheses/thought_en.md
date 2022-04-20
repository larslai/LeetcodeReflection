## Idea
I have no idea to slove this problem, so I looking for other people's idea.
And I found this smart solution.

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

## Solution
### Idea

```
1. Find the longest valid parentheses in the string start from left side to right side.
2. Then Find the longest valid parentheses in the string start from right side to left side.
3. Compare those two longest valid parentheses and return the longest one.
```

#### What is valid parentheses
```
Those are the valid parentheses
()     → valid parentheses length is 2
((())) → valid parentheses length is 6
()()   → valid parentheses length is 4
(())() → valid parentheses length is 6

So what is the best timing to calculate the length of valid parentheses? 
    when the number of left brackets is equal than the number of right brackets.
```

#### The idea of finding the longest valid parentheses in the string start from left side to right side.
```
According to the example as above.
We will always write the left bracket and then write the right bracket.
In other words, the number of left brackets will always greater than the number of right brackets, or both will have the same number of bracket.

So what is the invalid parentheses in this pattern?
The number of right brackets is greater than the number of left brackets that is the invalid parentheses.
And the right bracket is come first than left bracket that is also the invalid parentheses.

For example,
()) → The number of right brackets is greater than the number of left bracket
)() → The right bracket is come first than left bracket
```

#### The idea of finding the longest valid parentheses in the string start from right side to left side.
```
In this pattern, the order of writting bracket will be contray.
We will always write the right bracket and then write the left bracket.
In other words, the number of right brackets will always greater than the number of left brackets, or both will have the same number of bracket.

So what is the invalid parentheses in this pattern?
The number of left brackets is greater than the number of rigth brackets that is the invalid parentheses.
And the left bracket is come first than right bracket that is also the invalid parentheses.

For example, (looking from the back to front)
(() → The number of left brackets is greater than the number of rigth brackets
()( → the left bracket is come first than right bracket

```

### Example

```
"(()(()"
```

### Find the longest valid parentheses in the string start from left side to right side.
```
# 1
# init

l: 0 → The number of left brackets
r: 0 → The number of right brackets
max_: 0 → the longest valid parentheses

( ( ) ) ( ( )

```

```
# 2
# for loop begin
# found ( , so the number of left brackets + 1

l: 1
r: 0
max_: 0

( ( ) ) ( ( )
⬆
i
```

```
# 3
# found ( , so the number of left brackets + 1

l: 2
r: 0
max_: 0

( ( ) ) ( ( )
  ⬆
  i
```

```
# 4
# found ) , so the number of right brackets + 1

l: 2
r: 1
max_: 0

( ( ) ) ( ( )
    ⬆
    i
```

```
# 5
# found ) , so the number of right brackets + 1
# both of the number of bracket are same, so start to calculate the longest valid parentheses.
# now the longest valid parentheses is 4, so max_ is 4

l: 2
r: 2
max_: 4

( ( ) ) ( ( )
      ⬆
      i
```

```
# 6
# found ( , so the number of left brackets + 1

l: 3
r: 2
max_: 4

( ( ) ) ( ( )
        ⬆
        i
```

```
# 7
# found ( , so the number of left brackets + 1

l: 4
r: 2
max_: 4

( ( ) ) ( ( )
          ⬆
          i
```

```
# 8
# found ) , so the number of right brackets + 1
# for loop end

l: 4
r: 3
max_: 4

( ( ) ) ( ( )
            ⬆
            i
```

### Then Find the longest valid parentheses in the string start from right side to left side.
```
# 9
# init

l: 0 → The number of left brackets
r: 0 → The number of right brackets
max_: 4 → the longest valid parentheses

( ( ) ) ( ( )
```

```
# 10
# for loop begin
# found ) , so the number of right brackets + 1

l: 0
r: 1
max_: 4

( ( ) ) ( ( )
            ⬆
            i
```

```
# 11
# found ( , so the number of left brackets + 1
# both of the number of bracket are same, so start to calculate the longest valid parentheses.
# the longest valid parentheses in this time is 2, less than pervious one, so max_ no update.

l: 1
r: 1
max_: 4

( ( ) ) ( ( )
          ⬆
          i
```

```
# 12
# found ( , so the number of left brackets + 1
# the number of left brackets is greater than the number of rigth brackets, so we find the invalid parentheses

l: 2
r: 1
max_: 4

( ( ) ) ( ( )
        ⬆
        i
```

```
# 13
# in order to calculate the next longest valid parentheses, init the both of number of brackets

l: 0
r: 0
max_: 4

( ( ) ) ( ( )
        ⬆
        i
```

```
# 14
# found ) , so the number of right brackets + 1

l: 0
r: 1
max_: 4

( ( ) ) ( ( )
      ⬆
      i
```

```
# 15
# found ) , so the number of right brackets + 1

l: 0
r: 2
max_: 4

( ( ) ) ( ( )
    ⬆
    i
```

```
# 16
# found ( , so the number of left brackets + 1

l: 1
r: 2
max_: 4

( ( ) ) ( ( )
  ⬆
  i
```

```
# 17
# found ( , so the number of left brackets + 1
# both of the number of bracket are same, so start to calculate the longest valid parentheses.
# the longest valid parentheses in this time is 4, equal than pervious one, so max_ no update.
# for loop end
# return max_, 4

l: 2
r: 2
max_: 4

( ( ) ) ( ( )
⬆
i
```

## Discuss
### This is possible that the number of longest parentheses are different when we run loop from start to end or we run loop from end to start?

```
According to the example as below, here have the different the number of longest parentheses.

(()

```

### Find the longest valid parentheses in the string start from left side to right side.
```
# 1
# init

l: 0 → The number of left brackets
r: 0 → The number of right brackets
max_: 0 → the longest valid parentheses

( ( )

```

```
# 2
# for loop begin
# found ( , so the number of left brackets + 1

l: 1
r: 0
max_: 0

( ( )
⬆
i
```

```
# 3
# found ( , so the number of left brackets + 1

l: 2
r: 0
max_: 0

( ( )
  ⬆
  i
```

```
# 4
# found ) , so the number of right brackets + 1
# for loop end
# the number of right brackets is not equal than the number of left brackets, so max_ not to be calcualted.

l: 2
r: 1
max_: 0

( ( )
    ⬆
    i
```

### Then Find the longest valid parentheses in the string start from right side to left side.
```
# 5
# init

l: 0 → The number of left brackets
r: 0 → The number of right brackets
max_: 0 → the longest valid parentheses

( ( )
```

```
# 6
# for loop begin
# found ) , so the number of right brackets + 1

l: 0
r: 1
max_: 0
( ( )
    ⬆
    i
```

```
# 7
# found ( , so the number of left brackets + 1
# both of the number of bracket are same, so start to calculate the longest valid parentheses.
# now the longest valid parentheses is 2, so max_ is 2

l: 1
r: 1
max_: 2
( ( )
  ⬆
  i
```

```
# 8
# found ( , so the number of left brackets + 1
# for loop end
# return max_, 2

l: 2
r: 1
max_: 2

( ( )
⬆
i
```
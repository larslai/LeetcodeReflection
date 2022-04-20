## Thought
According to the structure of node link list, we can not use normal way, like for loop to fetch every node.
We need to use node.next method to fetch the next node.
Because of the return type is node, so we also need to create the new node link list to store our answer and return it.

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

## Time complexity 
```
We will need to fetch evey node from start to the end.
So the time complexity will be O(n)
```

## Space complexity 
```
According to the different length of node link list in the input, the length of output will be changed too.  
So the space complexity will be O(n)
```

## What is node link list
```
Here is the imgage of node link list:
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None

And here is the structure of node:
    def __init__(self, val=0, next=None):
    #         self.val = val
    #         self.next = next

Every node can only know which node is the next because there have the "next" mathod. And they don't know which one is the pervious node.
0  →  3  →  1  →  0  →  4  →  5  →  2  →  0 →  None
            ⬆
            This node only know node "0" is the next node, but have no idea which one is the pervious node.

This is the reason that why we need one variable, ansHead, always point to the head of node link list in the code.
Because we have no way to know the perious node after we use node.next to fetch the next node. 
So we can not return all of node information of answer's node link list.

If the structure of node is like as below, then every node will also know which one is the pervious node.
    def __init__(self, val=0, pervios=None ,next=None):
    #         self.val = val
    #         self.next = next
    #         self.pervious = pervios

Node list image:
None  ⇋ 0  ⇋  3  ⇋  1  ⇋  0  ⇋  4  ⇋  5  ⇋  2  ⇋  0  ⇋  None
                    ⬆
                    這This node only know node "0" is the next node, and also know node "3" is the pervious node.

```

## Solution
### Idea

```
1. Use while loop to fetch every node.
2. When we fetch the node, and the value of node is not 0, then calculate the sum.
3. if the value of node is 0, then store the sum into answer's node link list and initial the sum.
```

### Example

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
# while loop begin
# according the constraints, the value of first node must be 0, so we start from second node.
# the value of current node is not 0, then calculate the sum.
# currentRangeSum will become 3

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
# the value of current node is not 0, then calculate the sum.
# currentRangeSum will become 4

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
# the value of current node is 0, then store the sum in to answer's node link list.
# then reset currentRangeSum

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
# the value of current node is not 0, then calculate the sum.
# currentRangeSum will become 4

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
# the value of current node is not 0, then calculate the sum.
# currentRangeSum will become 9

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
# the value of current node is not 0, then calculate the sum.
# currentRangeSum will become 11

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
# the value of current node is 0, then store the sum in to answer's node link list.
# then reset currentRangeSum

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
# end, return ansHead

answer: [4, 11]

```
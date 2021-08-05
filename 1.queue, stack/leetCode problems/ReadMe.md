# 1. Stack, Queue 문제 풀어보기!!



## 1. Baseball-Game

간단히 문제를 해석하자면,

배열이 주어지고 배열안의 모든 값을 ouput으로 내는 문제이다. 하지만 특수한 경우들이있다.

1. "+" 문자가 있는경우 전 두값을 더한 값을 

2. "D" 문자가 있는경우 전 값의 두배를

3. "C" 문자가 있는경우 전 값을 삭제



![image-20210805233535639](/Users/user/Library/Application Support/typora-user-images/image-20210805233535639.png)

처음엔!

runtime Error...

for 문을 돌리면서 조건문(if)를 쓰면 거의 runtime Error 가 뜬다..

따라서 경우를 아예 분리했다

그럼에도 Error! --> 논리적으로 문제가 없는ㄷㅔ? 라고 생각했는데

return 위치를... for문 밖에서 안해주었다..항상 생각하자

```python
class Solution:
    def calPoints(self, ops: List[str]) -> int:
        record=[]
        for i in range(len(ops)):
            try : record.append(int(ops[i]))
            except:
                if ops[i]=="C":record.pop()
                elif ops[i] == "D" : record.append(2*record[-1])
                elif ops[i] == "+" : record.append(record[-2]+record[-1])
                                
        return sum(record)
      
      
     
```

 

다른 방법으로 생각해보자..



## 2. Palindrome Linked List



#### 풀이 2. 데크(Deque)를 이용한 최적화

1. 데크 자료형을 선언합니다.
2. 반복문을 통해 인풋 연결리스트를 끝까지 순회하여 데크 자료형에 연결리스트의 값을 하나씩 넣습니다.
3. 데크 자료형이 존재할 때까지 순회합니다.
   이 때, popleft()와 pop() 함수를 통해 맨 앞과 맨 뒤의 값을 비교합니다.



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        # 데크 자료형 선언 
        q: Deque = collections.deque()
            
        if not head:
            return True
        
        node = head
        
        while node is not None:
            q.append(node.val)
            node = node.next
            
        # 팰린드롬 판별
        while len(q) > 1:
            if q.popleft() != q.pop():
                return False
        
        return True

```



#### 풀이 3. 러너를 이용한 풀이

1. 느린 러너와 빠른 러너 모두 head에서 시작합니다.
2. 반복문을 통해 연결리스트가 끝날 때 까지 빠른 러너는 두 칸씩, 느린 러너는 한 칸씩 이동합니다.
3. 한 칸씩 이동할 때 마다 느린 러너의 역순 연결 리스트를 만듭니다.
4. 3번의 반복문이 끝나면 새로운 반복문을 통해 역순 연결 리스트의 값과 느린 연결 리스트의 값이 동일한지 비교합니다.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        rev = None
        # 초깃값은 모두 head에서 시작
        slow = fast = head
        # print('slow', slow) ListNode{val: 1, next: ListNode{val: 2, next: ListNode{val: 2, next: ListNode{val: 1, next: None}}}}
        
        # 러너를 이용해 역순 연결 리스트 구성
        while fast and fast.next:
            # 빠른 러너는 두 칸씩, 느린 러너는 한 칸씩 이동
            fast = fast.next.next
            # rev에 현재 slow, rev.next에 이전 rev를 넣어 역순 만듦, slow는 정방향으로 진행
            rev, rev.next, slow = slow, rev, slow.next
            
        if fast:
            slow = slow.next
        
        # 팰린드롬 여부 확인
        while rev and rev.val == slow.val:
            slow, rev = slow.next, rev.next
        return not rev
```


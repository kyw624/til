# 자료구조

## 파이썬에서 Deque 사용하기

### **Deque (Double Ended Queue)**

![Deque](https://w.namu.la/s/722018e62d96825bca2bb67a31b308d26382b801b8cfed848312b085de8f890f94ed9062d40278159537cff72a23fbedf4603727ba6723e3e81e106bc26d37257635aca421be8d39bd2e36555788163b444a44a99f39b67ba9e73adcc9072a9c)

일반적인 큐는 뒤에서는 삽입이, 앞에서는 인출이 이루어지지만,  
데크는 양쪽 어디서든 삽입과 인출이 가능하다.

<br />

### **deque 구현**

1. deque 생성

   ```python
   >>> from collections import deque
   >>> dq = deque()
   >>> dq
   deque([])
   ```

<br />

2. 스택 기능: `append()`, `pop()`  
   뒤에서 입출력

   ```python
   >>> dq.append(1)
   >>> dq.append(2)
   >>> dq.append(3)
   >>> dq
   deque([1, 2, 3])

   >>> dq.pop() # 3
   >>> dq
   deque([1, 2])
   ```

<br />

3. 큐 기능: `appendleft()`, `pop()` / `append()`, `popleft()`  
   앞에서 입력, 뒤에서 출력 / 뒤에서 입력, 앞에서 출력

   ```python
   >>> dq.appendleft(3)
   >>> dq
   deque([3, 1, 2])

   >>> dq.popleft() # 3
   >>> dq
   deque([1, 2])
   ```

<br />

4. deque 확장: `extend()`, `extendleft()`  
   기본적으로 오른쪽으로 확장되며,  
   왼쪽으로 확장시키고 싶다면 `extendleft()` 메소드 사용

   ```python
   >>> dq.extend('right')
   >>> dq
   deque([1, 2, 'r', 'i', 'g', 'h', 't'])

   >>> dq.extendleft('left')
   >>> dq
   deque(['t', 'f', 'e', 'l', 1, 2, 'r', 'i', 'g', 'h', 't'])
   ```

<br />

5. 리스트처럼 사용: `insert()`, `remove()`  
   리스트처럼 중간에 있는 값의 삽입, 삭제, 수정이 가능하다.

   ```python
   >>> dq[0] = 'modify' # 't' -> 'modify'
   >>> dq
   deque(['modify', 'f', 'e', 'l', 1, 2, 'r', 'i', 'g', 'h', 't'])

   >>> dq.insert(1, 'insert')
   >>> dq
   deque(['modify', 'insert', 'f', 'e', 'l', 1, 2, 'r', 'i', 'g', 'h', 't'])

   >>> dq.remove('insert') # 가장 처음 값 삭제
   deque(['modify', 'f', 'e', 'l', 1, 2, 'r', 'i', 'g', 'h', 't'])
   ```

<br />

6. deque 역순으로 반전: `reverse()`  
   deque 값들의 순서를 역순으로 취한다.

   ```python
   >>> dq
   deque(['modify', 'f', 'e', 'l', 1, 2, 'r', 'i', 'g', 'h', 't'])

   >>> dq.reverse()
   >>> dq
   deque(['t', 'h', 'g', 'i', 'r', 2, 1, 'l', 'e', 'f', 'modify'])
   ```

<br />

## **참고**

---

[dongdongfathe' Tistory >> 스택과 큐의 기능을 한번에 DEQUE](https://dongdongfather.tistory.com/72)

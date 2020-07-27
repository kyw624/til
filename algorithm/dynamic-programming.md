# 동적 계획법 (DP; Dynamic Programming)

### **개요**

복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 알고리즘이다.  
`대표적인 방식`으로 큰 문제를 작은 문제로 분할해가며 푸는 `Top-down`,  
작은 문제를 점점 쌓아 큰 문제를 푸는 `Bottom-up` 방식이 있다.

<br />

### **구현**

10까지의 피보나치 수열

> Top-down 방식

```python
def fibonacci(num):
    if memo[num] != 0:
        return memo[num]
    elif num <= 1:
        memo[num] = num
    else:
        memo[num] = fibonacci(num - 1) + fibonacci(num - 2)
    return memo[num]


n = 10
memo = [0] * (n + 1)

fibonacci(n) # 55
# memo = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

<br />

> Bottom-up 방식

```python
def fibonacci(num):
    memo[0] = 0
    memo[1] = 1
    for i in range(2, num + 1):
        memo[i] = memo[i - 1] + memo[i - 2]
    return memo[num]


n = 10
memo = [0] * (n + 1)

fibonacci(n) # 55
# memo = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

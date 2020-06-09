# 파이썬에서 2차원 배열 초기화 방법

## **Bad**

```python
arr = [[0] * n] * n
# 해당 코드는 [0] * n 이 n개 복제된 것과 같다.
```

<br />

## **Good**

```python
1.
arr = [[0] * n for i in range(n)]
```

```python
2.
arr = []
for i in range(n):
    arr.append([])
		for j in range(n):
		    arr[i].append(0)
```

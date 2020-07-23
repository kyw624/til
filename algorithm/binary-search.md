# 이분 탐색 (= 이진 탐색, Binary Search)

## **개요**

`정렬되어 있는` 리스트에서 범위를 절반씩 줄여가며 빠르게 탐색하는 알고리즘.  
정렬이 되어있어야 하며, 시작은 리스트의 중간 위치에서 출발한다.

탐색 할 때마다 크기를 반으로 줄여가는 점에서 `시간 복잡도`는 **O(Log<sub>2</sub>N)**

<br />

## **예제**

`1 3 5 7 9 11 13 15 17 19 21 23 25 27`에서 `11`의 위치 찾기

```python
A = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27]
data = 11

def binary_search(start, end):
    if start > end:
        return False # 리스트에 없는 값
    mid = (start + end) // 2
    if A[mid] == data:
        return mid
    elif A[mid] > data:
        return binary_search(start, mid - 1)
    else:
        return binary_search(mid + 1, end)

answer = binary_search(0, len(A) - 1) # 5

if answer:
    print('index: %d' % answer)
else:
    print('리스트에 존재하지 않는 값입니다.')
```

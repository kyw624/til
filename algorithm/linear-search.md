# 선형 탐색 (= 순차 탐색, Linear Search)

## **개요**

리스트의 정렬 유무에 상관없이 앞에서부터 끝까지 차례대로 탐색하는 알고리즘.  
리스트가 길어질수록 비효율적이지만, 가장 단순한 방법으로 구현이 쉽고 비정렬 리스트에도 사용할 수 있다.

`시간 복잡도`는 **O(n)**

<br />

## **예제**

`5 3 0 11 4 7 9`에서 `11`의 위치 찾기

```python
A = [5, 3, 0, 11, 4, 7, 9]
data = 11

def linear_search(arr, val):
    for i in range(len(arr)):
        if arr[i] == val:
            return i
    return False

answer = linear_search(A, data)

if answer:
    print('index: %d' % answer)
else:
    print('리스트에 존재하지 않는 값입니다.')
```

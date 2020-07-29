# 정렬

## 삽입 정렬 (Insertion Sort)

![Insertion Sort gif](https://upload.wikimedia.org/wikipedia/commons/2/25/Insertion_sort_animation.gif)

### **개요**

리스트의 모든 요소를 앞에서부터 차례대로 이미 정렬된 부분과 비교하여,  
자신의 위치를 찾아 삽입해가면서 정렬해간다.  
구현이 간단하지만, 배열이 길어질수록 효율이 떨어지는 알고리즘.  
같은 O(n<sup>2</sup>) 알고리즘인 선택, 버블 정렬에 비해 빠르다.

`시간 복잡도`는 **O(n<sup>2</sup>)**

<br />

### **구현**

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while (j >= 0 and arr[j] > key):
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr

a = [1, 3, 6, 2, 10, 4, 7, 5, 9, 8]

insertion_sort(a)
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

<br />

## **참고**

---

[위키백과 >> 삽입 정렬](https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC)

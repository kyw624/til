# 정렬

## 버블 정렬 (= 거품 정렬, Bubble Sort)

![Bubble Sort gif](https://upload.wikimedia.org/wikipedia/commons/3/37/Bubble_sort_animation.gif)

### **개요**

인접한 두 원소를 검사하며 정렬하는 알고리즘으로
가장 손쉽게 구현할 수 있고 직관적이지만, 비효율적인 정렬 알고리즘이다.

`시간 복잡도`는 **O(n<sup>2</sup>)**

<br />

### **구현**

```python
def bubble_sort(arr):
    for i in range(len(arr) - 1):
        for j in range(len(arr) - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

a = [1, 3, 6, 2, 10, 4, 7, 5, 9, 8]

bubble_sort(a)
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

<br />

## **참고**

---

[위키백과 >> 거품 정렬](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)

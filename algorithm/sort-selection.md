# 정렬

## 선택 정렬 (Selection Sort)

![Selection Sort gif](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b0/Selection_sort_animation.gif/220px-Selection_sort_animation.gif)

### **개요**

리스트에서 최소값을 찾고, 맨 앞에 위치한 값과 교체한다.  
맨 처음 위치를 뺀 나머지를 같은 방법으로 교체해 나간다.  
알고리즘이 단순하고 메모리가 제한적인 경우에 사용시 성능 상의 이점이 있는 알고리즘.

`시간 복잡도`는 **O(n<sup>2</sup>)**

<br />

### **구현**

```python
def selection_sort(arr):
    for i in range(len(arr) - 1):
        min = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[min]:
                min = j
        arr[i], arr[min] = arr[min], arr[i]
    return arr

a = [1, 3, 6, 2, 10, 4, 7, 5, 9, 8]

selection_sort(a)
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

<br />

## **참고**

---

[위키백과 >> 선택 정렬](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)

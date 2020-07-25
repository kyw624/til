# 그래프 탐색

## 너비 우선 탐색 (BFS; Breadth-First Search)

![BFS gif](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcXGMpF%2FbtqBg8hM2BK%2FqCwHO6sIrj668sLyjedGwK%2Fimg.gif)

### **개요**

`맹목적 탐색` 방법의 하나로 시작 정점을 방문 후 시작 정점에 인접한 모든 정점들을 우선 방문하는 알고리즘.  
`Queue`를 사용해 구현한다.

> **맹목적 탐색? (Blind Search)**
>
> 이미 정해진 순서에 따라 문제에 대한 정보를 고려하지 않고 상태 공간 그래프를 형성해가며 해를 탐색하는 방법으로,  
> 문제 정보를 이용하는 `정보이용 탐색`과 달리 비효율적이고 시간이 오래 걸린다.

<br />

### **구현**

파이썬에서 `list`를 사용해 구현하는 것보다  
`collections` 모듈의 `deque` 를 사용하면 시간을 절약할 수 있다.

```python
from collections import deque


def bfs(graph, root):
    visited = []
    q = deque()
    q.append(root)

    while q:
        node = q.popleft()
        if node in visited:
            continue
        visited.append(node)
        q.extend(graph[node])

    return visited


graph = dict()

graph['A'] = ['B', 'C']
graph['B'] = ['A', 'D']
graph['C'] = ['A', 'G', 'H', 'I']
graph['D'] = ['B', 'E', 'F']
graph['E'] = ['D']
graph['F'] = ['D']
graph['G'] = ['C']
graph['H'] = ['C']
graph['I'] = ['C', 'J']
graph['J'] = ['I']

bfs(graph, 'A')
# ['A', 'B', 'C', 'D', 'G', 'H', 'I', 'E', 'F', 'J']
```

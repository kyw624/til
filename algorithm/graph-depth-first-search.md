# 그래프 탐색

## 깊이 우선 탐색 (DFS; Depth-First Search)

![DFS gif](https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif)

### **개요**

정점들을 탐색할 때 최대한 깊숙히 들어가서 확인한 뒤 다시 돌아가 다른 루트를 탐색하는 방식이다.  
`Stack`을 사용해 구현한다.

<br />

### **구현**

```python
def dfs(graph, root):
    visited = []
    stack = []
    stack.append(root)

    while stack:
        node = stack.pop()
        if node in visited:
            continue
        visited.append(node)
        stack.extend(graph[node])

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

dfs(graph, 'A')
```

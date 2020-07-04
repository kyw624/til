# 여러 줄의 사용자 입력값 리스트에 받기

## 반복문 사용

### **기본형**

```python
arr = []

for _ in range(count):
    arr.append(input())
```

<br />

### **한 줄로 단축**

```python
arr = [input() for _ in range(count)]
```

# 최대공약수(GCD) 구하기

## **유클리드 호제법**

두 자연수를 서로 나누어떨어질 때까지 나눠가면서 최대공약수를 빠르게 얻을 수 있는 알고리즘이다.

예시로 1071과 1029의 최대공약수를 구하면,

1. `1071 % 1029 = 42`  
   => a = 1029, b = 42
2. `1029 % 42 = 21`  
   => a = 42, b = 21
3. `42 % 21 = 0` **_(나누어떨어짐)_**
4. 최대공약수는 `21`이다.

```python
def gcd(a, b):
    if a == b:
        return a
    else:
        r = a % b
        if r == 0:
            return b
        else:
            return gcd(b, r)

A, B = map(int, input().split())

if A < B:
    A, B = B, A

print(gcd(A, B))
```

<br />

## **참고**

---

[위키백과 >> 유클리드 호제법](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95)

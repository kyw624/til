# 소수 판별

## **N까지의 소수 구하기**

에라토스테네스의 체를 알기 전 사용한 방법이다.

1. `num`을 `2`부터 `num - 1`까지 나누어지는지 확인한다.
2. 단 `num`의 **제곱근**까지만 판별한다.
3. 나누어지지 않으면 소수이다.

```python
def prime(num):
    if num < 2:
        return False
    for i in range(2, num):
        if i * i > num:
            break
        elif num % i == 0:
            return False
    return True

N = int(input())
answer = []

for i in range(N + 1):
    if prime(i):
        answer.append(i)
```

<br />

## **에라토스테네스의 체**

2부터 N까지 수의 배열을 만든 후 2, 3, 5, 7의 배수를 순서대로 지워가며 소수를 찾는 방법이다.

1. `2`부터 `N`까지 구간의 모든 수의 배열을 만든다.
2. `2`는 **소수**이므로 자신을 제외한 `2의 배수`를 모두 지운다.
3. 남은 수에서 `3은` 소수이므로 자신을 제외한 `3의 배수`를 모두 지운다.
4. 남은 수에서 `5은` 소수이므로 자신을 제외한 `5의 배수`를 모두 지운다.
5. 남은 수에서 `7은` 소수이므로 자신을 제외한 `7의 배수`를 모두 지운다.
6. 위의 과정을 반복하면 배열에는 소수만 남게된다.

```python
def prime(n):
    sieve = [True] * (n + 1)
    m = int(n ** 0.5)
    for i in range(2, m + 1):
        if sieve[i] == True:
            for j in range(i+i, n + 1, i):
                sieve[j] = False
    return [i for i in range(2, n + 1) if sieve[i] == True]

N = int(input())
prime(N)
```

<br />

## **참고**

---

[위키백과 >> 에라토스테네스의 체](https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4)

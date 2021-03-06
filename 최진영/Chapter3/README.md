# 효율성

## 3.1 시간복잡도

시간복잡도(time complexity)는 입력에 대해 알고리즘이 얼마만큼의 시간을 사용할지를 근사적으로 알려준다. 시간복잡도를 계산해 보면, 알고리즘을 구현하지 않고도 문제를 풀 만큼 알고리즘의 속도가 빠른지 여부를 알 수 있다.

시간복잡도는 O(...)로 표기하며, 이때 괄호 안에는 어떤 함수가 들어간다. 보통 변수 n으로 입력의 크기를 나타낸다.

### 3.1.1 계산 규칙

단일 명령어로만 구성되어 있다면 시간 복잡도는 O(1)이다. 일반적으로 반복문이 k중으로 중첩되어 있으며 각 반복문이 n번씩 수행된다면, 그 시간 복잡도는 O(n^k)이다.

재귀함수의 시간 복잡도는 함수가 몇번 호출되는지, 그리고 각 호출 때의 시간복잡도가 어떻게 되는지에 따라 결정된다.

```
def f(n):
 if n==1 : return
 f(n-1)
```

f(n)을 수행하면 함수 호출이 n번 발생하고, 각 호출의 시간 복잡도는 O(1)이다. 따라서 전체 시간 복잡도는 O(n)이 된다.

```
def f(n):
 if n==1 : return
 f(n-1)
 f(n-1)
```

인자가 n-k인 함수 호출은 2^k번 일어나고, 이때, k = 0,1,..,n-1이다. 따라서 전체 시간 복잡도는 O(2^n)이 된다.

### 3.1.2 자주 접할 수 있는 시간 복잡도

- O(1) 상수 시간 알고리즘
- O(log n) 로그 시간 알고리즘
- O(n) 제곱근 시간 알고리즘
- O(n) 선형 시간 알고리즘
- O(n logn)
- O(n^2) 제곱 시간 알고리즘
- O(n^3) 세제곱 시간 알고리즘
- O(2^n)
- O(n!)

알고리즘의 시간 복잡도가 어떤 상수 k에 대해 O(n^k)을 넘지 않으면 다항 시간 알고리즘이라고 한다. 이 책에서 살펴볼 알고리즘은 대부분 다항 시간 알고리즘이다. 그러나 중요한 문제 중에서 아직 아무도 효율적인 풀이를 알지 못하는 문제도 많이 존재한다. (ex. NP-하드문제)

### 3.1.3 효율성 추정하기

| 입력의 크기 | 추정 시간 복잡도 |
|---|---|
| n <= 10 | O(n!) |
| n <= 20 | O(2^n) |
| n <= 500 | O(n^3) |
| n <= 5000 | O(n^2) |
| n <= 10^6 | O(n log n) or O(n) |
| n이 클때 | O(1) or O(log n) |

하지만, 이것은 추정 일 뿐이며 O(n) 시간 복잡도이지만 n/2 연산을 진행하는 알고리즘과 5*n 연산을 진행하는 알고리즘과 시간 차이는 많이 존재한다는 것을 염두해야한다

### 3.1.4 엄밀한 정의

알고리즘 수행 시간이 O(f(n))이라는 것의 정확한 의미는 어떤 상수 c와 n0이 존재하여, 크기가 n>=n0인 모든 입력에 대해 알고리즘이 최대 cf(n)번의 연산만 수행한다는 것이다. 즉, O표기법은 충분히 큰 입력에 대해 알고리즘 수행 시간의 상한을 알려준다.

이외에도 수행 시간의 하한을 알려주는 파이 표기법과 수행 시간의 평균을 알려주는 세타 표기법이 존재한다.

## 3.2 예제 문제

### 3.2.1 최대 부분 배열 합

배열에 수 n개가 들어있을 때 최대 부분 배열 합(maximum subarray sum)을 구하는 문제이다. 즉, 배열에서 연속해 있는 값들을 택하여 그 합을 최대로 만드는 것이다.

- O(n^3) 시간 풀이

```
best = 0
for a in range(n):
 for b in range(a,n):
  sum = 0
  for k in range(a,b+1):
   sum += array[k]
  best = max([best,sum])
print(best)
```

가능한 모든 부분 배열을 하나씩 살펴보고, 그 부분 배열의 합을 구한 후 최대 합을 관리해 나가는 것이다.

- O(n^2) 시간 풀이

```
best = 0
for a in range(n):
sum = 0
 for b in range(a,n):
  sum += array[k]
  best = max([best,sum])
print(best)
```

위의 알고리즘에서 반복문 하나를 줄인다. 이를 위해서는 부분 배열의 오른쪽 끝을 이동해 가면서 그 합도 같이 계산해나가면 된다.


- O(n) 시간 풀이

```
best = 0
sum = 0
for k in range(n):
 sum = max([array[k], sum+array[k]])
 best = max([best,sum])
print(best)
```
배열의 각 위치에 대해, 그 위치에서 끝나면서 합이 최대인 부분 배열을 계산해나가면 된다. 부분 배열이 위치 k의 원소 하나만으로 이루어진 경우,
와 위치 k-1에서 끝나는 부분 배열에 위치 k의 원소를 덧붙여 부분 배열을 만드는 경우 나누어서 생각해보자.

### 3.2.2 두 퀸 문제

크기가 nXn인 체스판이 있을 때 두개를 서로 공격할 수 없도록 배치하는 방법의 수를 세는 문제이다.

- O(n^4) 시간 풀이
```
count = 0
for a in range(n):
 for b in range(n):
  chess[a][b] = 1
  for c in range(n):
   for d in range(n):
    if a == c : continue
    if b == d : continue
    # 대각선에 있는지 확인하는 부분
    count += 1
  chess[a][b] = 0
print(count/2)
```

체스판 위에 퀸 두개를 배치하는 모든 방법을 하나씩 살펴보면서 둘이 서로 공격할 수 없는 경우의 수를 세는 것이다.

- O(n^2) 시간 풀이

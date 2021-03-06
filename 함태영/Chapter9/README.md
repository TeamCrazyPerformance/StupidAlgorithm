# Chapter9. 구간 질의
배열에 대한 구간 질의를 효율적으로 처리하는 자료구조를 살펴보자.  
예로는 구간 합 질의, 구간 최소 질의가 있다.  

## 9.1 정적 배열에 대한 질의
배열이 정적인 경우, 즉 질의 사이에 배열의 원소가 절대로 변하지 않는 경우를 살펴본다.  
이 경우에는 배열에 대한 전처리만 잘 수행해도 구간 질의에 효율적으로 답 할 수 있다.  
먼저, 누적 합 배열을 이용하여 구간 질의를 간단하게 처리하는 방법을 살펴본다.  

### 9.1.1 합 질의
구간 합 질의 `sum(a,b)`는 배열에서 위치가 구간 [a,b]에 속하는 원소들의 합을 나타낸다.  
만약 누적 합 배열을 미리 구해놓는다면 `PrefixSum[b]-PrefixSum[a-1]`로 쉽게 구간 합 질의를 해결 할 수 있다.  
누적합 배열의 각 원소는 주어진 배열에서 그 위치까지의 원소들의 합이다. 즉 위치 k의 원소는 `sum(0,k)`이다.  

누적 합 배열은 `O(n)`시간에 구할 수 있으며 `sum(a,b)`는 `O(1)`시간에 구할 수 있다.  

#### 고차원에 대한 일반화
책에 있는 그림을 표현하고 싶지만 마땅치 않아 그냥 글로 표현한다.  
임의의 직사각형 크기의 범위의 부분 합을 구하기 위해서는 다음 공식으로 나타낼 수 있다.  
`S(A)-S(B)-S(C)+S(D)`  
> 이 수식이 어떻게 유도된지는 잘 모르겠다.

### 9.1.2 최소 질의
구간 최소 질의 `min(a,b)`는 배열에서 위치가 구간 [a,b]에 속하는 원소들의 합을 나타낸다.  
이 책에서 소개하는 기법은 전처리를 `O(n log n)`시간에 해결하고 `O(1)`시간에 최소 질의를 해결하는 알고리즘이다. 
이 기법은 Bender와 Farach-colton이 만든 것으로 **희소 테이블** 이라는 이름으로 불린다.  

아이디어는 구간의 길이에 해당하는 `b-a+1`값이 2의 거듭제곱인 `min(a,b)`를 사전에 미리 전부 다 구해놓는 것이다.  
사실 이해가 잘 안됬는데 곰곰하게 생각해보니까 이해가 되었다.  

위의 내용에 대해서 간단하게 생각하자면 2의 거듭제곱의 길이가 되는 부분 집합에 대해 최소값을 구하는 것이다.  
주어진 배열이 [0,5]이고 모든 `min(a,b)`를 구해보면 아래와 같다.  
```
length 2 : min(0,1), min(1,2), min(2,3), min(3,4), min(4,5)
length 4 : min(0,4), min(1,5) 
```
이렇게 최소값 배열을 만든다.  
`minq(a,b) = min(minq(a, a+k-1), minq(b-k+1,b))`  
로 구할 수 있는데, 이 때 `b-a+1`이 2의 거듭제곱이고 `w=(b-a+1)/2`로 만들면 된다.  
a는 index로 설정하고 반복문으로 늘려나가면 b는 자동으로 구할 수 있다.(`b-a+1`이 2의 거듭제곱임을 이용)  

이렇게 구한 최소값 배열을 이용해서 어떤 `minq(a,b)`값도 `O(1)`에 구할 수 있다.  
k를 `b-a+1`을 초과하지 않는 가장 큰 2의 거듭제고 수라고 생각하면  
`minq(a,b) = min(minq(a, a+k-1),minq(b-k+1,b))` 로 구할 수 있다.  
이 공식은 구간 [a,b]를 k길이를 가진 구간 [a, a+k+1], [b-k+1, b]로 나눌 수 있다.  
그로 인해 이미 구해놓은 구간 길이 k에 대한 최소값을 구한 배열로 `O(1)`만에 구할 수 있게 된다.  
> 너무 어렵다.

## 9.2 트리형 자료구조

### 9.2.1 이진 인덱스 트리
이진 인덱스 트리 혹은 팬윅 트리는 누적합 배열의 동적인 자료구조라고 볼 수 있다.  
이 자료구조는 `O(log n)`시간이 걸리는 연산 두가지가 존재하는데, 첫 번째는 구간합 질의를 처리하는 연산. 두 번째는 배여의 원소를 갱싱하는 연산이다.  
이 자료구조는 트리라고 언급되지만 사실상 배열의 형태료 포시한다. 또한 모든 배열의 인덱스가 1부터 시작한다고 가정한다.  

`p(k)`를 2의 거듭 제곱인 k의 약수 중에서 그 크기가 제일 큰 것으로 생각하면 배열 `tree`에 다음과 같이 이진 인덱스 트리를 저장 할 수 있다.  
`tree[k] = sum(k-p(k)+1, k)`  
즉, tree[k]에 저장되는 값은 원래 배열에 대해 위치 k에 끝나면서 그 길이가 `p(k)`인 구간의 합이다.  
예를 들어서 `p(6)=2`이므로 `tree[6] = sum(5,6)`이다.  

이진 인덱스 트리를 이용하면 `sum(1,k)`에 대해 `O(log n)`에 구할 수 있는데 그 이유는 구간 [1,k]를 이진 인덱스 트리에 합이 저장된 부분 구간 `O(log n)`개로 나눌 수 있기 때문이다.  
`sum(1,7)`을 구하는 경우를 예를 들어보자.  
`sum(1,7) = sum(1,4) + sum(5,6) + sum(7,7)` 로 나눠서 구할 수 있다.  
만약 a가 1보다 크다면 이전에 누적 합 배열에서 사용한 공식을 토대로 개조하여 사용해도 된다.  
`sum(a,b) = sum(1,b) - sum(1,a-1)` 

만약 배열의 원소를 수정했다면 이를 갱신하기 위한 시간이 필요한데, 이 또한 `O(log n)`시간이 걸린다.

#### 구현

`p(k)`에 대한 값은 비트 연산을 이용하면 쉽게 구한다.  
`p(k) = k & -k`  
이 공식은 k의 비트 1중 최하위 비트 한 개만 남기는 공식이다.  
> 10011이라면 00001이 된다.

다음은 `sum(1,k)`를 구하는 함수이다.
```cpp
int sum(int k){
    int s = 0;
    while( k >= 1){
        s += tree[k];
        k -= k&-k;
    }
    return s;
}
```

배열 위치 k에 저장된 값을 x만큼 증가 시키는 함수이다.  
```cpp
void add(int k, int x){
    while(k <= n){
        tree[k] += x;
        k += k&-k;
    }
}
```
두 함수의 시간 복잡도는 모두 `O(log n)`이다.  

### 9.2.2 구간 트리
구간 트리는 `O(log n)` 시간이 걸리는 연산 두 가지를 지원하는 자료 구조이다.  
구간 질의를 처리하고, 배열의 원소를 갱신해주는 연산 두 가지를 지원하는데 구간 트리는 합, 최소 그 외의 질의를 처리한다.  
구간 트리는 이진 트리의 일종이며 말단 노드는 원래 배열의 원소에 Mapping된다. 나머지 노드들은 다양한 구간 질의를 처리하기 위한 정보를 담고 있다.  
**구간 트리는 배열의 크기가 2의 거듭제곱이라는 점과 인덱스가 0부터 시작된다는 점을 가정한다.**  
만약 배열의 크기가 2의 거듭제곱이 되지 않는다면 그렇게 만들어 줘야 한다.  

책에 있는 그림을 표현하고 싶은데 그러지 못해서 너무 아쉽지만 글로 설명을 적어본다.  
구간 트리의 연산들이 왜 `O(log n)`의 시간을 사용하는지에 대해 우선 설명하자면 임의의 구간[a,b]를 설정하고 이에 대한 합 질의를 처리하려고 한다고 가정하자.  
그렇다면 말단 노드를 대변하고 있는 원래 배열에서 이진 트리를 만들어가보면 가장 높은 레벨에 있는 노드를 합하면 합 질의에 대한 답이 나온다.  
그렇다면 이 노드들의 개수는 기존 배열의 크기인 n에 비해 log n개 밖에 없기 때문에 합 질의는 `O(log n)`에 처리가 가능한 것이다.  
> 배열 원소가 갱신되어 노드들의 갱신이 필요한 경우도 위의 맥락과 비슷하다.  

#### 구현
구간 트리의 내용을 저장하기 위해서는 Linked List를 사용하는 것보다 2n크기의 배열을 사용하는 것이 좋을 것이다.  
가장 앞 인덱스를 트리의 root라고 생각하고 붙여나가면 된다.
> 0번 인덱스는 사용하지 않는다. 그 이유는 상위 노드 인덱스를 계산할 때 복잡해지기 때문이다.  

tree에서 tree[k]의 부모는 tree[k/2]이며, 왼쪽 자식은 tree[2k], 오른쪽 자식은 tree[2k+1]임을 이용하면 쉽다.  

아래의 코드는 `sumq(a,b)`의 값을 구하는 함수이다.   
```cpp
int sum(int a, int b){
    a += n; b += n; // 원래 배열의 길이를 더해줌으로써 해당 구간이 노드의 말단에 맵핑되게 한다.
    int s = 0;
    while(a<=b){
        if(a%2==1) s+=tree[a++]; // 만약 a가 오른쪽 노드라면 답에 현재 값을 더해주고 a는 경로 이탈을 방지하기 위해 범위를 옮겨준다.
        if(b%2==0) s+=tree[b--]; // 만약 b가 왼쪽 노드라면 답에 현재 값을 더해주고 b는 경로 이탈을 방지하기 위해 범위를 옮겨준다.
        a /= 2; b /= 2; // level을 한 단계 올려준다.
    }
    return s;
}

다음 코드는 배열의 위치 k에 저장된 값을 x만큼 증가시키는 함수이다.  
```cpp
void add(int k, int x){
    k += n; // 말단 노드로 이동한다.
    tree[k] += x; // 말단 노드 값을 변경 한다.
    for(k/=2; k>1; k/=2){
        tree[k] = tree[2*k]+tree[2*k+1]; // 부모를 찾아가면서 값을 update를 해준다.
    }
}
```

### 9.2.3 고급 기법
#### 인덱스 압축
만약 인덱스 값이 많이 커진다면 메모리 이슈가 발생할 수 밖에 없을 것이다.  
만약 쓰일 인덱스를 미리 알고 있다면 맵핑을 해서 인덱스를 효율적으로 압축 시킬 수 있다!  
하지만 인덱스 a,b를 c(a), c(b)로 치환하려면 제약이 있다.  
`a<b라면 c(a)<c(b)가 보장되어야한다.`  
이렇게 인덱스를 압축시킨다면 메모리를 효율적으로 아낄 수 있을 것이다.  

#### 구간 단위 갱신
구간[a,b]에 대해서 모든 원소 값을 증가시키는 연산을 하려면 어떻게 해야할까?  
인접한 원소와 차이를 저장하는 차이 배열을 이용하는 문제가 있다면 구간 단위 갱신은 매우 민감한 문제일 수도 있다.  

결론적으로는 구간[a,b]에 대해 모든 원소값을 x만큼 증가시키고 차이 배열에 위치 a 원소를 x만큼 증가시키고 b+1원소를 x만큼 감소시키면 된다.  
> 음? 꿀팁인데 쓰이는 곳이 차이 배열 밖에 없지 않나.. 왜 넣어뒀지?


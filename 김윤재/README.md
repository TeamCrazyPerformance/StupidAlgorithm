# *알고리즘 트레이닝* 의 정리

### ( *Guide to Competitive Programming: Learning and Improving Algorithms Through Contests* by Antti Laaksonen )

## 1장 : 생략 (하기 싫어요)

## 2장 : 프로그래밍 기법 *Programming Techniques*

이 장은 C++ 프로그래밍의 특징, 재귀, 비트연산을 다룬다.

### 2.1 언어적 특성 *Language Features*
   
표쥰적 경진 프로그래밍 코드 형태 :
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    //code
}
```
<bits.stdc++> 은 표준 라이브러리 전체를 포함한다. (iostream, vector, algrotihm 등 포함)

#### 2.1.1 입력과 출력

- 표준 stream 으로 입출력 처리한다. 
  
```cpp
int a,b;
string str;
cin>>a>>b>>str;
```

개행이나 공백으로 입력 분리된다.

- 간혹 입출력이 프로그램의 병목이 될 때가 있으면 아래와 같이 처리한다.
    
```cpp
ios::sync_with_stdio(0); //standard stream synch -> false
cin.tie(0); //unties cin from cout
```

- endl 보다 "\n" 이 더 빠르다.
        
- 데이터 양을 모를때 반복문으로 처리한다. 하나씩 읽어 들이다 데이터가 없을 때 멈춘다.

```cpp
while(cin>>x){
//code
}
```

- 데이터가 파일로 주어지면 평소처럼 표준 스트림을 이용하는 방법은 입력이 input.txt, 출력을 output.txt로 할때 다음과 같다.

```cpp
freopen("input.txt","r",stdin);
freopen("output.txt","w",stdout);
```

#### 2.1.2 수의 처리

- 정수의 처리
알고리즘 경진에서 int의 범위(최대 2^31-1)를 벗어나게 되는 큰 수는 보통 long long으로 처리하게 된다. 이때 조심해야 할 버그가 있다.

```cpp
int x = 6666666666;
long long y = x*x;
cout<<y<<"\n"; //Overflows.
```

이는 y의 자료형이 long long이지만 x*x에 포함된 수의 자료형이 int로 결과도 int로 처리되기 때문이다.

```cpp
long long y = (long long)x*x;
```

로 바꾸어주면 된다.

- 나머지 연산
나머지 (Modulo) 연산은 (a+b) mod m = ( (a mod m) + (b mod m) ) mod m 을 만족한다. 이를 이용하여 값을 작게 유지하기 위해 연산을 수행할 때마다 mod를 취해 주면 된다.
예를 들어 n!을 m으로 나눈 나머지를 구하는 코드가 있다.

```cpp
long long x=1;
for(int i=1;i<=n;i++) x=(x*i)%m;
cout<<x<<"\n">>;
```

C++ 언어가 음수의 나머지를 음수로 취급함에 따라 음수면 m을 더해주면 된다.
```cpp
if(x<0) x += m;
```

- 부동 소수점 실수 (float, double) 
long double이 정밀도가 높아 사용하기 좋다. 부동 소수점 오차는 == 연산자를 사용하지 않고 두 수의 차이가 ε 이하로 처리하여 사용할 수 있다. 출력도 다음과 같이 할 수 있다.\
```cpp
double b=0.7*3-1.1;
double a=0.3*3+0.1;
if(abs(a-b)<1e-9){ // ε = 10^-9
printf("%.20f\n",a);  
printf("%.20f\n",b); 
// a,b 값은 오차로 미묘하게 다르다.
}
```
      
### 2.1.3 짧은 코드

- 자료형
typedef 로 자료형을 짧게 줄일 수 있다.
```cpp
typedef long long ll;
```
- 매크로
선처리 #define 으로 매크로를 정의할 수 있다.

```cpp
#define F first
#define SWAP(x, y, T) do { T SWAP = x; x = y; y = SWAP; } while (0)
```

SWAP 매크로는 타입을 사용해서

```cpp
SWAP(a, b, int);
```

### 2.2 재귀적 알고리즘

### 2.2.1 부분집합 생성하기

원소가 n개인 집합의 부분집합을 모두 생산하는 알고리즘을 재귀로 구현한다.
하나의 원소씩 포함하거나 제외하는 2^n 번의 과정을 재귀로 구현하기 좋다.

```cpp
void subsets(vector<int>& A, vector<vector<int>>& save, 
                 vector<int>& currentSet, int k) 
{ 
    save.push_back(currentSet); 
    for (int i = k; i < A.size(); i++) { 
        currentSet.push_back(A[i]); // 원소 포함 
        subsets(A, save, currentSet, i+1); 
        currentSet.pop_back(); // 원소 제외
    } 
  
    return; 
} 

```

#### 2.2.2 순열 ( Permutations )

부분집합과 유사한 방법으로 각 원소를 이번 순열에 포함 시켰는지 포함하지 않았는지를 결정하는 배열을 두고 재귀한다.
```cpp
int permutations(int input, vector<vector<int>>& save) 
{
    if (permutationSet.size()==input){
        save.push_back(permutationSet);
    }
    for (int i = 1; i <= input; i++) {
        if(chosen[i])continue;
        chosen[i]=true;
        permutationSet.push_back(i);
        permutations(input,save);
        chosen[i]=false;
        permutationSet.pop_back();
    }    
    return 0; 
}

```

#### 2.2.3 퇴각 검색 ( Backtracking )

## 3장 : 효율성 *Efficiency*

이 장은 알고리즘의 시간 복잡도를 다루어 알고리즘의 효율성을 다룬다.
빅오 표기법 (Big-O Notation) 과 앞서 소개된 문제의 시간 복잡도를 설명한다. 

## 4장 : 정렬과 탐색 

4장에서는 입력된 데이터를 정렬하고 탐색하는 알고리즘을 다룬다.

### 4.1 정렬 알고리즘 

기본적인 정렬 문제는 원소가 n개 주어질 때 원소들을 크기가 증가하거나 감소하는 순서로 정렬하는 문제이다.

#### 4.1.1 Bubble Sort

버블 정렬(Bubble Sort)는 가장 직관적 정렬 알고리즘으로 *O(n^2)* 시간 동안 동작하며 간단한 2중 for 문으로 구현된다.
```cpp
for(int i=0;i<n;i++){
    for(int j=0;j<n-1;j++){
        if(arr[j]>arr[j+1])
        swap(arr[j],arr[j+1]);
    }
}
```
가장 큰 원소부터 올바른 위치에 놓이게 되는 원리로 한 페어씩 비교되어 정렬된다.

##### *역위*
*역위 (inversion)* 는 정렬 알고리즘을 분석할 때 유용한 개념이다.
배열의 index 는 a<b 이지만 값 array[a]>array[b] 일때 이를 역위라 한다.
역위의 개수가 배열을 정렬하는데 필요한 작업량을 결정한다.
정렬 알고리즘이 연달아 있는 원소만 바꾼다면 역위의 개수대로 연산을 해야해서 
1+2+ ... (n-1) = n(n-1)/2 = *O(n^2)* 이 될 수 밖에 없다.

#### 4.1.2 Merge Sort

정렬 알고리즘의 효율을 높이기 위해서는 배열의 바로 옆이 아닌 다른 위치에 있는 원소들의 순서를 바로 잡아야 한다.
그러한 방식을 이용하는 병합 정렬 (Merge Sort) 은 *O(n log n)* 시간 복잡도를 가진다.

병합 정렬은 부분 배열 arr[a,...,b]에 대해 다음과 같은 절차를 따라 재귀적으로 실행된다.
1. 원소가 하나 (a==b) 라면 아무것도 하지 않는다.
2. 가운데 원소의 위치를 k = (a+b)/2 로 계산한다.
3. 재귀적으로 arr[a,...,k]를 정렬한다.
4. 재귀적으로 arr[k,...,b]를 정렬한다.
5. 3번의 부분배열과 4번의 부분배열을 병합하여 정렬된 배열 arr[a,...,b]를 만든다.

단계마다 부분배열의 크기를 절반으로 줄여 재귀 호출은 log2(n)으로 O(log n) 이 되고 각 단계의 처리가 *O(n)*이 되어 *O(n log n)* 복잡도가 된다.

```cpp
void merge_sort(int *arr, int low, int high)
{
    int mid;
    if (low < high){
       
        mid=(low+high)/2;
        merge_sort(arr,low,mid);
        merge_sort(arr,mid+1,high);
        merge(arr,low,high,mid);
    }
    else return;
}
```

#### 4.1.3 정렬의 하한 Lower Bound

배열의 원소를 비교하는 연산만으로는 *O(n log n)* 보다 빠르게 정렬할 수 없다.

#### 4.1.4 Counting Sort 

계수정렬(Counting Sort)은 배열의 원소를 직접 비교하지 않고 다른 정보를 이용하여 *O(n log n)* 이라는 하한이 적용되지 않는다. 이 알고리즘은 장부와 같은 배열을 사용하고 이 장부 배열의 인덱스가 원래 배열의 원소가 된다. 
( 원래 배열의 원소가 0 ... c 범위의 정수 일 때만 사용 할 수 있다 )
이 알고리즘은 원래 배열에 1이라는 원소가 3번 들어있으면 장부 배열의 인덱스 1의 값을 3으로 등록하는 알고리즘으로 *O(n)* 시간 만이 걸린다.

#### 4.1.5 실제 상황에서의 정렬

실제 상황에서는 정렬 알고리즘을 구현하지 않고 보통 라이브러리에 있는 정렬 함수를 사용한다.

```cpp
//일반적 벡터의 정렬
vector<int> vect = {6,3,1,5};
sort(vect.begin(),vect.end());
//일반적 배열의 정렬
int arr[] = {6,3,1,5};
int size=sizeof(arr)/sizeof(arr[0]);
sort(arr,arr+size);
```

##### 비교 연산자 (Comparison Operator)
sort 함수를 사용할 때는 원소의 자료형에 대한 비교 연산자가 정의되어 있어야한다.
대부분의 C++ 자료형은 비교연산자가 정의되어있어 숫자는 크기, 문자열은 알파벳 순으로 정렬된다.
두 원소의 pair 의 경우는 첫 원소를 기준으로, 그 다음에는 두 번째 원소를 기준으로 정렬된다.

사용자 정의 구조체는 기본 비교 연산자가 없어 operator< 형식으로 정의해야 한다.
```cpp
struct myStruct {
    int x,y;
    bool operator<(const myStruct &m){
        if(x==m.x)return y<m.y;
        else return x<m.x;
    }
}
```

##### 비교 함수 (Comparison Function)
외부에 정의한 비교 함수를 sort 함수에 콜백 형태로 넘길 수 있다.
```cpp
sort(vect.begin(),vect.end(),compare);
```

### 4.2 정렬을 이용한 문제 풀이

정렬을 하면 효율을 좋게 설게 할 수 있는 알고리즘이 많다. 예를 들어, 한 배열의 모든 원소가 같은가를 검사하는 알고리즘은 이중 for문으로 모든 원소를 모든 원소와 비교하는 방식보다 그 배열을 정렬 한 뒤 옆의 원소와의 비교만을 하는 방식이 효율이 좋다. 
( *O(n^2)* -> *O(n)*)

#### 4.2.1 스윕 라인 알고리즘 (Sweep Line Algorithm)

스윕 라인 알고리즘은 정렬된 순서대로 처리되는 이벤트의 집합으로 알고리즘을 모델링하는 방법이다.
예를 들어 가게에 손님이 한번에 가장 많은 경우 손님수의 최댓값을 구하는 문제가 있다면 입장을 시간순으로 정렬하여 손님의 입장과 퇴장에 따라 카운터로 둔 변수를 ++ -- 해주면 된다.

예를 들어 하루동안 가게에 손님들이 방문한 시간과 떠난 시간에 대한 정보가 있을 때, 동시에 손님이 가장 많은 시간을 구하는 문제를 해결한다고 하자.
이를 해결하기 위해 손님마다 방문과 떠남이라는 이벤트를 만들어 이의 시간을 정렬한 뒤 카운터 변수를 설정하여 손님이 방문하면 카운터++ 떠나면 카운터-- 로 문제를 해결할 수 있다.

#### 4.2.2 이벤트 스케줄링

스케줄링 문제 중 입력 데이터를 정렬하고 탐역 알고리즘으로 해결 할 수 있는 문제가 많다.
예를 들어 시작 시간과 종료 시간이 정해져있는 이벤트가 여러개 주어지고 일정한 시간 내에 최대한 많은 이벤트를 처리 할 수 있는 스케줄을 만들고자 하는 문제가 있다.
이를 길이가 짧은 이벤트를 우선적으로 선택하거나 시작 시간이 빠른 이벤트를 우선적으로 선택하는 알고리즘을 구현하면 올바른 답을 구할 수 없지만, 이벤트 종료시간을 기준으로 정렬한 후 가장 먼저 종료하는 이벤트를 차례대로 선택하는 알고리즘을 사용하면 올바른 해를 구할 수 있다.

### 4.3 이진 탐색

이진 탐색 (Binary Search) 는 정렬된 배열에 특정 원소가 존재하는지 파악 하는 문제를 해결하는 알고리즘인다. *O(log n)* 시간으로 해결된다.

#### 4.3.1 이진 탐색의 구현

이진 탐색은 두가지 방법으로 구현할 수 있다.
1. 사전 탐색 방법
   이진 탐색을 구현할 때 가장 많이 사용되는 방법으로 사전에서 단어를 찾는 방법과 유사하다. 
   처음에는 배열 전체를 가지고 가운데서 시작하여 찾는 값과 비교하여 배열의 원소값이 찾는 값보다 더 크면 부분 배열을 가운데에서 상한까지로 줄이는 식으로 진행한다. 
   구간이 반씩 줄어 log2 로 *O(log n)* 이 된다.
   
   ```cpp
   int l=0,r=n-1;
   while(l<=r){
       int i=(l+r)/2;
       if(array[i]==x){
           //찾았음 처리
       }
       if(array[i]<x) l=k+1;
       else r=k-1;
   }
   ```

2. 속도 제어 방법
   두번째 방법은 배열을 왼쪽에서 오른쪽까지 건너뛰며 탐색하며 속도를 2분의1로 줄여가며 탐색하는 방법이다.
   처음에는 배열 길이의 1/2 로 건너뛰며 탐색을 하여 오버슈팅하면 건너뛰지 않고 간격을 줄여 다시 탐색하는 방식이다.
   
   ```cpp
   int k = 0;
   for( int r = n/2; r>=1; r/=2 ){
       while ( k+r<n && array[k+r] <= x) k += r;
   }
   if(array[k]==x){
       //찾았음 처리
   }
   ```

#### 4.3.2 최적해 구하기

valid(x)라는 함수가 해가 k 일 때 x<k 면 false, x>=k면 true를 반환하는 함수 일 때, 이진 탐색으로 효율적으로 k를 구할수 있다.
false를 반환하는 최대의 x를 이진 탐색으로 구하는 원리이다.

```cpp
int x=-1;
//z는 최대값
for(int i = z; i>=1; i/=2){
    while(!valid(x+i)) x+=i;
}
int k = x+1;
```

## 5장 자료 구조

이장에서는 STL의 중요한 자료구조를 다룬다.

### 5.1 동적 배열

일반적으로 크기가 고정인 C++ 배열과 달리 크기를 동적으로 할당할 수 있는 자료 구조를 살펴본다.

#### 5.1.1 벡터

벡터는 배열의 끝에 원소를 추가하거나 삭제하는 과정을 효율적으로 할 수 있게 하는 동적 배열이다.

```cpp
vector<int> v;
v.push_back(1); //[1]
v.push_back(4); //[1,4]
int a = v.pop_back(); //[1],a=4
```

생성의 다른 방법도 있다.

```cpp
vector<int> v = {2,4,5,6};
vector<int> v1(7,4); //크기 7, 초깃값 4
```

출력하는 방법:
```cpp
for (int i=0;i<v.size();i++) cout << v[i] << "\n";
// or
for(auto x:v)cout<<x<<"\n";
```

#### 5.1.2 반복자와 범위

반복자(Iterator)는 자료구조의 원소를 가리키는 변수이다.
begin 반복자는 첫번째 요소를 가리키고 end 반복자는 마지막 원소 다음을 가리킨다. (끝)
정렬을 벡터에 가하는 등에 사용 될 수 있다.

```cpp
sort(v.begin(),v.end());
```

유용한 함수로 정렬된 범위에서 원소가 x 이상인 첫 번째 원소를 가리키는 반복자를 반환하는 lower_bound 함수와 x 보다 큰 첫 번째 원소의 반복자를 반환하는 upper_bound 함수가 있다.

```cpp
sort(v.begin(),v.end());
auto a = lower_bound(v.begin(),v.end(),5);
```

#### 5.1.3 다른 자료 구조

덱(Deque)은 양쪽 끝 원소를 처리할 수 있는 동적 배열이다. 벡터에 없는 push_front 와 pop_front 함수가 있다.

```cpp
deque<int> d;
d.push_back(3); // [3]
d.push_front(6); // [6,3]
int a = d.pop_front(); // [3] , a=6
```

스택(Stack)은 끝에서 원소를 넣고 빼는 push 와 pop 함수가 있다. top 함수는 마지막 원소를 반환한다.

큐(Queue)는 원소가 마지막에 추가되고 앞부터 삭제된다. (push, pop 함수) front 와 back 함수는 첫 원소와 마지막 원소를 반환한다.

### 5.2 집합 자료 구조

셋(Set, 집합)은 원소의 집합을 구현하는 자료구조이다. 기본적 연산은 추가, 탐색, 삭제이다.

#### 5.2.1 셋과 멀티셋

c++ STL에는 집합 자료구조가 두가지 있다.
-set은 이진 탐색 트리를 기반으로 만들어져 있다.
-unordered_set은 해시 테이블을 기반으로 만들어져 있다.

```cpp
set<int> s;
s.insert(3);
s.insert(3); //이미 있으면 추가되지 않음
s.insert(4);
cout<<s.count(3)<<"\n"; // 1
s.erase(3);
```
set은 일반적인 집합의 형태로 모든 원소가 서로 다르며 개수는 1보다 올라가지 않는다. (count는 항상 0 혹은 1을 반환.) 같은 원소의 중복을 허용하는 자료구조는 멀티셋이있다.
set은 대괄호 [] 로 원소에 접근 할 수 없다.
size로 크기를 확인 할 수 있다.
find(x)는 값이 x 인 원소를 가리키는 반복자를 반환한다. 원소가 없는경우에는 end()를 반환한다.

```cpp
auto it = s.find(x);
if(it==s.end()){
    //x가 s에 없다
}
```
가장 큰 c++ STL 집합의 차이는 set은 정렬되어있고 unordered_set은 그렇지 않다.

멀티셋은 같은값을 여러개 가질 수 있는 집합이다. multiset 과 unordered_multiset이 있다.

#### 5.2.2 맵

맵(Map)은 키와 쌍을 저장하는 집합이다. 

```cpp
map<string,int> m;
m["monkey"]=5; 
```

#### 5.2.3 우선순위큐

우선순위 큐(Priority Queue)는 원소의 추가, 탐색, 그리고 삭제가 되는 멀티셋이다.
우선순위 큐는 특별한 형태의 이진 트리인 힙(heap)을 기반으로 한다.
(push,pop,top)

## 6장 : 동적 계획법 Dynamic Programming

동적 계획법 (Dynamic Programming)은 문제의 최적해를 구하거나 만족하는 경우의 수를 구하는 과정 등에서 사용할 수 있는 알고리즘 기법이다.
이장에서는 동적 계획법의 기본 요소를 살피고 예시를 본다.

#### 6.1.1 탐욕법이 실패하는 경우

동전 교환 문제로 동적 계획법의 기본을 살펴본다.
여러 돈전의 값 coins[] = {c1,c2,...} 가 있고 만들어야하는 목표 액수 n 이 주어져, 최소한의 개수의 동전으로 합이 n 이 되도록 해야 한다.

ex) coins = [1,2,5] , n = 12 면 5 + 5 + 2 = 12 로 답은 3인 된다.

하지만 이때 탐욕 알고리즘으로 최대한 큰 동전을 우선적으로 쓴다고 항상 답이 나오지는 않는다.

ex) coins = [1,3,4] , n = 6 이면 탐욕알고리즘으로는 4 + 1 + 1 로 6이 되지만 실제 정답은 3 + 3 으로 2이다.

#### 6.1.2 최적해 구하기

동적 계획법을 사용하기 위해 재귀적인 구조나 반복문을 이용한 구현으로 점화식을 사용해야한다.
위의 예시의 문제로 보면 

```cpp
solve(x) = min(
    solve(x-1)+1,
    solve(x-3)+1,
    solve(x-4)+1)
```

이 점화식이 된다.

이를 재귀적으로 구현하면 Top Down 으로 풀 수 있다.
```cpp
int solve(int x, vector<int> coins){
    if(x<0)return INT_MAX;
    if(x=0)return 0;
    int minimum = INT_MAX;
    for(int i=0; i<coins.size();i++){
        minimum = min(minimum,solve(s-coins[i])+1);
    }
    return minimum;
}
```

이를 이미 게산한 값을 저장하는 메모화 (Memoization)을 이용하여 더 효율적으로 만들 수 있다.

```cpp
int solve(int x, vector<int> coins){
    if(x<0)return INT_MAX;
    if(x=0)return 0;
    if(memo[x]) return memo[x];

    // ...

}
```

재귀보다는 반복문을 이용한 해결이 더 효율적이라 선호된다.

```cpp
value[0]=0;
for(int x=1; x<=n; x++){
    value[x]=INT_MAX;
    for(int i=0; i<coins.size();i++){
        value[x] = min(value[x],value[x-coins[i]]+1);
    }
}

```

#### 6.1.3 해의 개수 세기

동전 문제를 변형하여 금액을 표현할 수 있는 동전의 수열의 개수를 구하는 문제로 만들수 있다.
이는 solve(x) = solve(x-1) + solve(x-3) + solve(x-4) 로 구현할 수 있다.
값이 너무 커져 modular 연산을 해야하는 경우 미리 modular을 적용하여도 된다.

```cpp
cout[x] += count[x-1] +  count[x-3] + count[x-4]; 
count[x] %= m;
```

### 6.2 다른 예제

#### 6.2.1 최장 증가 부분 수열 (Longest Increasing Subsequence)

{6,2,5,1,7,4,8,3} 이라는 수열이 있을때, 이 수열의 최장 증가 부분 수열은
{2,5,7,8} 이다.

이를 동적 계획법으로 *O(n^2)* 시간에 해결 할 수 있다.

length(k)를 인덱스 k에서 끝나는 최장 증가 부분 수열의 길이로 하면
length(k)는 array[i]<array[k] (증가 조건) 이면서 length(i)가 최대인 위치 i<k를 찾아야한다. (최장 조건)
이러면 length(k)=length(i)+1 이 되어야하는데, 이는 i에서 끝나는 최장 증가 부분 수열의 마지막에 array[k]를 추가하는게 최선이기 때문이다.
이를 만족시키는 i가 없다면 length(k)=1 로 k에서 시작하는 부분 배열을 새로 열어준다.

```cpp
for (int k=0; k<n; k++){
    length[k]=1;
    for (int i=0; i<k; i++){
        if (array[i]<array[k]){
            length[k]=max(length[k],length[i]+1);
        }
    })
}
```

이 문제를 *O(n^2)* 이 아닌 *O(n log n)* 으로 푸는 방법이 있다.
이는 이중 for 문이 아닌 처리하는 경우의 수를 나누어 탐색으로 푸는 방법이다.
다음과 같은 조건을 사용하여 코드를 구현하면 탐색시간인 *O(n log n)* 에 코드가 작동한다.

1. array[i]가 후보 리스트의 말단에 올 수 있는 원소 중 가장 작으면, array[i]로 시작하는 길이 1의 새로운 리스트를 생성한다.

2. array[i]가 후보 리스트의 말단에 올 수 있는 원소 중 가장 크면, 가장 큰 후보 리스트를 복사하고 array[i]를 추가해 연장한다.

3. array[i]가 그 사이라면, array[i]보다 말단 원소가 작은 리스트를 찾아 (탐색) 이를 복사하고 array[i]를 추가해 연장한다.
   같은 길이의 다른 후보 리스트는 모두 삭제한다.

## 7장 그래프 알고리즘

7장에서는 주어진 상황을 그래프로 표현하여 적절한 그래프 알고리즘을 이용하여 해결함을 살펴 볼 것이다.

### 7.1 그래프의 기본

#### 7.1.1 그래프 용어

그래프 (Graph) 는 노드 (Node) 혹은 정점 (Vertex, Verticies) 와 그를 잇는 간선 (edge) 로 구성된다.

경로 (Path)는 한 노드에서 그래프의 간선을 지나 다른 노드까지 가는 길을 뜻한다.

사이클은 (Cycle)은 처음 노드와 마지막 노드가 같은 경로를 의미한다.

그래프의 모든 노드간 경로가 있는 경우 연결 그래프 (Connected Graph)라 한다.

그래프의 연결된 한 부분을 컴포넌트 (Component)라고 한다.

트리 (Tree)는 사이클이 없는 연결 그래프이다. (필요충분조건)

방향 그래프 (Directed Graph)는 간선의 한 방향으로만 이동할 수 있는 그래프이다.

가중 그래프 (Weighted Graph)는 간선마다 가중치 (weight)가 있는 그래프이다.

두 노드를 잇는 간선이 있을 때 두 노드를 이웃 (neighbor), 혹은 인접한 노드 (adjacent node)라 한다.

노드의 이웃 노드의 개수를 노드의 차수 (degree)라 한다.

간선 개수가 m 일때 모든 노드의 차수의 합은 2m 이 된다. (직관적)

모든 노드의 차수가 동일한 경우 정규 그래프 (Regular Graph)라 하고

모든 노드의 차수가 n-1 로 다른 모든 노드와 연결된 경우 완전 그래프 (Complete Graph)라 한다.

노드로 들어오는 간선의 개수와 나가는 간선의 개수를 진입 차수 (indegree) 와 진출 차수 (outdegree)라 한다.

그래프의 노드를 두 색으로 칠할 때 이웃한 노드와 같은 색이 되지 않도록 하는 그래프를 이분 그래프 (Bipartite Graph)라 한다.

#### 7.1.2 그래프의 표현

그래프를 알고리즘에서 표현하는 방법을 알아본다. 그래프의 크기와 알고리즘에서 그래프를 처리하는 방법에 따라 사용될 자료구조가 결정된다.

#### 인접 리스트 (Adjacency List)

인접 리스트는 그래프의 각 노드 x에 대한 인접 리스트를 관리한다. (x에서 출발하여 그 노드에 도착하는 간선이 있는 노드의 리스트)
인접 리스트가 그래프를 나타내는 가장 대표적인 방법으로 대부분의 그래프 알고리즘에서 활용한다.
인접 리스트는 벡터의 배열 혹은 배열의 배열을 이용해 구현하여 저장할 수 있다.

```cpp
vector<int> adjacencyList[N];

adjacencyList[1].push_back(2); //1->2 edge 있다는 뜻
```

무방향 그래프는 각각의 간선이 양방향에 대해 저장되면 된다.

가중 그래프는 값을 추가하여 확장하면 된다.

```cpp
vector<pair<int,int>> adj[N];
```
이런 경우 인접 리스트에는 (b,w)로 값이 저장되어 b로 향하는 간선이 w의 가중치를 갖는다는 의미가 된다.

인접 리스트를 사용하면 주어진 노드에서 출발하여 갈 수 있는 노드를 효율적으로 처리할 수 있다.

```cpp
for(auto i : adj[s]){
    //노드 처리
}
```
#### 인접 행렬 (Adjacency Matrix)

그래프에 포함된 간선을 나타내는 행렬을 인접 행렬이라 한다.
인접 행렬은 두 노드 사이에 간선이 있는지를 효율적으로 확인 할 수 있도록 한다.
행렬의 인덱스 [m][n]에 1이 있으면 m->n 으로 향하는 간선이 있는것이다.

```cpp
int adj[N][N];
```

ex)
0 1 0 0
0 0 1 0
1 1 0 0
0 0 0 1

#### 간선 리스트 (Edge List)

간선 리스트는 그래프의 모든 간선을 특정한 순서에 따라 저장한 리스트이다.
특정한 노드에서 출발하는 간선보다 모든 간선을 살펴보는 형태를 필요로 할때 유용하다.

```cpp
vector<pair<int,int>> edges;

edges.push_back({1,2}); //1->2 edge 추가
```

가중 그래프는 다음과 같이 구현할 수 있다.

```cpp
vector<tuple<int,int,int>> edges;
```

### 7.2 그래프 순회

깊이 우선 탐색 (DFS : Depth First Search) 과 너비 우선 탐색 (BFS: Breadth First Search) 알고리즘을 알아본다.

#### 7.2.1 깊이 우선 탐색 (DFS)

<img src="https://he-s3.s3.amazonaws.com/media/uploads/9fa1119.jpg" width="200" height="150"/>

재귀적 알고리즘으로 간단히 구현 할 수 있다.

```cpp
#define N 100

vector<int> adj[N]; //adjacency list
bool visited[N];

void dfs(int i){
    if (visited[i]=true) return;
    visited[s] = true;
    // 해당 노드 i 처리
    for(auto u = adj[s]){ //간선타고 들어가서 노드 처리
        dfs(u);
    }
}
```

#### 7.2.2 너비 우선 탐색 (BFS)

너비 우선 탐색은 시작 노드에서 거리가 증가하는 순서대로 노드들을 방문한다. 

<img src="https://he-s3.s3.amazonaws.com/media/uploads/fdec3c2.jpg" width="200" height="120">

너비 우선 탐색은 그래프의 여러 부분에 걸쳐 진행되어 구현이 깊이 우선 보다 조금 어렵다.

다음 코드는 방문할 노드를 큐에 저장해 단계마다 큐에서 다음 노드를 가져와 처리하도록 한다.

```cpp
#define N 100

queue<int> q;
bool visited[N];
int distance[N];

//starts at node x
visited[x]=true;
distance[x]=0;
q.push(x);

while(!q.empty()){
    int s = q.front(); q.pop();
    // 노드 s 처리
    for(auto u: adj[s]){
        if (visited[u]) continue; //already transversed -> 넘어가기
        visited[u]=true;
        distance[u]=distance[s]+1;
        q.push(u);
    } //한번 돌면서 큐에 넣고 다시 처리
}

```

#### 7.2.3 응용

그래프 순회 알고리즘으로 그래프의 특성을 검사 할 수 있다.
아래의 예시에서는 그래프를 무방향 그래프로 간주한다.

#### 연결성 확인
그래프의 모든 노드 사이에 경로가 있는 경우 연결 그래프라 한다.
그래프의 연결성을 확인 하기 위해 임의의 노드에서 출발하여 다른 모든 노드를 방문 할 수 있는지를 확인하면 된다. (DFS)

#### 사이클 찾기
탐색과정에서 이미 방문했던 노드가 이웃노드에 포함되어 있다면 이 경우 그래프에 사이클이 있다고 판단할 수 있다. (DFS)

#### 이분성 확인 (Bipartite : Bipartality?, Bipartiality?)
두가지 색 X 와 Y를 정하고 시작노드를 X 로 칠하고 이웃을 Y로 칠하고 그의 이웃을 X 로 칠하는 과저을 반복 할 때 이웃한 두 노드가
같은 색인 경우를 찾으면 (DFS) 이 그래프는 이분 그래프가 아니라고 할 수 있다.
이 문제의 일반화인 k가지 색의 사용으로 (k>=3) 색을 칠 할 수 있는 지는 np - hard (Not NP-complete) 한 문제이다.

### 7.3 최단 경로 

가중 그래프에서 최단 경로를 구하기 위한 알고리즘을 구한다. (비가중 그래프는 각 간선의 가중치를 1로 하여 길이가 가장 짧은 경로를 찾으면 된다.)

#### 7.3.1 밸만-포드 알고리즘 (Bellman-Ford)

밸만-포드 알고리즘은 시작노드에서 다른 모든 노드로 가는 최단 경로를 구하는 알고리즘이다.
(단, 경로의 합계가 음수가 되는 사이클이 포함 되서는 안된다. *No negative cycles* )

이 알고리즘은 시작 노드에서 다른 노드까지의 길이를 모두 구한다.
거리의 초깃값은 시작 노드는 0, 다른 모든 노드는 std::numeric_limits<float>::infinity();, INT_MAX 등의 무한대를 상징하는 수로 두고 진행된다.
이 값을 계속 줄여나가는 과정을 반복한다. (Dynamic Programming Approach)

다음 코드는 간선 리스트에 저장된 그래프를 n-1번의 회수로 간선을 살펴 최단경로를 구한다. 

```cpp
for (int i=0; i<=n-1;i++){
    distance[i]=INT_MAX;
}
// 노드 x 에서 부터의 거리일때
distance[x]=0;
for(int i = 1; i<=n-1; i++){
    // edges == 간선리스트
    for(auto e : edges){
        int a, b, w;
        //tie 로 tuple 받아 변수에 값 저장
        std::tie(a,b,w)=e; //a -> b 간선 무게 w
        distance[b]=min(distance[b],distance[a]+w);
    }
}

```

음수 사이클이 있으면 안되는 이유는 자명하다. 그냥 음수 사이클로 계속 돌면서 가면 모든 노드로의 경로가 -INF 가 될것이기 때문.

#### 7.3.2 다익스트라 알고리즘

다익스트라 알고리즘도 특정한 노드에서 시작하여 그래프의 모든 노드로 가는 최단 경로를 구하는 알고리즘이다.
(가중치가 음수인 간선이 없을때만 사용 가능)
각 단계에서 아직 처리하지 않은 노드 중 거리가 가장 가까운 노드를 찾고, 그 노드에서 시작하는 모든 간선을 살피며 노드까지의 거리를 줄일 수 있다면 줄인다. 가중치가 음수인 간선이 없다는 사실을 활용하여 그래프의 모든 간선을 단 한번만 처리하여 매우 효율적이다.
각 노드를 처리한 뒤에는 해당 노드까지의 거리는 변하지 않는다. (그래서 가까운 순서대로)

다익스트라 알고리즘을 효율적으로 구현하기 위해서는 아직 처리하지 않은 노드 중 거리가 최소인 노드를 빠르게 찾을 수 있어야 한다.
이를 위해 우선 순위 큐를 사용하는 것이 좋다.

하지만 표준 라이브러리에는 우선순위 큐에 원소가 갖는 값을 수정하는 연산이 없어 보통 다른 구현 방식을 사용해야한다. 이는 거리가 바뀔 떄마다 그 노드를 우선순위 큐에 새로 추가하는 방법이다.

인접리스트에 저장된 그래프를 사용하고, 운선순위 큐를 사용한다.

```cpp
priority_queue<pair<int,int>> q;
```
이 우선순위 큐에 (-d,x) 형태로 값을 저장한다. 이는 시작점으로 지정한 노드에서 노드 x까지의 거리가 d 임을 의미한다.
-d로 저장하는 이유는 C++의 우선순위 큐는 최대원소를 찾게 되있어 음수를 취해줘서 작은 원소부터 찾도록 하는 것이다.

distance 배열은 각 노드까지의 거리를 담은 배열이며 processed 배열은 노드가 처리됬는지 여부를 담은 배열이다.
인접 리스트 adj 는 pair 의 배열(혹은 벡터)로 (b,w) 를 담는다. (a->b 간선 가중치 w)

```cpp
for(int i=1; i<=n; i++){
    distance[i]=INT_MAX;
}
distance[x]=0; // x 에서 시작
q.push({0,x});
while(!q.empty()){
    //음수로해서 top이 가장 가까운 새끼 내보냄
    int a =q.top().second' q.pop();
    if(processed[a]) continue; //불필요한 연산 줄이기
    processed[a]=true;
    for(auto u: adj[a]){ //인접리스트 adj
        int b = u.first, w = u.second;
        if (distance[a]+w < distance[b]){
            distance[b]=distance[a]+w; //더 짧은 경로 찾은것
            q.push({-distance[b],b)}); //dfs 같은 느낌
        }
    }
}
```

음수인 간선이 다익스트라 알고리즘에 있으면 안되는 이유도 자명하다.
가중치가 최소인 간선을 찾아 나가는 방식으로 동작하는데에 있어 오류를 발생시킨다.

#### 7.3.3 플로이드-워셜 알고리즘 (Floyd-Warshall)

## 11장 : 수학 *Mathematics*

이 장에서는 수학 관련 주제를 몇가지 살핀다. 이론적인 내용 조금과 이의 알고리즘적 대입을 다룬다.

### 11.1 정수론 *Number Theory*

#### 11.1.1 소수와 인수

소인수 분해 관련 내용

인수의 개수 = ∏ 소인수의 차수 + 1

인수의 합 = ∏ ( ∑ 소인수의 각 차수 ) 
인수의 합 공식은 등비수열의 합 공식으로 정리하여 더욱 간결히 할 수 있다.

정수 n이 소수가 아니라면 두 정수의 곱 a*b로 나타낼 수 있다. 이때 a<= n^(1/2) 혹은 b<= n^(1/2) 가 성립한다. 
이를 이용하여 정수가 소수인지 판별하는 과정과 소인수 분해를 구하는 과정을 모두 O( n^(1/2) ) 시간에 해결할 수 있다.

다음 코드는 정수의 소수 여부를 확인하는 함수이다.

```cpp
bool is_prime(int n){
    if (n<2) return false;
    for (int i=2; i*i<=n; a++){
        if (n%i==0)return false;
    }
    return true;
}
```

다음 코드는 소인수 분해를 하여 소인수를 벡터에 저장하는 함수이다.

```cpp
vector<int> factors(int n){
    vector<int> f;
    for (int i=2; i*i <= n; i++){
        while (n%i==0){ //소인수 i를 최대한 뺀다
            f.push_back(x);
            n /= i;
        }
    }
    if(n>1) f.push_back(n); //마지막 소인수
    return f;
}
```
각각의 소인수가 등장하는 회수가 소인수분해의 소인수의 차수에 해당한다.

소수는 무한히 많다. 이는 다양한 증명이 있는데 가장 유명한 것은 유클리드의 증명과 오일러의 증명이다.
(유클리드는 기원전 300년 사람이고 오일러는 18세기 사람으로 당연히 유클리드의 수학이 단순하여 이해하는데 부담이 없다)

유클리드의 증명은 귀류법 (Proof by Contradiction)에 의한것으로 {p1,p2,...pn}으로 된 유한한 소수의 집합이 있다면
p1*p2*...*pn + 1은 이 집합에 있는 어떠한 수로도 나누어지지 않으니 모순이 생겨 소수가 더 존재한다는 증명이다.

오일러의 증명은 소수의 역수의 합이 무한대가 됨을 보이는 증명이다. (내용 재밌으니 따로 찾아보길)

소수 계량 함수(Prime-counting Function)는 n 이하의 소수의 개수를 나타내는 함수이다.
이 함수는 ~ n / ln(n) 을 성립한다. (이 근사는 가우스의 근사)
이 식에 의하면 생각보다 소수는 빈번하게 발생된다.

#### 11.1.2 에라토스테네스의 체 (Sieve of Eratosthenes)

에라토스테네스의 체는 2이상 n 이하의 정수 x가 소수인지를 효율적으로 판별하기 위해 sieve (체, 그물망) 배열을 만드는 전처리 알고리즘이다.
x가 소수면 sieve[x]=0 이고 x가 소수가 아니면 sieve[x]=1이다.
이러한 배열을 만들기 위해 2부터 n까지의 정수를 하나씩 살피며 새로운 소수 x를 발견할 때마다 x의 배수(2x, 3x, 4x 등)를 소수가 아니라고 기록한다.
sieve의 초기값은 모두 0으로 둔다.

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif">

```cpp
int sieve[x]={0};
for (int i=2; i<=n; i++){
    if (sieve[i]) continue; //이미 소수 아니라고 등록해놨으면 넘어가기  
    for (int u=2*i; u<=n; u+=i){
        sieve[i]=1;
    }
}
```

이 알고리즘은 각각의 i 에 대해 n/i 번 실행된다. 즉, 알고리즘 수행시간의 상한은 조화수열(Harmonic series)의 합이 된다. 

∑(n/i) = (n/2) + (n/3) + ... = n log(n)

## 12장 : 고급 그래프 알고리즘




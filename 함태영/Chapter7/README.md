# Chapter7. 그래프 알고리즘

## 7.1 그래프 기본
그래프를 나타내기 위해서 사용하는 자료구조를 살펴보자.  

### 7.1.1 그래프 용어
그래프는 노드(node 혹은 vertex)와 그들 사이를 잇는 간선(edge)으로 구성되어있다.  

**경로(Path)** 는 임의의 노드 N1에서 N2로 갈 수있는 길을 의미한다.  
경로의 길이는 해당 경로에서 지나치는 간선들의 개수로 나타낼 수 있으며 만약 간선에 가중치가 존재한다면 가중치를 길이라 생각하면 된다.  

**연결 그래프(Connected Graph)** 는  그래프에 존재하는 노드들 중에 도달할 수 없는 노드가 없다면 연결 그래프라고 부른다.  
**컴포넌트(Component)** 는 비 연결 그래프에서 사이클을 구성하는 집합을 의미한다.  
**트리(Tree)** 는 그래프에서 사이클이 없는 연결 그래프를 의미한다.  
**방향 그래프(Directed Graph)** 는 노드간 이동 방향이 존재하는 그래프이다.
> 비 방향 그래프는 간선으로 연결만 되있다면 오고갈 수 있다.  
> 방향 그래프는 간선 하나가 어떤 노드로 갈 수 있는지 정의하고 있다.  

**가중 그래프(Weighted Graph)** 는 간선에 가중치가 존재하는 그래프이다.  
**이웃 또는 인접노드** 는 두 노드 사이를 이어주는 간선이 존재할 경우, 서로를 이웃 또는 인접노드라고 부른다.  
**차수** 는 임의의 노드 N의 인접노드의 갯수이다.  
**정규 그래프(Regular Graph)** 는 그래프에 존재하는 노드들의 차수가 모두 같은 경우 정규 그래프라고 부른다.  
**완전 그래프(Complete Graph)** 는 그래프에 존재하는 노드들이 해당 노드를 제외한 모든 노드들과 연결되어 있다면 완전 그래프라고 부른다.  
**정규 그래프(Regular Graph)** 는 그래프에 존재하는 노드들의 차수가 모두 같은 경우 정규 그래프라고 부른다.   
**이분 그래프(Bipartite Graph)** 는 이웃 노드 간 같은 색으로 칠할 수 없고 2가지 색상으로 모든 노드를 칠할 수 있다면 이분 그래프라고 부른다.  


### 7.1.2 그래프의 표현

#### **인접 리스트**
인접 리스트 표현법은 각 노드 x에 대해 간선으로 연결된 노드를 관리하는 방법이다.  
가장 대중적이며 효율성도 꽤 좋다.  

그래프를 그리기 힘들어서 말로 설명하자면  
노드가 5개가 존재한다고 가정하에 각 노드는 인접 노드를 관리하는 자료구조를 가지게된다.  
```cpp
vector<int> adj[5];
adj[0].push(1); // Node0은 1과 연결되어있다.
adj[0].push(4); // Node0은 4와 연결되어있다.
.
.
.
```

추가적으로 간선에 가중치를 줄 수 있는데 `{n,w}` 형태로 저장을 해주면된다.
```cpp
vector<pair<int,int>> adj[5];
adj[0].push({1,3}); // Node0은 1과 3의 가중치를 가지고 연결되어있다.
adj[0].push({4,1}); // Node0은 4와 1의 가중치를 가지고 연결되어있다.
.
.
.
```

#### **인접 행렬**
인접 행렬은 그래프에 포함된 간선을 나타내는 2차원 형태의 행렬이다.  
`int adj[N][N]`으로 선언하여 관리하는데, `int adj[x][y]`라면 x에서 y로 이동할 수 있는지를 나타낸다.
직관적으로 표현이 가능하지만 원소의 개수가 너무 많고, 노드의 개수가 많아지면 효율성이 떨어진다.  

#### **간선 리스트**
간선 리스트는 그래프에 존재하는 간선들을 특정한 순서에 따라서 저장한 리스트이다.  
형태는 `{n1, n2}`이며 n1에서 n2로 가는 간선이 있다는 것을 의미한다.  
```cpp
vector<pair<int, int>> edges; // 간선 리스트
edges.push_back({1,2}); // 1에서 2로 가는 간선이 존재한다.
edges.push_back({3,2}); // 3에서 2로 가는 간선이 존재한다.
edges.push_back({2,4}); // 2에서 4로 가는 간선이 존재한다.
vector<pair<int, int, int>> edges; // 가중치가 있는 간선 리스트
edges.push_back({1,2,5}); // 1에서 2로 가는 간선이 존재한다. 가중치는 5
edges.push_back({3,2,2}); // 3에서 2로 가는 간선이 존재한다. 가중치는 2
edges.push_back({2,4,9}); // 2에서 4로 가는 간선이 존재한다. 가중치는 9
```

## 7.2 그래프 순회
그래프 순회의 방법은 기본적인 2개의 알고리즘이 존재한다.  
### 7.2.1 깊이 우선 탐색(Depth-First Search, DFS)  
시작노드에서 연결된 간선을 따라서 이동하면서 갈 수 있는 여러 노드들 중 하나를 선택한다.
룰은 아래와 같다.
```
1. 시작 노드를 설정한다.
2. 시작 노드에서 갈 수 있는 노드를 확인한다.
3. 갈 수 있는 노드가 여러 개라면 하나만 택하여 2번 룰을 따른다.
4. 탐색한 노드가 이미 방문한 노드라면 갈 수 있는 다른 노드를 탐색한다.
```
위 룰을 기반으로 탐색을 진행하며 그래프 상에서 **깊게 들어가는 것** 을 확인 할 수 있다.  
그래서 Depth-First라고 한다.  
> 코드는,, 나중에

### 7.2.2 너비 우선 탐색(Breadth-First Search, BFS)  
시작노드에서 연결된 간선을 따라서 이동하면서 갈 수 있는 여러 노드들을 먼저 탐색한다.
룰은 아래와 같다.
```
1. 시작 노드를 설정한다.
2. 시작 노드에서 갈 수 있는 노드를 찾는다.
3. 갈 수 있는 노드가 여러 개라면 방문 Queue에 쌓아놓는다.
4. Queue에서 하나 씩 pop하여 방문하고 해당 노드에서 갈 수 있는 노드들은 3번 룰을 따른다.
```
위 룰을 기반으로 탐색을 진행하며 그래프 상에서 **넓게 탐색하는 것** 을 확인 할 수 있다.  
그래서 Breadth-First라고 한다.  
> 코드는,, 나중에

### 7.2.3 응용
#### 연결성 확인
주어진 그래프에서 탐색을 진행하였을 때, 모든 노드를 방문 할 수 없으면 연결 그래프라고 한다.  
만약 DFS를 이용해서 모든 노드를 확인할 수 없다면 이 그래프는 연결 그래프가 아님을 확인할 수 있다.  

#### 사이클 찾기
그래프 탐색과정에서 이미 방문한 노드(이웃노드)를 찾는다면 이는 사이클이 있음을 확인할 수 있다.  
혹은 간선의 개수로 체크하는 방법도 있다. 노드가 c개인데 간선의 개수가 c개 이상이라면 사이클이 있음을 확인할 수 있다.  

### 이분성 확인
단순히 탐색을 진행하면서 해당 노드에 색을 칠하면 간단하게 확인이 가능하다.  
만약 다음 탐색범위에 있는 노드가 이전 노드와 같은 색이라면 이분 그래프를 구성할 수 없는 것을 할 수 있을 것이다.  


## 7.3 최단 경로
가중치가 존재하는 그래프에서 최단 경로를 찾는 알고리즘을 살펴보자.  

### 7.3.1 벨만 포드 알고리즘
벨만 포드 알고리즘은 시작 노드에서부터 다른 모든 노드로 가는 최단 경로를 구하는 알고리즘이다.  
BF알고리즘은 시작 노드에서부터 다른 모든 노드까지의 경로를 전부 다 추적한다.  
시작점으로부터 다른 노드사이의 거리를 모두 INF로 초기화하는 것을 시작으로 해당 길이를 줄여나가는 과정을 반복한다.  

n-1번의 반복을 진행하는데 해당 반복 과정에서 모든 간선을 확인한다.  
이때 모든 간선을 확인하는 과정은 이미 저장된 distance를 줄일 수 있는지 판별하기 위해서이다.  
아래 코드를 참고하자.  
```cpp
int[] distance = new int[n];
for(int i=0; i<n; i++) distance[i] = INF;
distance[0] = 0;
for(int i=0; i<=n; i++){
    for(auto e : edges){
        int a,b,w;
        tie(a,b,w) = e;
        distance[b] = min(distance[b], distance[a]+w);
    }
}
```

위 알고리즘은 `O(nm)`의 시간복잡도를 가지며, 매 라운드마다 m개의 간선을 처리하면서  
n-1번의 라운드가 끝나면 모든 노드에 대한 거리를 비교할 수 있다.

위의 알고리즘을 더 빠르게 튜닝할 수 있는 방법이 존재한다.  
1. 한 라운드에서 바뀌는 distance가 없다면 종료해도 무관하다.  
2. SPFA를 사용하여 거리를 줄이기 위한 노드를 별도로 관리한다. 
근데 난 모르겠다. 그냥 저것도 간단해 보인다.  

또한 음수 사이클이 존재하는 그래프가 있을 경우에는 n-1번의 라운드 진행이 아니라 n번의 라운드 진행을 하면 찾아 낼 수 있다.  

### 7.3.2 다익스트라 알고리즘
다익스트라 알고리즘은 특정 노드에서 시작하여 그래프의 모든 노드로 가는 최단 경로를 구하는 알고리즘이다.  
> 말만 들어서는 BF 알고리즘이랑 똑같다.  

BF알고리즘보다 효율적이라 더 큰 그래프에서 활용이 가능하다는 장점이 있지만, 음수 간선이 있는 경우에는 사용할 수 없다.  

먼저 BF알고리즘처럼 시작점과 다른 노드 사이의 거리를 INF로 초기화 한다.  
각 단계마다 아직 처리하지 않은 노드 중 거리가 가장 작은 노드를 순서대로 방문을 한다.  
해당 노드에 방문하여 distance를 줄일 수 있으면 줄인다.  

위 방법으로는 모든 간선을 한 번만 처리하기 때문에 매우 효율적인 알고리즘이라 소개된다.  

다익스트라 알고리즘을 효율적으로 구현하고 싶다면 처리하지 않은 노드들 중 가장 작은 노드를 먼저 방문하게 끔 구현해야한다.  
우선순위 큐를 이용하면 쉽게쉽게 해결할 수 있다.  

노드에서 다른 노드로 가는 경로를 표현할 때는 인접 리스트의 형태로 저장되며  
`adj[a]={b,w}`형태로 저장되고, a노드에서 w의 가중치를 가지고 b로 간다는 의미를 가진다.  
또한 우선순위 큐에는 (-w, x)의 형태로 값을 저장한다.  
distance에는 각 노드까지의 거리이며 processed는 해당 노드를 방문했는지를 체크한다.  

```cpp
priority_queue<pair<int, int>> q;
for(int i=1;i<=n; i++) distance[i] = INF;
distance[x] = 0;
q.push({0,x});
while(!q.empty()){
    int a = q.top().second; q.pop();
    if(processed[a]) continue;
    processed[a] = true;
    for(auto u : adj[a]){
        int b = u.first, w = u.second;
        if(distance[a]+w < distance[b]){
            distance[b] = distance[a]+w;
            q.push({-distance[b], b});
        }
    }
}
```



### 7.3.3 플로이드 워셜 알고리즘
위의 알고리즘과 다른점은 한 번의 실행으로 모든 노드간 최단 경로를 구할 수 있다.
근데, 너무 비효율적이니 위의 알고리즘을 쓰자.  
> 무려 O(n^3)..

## 7.4 사이클 없는 방향 그래프
DAG(Directed Acyclie Graph), 사이클이 없는 방향그래프는 그래프 안에 사이클이 없다는 것이다.  

### 7.4.1 위상 정렬
Topological sort는 방향그래프 노드에 순서를 매김으로써 정렬 시키는 방법이다.  
만약 사이클이 존재하는 방향그래프라면 위상정렬이 불가능하다.  

DFS의 결과로 위상 정렬이 완성되므로 간단하게 만들 수 있다.  

### 7.4.2 동적 계획법
동적 계획법을 이용하면 DAG로 만든 경로와 관련된 문제를 해결할 수 있다.
1. 노드 a에서 b로가는 최단, 최장 경로  
2. 서로 다른 경로의 개수  
3. 경로 중 간선의 개수가 가장 적거나 가장 많거나 해당 개수  
4. 모든 경로에 포함된 노드  
 등등.  

 예를 들어서 a노드에서 b노드로 가는 방법을 생각해보자.  
 paths(x)가 노드 a에서 x로 가는 경로의 개수라고 정의가 되었다면  
 paths(x)는 paths(s1)+....paths(sk) 일 것이다.  
 이를 동적 계획법으로 코드를 작성한다면 손쉽게 해당 노드로 갈 수 있는 모든 경로를 찾을 수 있을 것이다.  
 >s1,,,sk는 x로 가는 노드를 의미한다.  

#### 최단 경로 처리하기
다익스트라 알고리즘이나 BF알고리즘으로 최단 거리를 구했다면 이를 이용하여 최단 경로 그래프를 만들 수 있다.  
이 그래프는 앞에서 나온 DAG이고 시작 노드에서 각 노드까지 최단 경로는 이 그래프의 간선으로 찾아갈 수 있다.  
최단 경로로 업데이트를 할 때마다 최단 경로를 저장하는 그래프에 간선을 저장하면 DAG가 된다.  

## 7.5 후속 노드 그래프
후속 노드 그래프(Successor Graph)는 모든 노드의 진출 차수가 1인, 즉 모든 노드에 대해서 후속 노드가 유일하게 존재하는 그래프이다.  
무조건 1개의 사이클을 가지고 있으며 그 사이클에 들어가는 간선으로 구성된다.  
후속 노드 그래프를 함수형 그래프라고 하기도 하는데 `succ(x)`형태의 함수로 모든 간선을 표현할 수 있기 때문이다.  


> 이는 1:1 매핑이 되기 때문이다. 

### 7.5.1 후속 노드 구하기
후속 노드를 구하기 위해서 `succ(x)` 함수를 k번 호출하면 무조건 도달하는 노드를 알 수 있다.  
이는 `O(k)`만큼의 시간복잡도를 가지는데 이것만 해도 많이 짧지만 적절한 전처리를 통해서 줄일 수 있다.  
`succ(x,k)`를 아래의 점화식을 통해 구하면 `O(log u)`만큼의 시간 내에 구할 수 있다.  
```
succ(x,k) = succ(x) (k=1)
succ(x,k) = succ(succ(x, k/2), k/2) (k>1)
```


### 7.5.2 사이클 찾기
사이클을 찾는 가장 간단한 방법은 기존에 방문한 이력을 저장하면 된다.  
`O(n)`의 시간복잡도와 n만큼의 메모리를 사용한다. 얼핏 보면 좋은 효율을 가지는 것 같지만 n이 크다면 메모리에 무리를 줄 수 있다.  
메모리를 1만큼만 쓸 수 있는 알고리즘이 존재하는데 **플로이드 알고리즘** 이라는 것이다.  

두 개의 포인터를 이용하여 노드를 저장하는데 하나의 포인터는 1step을 이동하지만 다른 하나의 포인터는 2step을 이동한다.  
```cpp
 a = succ(x);
 b = succ(succ(x));
 while(a!=b){  //포인터 a와 b가 만나는 지점까지 반복을 한다. 그래야 사이클이 존재함을 증명할 수 있다.
     a = succ(a);
     b = succ(succ(b));
 }

 a = x;
 while(a!=b){ // 포인터 a를 노드 x로 변경하고 사이클을 돌고있는 b포인터와 만나는 지점을 찾는다.
     a = succ(a);
     b = succ(b);
 }
 first = a;

 b = succ(a); // 첫 지점을 찾은 것을 토대로 b는 사이클의 시작점을 가진다.  
 length = 1;
 while(a!=b){ // 해당 사이클을 돌면서 첫 시작점까지 온다.
     b = succ(b);
     length++;
 }
```

## 7.6 최소 신장 트리
신장 트리(Spanning Tree)는 어떤 그래프의 부분 그래프로, 기존 그래프의 모든 노드를 포함하지만 간선은 일부만 포함하고 있다.  
모든 노드간의 경로가 존재하며 연결그래프 겸 사이클이 없는 그래프이다.  

최소신장 트리는 신장 트리 중 가중치가 가장 적은 것을 의미하며 **최소**를 만드는 신장 트리가 여러 개 존재할 수 있으므로 유일하지 않다.  
탐욕기반의 알고리즘으로 최소 신장 트리를 구할 수 있다.  

### 7.6.1 크루스칼 알고리즘
크루스칼 알고리즘은 탐욕법을 기반으로 간선을 추가하여 최소 신장 트리를 만드는 방법이다.  

처음에는 그래프의 노드만 포함하고 간선이 없는 상태로 시작하며, 간선을 가중치 순으로 정렬한 뒤 하나 씩 살펴보기 시작한다.  
만약 간선을 추가해도 사이클이 생기지 않으면 간선을 추가해주는 과정을 반복한다.  

#### 성립하는 이유
먼저 간선의 가중치가 가장 작은 순으로 정렬을 하는 이유는 **최소** 값을 가지는 신장 트리를 만들기 위함이며,
사이클만 존재하지 않게 만든다면 탐욕법으로 선택하는 것이 가장 좋은 선택이다.

#### 구현
먼저 정렬을 시작하기 때문에 `O(m log m)`의 시간복잡도가 필요하며 별다른 자료구조를 사용하지 않는다면 
앞에서 소개한 사이클을 확인하는 알고리즘 + 모든 간선에 대한 search가 필요하니 `O(mn)`의 시간이 걸릴 것이다.  
앞으로 배울 유니온 파인드 자료구조를 사용한다면 이 시간 복잡도는 `O(m log n)` 시간에 구현이 가능 할 것이다.  



### 7.6.2 유니온 파인드 알고리즘
작성중

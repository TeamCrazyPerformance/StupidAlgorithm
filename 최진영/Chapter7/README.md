# 그래프 알고리즘

## 그래프 기본

### 그래프 용어
- 그래프 : 노드와 그들을 잇는 간선으로 구성
- 경로 : 한 노드에서 그래프의 간선을 지나 다른 노드까지 가는 길을 의미
- 연결 그래프 : 모든 노드간에 경로가 있는 그래프
- 컴포넌트 : 그래프의 연결된 부분
- 방향 그래프 : 간선의 한 방향으로만 이동할 수 있는 그래프
- 가중 그래프 : 간선마다 가중치가 존재하는 그래프
- 이웃 : 간선으로 연결되어 인접한 노드
- 차수 : 이웃노드의 개수
- 정규 그래프 : 모든 노드의 차수가 같은 그래프
- 완전 그래프 : 모든 두 노드 간에 간선이 있는 그래프
- 진입 차수 : 그 노드로 향하는 간선의 개수
- 진출 차수 : 그 노드에서 시작하는 간선의 개수
- 이분 그래프 : 모든 노드를 두 가지 색깔 중 하나로 칠하되, 이웃 노드의 색깔이 같은 경우가 없는 그래프

### 그래프 표현
- 인접 리스트 : 그래프의 각 노드 x에 대한 인접 리스트, 즉 x에서 출발하는 간선이 있는 노드의 리스트
- 인접 행렬 : 그래프에 포함된 간선을 나타내는 행렬
- 간선 리스트 : 그래프의 모든 간선을 특정한 순서에 따라 저장한 리스트

## 그래프 순회

### 깊이우선탐색 DFS
- 시작 노드에서부터 출발하여 그래프의 간선을 따라 이동해가며 도달 가능한 모든 노드를 처리
![.](https://t1.daumcdn.net/cfile/tistory/23357644591DCAA123)


### 너비우선탐색 BFS
- 시작 노드에서 각 노드까지의 거리가 증가하는 순서대로 노드를 방문
![.](https://t1.daumcdn.net/cfile/tistory/23357644591DCAA123)

### 응용
- 연결성 확인 : 임의의 노드에서 출발하여 모든 노드를 방문한다면 연결 그래프라고 판단
- 사이클 찾기 : 이미 방문했던 노드가 이웃노드에 포함되어 있다면 사이클이 있다고 판단
- 이분성 확인 : 이웃한 두 노드가 같은 색깔이라면 이분 그래프라고 판단

## 최단경로
- 가중치가 없는 그래프의 경우, 경로의 길이가 간선의 개수와 같으므로 너비우선탐색을 이용하여 최단 경우를 구할 수 있음
- 가중치가 있는 그래프의 경우, 다음과 같은 알고리즘으로 구할 수 있음

### 벨만 포드 알고리즘
- 시작노드에서 다른 노드 까지의 길이를 모두 추적한다. 거리의 초깃값은 시작 노드의 경우 0 이고 다른 모든 노드의 경우 무한대의 값으로 설정된다. 그리고 이값을 계속해서 줄여나가는 과정을 더는 줄일 수 있는 값이 없을때까지 반복한다.
- 벨만 포드 알고리즘을 n번의 라운드로 진행했을 때, 마지막 라운드에서도 거리가 줄어드는 경우가 있다면 음수사이클이 존재함을 알 수 있다.

![.](http://mblogthumb3.phinf.naver.net/MjAxNzAxMDVfMTY4/MDAxNDgzNTQzMzA1MDIy.LAQdHxB9WtH_CBECnXc8ynylJmnwwLHakc0CfsRd0h4g.o4W4vUQvIsK6jw2di-IUHBCxFyRIK5Ptwvjme7LVmjEg.JPEG.yeop9657/bellman.jpg?type=w800)

### 다익스트라 알고리즘
- 효율적이지만 음수 가중치가 없는 경우에만 사용할 수 있다.
- 아직 처리하지 않은 노드 중 거리가 가장 작은 노드를 찾고, 그 노드에서 시작하는 모든 간선을 쭉 살펴보며 노드까지의 거리를 줄일 수 있다면 줄인다.

![.](https://mblogthumb-phinf.pstatic.net/20151001_11/babobigi_1443697819836o9NVh_JPEG/dijkstra.jpg?type=w2)

### 플로이드-워셜 알고리즘
- 알고리즘을 단 한번 실행하므로써 모든 노드 간 최단 경로를 구할 수 있다는 데 있다.
- 인접 행렬을 초기값으로 가진 후, 라운드마다 각 경로에서 새로운 중간 노드로 사용할 수 있는 노드를 선택하고 거리를 줄이는 과정을 반복한다.

![.](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F996CA43359E578C712C312)
![.](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile2.uf.tistory.com%2Fimage%2F99D28C3359E578ED1CAB50)
![.](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile27.uf.tistory.com%2Fimage%2F990A863359E5791214F6CF)
![.](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile30.uf.tistory.com%2Fimage%2F999F2A3359E57954021647)
![.](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F996FEF3359E5798A11640A)


## 사이클이 없는 방향 그래프 DAG
- DAG(Directed Acyclic Graph)는 사이클이 없는 방향 그래프를 뜻한다.
- 그래프가 DAG라면 항상 위상정렬 가능하며, 동적계획법을 적용하는 것도 가능하다.

### 위상정렬
- 위상정렬(Topolpgical sort)은 방향 그래프에 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점을 나열하는 정렬방법이다.
- 깊이 우선 탐색을 통해서, 그래프가 DAG인지 아닌지 여부를 파악할 수 있다.
- 하나의 방향 그래프에는 여러 위상 정렬이 가능하다.
- 위상 정렬의 과정에서 선택되는 정점의 순서를 위상 순서(Topological Order)라 한다.

![위상정렬 방법](https://gmlwjd9405.github.io/images/algorithm-topological-sort/topological-sort-example.png)
- 진입 차수가 0인 정점(큐에 존재하지 않는)인 모든 정점을 큐에 추가
- 큐에서 하나의 정점 선택
- 선택된 정점과 여기에 부속된 모든 간선을 삭제
- 선택된 정점을 큐에서 삭제
- 위상 순서에 추가
- 위의 과정을 반복해서 모든 정점이 선택, 삭제되면 알고리즘 종료
>dp..

### 후속 노드 그래프
- 후속 노드 그래프(successor grapgh)는 모든 노드의 진출 차수가 1인 그래프이다.
- 후속 노드 그래프는 하나 이상의 컴포넌트로 구성되어 있고, 각각은 사이클 하나와 그 사이클로 가는 경로로 구성되어 있다.
- 후속 노드 그래프는 함수형 그래프(functional gragh)라고도 부르는데, 함수 succ(x)의 형태로 후속 노드 그래프의 모든 간선을 표현할 수 있기 때문이다. (x는 그래프의 노드이고 함수의 결과는 후속노드가 된다.)

<strong>후속 노드 구하기</strong>
- 후속 노드 그래프에서는 모든 노드의 후속 노드가 유일하기 때문에, 노드x에서 시작하여 다음 노드로 이동하는 과정을 k번 반복했을 때 도착하는 노드를 함수 succ(x,k)로 정의할 수 있다.
- k = 1일때, succ(x,k) = succ(x)
- k > 1일때, succ(x,k) = succ(succ(x,k/2),k/2)
> 왜 위 알고리즘이 succ(x,k) 을 그냥 구하는 것보다 더 효율적일까?

<strong>사이클 찾기</strong>
- 그래프에서 이동하면서 방문했던 노드를 모두 기록하는 방법이 존재하지만, 이 방법은 O(n)시간이 걸리며, O(n)메모리를 사용하게 된다.
- 아래 알고리즘은 O(n)시간이 걸리지만 메모리를 O(1)만큼만 사용하는 플로이드 알고리즘<Floyd's algorithm>을 소개한다.
- 두 개의 포인터 a,b를 이용하여 이동한다. 단계마다 포인터 a는 한 번 이동하고 포인터 b는 두 번 이동한다. 이 과정을 두 포인터가 만날 때까지 반복한다.

```
a = succ(x);
b = succ(succ(x));

while(a!=b){
  a=succ(a);
  b=succ(succ(b));
}
```
>?

## 최소 신장 트리
- 신장 트리(Spanning tree)는 모든 노드를 포함하고, 노드간 서로 연결되면서 사이클이 존재하지 않는 그래프
- 신장 트리의 가중치는 간선 가중치의 합이다.
- 최소 신장 트리(Minimum spanning tree)는 신장 트리 중 가중치가 가장 작은 것을 의미한다.
- 최대 신장 트리(Maximun spanning tree)는 신장 트리 중 가중치가 가장 큰 것을 의미한다.
- 한 그래프에 대한 최소 신장 트리나 최대 신장 트리는 유일하지 않으며 여러 개 존재 할 수 있다.
- 아래는 최소 신장 트리를 구하는 두가지 알고리즘이다.

### 크루스칼 알고리즘
- 간선들을 오름차순으로 정렬한 뒤에 순서대로 사이클을 형성하지 않는 간선을 선택한다.
- 사이클을 형성하는지 확인하고 간선을 선택하기 위해 유니온-파인드 자료구조를 사용한다.
![.](https://gmlwjd9405.github.io/images/algorithm-mst/kruskal-example2.png)

<strong>유니온-파인드 자료구조</strong>
- 유니온-파인드 자료구조(Union-find Structure)는 집합의 묶음을 관리하는 구조이다.이때, 집합은 한 원소가 둘이상의 집합에 속할 수 없다.
- 연산은 두가지 존재한다. 연결을 하는 작업인 unite연산과 연결을 확인하는 작업인 find연산이 존재한다.
- 만약, 선택한 간선의 두 정점이 find 연산을 통해 이미 연결되어 있음을 알았다면 이 간선은 사이클을 형성하는 간선이므로 선택하지 않는다.
- 사이클을 형성하지 않는 간선이라면 unite연산을 통해 두 정점을 연결한다.

### 프림 알고리즘
- 그래프에서 하나의 꼭짓점을 선택하여 트리를 만든 후, 트리와 연결된 변 가운데 선택하지 않는 노드와 연결되면서 가장 가중치가 작은 변을 트리에 추가한다.
![.](https://gmlwjd9405.github.io/images/algorithm-mst/prim-example.png)

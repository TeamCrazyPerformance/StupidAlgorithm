# 고급 그래프 알고리즘

## 그래프의 강결합성

- 강결합 그래프 : 방향그래프의 모든 노드에서 다른 모든 노드로 가는 경로가 있는 그래프
- 강결합 컴포넌트 : 모든 노트에서 다른 모드로 가는 경로가 있는 최대 노드의 집합
- 위상정렬 후 동적계획법을 이용하여 처리하는 일반적인 방법이 있다.

### 코사라주 알고리즘
- 강결합 컴포넌트를 찾는 유용한 방법으로, O(n+m)이 걸린다.
- 두번의 DFS과정을 거치는데, 방법은 다음과 같다.
  - 첫번째 깊이 우선 탐색에서 깊이 우선 탐색에 따라 방문한 노드 리스트를 만든다.
  - 두번째 깊이 우선 탐색에서 간선의 방향을 모두 바꾸고 위에서 생성한 방문노드 리스트 순으로 새로운 컴포넌트를 만든다.
  - 이때, 모든 간선의 방향이 반대이기 때문에 컴포넌트에 속한 노드가 컴포넌트 바깥의 노드로 흐르지 않는다.

### 2SAT문제
- `(a1||b1)&&(a2||b2)&&(a3||b3)...` 에서 각 변수에 값을 할당하여 식을 참으로 만드는 조합을 찾거나, 그러한 조합이 없음을 찾는 것이 목표이다.
- 이에 대해서 2SAT문제는 함의 그래프를 만들 수 있다.
  - `(ai||bi)`에 대해 `!ai->bi`와 `!bi->ai`의 두 간선을 만든다.
  - xi와 !xi노드가 같은 강결합 컴포넌트에 속하는 일이 없는 경우에 조합이 존재할 수 있다.
  - 그 이유는, xi에서 xj로 가는 경로와 xj에서 !xj로 가는 경로가 모두 있다면 !xj에서 !xi로 가는 경로도 있어서 xi와 xj가 모두 거짓이 되기 때문이다.

# 완전 경로

## 오일러 경로
- 오일러 경로란 그래프의 각 간선을 정확히 한 번씩 지나는 경로이다.
- 오일러 경로가 존재하는지는 노드의 차수에 따라 결정된다.
  - 모든 노드의 차수가 짝수이거나
  - 정확히 두 노드의 차수가 홀수이고, 다른 모든 노드의 차수는 짝수이다.
- 방향그래프에선 다음과 같은 룰에 따라 결정된다.
  - 모든 노드의 진입 차수와 진출 차수가 같거나
  - 한 노드의 진입 차수가 진출 차수보다 1 크고, 다른 한 노드의 진출 차수가 진입 차수보다 1 크며, 나머지 노드는 진입 차수와 진출 차수가 같다.

### 히어홀처 알고리즘
- 히어홀처 알고리즘은 오일러 회로를 찾는 효율적인 방법이다.
- 히어홀처 알고리즘은 다음과 같은 방법으로 구현한다.
  - 노드 하나가 있고 간선이 없는 빈 회로에서 알고리즘을 시작하고 부분 회로를 추가하는 식으로 회로를 한단계씩 확장해 나간다.
  - 회로를 확장하기 위해서 회로에 속한 노드 중 회로에 속하지 않은 진출 간선이 있는 노드 x를 찾는다.
  - 이 노드 x에서 시작하여 아직 회로에 포함되지 않는 간선으로 이루어진 부분회로를 추가한다.

## 해밀턴 경로
- 해밀턴 경로란 그래프의 모든 노드를 정확히 한 번씩만 방문하는 경로이다.
- 이 경로의 시작 노드와 마지막 노드가 같은 경우 해밀턴 회로라고 부른다.
- 해밀턴 경로와 관련된 문제는 NP하드이다. 해밀턴 경로가 있는지를 효율적으로 찾는 알고리즘은 알려지지 않았다.
- 풀 수 있는 유일한 방법은 퇴각검색 알고리즘을 이용하여 모든 가능한 경우를 탐색하는 것이다.

### 드 브루인 수열
 - 드 브루드인 수열은 문자열의 일종으로, 글자의 종류가 k일 때 만들 수 있는 길이가 n인 모든 문자열을 정확히 한 번씩 포함하는 것을 말한다.
 - 드 브루드인 수열은 항상 대응된다.
  - 길이가 n-1인 모든 문자열이 노드에 대응되고, 수열에 한 글자를 덧붙이는 것이 간선에 대응되는 그래프를 생각하자.
  - 드 부르인 경로를 찾기 위해서는 임의의 노드에서 시작하여 모든 간선을 정확히 한번 방문하는 오일러 경로를 찾으면 된다.
  - 시작노드와 간선의 글자를 차례로 더하면 K^n+n-1에의 글자로 이루어진 드 브루인 수열을 얻을 수 있다.
> 왜 개수가 K^n+n-1 일까?

### 나이트 투어
- 나이트 투어는 nXn 체스판에서 나이트를 체스의 규칙에 맞게 움직이면서 모든 칸을 정확히 한 번 방문하는 경로이다.
- 나이트 투어를 찾는 가장 자연스러운 방법은 퇴각검색이다.

#### 바른스도르프 규칙
- 나이트 투어를 찾기 위한 간단하고 효율적인 방법이다.
- 아이디어는 나이트를 움직일 수 있는 여러 칸이 있을 때, 다음으로 움직일 수 있는 경우가 가장 적은 곳으로 움직이는 것이다.

# 최대 유량
- 소스는 들어오는 간선이 없는 노드이고 싱크는 나가는 간선이 없는 노드이다.
- 최대 유량이란 소스에서 시작되어 싱크로 흘러갈때 각 간선의 가중치를 넘기지 않는 유량의 최대값을 의미한다.
- 최소 컷 문제는 간선 중 일부를 제거하여 소스에서 싱크로 가는 경로를 없애되, 그러면서 제거한 간선의 가중치 합을 최소로 만드는 문제이다.

## 포드-풀커슨 알고리즘
- 그래프의 최대 유량을 찾는 알고리즘이다.
- 포드-풀거슨 알고리즘은 다음과 같이 구현된다.
  - 유량이 0에서 알고리즘을 시작한다.
  - 모든 간선의 가중치가 양수인 경로를 찾는다.(여러개라면 아무거나 상관없다)
  - 선택한 경로에 포함된 간선의 가중치의 최솟값만큼 유량을 증가시키고 이에 대해서 간선의 가중치를 업데이트 한다.
  - 더는 유량을 늘릴 수 없을 떄의 값이 최대유량이 된다.
- 아이디어는 간선의 유량이 증가하면 앞으로 추가할 수 있는 유량은 감소한다는 데 바탕을 두고 있다.

### 경로찾기
- 포드-풀거슨 알고리즘에서 경로를 좀 더 효율적으로 고르기 위한 알고리즘이 존재한다.
- 에드몬드-카프 알고리즘에서는 경로를 이루는 간선의 개수가 최소가 되도록 경로를 선택한다.
- 용량 조절 알고리즘은 깊이 우선 탐색을 통해 경로를 찾는데, 이때 각 간선의 가중치가 지정된 값 이상이어야한다.

### 최소컷
- 최대 유량을 찾았다면 최소 컷 또한 구할 수 있다.
- A를 소스에서 가중치가 양수인 간선을 이용하여 갈 수 있는 노드의 집합이라고 할 때, 최소 컷은 원래 그래프에서 A에 속한 노드에서 A에 속하지 않는 노드로 가는 간선으로 구성된다.

## 서로소 경로
- 소스에서 싱크로 가는 서로소 경로의 최대 갯수를 찾으려 한다.

### 간선 서로소 경로
- 소스에서 싱크로 가는 경로 중, 각 각선이 최대 하나의 경로에만 포함될 수 있다.
- 간선 서로소 경로의 최대 개수는 각 간선의 용량이 1인 그래프의 최대 유량과 같다.

### 노드 서로소 경로
- 소스에서 싱크로 가는 경로 중, 소스와 싱크를 제외한 모든 노드는 최대 하나의 경로에만 포함될 수 있다.
- 노드를 지나는 유량을 제한하며 최대 유량 문제로 변환할 수 있다.
  - 각각의 노드를 두 개의 노드로 나누는데, 첫 번째 노드에는 들어오는 간선을 연결하고
  - 두 번째 노드에는 나가는 간선을 연결하며, 첫 번째 노드에서 두 번째 노드로 가는 간선을 추가한다.

## 최대 매칭
- 두 노드의 조합으로 이루어진 최대 크기의 집합으로, 하나의 조합을 이루는 두 노드는 간선으로 연결되어 있어야 하며 각 노드는 최대 하나의 조합에만 속할 수 있다.
- 이분 그래프에서 최대 매칭 문제를 최대 유량 문제로 변환할 수 있다.
  - 그래프에 소스와 싱크 노드를 추가한다.
  - 그 다음 소스에서 왼쪽 그룹의 각 노드로 가는 간선을 추가하고, 오른쪽 그룹의 각 노드에서 싱크로 가는 간선도 추가한다.
  - 최대 유량의 크기가 원래 그래프의 최대 매칭의 크기와 같아진다.

### 홀 정리
- 이분 그래프에서 모든 왼쪽 노드 혹은 모든 오른쪽 노드를 포함하는 매칭이 있는지를 확인하는데 사용할 수 있는 정리이다.
- 모든 왼쪽 노드를 포함하는 매칭을 찾는 방법은 다음과 같다.
  - x를 왼쪽 노드로 구성된 임의의 집합으로 정의하고 f(x)를 그 노드의 이웃 노드 집합으로 정의하자
  - 이때, 모든 x에 대해 |x|<=|f(x)|이 성립한다면 모든 왼쪽 노드를 포함하는 매칭이 존재한다.
- x의 원소의 개수가 f(x)보다 많으면 성립하지 않는다.
- 왼쪽 노드와 오른쪽 노드의 개수가 같다면 완전 매칭이 있는지 확인하는데에 사용할 수 있다.

### 콰니그 정리
- 최소 노드 커버는 모든 간선의 두 노드 중 적어도 하나를 포함하고 있는 노드의 집합 중 그 크기가 최소인 것을 말한다.
- 이분 그래프에서는 콰니그 정리에 의해 최소 노드 커버의 크기와 최대 매칭의 크기가 항상 같음을 알 수 있다.
- 따라서 최소 유량 알고리증을 통해 최소 노드 커버의 크기를 구할 수 있다.

## 경로 커버
- 경로 집합의 일종으로, 모든 노드가 최소 하나의 경로에 속하는 경우를 말한다.

### 노드 서로소 경로 커버
- 모든 노드가 정확히 하나의 경로에만 속하는 경우를 말한다.
- 최대 유량 문제로 변환가능하며 방법은 다음과 같다.
  - 매칭 그래프를 만든다.
  - 매칭 그래프에 소스와 싱크를 추가하고, 소스에서 모든 왼쪽 노드로 가는 간선과 모든 오른쪽 노드에서 싱크로 가는 간선을 추가한다.
  - 매칭 그래프의 최대 매칭에 대응되는 간선은 원래 그래프의 최소 노드 서로소 경로 커버를 구성하는 간선이 된다.

### 일반 경로 커버
- 노드가 하나 이상의 경로에 속할 수 있는 경로 커버이다.
- 최소 일반 경로 커버는 최소 노드 서로소 경로 커버와 비슷한 방법으로 구할 수 있다.
  - 원래 그래프에 노드 a에서 노드 b로 가는 경로가 있는 경우 매칭 그래프에 a->b 간선을 추가한다.

### 딜워스 정리
- 반사슬은 그래프의 노드 집합의 일종으로 그래프의 간선을 이용하여 집합에 속한 노드 간에 경로를 만들 수 없는 경우를 말한다.
- 방향 그래프에서 최소 일반 경로 커버의 크기는 최대 반사슬의 크기와 같다.

## 깊이 우선 탐색 트리
- 연결그래프에 대한 깊이 우선 탐색을 진행하는 과정에서 루트가 있는 방향성 신장 트리를 깊이 우선 탐색트리라고 한다.
- 트리 간선은 깊이 우선 탐색 트리에 포함된 간선을 의미하고 역방향 간선은 이미 방문한 노드로 향하는 간선을 의미한다.

### 이중 연결성
- 연결 그래프의 임의의 노드 하나를 삭제하더라도 연결 그래프라는 성질을 유지할 때 이를 이중 연결 그래프라고 한다.
- 어떤 노드를 삭제함으로써 그래프가 연결 그래프가 아니게 되는 경우 이 노드를 단절점이라고 한다.
- 간선을 제거함으로써 그래프가 연결 그래프가 아니게 된다면 이 간선을 단절선이라고 한다.
- 깊이 우선 탐색을 이용하면 그래프의 모든 단절점과 단절선을 효율적으로 찾을 수 있다.
  - 깊으 우선 탐색 트리를 만든다.
  - 간선 a->b가 단절선인 경우 이 간선이 트리 간선이며 b의 서브트리에서 a나 a의 조상으로 가는 역방향 간선이 없는 경우와 같다.
  - 노드 x가 트리 루트라면, 이 노드가 단절점인 경우는 자식이 둘 이상인 경우와 같다.
  - 노드 x가 트리 루트가 아니라면, 이 노드가 단절점인 경우는 자식 노드 중 둘 이상인 경우와 같다.
  - 단절선과 단절점이 없는 경우 이중연결 그래프이다.

### 오일러 서브그래프
- 오일러 서브그래프는 그래프의 모든 노드를 포함하고 있으며, 간선 중 일부를 포함하면서 모든 노드의 차수가 짝수가 되는 서브그래프를 의미한다.
- 연결 그래프에 대하 오일러 서브그래프의 개수는 역방향 간선 개수 k에 대해 2^k 이고 이때, `k = 간선의 개수-(노드의 개수-1)이다.`

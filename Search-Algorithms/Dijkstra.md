# dijkstra algorithm

> 마인드
> - 우선순위 큐를 사용
> - 지금 가장 가까운 노드에서 가장 적은 비용을 가진 노드를 선택한다.
> - 시작에서 가장 가까운 k개의 노드를 알고 있으면,k+1번째 가까운 노드는 k개 중에서 하나와 바로 연결되는 길이 Shortest Path이다.

![](https://mblogthumb-phinf.pstatic.net/20151001_11/babobigi_1443697819836o9NVh_JPEG/dijkstra.jpg?type=w2)

### pseudo code
    
```pseudo code
function Dijkstra(Graph, source):
    dist[source] ← 0                           // Distance from source to source
    for each vertex v in Graph:                // Initializations
        if v ≠ source
            dist[v] ← INFINITY                 // Unknown distance function from source to v
            previous[v] ← UNDEFINED            // Previous node in optimal path from source
        add v to Q                            // All nodes initially in Q (unvisited nodes)
    while Q is not empty:                      // The main loop
        u ← vertex in Q with min dist[u]      // Source node will be selected first
        remove u from Q 
        for each neighbor v of u:             // where v is still in Q.
            alt ← dist[u] + length(u, v)
            if alt < dist[v]:                 // A shorter path to v has been found
                dist[v] ← alt 
                previous[v] ← u 
    return dist[], previous[]
```

            다익스트라 알고리즘은  그리디 알고리즘의 일종이며 많은 설명보다는 코드를 보는게 이해가 빠르다.

---
## Appendix
### 시간복잡도
- O(ElogV)

### 공간복잡도
- O(V)

### 안정성
- 안정적

### 특징
- 음의 가중치를 가진 간선이 없어야 한다.

### 응용
- 최단 경로 문제

### 참고
- [https://ko.wikipedia.org/wiki/다익스트라_알고리즘](https://ko.wikipedia.org/wiki/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)


###  python

```python
import heapq  # 우선순위 큐를 위한 라이브러리
import sys   # 최대값을 위한 라이브러리

input = sys.stdin.readline   # 입력속도를 빠르게 하기 위한 라이브러리
INF = int(1e9)  # 무한을 의미하는 값으로 10억을 설정

n, m = map(int, input().split()) # 노드의 개수, 간선의 개수를 입력받기
start = int(input()) # 시작 노드 번호를 입력받기
graph = [[] for i in range(n + 1)] # n(노드의 개수) :각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트를 만들기
distance = [INF] * (n + 1) # 최단 거리 테이블을 모두 무한으로 초기화

for _ in range(m):  # 모든 간선 정보를 입력받기
    a, b, c = map(int, input().split()) # a번 노드에서 b번 노드로 가는 비용이 c라는 의미
    graph[a].append((b, c)) # a번 노드에 연결되어 있는 노드에 대한 정보를 튜플 형태로 저장

def dijkstra(start): # 다익스트라 알고리즘
    q = [] # 우선순위 큐를 위한 리스트
    heapq.heappush(q, (0, start)) # 시작 노드로 가기 위한 최단 경로는 0으로 설정하여 큐에 삽입
    distance[start] = 0 # 시작 노드에 대해서 초기화
    while q: # 큐가 비어있지 않다면 
        dist, now = heapq.heappop(q) # 가장 최단 거리가 짧은 노드에 대한 정보 꺼내기
        if distance[now] < dist: # 현재 노드가 이미 처리된 적이 있는 노드라면 무시
            continue 
        for i in graph[now]: # 현재 노드와 연결된 다른 인접한 노드들을 확인
            cost = dist + i[1] # 현재 노드를 거쳐서 다른 노드로 이동하는 거리를 계산
            if cost < distance[i[0]]: # 현재 노드를 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우
                distance[i[0]] = cost # 최단 거리 테이블 갱신
                heapq.heappush(q, (cost, i[0])) # 우선순위 큐에 삽입

dijkstra(start)

for i in range(1, n + 1): 
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])
```

### java

```java
import java.util.*;
import java.io.*;

public class Main {
    public static int n, m, start; // 노드의 개수, 간선의 개수, 시작 노드
    public static ArrayList<ArrayList<Node>> graph = new ArrayList<ArrayList<Node>>(); // 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트
    public static int[] d = new int[100001]; // 최단 거리 테이블을 모두 무한으로 초기화
    public static final int INF = (int) 1e9; // 무한을 의미하는 값으로 10억을 설정

    public static class Node implements Comparable<Node> {
        private int index;
        private int distance;

        public Node(int index, int distance) {
            this.index = index;
            this.distance = distance;
        }

        @Override
        public int compareTo(Node other) {
            if (this.distance < other.distance) {
                return -1;
            }
            return 1;
        }
    }

    public static void dijkstra(int start) {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));
        d[start] = 0;
        while (!pq.isEmpty()) {
            Node node = pq.poll();
            int dist = node.distance;
            int now = node.index;
            if (d[now] < dist) {
                continue;
            }
            for (int i = 0; i < graph.get(now).size(); i++) {
                int cost = d[now] + graph.get(now).get(i).distance;
                if (cost < d[graph.get(now).get(i).index]) {
                    d[graph.get(now).get(i).index] = cost;
                    pq.offer(new Node(graph.get(now).get(i).index, cost));
                }
            }
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        start = Integer.parseInt(br.readLine());
        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<Node>());
        }
        Arrays.fill(d, INF);
        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());
            graph.get(a).add(new Node(b, c));
        }
        dijkstra(start);
        for (int i = 1; i <= n; i++) {
            if (d[i] == INF) {
                System.out.println("INFINITY");
            } else {
                System.out.println(d[i]);
            }
        }
    }
}
```

### c++

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <climits>
using namespace std;

int n, m, start;
vector<pair<int, int>> graph[100001]; // 각 노드에 연결되어 있는 노드에 대한 정보를 담는 배열
int d[100001];

void dijkstra(int start) {
    priority_queue<pair<int, int>> pq;
    pq.push(make_pair(0, start));
    d[start] = 0;
    while (!pq.empty()) {
        int dist = -pq.top().first;
        int now = pq.top().second;
        pq.pop();
        if (d[now] < dist) {
            continue;
        }
        for (int i = 0; i < graph[now].size(); i++) {
            int cost = dist + graph[now][i].second;
            if (cost < d[graph[now][i].first]) {
                d[graph[now][i].first] = cost;
                pq.push(make_pair(-cost, graph[now][i].first));
            }
        }
    }
}

int main() {
    cin >> n >> m >> start;
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        graph[a].push_back(make_pair(b, c));
    }
    fill(d, d + 100001, INT_MAX);
    dijkstra(start);
    for (int i = 1; i <= n; i++) {
        if (d[i] == INT_MAX) {
            cout << "INFINITY" << '\n';
        } else {
            cout << d[i] << '\n';
        }
    }
    return 0;
}
```






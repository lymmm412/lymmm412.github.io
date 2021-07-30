# Graph Template

## Build Adjacant List

Undirected and weighted input [u,v,w]:

```java
Map<Integer,List<int[]>> map=new HashMap<>();
for(int[] edge:edges){
    if(!map.containsKey(edge[0])){
        map.put(edge[0], new ArrayList<>());
    }
    map.get(edge[0]).add(new int[]{edge[1],edge[2]});
    if(!map.containsKey(edge[1])){
        map.put(edge[1], new ArrayList<>());
    }
    map.get(edge[1]).add(new int[]{edge[0],edge[2]});
}
```

## Dijkstra's Algorithm
n nodes noted from 1 to n. Undirected, weighted. Find the shortest distance from every node to the last node n.

```java
int[] dist=new int[n+1];
Arrays.fill(dist, Integer.MAX_VALUE);
dist[n]=0;
PriorityQueue<int[]> pq=new PriorityQueue<>((a,b) -> (a[1]-b[1]));
pq.add(new int[]{n,0});

while(!pq.isEmpty()){
    int[] top=pq.poll();
    int curr=top[0], weight=top[1];
    for(int[] next : graph.get(curr)){
        int nei=next[0], w=next[1]+weight;
        if(w<dist[nei]){
            dist[nei]=w;
            pq.add(new int[]{nei,w});
        }

    }
}
```
- **相关题目**
1135. Connecting Cities With Minimum Cost

## Floyd Warshall's shortest path
[geeks for geeks](https://www.geeksforgeeks.org/floyd-warshall-algorithm-dp-16/) 

The Floyd Warshall Algorithm is for solving the All Pairs Shortest Path problem. The problem is to find shortest distances between every pair of vertices in a given edge weighted directed Graph.

We initialize the solution matrix same as the input graph matrix as a first step. Then we update the solution matrix by considering all vertices as an intermediate vertex. The idea is to one by one pick all vertices and updates all shortest paths which include the picked vertex as an intermediate vertex in the shortest path. When we pick vertex number k as an intermediate vertex, we already have considered vertices {0, 1, 2, .. k-1} as intermediate vertices. For every pair (i, j) of the source and destination vertices respectively, there are two possible cases. 

(1) k is not an intermediate vertex in shortest path from i to j. We keep the value of dist[i][j] as it is. 

(2) k is an intermediate vertex in shortest path from i to j. We update the value of dist[i][j] as dist[i][k] + dist[k][j] if dist[i][j] > dist[i][k] + dist[k][j]

```java
public int FloydWarshall(int n, int[][] edges) {
    int INF = (int) 1e6; // INF = n * maxWeight = 100 * 10^4 = 10^6
    int[][] dist = new int[n][n]; // dist[i][j] is the minimum distance from i to j
    for (int i = 0; i < n; i++) {
        Arrays.fill(dist[i], INF);
        dist[i][i] = 0;
    }
    for (int[] edge : edges) {
        int v1 = edge[0], v2 = edge[1], w = edge[2];
        dist[v1][v2] = dist[v2][v1] = w;
    }
    // Floyd Warshall's shortest path algorithm
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
 }
 ```
- **相关题目**
1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance

## check if there's a cycle in the undirected graph

```java
 boolean hasCycle(List<List<Integer>> adjList, int u, boolean[] visited, int parent) {
    visited[u] = true;

    for (int i = 0; i < adjList.get(u).size(); i++) {
        int v = adjList.get(u).get(i);

        if ((visited[v] && parent != v) || (!visited[v] && hasCycle(adjList, v, visited, u)))
            return true;
    }

    return false;
}
```
- **相关题目**
261. Graph Valid Tree


## Graph Coloring
- graph分组

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int len=graph.length;
        int[] colors=new int[len];
        for(int i=0;i<len;i++){
            if(colors[i]!=0){
                continue;
            }
            Queue<Integer> q=new LinkedList<>();
            q.add(i);
            colors[i]=1;
            while(!q.isEmpty()){
                int curr=q.poll();
                for(int next:graph[curr]){
                    if(colors[next]==0){
                        colors[next]=-colors[curr];
                        q.add(next);
                    }else if(colors[next]!=-colors[curr]){
                        return false;
                    }
                }
            }
            
        }
        return true;
    }
}
```
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] colors = new int[n];			
				
        for (int i = 0; i < n; i++) {              //This graph might be a disconnected graph. So check each unvisited node.
            if (colors[i] == 0 && !validColor(graph, colors, 1, i)) {
                return false;
            }
        }
        return true;
    }
    
    public boolean validColor(int[][] graph, int[] colors, int color, int node) {
        if (colors[node] != 0) {
            return colors[node] == color;
        }       
        colors[node] = color;       
        for (int next : graph[node]) {
            if (!validColor(graph, colors, -color, next)) {
                return false;
            }
        }
        return true;
    }
}
```
- **相关题目**
    - 785. Is Graph Bipartite?

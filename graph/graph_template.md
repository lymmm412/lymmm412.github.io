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

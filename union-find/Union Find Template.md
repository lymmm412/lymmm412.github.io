# My LC Template

## Union Find

参考：[花花酱](https://www.youtube.com/watch?v=VJnUwsE4fWA&t=791s)

## Overview

![union find overview](uf-overview.png)

## Pseudo Code

![pesudo code](uf-pseudo-code.png)

## My Java code

```java
    class UnionFind{
        public int[] parent,rank;
        public int count;
        UnionFind(int n){
            parent=new int[n];
            rank=new int[n];
            for(int i=0;i<n;i++){
                parent[i]=i;
            }
            count=n;
        }
        
        public void union(int x, int y){
            int rootX=find(x);
            int rootY=find(y);
            if(rootX!=rootY){
                if(rank[rootX]<rank[rootY]){
                    parent[rootX]=rootY;
                }else if(rank[rootX]>rank[rootY]){
                    parent[rootY]=rootX;
                }else{
                    parent[rootX]=rootY;
                    rank[rootY]++;
                }
                
                count--;
            }
        }
        
        public int find(int x){
            if(parent[x]!=x){
                parent[x]=find(parent[x]);
            }
            return parent[x];
        }
        
        public int getCount(){
            return count;
        }
    }
```


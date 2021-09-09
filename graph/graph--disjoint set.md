来自leetcode专题 https://leetcode.com/explore/learn/card/graph/
# Overview of Disjoint Set

**purpose: quickly check whether two vertices are connected**

**Algorithm: Union Find. we can use a disjoint set to determine if two people share a common ancestor**

## The two important functions of a disjoint set
- The ***find*** function finds the root node of a given vertex.
- The ***union*** function unions two vertices and makes their root nodes the same. 

## There are two ways to implement a disjoint set-- quick find and quick union
- Implementation with ***Quick Find***: in this case, the time complexity of the ***find*** function will be O(1). However, the ***union*** function will take more responsibility with the time complexity of O(N).
```java
class UnionFind {
    private int[] root;

    // O(N)
    public UnionFind(int size) {
        root = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
        }
    }
    // O(1)
    public int find(int x) {
        return root[x];
    }
		
    //O(N)
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            for (int i = 0; i < root.length; i++) {
                if (root[i] == rootY) {
                    root[i] = rootX;
                }
            }
        }
    }

    // O(1)
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}

```
- Implementation with ***Quick Union***: compared with the Quick Find implementation, the time complexity of the ***union*** function is better. Meanwhile, the ***find*** function takes more responsibility. **Generally speaking, Quick Union is more efficient than Quick Find.**
```java
class UnionFind {
    private int[] root;

    // O(N)
    public UnionFind(int size) {
        root = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
        }
    }

    // worst case O(N)
    // average O(H)  H:height of the tree
    public int find(int x) {
        while (x != root[x]) {
            x = root[x];
        }
        return x;
    }

    // depend on find func
    // so worst case O(N), avg: O(H)
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            root[rootY] = rootX;
        }
    }

    // depend on find func
    // so worst O(N) avg: O(H)
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```
**So if we want to union N nodes, the time complexity of quick find is always 0(N^2), but the one of quick union is O(N^2) in worst case

## Union by Rank
Previously, for the union function, we randomly choose a root node/parent node of x or y and set it as the new root node for the other vertex. However, to union by rank, we must choose the parent node based on certain criteria: When we union two vertices, instead of randomly picking one of the root nodes as the new root node, we choose the root node of the vertex with a larger “rank”. We will merge the shorter tree under the taller tree and assign the root node of the taller tree as the root node for both vertices. **Try to make the tree as balanced as possible**
![Why use rank](https://github.com/lymmm412/lymmm412.github.io/blob/master/graph/uf-rank.png)

```java
class UnionFind {
    private int[] root;
    private int[] rank;

    // O(N)
    public UnionFind(int size) {
        root = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
            rank[i] = 1; 
        }
    }
    
    // O(logN)
    public int find(int x) {
        while (x != root[x]) {
            x = root[x];
        }
        return x;
    }

    // O(logN)
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            } else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
        }
    }

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```

## Path Compression Optimization
After finding the root node, we can update the parent node of all traversed elements to their root node. When we search for the root node of the same element again, we only need to traverse two elements to find its root node, which is highly efficient. **how could we efficiently update the parent nodes of all traversed elements to the root node? The answer is to use “recursion”. This optimization is called “path compression”, which optimizes the find function.**
```java
class UnionFind {
    private int[] root;

    public UnionFind(int size) {
        root = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
        }
    }

    // Path compression
    public int find(int x) {
        if (x != root[x]) {
            root[x] = find(root[x]);
        }
        return root[x];
    }

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            root[rootY] = rootX;
        }
    }

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```

## Optimized Union Find with union by rank and path compression
```java
class UnionFind {
    private int[] root;
    // Use a rank array to record the height of each vertex, i.e., the "rank" of each vertex.
    private int[] rank;

    // O(N)
    public UnionFind(int size) {
        root = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
            rank[i] = 1; // The initial "rank" of each vertex is 1, because each of them is
                         // a standalone vertex with no connection to other vertices.
        }
    }

    // The find function here is the same as that in the disjoint set with path compression.
    // O(logN)
    public int find(int x) {
        if (x == root[x]) {
            return x;
        }
        return root[x] = find(root[x]);
    }

    // The union function with union by rank
    // O(logN)
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            } else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
        }
    }

    // O(logN)
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```

## Summary of the “disjoint set” data structure
The main idea of a “disjoint set” is to have all connected vertices have the same parent node or root node, whether directly or indirectly connected. To check if two vertices are connected, we only need to check if they have the same root node.

The two most important functions for the “disjoint set” data structure are the find function and the union function. The ***find*** function locates the root node of a given vertex. The ***union*** function connects two previously unconnected vertices by equating their root node. There is another important function named ***connected***, which checks the “connectivity” of two vertices. The ***find*** and ***union*** functions are essential for any question requiring the “disjoint set” data structure, while only some of the questions need the ***connected*** function.

来自leetcode专题 https://leetcode.com/explore/learn/card/graph/
# Overview of Disjoint Set

**purpose: quickly check whether two vertices are connected**

**Algorithm: Union Find. we can use a disjoint set to determine if two people share a common ancestor**

## The two important functions of a disjoint set
- The ***find*** function finds the root node of a given vertex.
- The ***union*** function unions two vertices and makes their root nodes the same. 

## There are two ways to implement a disjoint set
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
    public int find(int x) {
        while (x != root[x]) {
            x = root[x];
        }
        return x;
    }

    // depend on find func
    // so worst case O(N)
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            root[rootY] = rootX;
        }
    }

    // O(1)
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```
**So if we want to union N nodes, the time complexity of quick find is always 0(N^2), but the one of quick union is O(N^2) in worst case

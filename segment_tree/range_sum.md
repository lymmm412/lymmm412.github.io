# Segment Tree -- Range Sum

## References
[Segment Tree References](https://leetcode.com/articles/a-recursive-approach-to-segment-trees-range-sum-queries-lazy-propagation/)

## Examples
[303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

[307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)


**interface**
```java
public NumArray(int[] nums) {

}

public int sumRange(int left, int right) {

}
```
**segment tree part**

**segment tree definition**
```java
class SegmentTree{
    int start, end, sum;
    SegmentTree left, right;
    SegmentTree(int start, int end, int val){
        this.start=start;
        this.end=end;
        this.sum=val;
    }
}
```
**build a tree**
```java
private SegmentTree build(int[] nums, int l, int r){
    if(l==r){
        return new SegmentTree(l,r,nums[l]);
    }
    int mid=l+(r-l)/2;
    SegmentTree left=build(nums,l,mid);
    SegmentTree right=build(nums,mid+1,r);
    SegmentTree node=new SegmentTree(l,r,left.sum+right.sum);
    node.left=left;
    node.right=right;
    return node;
}
```
**query a tree**
```java
private int query(SegmentTree node, int l, int r){
    int mid=node.start+(node.end-node.start)/2;
    if(l<=node.start&&r>=node.end){
        return node.sum;
    }else if(l>mid){
        return query(node.right,l,r);
    }else if(r<=mid){
        return query(node.left,l,r);
    }else if(l<=mid&r>mid){
        return query(node.left,l,mid)+query(node.right,mid+1,r);
    }
    return 0;
}
```

**update a tree**
```java
private void updateTree(SegmentTree node, int index, int value){
    int mid=node.start+(node.end-node.start)/2;
    if(node.left==node.right){
        node.sum=value;
        return;
    }else if(index<=mid){
        updateTree(node.left,index,value);
    }else{
        updateTree(node.right, index, value);
    }
    node.sum=node.left.sum+node.right.sum;
}
```

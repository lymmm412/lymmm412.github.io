# Tree Traversal Template
## inorder
**iteration**
```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();

    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode cur = root;

    while(cur!=null || !stack.empty()){
        while(cur!=null){
            stack.add(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        list.add(cur.val);
        cur = cur.right;
    }

    return list;
}
```
**recursion**
```java
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        helper(list, root);
        return list;
    }
    
    private void helper(List<Integer> list, TreeNode root){
        if(root==null){
            return;
        }
        
        helper(list,root.left);
        list.add(root.val);
        helper(list,root.right);
    }
```

## preorder
**iteration**
```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list=new ArrayList<>();
    Stack<TreeNode> stack=new Stack<>();
    TreeNode curr=root;
    while(curr!=null||!stack.isEmpty()){
        while(curr!=null){
            list.add(curr.val);
            stack.push(curr);
            curr=curr.left;
        }

        curr=stack.pop();
        curr=curr.right;
    }
    return list;
}
```
**recursion**
```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list=new ArrayList<>();
    helper(list, root);
    return list;
}

private void helper(List<Integer> list, TreeNode root){
    if(root==null){
        return;
    }

    list.add(root.val);
    helper(list, root.left);
    helper(list, root.right);
}
```

## postorder
**iteration**
```java
  public List<Integer> postorderTraversal(TreeNode root) {
      Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
      List<Integer> res = new ArrayList<Integer>();
      while (!stack.isEmpty() || root != null) {
          // find leaf nodes
          while (root != null) {
              stack.push(root);
              if (root.left != null) {
                  root = root.left;
              } else {
                  root = root.right;
              }
          }
          TreeNode node = stack.pop();
          res.add(node.val);
          if (!stack.isEmpty() && stack.peek().left == node) {
              root = stack.peek().right;
          }
      }
      return res;
  }
```
**recursion**
```java
  public List<Integer> postorderTraversal(TreeNode root) {
      List<Integer> list=new ArrayList<>();
      helper(list, root);
      return list;
  }

  private void helper(List<Integer> list, TreeNode root){
      if(root==null){
          return;
      }

      helper(list, root.left);
      helper(list, root.right);
      list.add(root.val);
  }
```

## level traversal from left to right
**BFS**

**Time: O(#nodes), space: O(#nodes)**
```java
public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> q=new LinkedList<>();
        List<List<Integer>> list=new ArrayList<>();
        if(root!=null){
            q.add(root);
        }
        
        while(!q.isEmpty()){
            int size=q.size();
            List<Integer> tmp=new ArrayList<>();
            for(int i=0;i<size;i++){
                TreeNode curr=q.poll();
                tmp.add(curr.val);
                if(curr.left!=null){
                    q.add(curr.left);
                }
                
                if(curr.right!=null){
                    q.add(curr.right);
                }
                
            }
            list.add(new ArrayList(tmp));
        }
        return list;
    }
```

**DFS: 用一个level和当前的size做比较，如果当前的size<=level说明我们进入了一个新的level，要给当前array初始化**

**Time: O(#nodes), space: O(#nodes)**
```java
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res=new ArrayList<>();
        dfs(0,root, res);
        return res;
    }
    
    private void dfs(int level, TreeNode node, List<List<Integer>> res){
        if(node==null){
            return;
        }
        
        if(res.size()<=level){
            res.add(new ArrayList<>());
        }
        
        res.get(level).add(node.val);
        dfs(level+1, node.left, res);
        dfs(level+1, node.right, res);
    }
    ```

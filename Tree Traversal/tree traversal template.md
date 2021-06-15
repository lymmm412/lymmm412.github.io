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

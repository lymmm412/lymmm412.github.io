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
**iteration with tracking of prev**
```java
public class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        int SUM = 0;
        TreeNode cur = root;
        TreeNode pre = null;
        while(cur!=null || !stack.isEmpty()){
            while(cur!=null){
                stack.push(cur);
                path.add(cur.val);
                SUM+=cur.val;
                cur=cur.left;
            }
            cur = stack.peek();
            // to avoid revisit the right subtree
            // cur.right!=pre
            if(cur.right!=null && cur.right!=pre){
                cur = cur.right;
                continue;
            } 
            if(cur.left==null && cur.right==null && SUM==sum) 
                res.add(new ArrayList<Integer>(path));
  
            pre = cur;
            stack.pop();
            path.remove(path.size()-1);
            SUM-=cur.val;
            cur = null;
        
        }
        return res;
    }
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
    
 # Construct a tree from list
 ## Construct Binary Tree from Inorder and Postorder Traversal
 
 **Time: O(N), space: O(N)**
 ```java
 class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder.length==0||postorder.length==0||inorder.length!=postorder.length){
            return null;
        }
        
        Map<Integer, Integer> map=new HashMap<>();
        for(int i=0;i<inorder.length;i++){
            map.put(inorder[i],i);
        }
        
        return build(inorder, 0, inorder.length-1, postorder, 0, postorder.length-1, map);
    }
    
    private TreeNode build(int[] inorder, int inLeft, int inRight, int[] postorder, int postLeft, int postRight, Map<Integer, Integer> map){
        if(inLeft>inRight||postLeft>postRight){
            return null;
        }
        TreeNode root=new TreeNode(postorder[postRight]);
        int bound=map.get(postorder[postRight]);
        TreeNode left=build(inorder,inLeft,bound-1,postorder,postLeft, postLeft+bound-1-inLeft,map);
        TreeNode right=build(inorder,bound+1,inRight,postorder, postLeft+bound-inLeft,postRight-1,map);
        root.left=left;
        root.right=right;
        return root;
    }
}
```

## Construct Binary Tree from Preorder and Inorder Traversal
 **Time: O(N), space: O(N)**
 ```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        Map<Integer, Integer> map=new HashMap<>();
        for(int i=0;i<inorder.length;i++){
            map.put(inorder[i],i);
        }
        return build(preorder, 0, preorder.length-1, inorder, 0, inorder.length-1, map);
    }
    
    private TreeNode build(int[] preorder, int pStart, int pEnd, int[] inorder, int iStart, int iEnd, Map<Integer, Integer> map){
        if(pStart>pEnd||iStart>iEnd){
            return null;
        }
        TreeNode root=new TreeNode(preorder[pStart]);
        int bound=map.get(preorder[pStart]);
        root.left=build(preorder,pStart+1,pEnd, inorder, iStart, bound-1, map);
        root.right=build(preorder, pStart+bound-iStart+1, pEnd, inorder, bound+1, iEnd, map);
        return root;
    }
}
```

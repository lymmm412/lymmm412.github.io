# Monotonely Decreasing Queue
1499. Max Value of Equation
```java
class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        int max=Integer.MIN_VALUE;
        LinkedList<int[]> q=new LinkedList<>();
        for(int[] point:points){
            while(q.size()>0 && point[0]-q.peekFirst()[0]>k){
                q.pollFirst();
            }
            
            if(q.size()>0){
                int curr=point[1]-point[0];
                max=Math.max(max, point[1]+q.peekFirst()[1]+point[0]-q.peekFirst()[0]);
                while(q.size()>0 && (q.peekLast()[1]-q.peekLast()[0] <= curr)){
                    q.pollLast();
                }
            }
            q.add(point);
        }
        return max;
    }
}
```

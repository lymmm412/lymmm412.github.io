# Problem 1 Medium
[75. Sort Colors](https://leetcode.com/problems/sort-colors/)

**solution 1 AC**
```java
class Solution {
    public void sortColors(int[] nums) {
        int zeros=0, ones=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]==0){
                zeros++;
            }else if(nums[i]==1){
                ones++;
            }
        }
        
        for(int i=0;i<nums.length;i++){
            if(i<zeros){
                nums[i]=0;
            }else if(i<zeros+ones){
                nums[i]=1;
            }else{
                nums[i]=2;
            }
        }
    }
}
```
**solution 2 fail to AC: 试图遇到0把0全放到前面，遇到2把2全放到后面**
```java
//过不了[2,2]等
class Solution {
    public void sortColors(int[] nums) {
        int zero=0, two=nums.length-1;;
        for(int i=0;i<nums.length;i++){
            if(nums[i]==2){
                nums[zero]=nums[two];
                nums[two--]=2;
            }
            
            if(nums[i]==0&&nums[zero]==0){
                zero++;
            }else if(nums[i]==0){
                nums[two]=nums[zero];
                nums[zero++]=0;
            }
            
            if(i==two){
                break;
            }

        }
        
        for(int i=zero;i<=two;i++){
            nums[i]=1;
        }
    }
}
```
修改后
```java
class Solution {
    public void sortColors(int[] nums) {
        int zero=0, two=nums.length-1;;
        for(int i=0;i<=two;i++){//注意边界
            if(nums[i]==0){
                swap(nums, zero++, i);//单纯遇到0就更新nums
            }else if(nums[i]==2){
                swap(nums, i--, two--);//如果把当前数字与后面的交换，交换完可能会存在当前数字还是不符合排序，所以要重新判断一次
            }
        }
       
    }
    
    private void swap(int[] nums, int i, int j){
        int tmp=nums[j];
        nums[j]=nums[i];
        nums[i]=tmp;
    }
}
```

# Problem 2 easy
**two pointers**
[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        for(int i=m+n-1, p1=m-1, p2=n-1;i>=0;i--){
            if(p1>=0&&p2>=0&&nums1[p1]>nums2[p2]){
                nums1[i]=nums1[p1--];
            }else if(p2>=0){
                nums1[i]=nums2[p2--];
            }
        }
    }
}
```

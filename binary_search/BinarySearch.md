# Binary Search

162.find peek element
 ```java
   public int binarySearch(int[] nums){
      int start=0, end=nums.length-1;
      while(start<end){
        int mid=start+(end-start)/2;
        if(nums[mid]<nums[mid+1]){
          start=mid+1;
        }else{
          end=mid;
        }
      }
   return start;
   }
```

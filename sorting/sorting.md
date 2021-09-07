# Sorting Algorithm

## Bubble Sort
**Time Complexity: O(N^2)**

**Space:**

```java
// the worst scenario: in complete descending order and every element need to be
// bubble up except the smallest one//持续的当前值与后一个值比较，如果没满足sort的顺序就isSorted=false，直到变为true为止
public static void bubbleSort(int[] nums) {
    boolean sorted = false;
    int temp;
    while(!sorted) {
        sorted = true;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > nums[i+1]) {
                temp = nums[i];
                nums[i] = nums[i+1];
                nums[i+1] = temp;
                sorted = false;
            }
        }
    }
    //print the sorted array
    System.out.println("bubble sort(O(N^2)):");
    for(int i=0;i<nums.length;i++) {
    System.out.print(nums[i]);
  }
    System.out.println();
}
  ```

## Insertion Sort
**Time Complexity: O(N^2)**

**Space:**

```java
//The sorted part is of length 1 at the beginning and is corresponding to the first 
//(left-most) element in the array. We iterate through the array and during each iteration,
//we expand the sorted portion of the array by one element.
//Upon expanding, we place the new element into its proper place within the sorted subarray
public static void insertionSort(int[] nums) {
  for (int i = 1; i < nums.length; i++) {
        int current = nums[i];
        int j = i - 1;
        while(j >= 0 && current < nums[j]) {
            nums[j+1] = nums[j];
            j--;
        }
        // at this point we've exited, so j is either -1
        // or it's at the first element where current >= a[j]
        nums[j+1] = current;
    }
  System.out.println("insertion sort(O(N^2)):");
    for(int i=0;i<nums.length;i++) {
    System.out.print(nums[i]);
  }
    System.out.println();
}
	
```

## Selection Sort
**Time Complexity: O(N^2)**

**Space:**
```java
//Selection Sort also divides the array into a sorted and unsorted subarray. 
//Though, this time, the sorted subarray is formed by inserting the minimum element 
//of the unsorted subarray at the end of the sorted array, by swapping
public static void selectionSort(int[] nums) {
  for (int i = 0; i < nums.length; i++) {
        int min = nums[i];
        int minId = i;
        for (int j = i+1; j < nums.length; j++) {
            if (nums[j] < min) {
                min = nums[j];
                minId = j;
            }
        }
        // swapping

        int temp = nums[i];
        nums[i] = min;
        nums[minId] = temp;
    }
  System.out.println("selection sort(O(N^2)):");
    for(int i=0;i<nums.length;i++) {
    System.out.print(nums[i]);
  }
    System.out.println();
}
  ```
  
 ## Merge Sort
 **Time Complexity: O(NlogN)**

**Space:**
 ```java
//Merge Sort uses recursion to solve the problem of sorting more efficiently than 
//algorithms previously presented, and in particular it uses a divide and conquer approach.
//Using both of these concepts, we'll break the whole array down into two subarrays and then:
//1.Sort the left half of the array (recursively)
//2.Sort the right half of the array (recursively)
//3.Merge the solutions
//We're just passing indexes left and right which are indexes of the left-most and right-most element 
//of the subarray we want to sort. Initially, these should be 0 and array.length-1, depending on implementation.
//Merge Sort already doesn't work in-place because of the merge step, and this would only serve to worsen its memory efficiency.
public static void mergeSort(int[] nums, int left, int right) {
  if (right<=left) return;
  int mid=left+(right-left)/2;
  mergeSort(nums,left,mid);
  mergeSort(nums,mid+1,right);
  merge(nums,left,mid,right);
  System.out.println("merge sort(O(NlogN)):");
    for(int i=0;i<nums.length;i++) {
    System.out.print(nums[i]);
  }
    System.out.println();
}

private static void merge(int[] nums,int left, int mid, int right) {
  // calculating lengths
    int lengthLeft = mid - left + 1;
    int lengthRight = right - mid;

    // creating temporary subarrays
    int leftArray[] = new int [lengthLeft];
    int rightArray[] = new int [lengthRight];

    // copying our sorted subarrays into temporaries
    for (int i = 0; i < lengthLeft; i++)
        leftArray[i] = nums[left+i];
    for (int i = 0; i < lengthRight; i++)
        rightArray[i] = nums[mid+i+1];

    // iterators containing current index of temp subarrays
    int leftIndex = 0;
    int rightIndex = 0;

    // copying from leftArray and rightArray back into array
    for (int i = left; i < right + 1; i++) {
        // if there are still uncopied elements in R and L, copy minimum of the two
        if (leftIndex < lengthLeft && rightIndex < lengthRight) {
            if (leftArray[leftIndex] < rightArray[rightIndex]) {
                nums[i] = leftArray[leftIndex];
                leftIndex++;
            }
            else {
                nums[i] = rightArray[rightIndex];
                rightIndex++;
            }
        }
        // if all the elements have been copied from rightArray, copy the rest of leftArray
        else if (leftIndex < lengthLeft) {
            nums[i] = leftArray[leftIndex];
            leftIndex++;
        }
        // if all the elements have been copied from leftArray, copy the rest of rightArray
        else if (rightIndex < lengthRight) {
            nums[i] = rightArray[rightIndex];
            rightIndex++;
        }
    }
}
```

## Heap Sort
 **Time Complexity: O(NlogN)**

**Space:**
```java
//https://www.cnblogs.com/chengxiao/p/6129630.html
//Heapsort is an in-place sort, meaning it takes O(1) additional space, 
//as opposed to Merge Sort, but it has some drawbacks as well, such as being difficult to parallelize.
public static void heapSort(int[] nums) {
  int len=nums.length;
  for(int i=len/2-1;i>=0;i--) {
    adjustHeap(nums,len,i);
  }

  for(int i=len-1;i>=0;i--) {
    int tmp=nums[0];
    nums[0]=nums[i];
    nums[i]=tmp;

    adjustHeap(nums,i,0);
  }

  System.out.println("heap sort(O(NlogN)): ");
  for(int i=0;i<nums.length;i++) {
    System.out.print(nums[i]);
  }
  System.out.println();
}

private static void adjustHeap(int[] nums, int len, int curr) {
  int leftChildIdx=2*curr+1;
  int rightChildIdx=2*curr+2;
  int largest=curr;

  if(leftChildIdx<len&&nums[leftChildIdx]>nums[largest]) {
    largest=leftChildIdx;
  }

  if(rightChildIdx<len&&nums[rightChildIdx]>nums[largest]) {
    largest=rightChildIdx;
  }

  if(largest!=curr) {
    int tmp=nums[curr];
    nums[curr]=nums[largest];
    nums[largest]=tmp;
    adjustHeap(nums,len,largest);
  }
}
```

## Quick Sort
 **Time Complexity: O(N^2)**

**Space:**
```java 
public static void quickSort(int[] nums,int begin, int end) {
  if(end<=begin) {
    return;
  }
  int pivot=partition(nums,begin,end);
  quickSort(nums, begin,pivot-1);
  quickSort(nums,pivot+1,end);
  System.out.println("quick sort(O(N^2))");
  for(int i=0;i<nums.length;i++) {
    System.out.print(nums[i]);
  }
  System.out.println();
}

private static int partition(int[] nums, int begin,int end) {
  int pivot=end;
  int counter=begin;
  for(int i=begin;i<end;i++) {
    if(nums[i]<nums[pivot]) {
      int tmp=nums[counter];
      nums[counter]=nums[i];
      nums[i]=tmp;
      counter++;
    }
  }
  int tmp=nums[pivot];
  nums[pivot]=nums[counter];
  nums[counter]=tmp;
  return counter;
}
```

```java
public class QuickSort {
  public static void sort(int a[], int low, int hight) {
      int i, j, index;
      if (low > hight) {
          return;
      }
      i = low;
      j = hight;
      index = a[i]; // 用子表的第一个记录做基准
      while (i < j) { // 从表的两端交替向中间扫描
          while (i < j && a[j] >= index)
              j--;
          if (i < j)
              a[i++] = a[j];// 用比基准小的记录替换低位记录
          while (i < j && a[i] < index)
              i++;
          if (i < j) // 用比基准大的记录替换高位记录
              a[j--] = a[i];
      }
      a[i] = index;// 将基准数值替换回 a[i]
      sort(a, low, i - 1); // 对低子表进行递归排序
      sort(a, i + 1, hight); // 对高子表进行递归排序

  }

  public static void quickSort(int a[]) {
      sort(a, 0, a.length - 1);
  }
}
```

## Other Sorting

**峰谷峰谷峰谷型**
- 1968. Array With Elements Not Equal to Average of Neighbors
- 

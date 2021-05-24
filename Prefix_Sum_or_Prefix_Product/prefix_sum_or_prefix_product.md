# Prefix Sum/Product Problem

## get the last k product
[1352. Product of the Last K Numbers](https://leetcode.com/problems/product-of-the-last-k-numbers/)

逆向获取乘积：
```java
public void add(int num) {
    arr[count] = num;
    for (int i = 0; i < count; i++) {
        arr[i] *= num;
    }
    count++;           
}
```

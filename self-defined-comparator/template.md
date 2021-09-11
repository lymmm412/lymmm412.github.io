# Self-defined Comparator
```java
        Comparator<T> myComp = new Comparator<T>() {
            @Override
            public int compare(T s1, T s2) {
              // do something
                return 0;
             // do something
                return 1;
            }
        };
        
        Arrays.sort(logs, myComp);
```

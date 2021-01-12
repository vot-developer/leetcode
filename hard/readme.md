[295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)
```java
//time - O(log n), space - O(n)
class MedianFinder {
    private final PriorityQueue<Integer> max;
    private final PriorityQueue<Integer> min;

    /**
     * initialize your data structure here.
     */
    public MedianFinder() {
        this.max = new PriorityQueue<>();
        this.min = new PriorityQueue<>(Comparator.reverseOrder());//max queue
    }

    public void addNum(int num) {
        max.add(num);
        min.add(max.poll());
        if (max.size() < min.size())
            max.add(min.poll());
    }

    public double findMedian() {
        if (max.size() > min.size()) return max.peek();
        else return (max.peek() + min.peek()) / 2.0;
    }
}
```

[887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/)
```java
class Solution {
    public int superEggDrop(int K, int N) {
        int[][] map = new int[N+1][K+1];
        int m = 0;
        while (map[m][K] < N){
            m++;
            for (int k = 1; k <= K; k++){
                map[m][k] = 1 + map[m-1][k] + map[m-1][k-1];
            }
        }
        return m;
    }
}
```
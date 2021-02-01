[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
```java
class Solution {
    //pattern - sliding window, time - O(n + m), space - O(m)
    public String minWindow(String str, String pattern) {
        int matcher, startMatch = 0, minLength = Integer.MAX_VALUE;
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < pattern.length(); i++) {
            char c = pattern.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        matcher = map.size();

        for (int start = 0, end = 0; end < str.length(); end++) {
            char c = str.charAt(end);
            if (map.containsKey(c)) {
                map.put(c, map.getOrDefault(c, 0) - 1);
                if (map.get(c) == 0)
                    matcher--;
            }

            while (matcher == 0) {
                if (end - start + 1 < minLength) {
                    startMatch = start;
                    minLength = end - start + 1;
                }
                char r = str.charAt(start++);
                if (map.containsKey(r)) {
                    map.put(r, map.getOrDefault(r, 0) + 1);
                    if (map.get(r) > 0)
                        matcher++;
                }
            }
        }

        if (minLength == Integer.MAX_VALUE)
            return "";

        return str.substring(startMatch, startMatch + minLength);
    }
}
```

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
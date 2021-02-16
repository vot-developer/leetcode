[25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    //pattern - reversal of linkedlist, time - O(n), space - O(1)
    public ListNode reverseKGroup(ListNode head, int k) {
        if (k <= 1 || head == null)
            return head;

        ListNode current = head;
        ListNode next, beforeSublist = null, prev = null;
        while (current != null) {
            if (!isExistKItems(current, k))
                break;

            ListNode firstInSubList = current;
            for (int i = 0; i < k && current != null; i++) {
                next = current.next;
                current.next = prev;
                prev = current;
                current = next;
            }
            firstInSubList.next = current;
            if (beforeSublist == null)
                head = prev;
            else
                beforeSublist.next = prev;
            beforeSublist = firstInSubList;
        }

        return head;        
    }
    
    private boolean isExistKItems(ListNode head, int k){
        int i = 0;
        for (; i < k && head != null; i++)
            head = head.next;
        return i == k;
    }
}
```

[30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)
```java
class Solution {
    //pattern - sliding window, time - O(n * {words.length}), space - O({words size})
    public List<Integer> findSubstring(String str, String[] words) {
        int length = words[0].length();
        List<Integer> resultIndices = new ArrayList<>();

        Map<String, Integer> frequencies = new HashMap<>(words.length);
        for (int i = 0; i < words.length; i++)
            frequencies.put(words[i], frequencies.getOrDefault(words[i], 0) + 1);

        for (int i = 0; i <= str.length() - words.length * length; i++) {
            Map<String, Integer> wordsSeen = new HashMap<>();
            for (int j = 0; j < words.length; j++) {
                int nextWordIndex = i + j * length;

                String word = str.substring(nextWordIndex, nextWordIndex + length);
                if (!frequencies.containsKey(word))
                    break;

                wordsSeen.put(word, wordsSeen.getOrDefault(word, 0) + 1);

                if (wordsSeen.get(word) > frequencies.getOrDefault(word, 0))
                    break;

                if (j + 1 == words.length)
                    resultIndices.add(i);
            }
        }

        return resultIndices;
    }
}
```

[41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
```java
class Solution {
    //pattern - cyclic sort, time - O(n), space - O(1)
    public int firstMissingPositive(int[] nums) {
        int index = 0;
        while (index < nums.length){
            int expectedIndex = nums[index] - 1;
            if (expectedIndex >= 0 && expectedIndex < nums.length && nums[index] != nums[expectedIndex])
                swap(index, expectedIndex, nums);
            else
                index++;
        }
        for (int i = 0; i < nums.length; i++)
            if (i != nums[i] - 1)
                return i + 1;

        return nums.length + 1;
    }
    
    private void swap(int i, int j, int[] nums) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

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

[124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int max;
    //pattern - dfs, time - O(n), space - O(n)
    public int maxPathSum(TreeNode root) {
        max = Integer.MIN_VALUE;
        doFind(root);
        return max;
    }
    
    private int doFind(TreeNode node) {
        if (node == null)
            return 0;
        int left = Math.max(doFind(node.left), 0);
        int right = Math.max(doFind(node.right), 0);

        int sum = left + right + node.val;
        max = Math.max(sum, max);

        return Math.max(left, right) + node.val;
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

[480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)
```java
class Solution {    
    //pattern - sliding window, two heaps; time - O(n log k), space - O(k)
    public double[] medianSlidingWindow(int[] nums, int k) {
        double[] result = new double[nums.length - k + 1];

        Comparator<Integer> comparator = (a, b) -> nums[a] != nums[b] ? Integer.compare(nums[a], nums[b]) : Integer.compare(a, b);
        TreeSet<Integer> left = new TreeSet<>(comparator.reversed()); //ordered by max
        TreeSet<Integer> right = new TreeSet<>(comparator);

        for (int i = 0; i < nums.length; i++) {
            if (i < k - 1) {
                right.add(i);
                left.add(right.pollFirst());
                balance(left, right);
                continue;
            }

            right.add(i);
            left.add(right.pollFirst());
            balance(left, right);
            result[i - k + 1] = findMedian(nums, left, right);
            if (!right.remove(i - k + 1))
                left.remove(i - k + 1);
            balance(left, right);
        }

        return result;
    }

    private void balance(TreeSet<Integer> left, TreeSet<Integer> right){
        while (right.size() > left.size() + 1)
            left.add(right.pollFirst());
        while(right.size() < left.size())
            right.add(left.pollFirst());
    }

    private double findMedian(int[] nums, TreeSet<Integer> left, TreeSet<Integer> right) {
        if (right.size() != left.size()) return nums[right.first()];
        return nums[right.first()] / 2.0 + nums[left.first()] / 2.0;
    }
}
```

[887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/)
```java
class Solution {
    //pattern - dynamic programming (bottom to up)
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
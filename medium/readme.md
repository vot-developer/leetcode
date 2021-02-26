[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(n)
    public int lengthOfLongestSubstring(String s) {        
        int start = 0, maxLength = 0;
        Map<Character, Integer> symbols = new HashMap<>();

        for (int end = 0; end < s.length(); end++) {
            char c = s.charAt(end);
            if (symbols.containsKey(c))
                start = Math.max(symbols.get(c) + 1, start);

            symbols.put(c, end);
            
            maxLength = Math.max(end - start + 1, maxLength);
        }

        return maxLength;
    }
}
```

[15. 3Sum.](https://leetcode.com/problems/3sum/)
```java
class Solution {
    //pattern - two pointers, time - O(n^2), space - O(1)
    public List<List<Integer>> threeSum(int[] arr) {
        if (arr.length < 3) return new ArrayList<>();
        Arrays.sort(arr);
        List<List<Integer>> solutions = new ArrayList<>();
        for (int i = 0; i < arr.length; i++) {
            if (i > 0 && arr[i] == arr[i - 1])
                continue;

            int left = i + 1;
            int right = arr.length - 1;

            while (left < right) {
                int sum = arr[i] + arr[left] + arr[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    solutions.add(Arrays.asList(arr[i], arr[left], arr[right]));
                    left++;
                    right--;
                    while (left < right && arr[left] == arr[left - 1])
                        left++; // skip same element to avoid duplicate triplets
                    while (left < right && arr[right] == arr[right + 1])
                        right--; // skip same element to avoid duplicate triplets
                }
            }
        }
        return solutions;
    }
}
```

[16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)
```java
class Solution {
    //pattern - two pointers, time - O(n^2), space - O(1)
    public int threeSumClosest(int[] arr, int targetSum) {
        int min = Integer.MAX_VALUE;
        Arrays.sort(arr);

        for (int i = 0; i < arr.length - 2; i++) {
            int l = i + 1;
            int r = arr.length - 1;

            while (l < r) {
                int diff = targetSum - arr[i] - arr[l] - arr[r];

                if (Math.abs(diff) < Math.abs(min))
                    min = diff;
                if (diff > 0)
                    l++;
                else if (diff < 0)
                    r--;
                else
                    return targetSum;
            }
        }
        return targetSum - min;
    }
}
```

[18. 4Sum](https://leetcode.com/problems/4sum/)
```java
class Solution {
    //pattern - two pointers, time - O(n^3), space - O(n)
    public List<List<Integer>> fourSum(int[] arr, int target) {
        Arrays.sort(arr);
        List<List<Integer>> quadruplets = new ArrayList<>();

        for (int i = 0; i < arr.length - 3; i++){
            if (i > 0 && arr[i] == arr[i-1])
                continue;
            for (int j = i + 1; j < arr.length - 2; j++){
                if (j > i + 1 && arr[j] == arr[j-1])
                    continue;

                int sum = target - arr[i] - arr[j];
                int l = j + 1, r = arr.length - 1;
                while (l < r){
                    int localSum = sum - arr[l] - arr[r];
                    if (localSum > 0){
                        l++; //add check dubles
                    } else if (localSum < 0){
                        r--;
                    } else {
                        quadruplets.add(Arrays.asList(arr[i], arr[j], arr[l], arr[r]));
                        l++;
                        r--;
                        while (l < r && arr[l] == arr[l - 1]) // skip same element to avoid duplicate quadruplets
                            l++;
                        while (l < r && arr[r] == arr[r + 1])
                            r--;
                    }
                }
            }
        }

        return quadruplets;        
    }
}
```

[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
```java
class Solution {
    //pattern - dp, time - O(n*2^n), space - O(2^n)
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        char[] s = new char[2*n];
        doDP(s, 0, 0, 0, n, result);
        return result;
    }  
    
    private void doDP(char[] s, int index, int leftCount, int rightCount, int n, List<String> result){
        if (index == s.length) {
            result.add(new String(s));
            return;
        }

        if (leftCount < n) {
            s[index] = '(';
            doDP(s, index + 1, leftCount + 1, rightCount, n, result);
        }

        if (rightCount < leftCount){
            s[index] = ')';
            doDP(s, index + 1, leftCount, rightCount + 1, n, result);
        }
    }
}
```
```java
class Solution {
    //pattern - subsets-bfs(cascade), time - O(n*2^n), space - O(n*2^n)
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        Queue<ParString> queue = new LinkedList<>();
        ParString first = new ParString();
        first.left = 1;
        first.sb.append('(');
        queue.add(first);
        while (!queue.isEmpty()){
            int size = queue.size();
            for (int i = 0; i < size; i++){
                ParString p = queue.poll();
                if (p.left == n && p.right == n)
                    result.add(p.sb.toString());
                if (p.left < n){
                    ParString left = new ParString(p);
                    left.sb.append('(');
                    left.left++;
                    queue.offer(left);
                }
                if (p.left > p.right){
                    ParString right = new ParString(p);
                    right.sb.append(')');
                    right.right++;
                    queue.offer(right);
                }
            }
        }

        return result;
    }
}

class ParString {
    int left;
    int right;
    StringBuilder sb;

    public ParString() {
        this.sb = new StringBuilder();
    }

    public ParString(ParString parString) {
        this.sb = new StringBuilder(parString.sb);
        this.left = parString.left;
        this.right = parString.right;
    }
}
```

[33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
```java
class Solution {
    //pattern - mbs, time - O(log n), space - O(1)
    public int search(int[] nums, int target) {
        int maxIndex = findMax(nums);
        int keyIndex = binarySearch(nums, target, 0, maxIndex);
        if (keyIndex != -1)
            return keyIndex;
        return binarySearch(nums, target, maxIndex + 1, nums.length - 1);
    }
    
    private int findMax(int[] arr) {
        int start = 0, end = arr.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (arr[mid] > arr[mid + 1] || arr[mid] < arr[start])
                end = mid;
            else
                start = mid + 1;
        }
        return start;
    }

    private int binarySearch(int[] arr, int key, int start, int end) {
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (key == arr[mid])
                return mid;

            if (key < arr[mid])
                end = mid - 1;
            else  // key > arr[mid]
                start = mid + 1;
        }
        return -1;
    }
}
```
```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums.length == 0)
            return -1;

        int left = 0;
        int right = nums.length - 1;

        int mid;
        while (left <= right) {
            mid = left + (right - left) / 2;

            if (nums[mid] == target)
                return mid;

            if (nums[left] <= nums[mid]) {   //linear case - LEFT
                if (target < nums[mid] && target >= nums[left]) {   //target in left
                    right = mid - 1;
                } else {                    //target in right, NOT linear
                    left = mid + 1;
                }
            } else {                        //linear case - RIGHT
                if (target > nums[mid] && target <= nums[right]){ //target in right
                    left = mid + 1;
                } else {                    //target in left, NOT linear
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```

[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
```java
class Solution {
    //pattern - mbs, time - O(log n), space - O(1)
    public int[] searchRange(int[] nums, int target) {
        int[] result = new int[]{-1, -1};

        int index = findTarget(nums, target);
        if (index == -1)
            return result;

        int left = findLeft(nums, target, index);
        int right = findRight(nums, target, index);
        return new int[]{left, right};        
    }
    
    private int findRight(int[] nums, int target, int start) {
        int end = nums.length - 1;
        int result = start;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] > target)
                end = mid - 1;
            else {
                result = mid;
                start = mid + 1;
            }
        }
        return result;
    }

    private int findLeft(int[] nums, int target, int end) {
        int start = 0;
        int result = end;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < target)
                start = mid + 1;
            else {
                result = mid;
                end = mid - 1;
            }
        }
        return result;
    }

    private int findTarget(int[] nums, int target) {
        int start = 0, end = nums.length - 1;

        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target)
                return mid;

            if (nums[mid] > target)
                end = mid - 1;
            if (nums[mid] < target)
                start = mid + 1;
        }
        return -1;
    }
}
```

[46. Permutations](https://leetcode.com/problems/permutations/)
```java
class Solution {
    //pattern - dp, time - O(n*n!), space - O(n*n!) 
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        doPermute(nums, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void doPermute(int[] nums,  int index, List<Integer> permutation,
                                  List<List<Integer>>  permutations){
        if (permutation.size() == nums.length) {
            permutations.add(permutation);
            return;
        }

        for (int i = 0; i <= permutation.size(); i++) {
            List<Integer> newPermutation = new ArrayList<>(permutation);
            newPermutation.add(i, nums[index]);
            doPermute(nums, index + 1, newPermutation, permutations);
        }
    }
}
```
```java
class Solution {
    //pattern - subsets-dfs(cascade), time - O(n*n!), space - O(n*n!) 
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Queue<List<Integer>> queue = new LinkedList<>();
        queue.add(new ArrayList<>());
        for (int num : nums){
            int size = queue.size();
            for (int i = 0; i < size; i++){
                List<Integer> set = queue.poll();
                int setSize = set.size();
                for (int j = 0; j <= setSize; j++){
                    List<Integer> perm = new ArrayList<>(set);
                    perm.add(j, num);
                    if (perm.size() == nums.length)
                        result.add(perm);
                    else
                        queue.offer(perm);
                }
            }
        }
        return result;
    }
}
```

[55. Jump Game](https://leetcode.com/problems/jump-game/)
```java
//time - O(n), space - O(1)
class Solution {
    public boolean canJump(int[] nums) {           
        int possibleIndex = nums.length - 1;
        for (int i = nums.length - 1; i >= 0; i--) 
            if (i + nums[i] >= possibleIndex)
                possibleIndex = i;

        return possibleIndex == 0;
    }
}
```

[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)
```java
class Solution {
    //pattern - merge intervals, time - O(n), space - O(n)
	public int[][] merge(int[][] intervals) {
        if (intervals.length < 2)
            return intervals;

        Arrays.sort(intervals, Comparator.comparingInt(o -> o[0]));
        List<int[]> result = new ArrayList<>();

        int[] intervalOne = intervals[0];
        for (int[] intervalTwo : intervals) {
            if (intervalOne[1] < intervalTwo[0]) {
                result.add(intervalOne);
                intervalOne = intervalTwo;
            } else {
                intervalOne[1] = Math.max(intervalOne[1], intervalTwo[1]);
            }
        }
        result.add(intervalOne);

        return result.toArray(new int[result.size()][]);
	}
}
```
```java
class Solution {
    //pattern - merge intervals, time - O(n), space - O(1)
    public int[][] merge(int[][] intervals) {
        if (intervals.length < 2)
            return intervals;

        Arrays.sort(intervals, Comparator.comparingInt(o -> o[0]));

        int startIndex = 0, endIndex = 1;
        int start = intervals[startIndex][0];
        int end = intervals[startIndex][1];
        while (endIndex < intervals.length){
            if (end < intervals[endIndex][0]){
                intervals[startIndex][0] = start;
                intervals[startIndex][1] = end;
                start = intervals[endIndex][0];
                end = intervals[endIndex][1];
                startIndex++;
                endIndex++;
            } else {
                intervals[startIndex][0] = start;
                intervals[startIndex][1] = Math.max(
                        end, intervals[endIndex][1]);
                end = intervals[startIndex][1];
                endIndex++;
            }
        }
        intervals[startIndex][0] = start;
        intervals[startIndex][1] = end;

        return Arrays.copyOfRange(intervals, 0, startIndex + 1);
    }
}
```

[57. Insert Interval](https://leetcode.com/problems/insert-interval/)
```java
class Solution {
    //pattern - merge intervals, time - O(n), space - O(n)
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> mergedIntervals = new ArrayList<>();

        int i = 0;
        for (; i < intervals.length && intervals[i][1] < newInterval[0]; i++)
            mergedIntervals.add(intervals[i]);

        //merging
        while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
            newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
            i++;
        }

        mergedIntervals.add(newInterval);

        for (; i < intervals.length; i++)
            mergedIntervals.add(intervals[i]);

        return mergedIntervals.toArray(new int[mergedIntervals.size()][]);        
    }
}
```

[75. Sort Colors](https://leetcode.com/problems/sort-colors/)
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(1)
    public void sortColors(int[] arr) {
        int i = 0, l = 0, r = arr.length - 1;
        while (i <= r) {
            if (arr[i] == 0)
                swap(arr, i++, l++);
            else if (arr[i] == 1)
                i++;
            else
                swap(arr, i, r--);
        }
    }
    
    private void swap(int[] array, int indexOne, int indexTwo) {
        int temp = array[indexOne];
        array[indexOne] = array[indexTwo];
        array[indexTwo] = temp;
    }
}
```

[78. Subsets](https://leetcode.com/problems/subsets/)
```java
class Solution {
    //pattern - subset-bfs(cascade), time - O(n * 2^n), space - O(n * 2^n), total of subsets - O(2^n) 
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> subsets = new ArrayList<>();
        subsets.add(new ArrayList<>()); //empty set

        for (int num : nums){
            int size = subsets.size();
            for (int i = 0; i < size; i++){
                List<Integer> set = new ArrayList<>(subsets.get(i));
                set.add(num);
                subsets.add(set);
            }
        }

        return subsets;
    }
}
```

[90. Subsets II](https://leetcode.com/problems/subsets-ii/)
```java
class Solution {
    //pattern - subset-bfs(cascade), time - O(n * 2^n), space - O(n * 2^n)
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> subsets = new ArrayList<>();
        subsets.add(new ArrayList<>()); //empty set

        int start, end = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1])
                start = end;
            else
                start = 0;
            end = subsets.size();
            for (int j = start; j < end; j++) {
                List<Integer> set = new ArrayList<>(subsets.get(j)); //O(n)
                set.add(nums[i]);
                subsets.add(set);
            }
        }
        return subsets;
    }
}
```

[92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
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
    public ListNode reverseBetween(ListNode head, int p, int q) {
        if (head.next == null || p == q)
            return head;

        ListNode current = head;
        ListNode prev = null, next = null;
        for (int i = 1; i < p  && current != null; i++){
            prev = current;
            current = current.next;
        }
        ListNode beforeP = prev;
        ListNode nodeP = current;
        for (int i = 0; i < q - p + 1 && current != null; i++){ //finish on q + 1 node;
            next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }

        if (beforeP != null)
            beforeP.next = prev;
        else
            head = prev;

        nodeP.next = current;
        return head;
    }
}
```

[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
```java
//time - O(n), space - O(n)
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        doIterate(root, result);
        return result;
    }
    
    private void doIterate(TreeNode node, List<Integer> result){
        if (node == null) return;

        doIterate(node.left, result);
        result.add(node.val);
        doIterate(node.right, result);
    }
}
```
```java
//non-recursive
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode current = root;
        while(current != null || !stack.isEmpty()){
            while (current != null){
                stack.addFirst(current);
                current = current.left;
            }
            current = stack.removeFirst();
            result.add(current.val);
            current = current.right;
        }
        return result;
    }
}
```
//use Morris method for space - O(1) (rewrite linking of nodes and on iterator - move it back)

[95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)
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
    //pattern - subsets-dp; time - O(n * 2^n), space - (n * 2^n)
    public List<TreeNode> generateTrees(int n) {
        if (n <= 0)
            return new ArrayList<>();
        return findUniqueTreesRecursive(1, n);
    }
    
    private List<TreeNode> findUniqueTreesRecursive(int start, int end) {
        List<TreeNode> result = new ArrayList<>();

        if (start > end) {
            result.add(null);//for do loop by other side
            return result;
        }

        if (start == end){
            result.add(new TreeNode(start));
            return result;
        }

        for (int i = start; i <= end; i++){
            List<TreeNode> left = findUniqueTreesRecursive(start, i - 1);
            List<TreeNode> right = findUniqueTreesRecursive(i + 1, end);
            for (TreeNode l : left)
                for (TreeNode r : right){
                    TreeNode node = new TreeNode(i);
                    node.left = l;
                    node.right = r;
                    result.add(node);
                }
        }
        return result;
    }
}
```

[96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)
```java
class Solution {
    //pattern - subsets-dp; time - O(n^2), space - O(n)
    public int numTrees(int n) {
        return doDP(n, new Integer[n + 1]);
    }
    
    private int doDP(int n, Integer[] dp) {
        if (n <= 1)
            return 1;

        if (dp[n] != null)
            return dp[n];

        int count = 0;
        for (int i = 1; i <= n; i++) {
            int left = doDP( i - 1, dp);
            int right = doDP(n - i, dp);
            count += left * right;
        }

        dp[n] = count;
        return dp[n];
    }
}
```

[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;

        return validate(root.left, Long.MIN_VALUE, root.val) && validate(root.right, root.val, Long.MAX_VALUE);
    }
    
    private boolean validate(TreeNode root, long min, long max){
        if (root == null) return true;

        if (root.val >= max) return false;
        if (root.val <= min) return false;

        return validate(root.right, root.val, max) && validate(root.left, min, root.val);
    }
}
```
[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
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
    //pattern - bfs, time - O(n), space - O(n)
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null)
            return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            int size = queue.size();
            List<Integer> level = new ArrayList<>(size);
            for (int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
            }
            result.add(level);
        }
        return result;        
    }
}
```

[103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
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
    //pattern - bfs, time - O(n), space - O(n)
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null)
            return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new LinkedList<>();
            boolean isDirectOrder = result.size() % 2 == 0;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (isDirectOrder)
                    level.add(node.val);
                else
                    level.add(0, node.val);
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
            }
            result.add(level);
        }
        return result;        
    }
}
```

[113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)
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
    //patterns - dfs, backtrack; time - O(n^2), space - O(n * log n (h))
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> allPaths = new ArrayList<>();
        doFindPaths(root, targetSum, new ArrayList<>(), allPaths);
        return allPaths;
    }
    
    private void doFindPaths(TreeNode root, int sum, List<Integer> path, List<List<Integer>> allPaths){
        if (root == null)
            return;

        path.add(root.val);

        if (root.val == sum && root.left == null && root.right == null)
            allPaths.add(new ArrayList<>(path));

        doFindPaths(root.left, sum - root.val, path, allPaths);
        doFindPaths(root.right, sum - root.val, path, allPaths);

        path.remove(path.size() - 1);
    }
}
```

[116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/
class Solution {
    //pattern - bfs, time - O(n), space - O(n)
    public Node connect(Node root) {
        if (root == null)
            return root;

        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            int size = queue.size();
            Node prev = null;
            for (int i = 0; i < size; i++){
                Node node = queue.poll();
                if (prev != null)
                    prev.next = node;
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
                prev = node;
            }
        }
        return root;
    }
}
```
```java
class Solution {
    //pattern - bfs, time - O(n), space - O(1) (using property - perfect binary tree)
    public Node connect(Node root) {
        if (root == null)
            return root;
        
        Node node = root;
        while (node != null){
            Node levelNode = node;
            while (levelNode!= null){
                if (levelNode.left != null)
                    levelNode.left.next=levelNode.right;
                if (levelNode.right != null && levelNode.next != null)
                    levelNode.right.next = levelNode.next.left;                
                levelNode = levelNode.next;
            }
            node = node.left;
        }
                
        return root;
    }
}
```

[129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)
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
    //pattern - dfs, time - O(n), space - O(n)
    public int sumNumbers(TreeNode root) {
        return doFindSumOfPathNumbers(root, 0);
    }
    
    private int doFindSumOfPathNumbers(TreeNode node, int sum) {
        if (node == null)
            return 0;

        sum *= 10;
        sum += node.val;
        if (node.left == null && node.right == null)
            return sum;

        return doFindSumOfPathNumbers(node.left, sum)
                + doFindSumOfPathNumbers(node.right, sum);
    }
}
```

[143. Reorder List](https://leetcode.com/problems/reorder-list/)
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
    //pattern - fast and slow pointers, time - O(n), space - O(1)
    public void reorderList(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }

        ListNode reverse = makeReverse(slow);
        while (head != null && reverse != null){
            ListNode temp = head.next;
            head.next = reverse;
            head = temp;

            temp = reverse.next;
            reverse.next = head;
            reverse = temp;
        }

        if (head != null)
            head.next = null;
    }

    private ListNode makeReverse(ListNode head){
        ListNode prev = null;
        while (head != null){
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}
```

[153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
```java
class Solution {
    public int findMin(int[] nums) {
        int start = 0, end = nums.length - 1;
        if (nums[start] < nums[end])
            return nums[start];
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (start == mid){
                return Math.min(nums[start], nums[end]);
            }
            if (nums[mid] < nums[start])
                end = mid;
            else
                start = mid;
        }
        return nums[end];
    }
}
```

[162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)
```java
class Solution {
    //pattern - mbs, time - O(log n), space - O(1)
    public int findPeakElement(int[] nums) {
        int start = 0, end = nums.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] > nums[mid + 1])
                end = mid;
            else
                start = mid + 1;
        }
        return start;
    }
}
```

[173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)
```java
class BSTIterator {
    private Stack<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        stack = new Stack<>();
        deepLeft(root);
    }
    
    /** @return the next smallest number */
    public int next() {
        TreeNode current = stack.pop();
        int result = current.val;

        if (current.right != null) deepLeft(current.right);

        return result;        
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();        
    }
    
    private void deepLeft(TreeNode node) {
        while (node != null){
            stack.push(node);
            node = node.left;
        }
    }
}
```

[199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
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
    //pattern - bfs, time - O(n), space - O(n)
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null)
            return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            int size = queue.size();
            for (int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                if (i == size - 1)
                    result.add(node.val);
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
            }
        }
        return result;
    }
}
```

[222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)
```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        
        int count = 0;
        if (root.left != null) count += countNodes(root.left);
        if (root.right != null) count += countNodes(root.right);
        return ++count;
    }
}
```

[230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        deepLeft(root, stack);

        while (!stack.isEmpty()) {
            TreeNode current = stack.pop();
            if (--k == 0) return current.val;
            if (current.right != null){
                deepLeft(current.right, stack);
            }
        }
        return -1;
    }
    
    private void deepLeft(TreeNode node, Stack<TreeNode> stack) {
        while (node != null){
            stack.push(node);
            node = node.left;
        }
    }
}
```

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
```java
class Solution {
    //pattern - cyclic sort, time - O(n), space - O(1)
    public int findDuplicate(int[] nums) {
        int index = 0;
        while (index < nums.length) {
            int expectedIndex = nums[index] - 1;
            if (index != expectedIndex) {
                if (nums[index] != nums[expectedIndex])
                    swap(index, expectedIndex, nums);
                else
                    return nums[index];
            } else
                index++;
        }

        return -1;
    }
    
     private void swap(int i, int j, int[] nums) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

[384. Shuffle an Array](https://leetcode.com/problems/shuffle-an-array/)
```java
class Solution {

    private final int[] originalArray;
    private final int[] array;
    private final Random random;
    
    public Solution(int[] array) {
        this.originalArray = array;
        this.array = Arrays.copyOf(originalArray, array.length);
        this.random = new Random();
    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return originalArray;
    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        for (int i = 0; i < array.length; i++) {
            swap(array, i, random.nextInt(i + 1));
        }
        return array;
    }
    
    private void swap(int[] array, int indexOne, int indexTwo){
        int temp = array[indexOne];
        array[indexOne] = array[indexTwo];
        array[indexTwo] = temp;
    }
}
```

[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
````java
//time - O(n * S), space - O(n * S)
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = Arrays.stream(nums).sum();
        if ((sum & 1) != 0)
            return false;

        int sumForFind = sum / 2;
        Boolean[][] aux = new Boolean[nums.length][sumForFind + 1];
        return doToToDown(nums, 0, sumForFind, aux);    
    }
    
    private boolean doToToDown(int[] nums, int index, int sum, Boolean[][] aux){
        if (sum == 0)
            return true;

        if (index == nums.length)
            return false;

        if (aux[index][sum] != null)
            return aux[index][sum];

        if (nums[index] <= sum)
            if (doToToDown(nums, index + 1, sum - nums[index], aux)){ //add something
                aux[index][sum] = true;
                return true;
            }

        aux[index][sum] = doToToDown(nums, index + 1, sum, aux); //add nothing
        return aux[index][sum];
    }
}
````

[424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(R)
    public int characterReplacement(String s, int k) {
        int max = 0, maxRepeat = 0;
        char[] counts = new char[26];
        for (int start = 0, end = 0; end < s.length(); end++) {
            char c = s.charAt(end);
            counts[c - 'A']++;
            maxRepeat = Math.max(counts[c - 'A'], maxRepeat) ;

            if (end - start + 1 - maxRepeat > k)
                counts[s.charAt(start++) - 'A']--;

            max = Math.max(end - start + 1, max);
        }
        return max;
    }
}
```

[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
```java
class Solution {
    //patterns - 'greedy algorithm', 'merge intervals', time - O(n * log n), space - O(1) 
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length < 2)
            return 0;

        Arrays.sort(intervals, Comparator.comparingInt(value -> value[0])); //by start

        int count = 0;
        int prevEnd = intervals[0][1];
        for (int i = 1; i < intervals.length; i++){
            if (prevEnd > intervals[i][0]){ //overlapping
                prevEnd = Math.min(intervals[i][1], prevEnd);//chosen smaller
                count++;
            } else {
                prevEnd = intervals[i][1];
            }
        }

        return count;
    }
}
```

[436. Find Right Interval](https://leetcode.com/problems/find-right-interval/)
```java
class Solution {
    //pattern - two heaps, time - O(n log n), space - O(n)
    public int[] findRightInterval(int[][] intervals) {
        int[] result = new int[intervals.length];
        PriorityQueue<Integer> startPQ = new PriorityQueue<>(intervals.length,
                (a, b) -> intervals[a][0] - intervals[b][0]);
        PriorityQueue<Integer> endPQ = new PriorityQueue<>(intervals.length,
                (a, b) -> intervals[a][1] - intervals[b][1]);

        for (int i = 0; i < intervals.length; i++){
            startPQ.offer(i);
            endPQ.offer(i);
        }

        for (int i = 0; i < intervals.length; i++){
            int index = endPQ.poll();
            int end = intervals[index][1];
            while (!startPQ.isEmpty() && intervals[startPQ.peek()][0] < end)
                startPQ.poll();

            if (!startPQ.isEmpty())
                result[index] = startPQ.peek();
            else
                result[index] = -1;
        }

        return result;        
    }
}
```
```java
class Solution {
    //pattern - BST, time - O(n log n), space - O(n)
    public int[] findRightInterval(int[][] intervals) {
        int[] result = new int[intervals.length];
        java.util.NavigableMap<Integer, Integer> map = new TreeMap<>();

        for (int i = 0; i < intervals.length; i++)
            map.put(intervals[i][0], i);

        for (int i = 0; i < intervals.length; i++) {
            Map.Entry<Integer, Integer> entry = map.ceilingEntry(intervals[i][1]);
            result[i] = entry != null ? entry.getValue() : -1;
        }
        return result;
    }
}
```

[437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)
```java
class Solution {
    //pattern  - dsf, dp; time - O(n^2), space - O(n)
    public int pathSum(TreeNode root, int sum) {
        return doCount(root, sum, false);
    }
    
    private int doCount(TreeNode node, int sum, boolean isCounting){
        if (node == null)
            return 0;

        int result = (node.val == sum ? 1 : 0)
                + doCount(node.right, sum - node.val, true)
                + doCount(node.left, sum - node.val, true);
        if (!isCounting)
            result += doCount(node.right, sum, false)  + doCount(node.left, sum, false);
        return result;
    }
}
```
```java
class Solution {
    //pattern  - dsf; time - O(n), space - O(n)
    public int pathSum(TreeNode root, int sum) {
        Map<Integer, Integer> sumPrefixes = new HashMap();
        sumPrefixes.put(0, 1); //if 'prefixSum - target' = 0 -> it's 1.
        return doCount(root, 0, sum, sumPrefixes);
    }
    
    private static int doCount(TreeNode node, int currSum, int target, Map<Integer, Integer> sumPrefixes) {
        if (node == null)
            return 0;

        currSum += node.val;
        int result = sumPrefixes.getOrDefault(currSum - target, 0); //found target sum by remove prefix path
        sumPrefixes.put(currSum, sumPrefixes.getOrDefault(currSum, 0) + 1); //save sum for current path (prefix)


        result += doCount(node.left, currSum, target, sumPrefixes)
                + doCount(node.right, currSum, target, sumPrefixes);
        sumPrefixes.put(currSum, sumPrefixes.get(currSum) - 1); //backtrack
        return result;
    }
}
```

[438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(R)
    public List<Integer> findAnagrams(String str, String pattern) {
        List<Integer> resultIndices = new ArrayList<>();
        if (str.length() < pattern.length())
            return resultIndices;

        int[] count = new int[26];
        for (int i = 0; i < pattern.length(); i++) {
            count[pattern.charAt(i) - 'a']++;
            count[str.charAt(i) - 'a']--;
        }
        if (isAllNull(count)) resultIndices.add(0);

        for (int i = pattern.length(); i < str.length(); i++){
            count[str.charAt(i - pattern.length()) - 'a']++;
            count[str.charAt(i) - 'a']--;
            if (isAllNull(count)) resultIndices.add(i - pattern.length() + 1);
        }

        return resultIndices;
    }
    
     private boolean isAllNull(int[] count) {
        for (int i = 0; i < count.length; i++)
            if (count[i] != 0) return false;
        return true;
    }
}
```

[442. Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
```java
class Solution {
    //pattern - cyclic sort, time - O(n), space - O(1)
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> duplicateNumbers = new ArrayList<>();
        int index = 0;
        while (index < nums.length) {
            int expectedIndex = nums[index] - 1;
            if (nums[index] != nums[expectedIndex])
                swap(index, expectedIndex, nums);
            else
                index++;
        }

        for (int i = 0; i < nums.length; i++)
            if (i != nums[i] - 1)
                duplicateNumbers.add(nums[i]);

        return duplicateNumbers;      
    }
    
    private static void swap(int i, int j, int[] nums) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

[457. Circular Array Loop](https://leetcode.com/problems/circular-array-loop/)
```java
class Solution {
    //pattern - fast and easy pointers, time - O(n), space - O(1)
    public boolean circularArrayLoop(int[] arr) {
        if (arr.length == 1)
            return false;

        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == 0)
                continue;

            int fast = stepNext(i, arr);
            int slow = i;
            while (arr[fast] * arr[i] > 0 && arr[stepNext(fast, arr)] * arr[i] > 0) {
                if (slow == fast) {
                    if (slow == stepNext(slow, arr))
                        break; //one element loop is not loop
                    else
                        return true;
                }
                fast = stepNext(fast, arr);
                fast = stepNext(fast, arr);
                slow = stepNext(slow, arr);
            }

            //loop checked - mark it by 0 values :
            int current = i;
            int prev = i;
            int prevValue = arr[i];
            while (arr[current]*prevValue > 0){
                prevValue = arr[current];
                prev = current;
                current = stepNext(current, arr);
                arr[prev] = 0;
            }
        }
        return false;
    }
    
     private int stepNext(int i, int[] arr) {
        int next = i + arr[i];
        if (next >= arr.length)
            next %= arr.length;
        else if (next < 0)
            next = next % arr.length + arr.length;
        return next;
    }
}
```

[567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(R)
    public boolean checkInclusion(String pattern, String str) {
        int[] counts = new int[26];
        if (pattern.length() > str.length()) return false;
        
        for (int i = 0; i < pattern.length(); i++) {
            counts[pattern.charAt(i) - 'a']++;
            counts[str.charAt(i) - 'a']--;
        }
        if (allZero(counts)) return true;

        for (int i = pattern.length(); i < str.length(); i++) {
            counts[str.charAt(i) - 'a']--;
            counts[str.charAt(i - pattern.length()) - 'a']++;
            if (allZero(counts)) return true;
        }

        return false;
    }
    
    private boolean allZero(int[] count) {
        for (int i = 0; i < 26; i++) {
            if (count[i] != 0) return false;
        }
        return true;
    }
}
```

[641. Design Circular Deque](https://leetcode.com/problems/design-circular-deque/)
```java
class MyCircularDeque {
        private final int[] items;
        private int tail;
        private int head;
        private int size;

        /** Initialize your data structure here. Set the size of the deque to be k. */
        public MyCircularDeque(int k) {
            this.items = new int[k];
            this.size = 0;
        }

        /** Adds an item at the front of Deque. Return true if the operation is successful. */
        public boolean insertFront(int item) {
            if (isFull())
                return false;

            if (head == items.length - 1) {
                items[head] = item;
                head = 0;
            } else {
                items[head++] = item;
            }
            size++;
            return true;
        }

        /** Adds an item at the rear of Deque. Return true if the operation is successful. */
        public boolean insertLast(int item) {
            if (isFull())
                return false;

            if (tail == 0) {
                tail = items.length - 1;
                items[tail] = item;
            } else {
                items[--tail] = item;
            }
            size++;
            return true;
        }

        /** Deletes an item from the front of Deque. Return true if the operation is successful. */
        public boolean deleteFront() {
            if (isEmpty())
                return false;

            if (head == 0){
                head = items.length - 1;
            } else {
                --head;
            }
            size--;
            return true;
        }

        /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
        public boolean deleteLast() {
            if (isEmpty())
                return false;

            if (tail == items.length - 1){
                tail = 0;
            } else {
                tail++;
            }
            size--;
            return true;
        }

        /** Get the front item from the deque. */
        public int getFront() {
            if (size == 0)
                return -1;
            
            if (head == 0){
                return items[items.length - 1];
            } else {
                return items[head - 1];
            }
        }

        /** Get the last item from the deque. */
        public int getRear() {
            if (size == 0)
                return -1;
            
            return items[tail];
        }

        /** Checks whether the circular deque is empty or not. */
        public boolean isEmpty() {
            return size == 0;
        }

        /** Checks whether the circular deque is full or not. */
        public boolean isFull() {
            return size == items.length;
        }

        private int size() {
            return size;
        }
    }
```

[713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)
```java
class Solution {
    //patterns - two pointers and sliding window, time - O(n), space - O(1)
    public int numSubarrayProductLessThanK(int[] arr, int target) {
        if (target <= 1) return 0;
        
        int result = 0;
        int product = 1, left = 0;
        for (int right = 0; right < arr.length; right++){
            product *= arr[right];
            while (product >= target && left < arr.length)
                product /= arr[left++];

            result += right - left + 1;                
        }
        return result;        
    }
}
```

[784. Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)
```java
class Solution {
    //pattern - subsets-dfs(cascade); time - O(n*2^n), space - O(n*2^n), total - (2^n)
    public List<String> letterCasePermutation(String str) {
        List<String> permutations = new ArrayList<>();
        permutations.add(str);

        for (int i = 0; i < str.length(); i++){
            if (!Character.isLetter(str.charAt(i)))
                continue;

            int size = permutations.size();
            for (int j = 0; j < size; j++){
                char[] c = permutations.get(j).toCharArray();
                if (Character.isUpperCase(c[i])){
                    c[i] = Character.toLowerCase(c[i]);
                } else {
                    c[i] = Character.toUpperCase(c[i]);
                }
                permutations.add(new String(c));
            }
        }

        return permutations;
    }
}
```

[904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(K(uniq symbols))
    public int totalFruit(int[] tree) {
        int max = Integer.MIN_VALUE;
        Map<Integer, Integer> map = new HashMap<>();
        for (int start = 0, end = 0; end < tree.length; end++){
            int i = tree[end];
            if (!map.containsKey(i))
                map.put(tree[end], 1);
            else
                map.put(tree[end], map.get(i) + 1);

            while (map.size() > 2) {
                int j = tree[start++];
                if (map.get(j) > 1)
                    map.put(j, map.get(j) - 1);
                else if (map.get(j) == 1)
                    map.remove(j);
            }
            max = Math.max(end - start + 1, max);
        }
        return max;
    }
}
```

[912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
```java
class Solution {
    public int[] sortArray(int[] array) {
        //merge sort
        sort(0, array.length - 1, array);
        return array;
    }
    
    private void sort(int start, int end, int[] array) {
        if (start >= end)
            return;

        int mid = start + (end - start) / 2;

        sort(start, mid, array);
        sort(mid + 1, end, array);
        mergeSortedArrays(start, mid, end, array);
    }

    private void mergeSortedArrays(int start, int mid, int end, int[] array){
        if (mid + 1 <= end && array[mid] <= array[mid + 1])
            return;

        int sortedArray[] = new int[end - start + 1];
        int i = start;
        int j = mid + 1;
        for (int k = 0; k < sortedArray.length; k++){
            if (i >= mid + 1){
                sortedArray[k] = array[j++];
            } else if (j >= end + 1){
                sortedArray[k] = array[i++];
            } else if (array[i] < array[j]){
                sortedArray[k] = array[i++];
            } else {
                sortedArray[k] = array[j++];
            }
        }
        System.arraycopy(sortedArray, 0, array, start, sortedArray.length);
    }
}
```

[986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)
```java
class Solution {
    //pattern - merge intervals, time - O(n+m), space - O(n+m)
    public int[][] intervalIntersection(int[][] arr1, int[][] arr2) {
        List<int[]> intervalsIntersection = new ArrayList<>();
        if (arr1.length == 0 || arr2.length == 0)
            return new int[0][];

        int index1 = 0;
        int index2 = 0;
        while (index1 < arr1.length && index2 < arr2.length) {
            if (arr1[index1][1] < arr2[index2][0]){
                index1++;
            } else if (arr2[index2][1] < arr1[index1][0]){
                index2++;
            } else {
                int[] intersection = new int[2];
                intersection[0] = Math.max(arr1[index1][0], arr2[index2][0]);
                intersection[1] = Math.min(arr1[index1][1], arr2[index2][1]);                        
                intervalsIntersection.add(intersection);
                if (arr1[index1][1] < arr2[index2][1])
                    index1++;
                else
                    index2++;
            }
        }

        return intervalsIntersection.toArray(new int[intervalsIntersection.size()][]);
    }
}
```

[1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(1)
    public int longestOnes(int[] A, int K) {
        int start = 0, end = 0;
        for (; end < A.length; end++){
            if (A[end] == 0)
                K--;

            if (K < 0 && A[start++] == 0)
                    K++;
        }
        return end - start;
    }
}
```
```java
class Solution {
    //pattern - sliding window, time - O(n), space - O(1)
    public int longestOnes(int[] A, int K) {
        int maxLength = 0;
        int count = 0;
        for (int start = 0, end = 0; end < A.length; end++){
            if (A[end] == 1)
                count++;

            if (end - start + 1 - count > K){
                if (A[start++] == 1)
                    count--;
            }
            maxLength = Math.max(end - start + 1, maxLength);
        }
        return maxLength;
    }
}
```

[1313. Decompress Run-Length Encoded List](https://leetcode.com/problems/decompress-run-length-encoded-list/)
```java
//time - O(n)
class Solution {
    public int[] decompressRLElist(int[] nums) {
        int count = 0;
        for (int i = 0; i < nums.length; i += 2)
            count += nums[i];
        int[] r = new int[count];

        for (int i = 1, k = 0; i < nums.length; i += 2) {
            Arrays.fill(r, k, k + nums[i - 1], nums[i]);
            k += nums[i - 1];
        }
        return r;
    }
}
```
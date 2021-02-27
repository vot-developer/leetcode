[1. Two Sum](https://leetcode.com/problems/two-sum/)
```java
class Solution {
    //time - O(n), space - O(n)
    public int[] twoSum(int[] arr, int targetSum) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < arr.length; i++){
            int target = targetSum - arr[i];
            if (map.containsKey(target) && i != map.get(target))
                return new int[]{i, map.get(target)};
            else
                map.put(arr[i], i);
        }

        return new int[] { -1, -1 };
    }
}
```

[26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(1)
    public int removeDuplicates(int[] arr) {
        int k = 1;
        for (int i = 1; i < arr.length; i++)
            if (arr[k - 1] != arr[i])
                arr[k++] = arr[i];
        return k;
    }
}
```

[27. Remove Element](https://leetcode.com/problems/remove-element/)
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(1)
    public int removeElement(int[] arr, int key) {
        int k = 0;
        for (int i = 0; i < arr.length; i++)
            if (arr[i] != key)
                arr[k++] = arr[i];
        return k;
    }
}
```
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(1)
    public int removeElement(int[] nums, int val) {
        int i = 0;
        int n = nums.length;
        while (i < n) {
            if (nums[i] == val) {
                nums[i] = nums[n - 1];
                n--;
            } else {
                i++;
            }
        }
        return n;
    }
}
```

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
```java
class Solution {
    //time - O(n), space - O(1)
    public double findMaxAverage(int[] nums, int k) {
        int sum = 0, max = 0;

        for (int i = 0; i < k; i++)
            sum += nums[i];

        max = sum;

        for (int i = k; i < nums.length; i++){
            sum+= nums[i] - nums[i-k];
            max = Math.max(sum, max);
        }
        return (max + 0.0)/k;
    }
}
```

[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {    
        if (n == 0)
            return;

        if (m == 0)
            System.arraycopy(nums2, 0, nums1, 0, m+n);

        int[] result = new int[m+n];
        int i = 0, j = 0;
        for (int k = 0; k < m+n; k++){
            if (j < n && (i == m || nums2[j] <= nums1[i])){
                result[k] = nums2[j++];
            } else {
                result[k] = nums1[i++];
            }
        }
        System.arraycopy(result, 0, nums1, 0, m+n);
    }
}
```

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
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
    public int maxDepth(TreeNode root) {
        if (root == null)
            return 0;

        int count = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            count++;
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
        }
        return count;
    }
}
```

[107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> result = new LinkedList<>();
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
            result.add(0, level);
        }
        return result;
    }
}
```

[111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
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
    public int minDepth(TreeNode root) {
        if (root == null)
            return 0;

        int count = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            count++;
            int size = queue.size();
            List<Integer> level = new ArrayList<>(size);
            for (int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                if (node.right == null && node.left == null)
                    return count;
                level.add(node.val);
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
            }
        }
        return count;
    }
}
```

[112. Path Sum](https://leetcode.com/problems/path-sum/)
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
    //patterns - dp and dfs, time - O(n), space - O(n)
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null)
            return false;

        if (root.val == sum && root.left == null && root.right == null)
            return true;

        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);        
    }
}
```

[141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    //pattern - fast slow pointers, time - O(n), space - O(1)
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (slow == fast)
                return true; // found the cycle
        }
        return false;
    }
}
```

[142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    //pattern - fast and slow pointers, time - O(n), space - O(1)
    public ListNode detectCycle(ListNode head) {
        int k = getCycleLength(head);
        if (k == 0)
            return null;

        ListNode pointer1 = head;
        ListNode pointer2 = head;
        for (int i = 0; i < k; i++)
            pointer1 = pointer1.next;

        while (pointer1 != pointer2) {
            pointer1 = pointer1.next;
            pointer2 = pointer2.next;
        }
        return pointer1;
    }
    
    private int getCycleLength(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                int count = 0;
                ListNode counter = slow;
                do {
                    counter = counter.next;
                    count++;
                } while (counter != slow);
                return count;
            }
        }
        return 0;
    }
}
```

[155. Min Stack](https://leetcode.com/problems/min-stack/)
```java
class MinStack {
private Node top;

    private class Node {
        private Node next;
        private int value;
        private int min;

        public Node(Node next, int value, int min) {
            this.next = next;
            this.value = value;
            this.min = min;
        }
    }

    public void push(int value) {
        if (top == null){
            top = new Node(null, value, value);
        } else {
            Node prevTop = top;
            top = new Node(prevTop, value, Math.min(prevTop.min, value));
        }
    }

    public int pop() {
        if (top == null) {
            return 0;
        }

        Node prevTop = top;
        top = prevTop.next;
        return prevTop.value;
    }

    public int top() {
        if (top == null) {
            return 0;
        } else {
            return top.value;
        }
    }

    public int getMin() {
        if (top == null) {
            return 0;
        } else {
            return top.min;
        }
    }
}
```
[167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(1)
    public int[] twoSum(int[] arr, int targetSum) {
        int l = 0, r = arr.length - 1;

        while (l <= r){
            if (arr[l] + arr[r] > targetSum)
                r--;
            else if (arr[l] + arr[r] < targetSum)
                l++;
            else
                return new int[] { l + 1, r + 1 };
        }

        return new int[] { -1, -1 };
    }
}
```

[202. Happy Number](https://leetcode.com/problems/happy-number/)
```java
class Solution {
    //pattern - fast and slow pointers, time - O(log n), space - O(1)
    public boolean isHappy(int num) {
        int slow = num;
        int fast = num;
        do {
            slow = next(slow);
            fast = next(next(fast));
        } while (slow != fast);

        return slow == 1;
    }
    
    private int next(int num){
        int sum = 0;
        while(num != 0){
            sum += Math.pow(num % 10, 2);
            num /= 10;
        }
        return sum;
    }
}
```

[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
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
    //pattern - reverse linkedlist, time - O(n), space - O(1)
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode next;
        while (head != null){
            next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}
```

[232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)
```java
class MyQueue {

    private Stack<Integer> pushStack = new Stack<>();
    private Stack<Integer> peekStack = new Stack<>();

    /**
     * Initialize your data structure here.
     */
    public MyQueue() {

    }

   /**
     * Push element x to the back of queue.
     */
    public void push(int x) {
        pushStack.push(x);
    }

    /**
     * Removes the element from in front of queue and returns that element.
     */
    public int pop() {
        if (pushStack.empty() && peekStack.empty())
            return -1;

        if (peekStack.empty()){
            while (!pushStack.empty()){
                peekStack.push(pushStack.pop());
            }
        }

        return peekStack.pop();
    }

    /**
     * Get the front element.
     */
    public int peek() {
        if (pushStack.empty() && peekStack.empty())
            return -1;

        if (peekStack.empty()) {
            while (!pushStack.empty()) {
                peekStack.push(pushStack.pop());
            }
        }

        return peekStack.peek();
    }

    /**
     * Returns whether the queue is empty.
     */
    public boolean empty() {
        return pushStack.empty() && peekStack.empty();
    }
}
```

[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
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
    public boolean isPalindrome(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }

        ListNode checker = head;
        ListNode reverseChecker = makeReverse(slow);
        while (checker != null && reverseChecker != null) {
            if (checker.val != reverseChecker.val)
                return false;
            checker = checker.next;
            reverseChecker = reverseChecker.next;
        }

        return true;
    }
    
    private ListNode makeReverse(ListNode head) {
        ListNode prev = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}
```

[268. Missing Number](https://leetcode.com/problems/missing-number/)
```java
class Solution {
    //pattern - xor; time - O(n), space - O(1)
    public int missingNumber(int[] nums) {
        int sum = 0;
        for (int num : nums)
            sum ^= num;        
        
        for (int i = 0; i <= nums.length; i++)
            sum ^= i;
        
        return sum;
    }    
}
```
```java
class Solution {
    //pattern - cyclic sort, time - O(n), space - O(1)
    public int missingNumber(int[] nums) {
        int index = 0;
        while (index < nums.length){
            int expectedIndex = nums[index];
            if (expectedIndex < nums.length && expectedIndex != index)
                swap(index, expectedIndex, nums);
            else
                index++;
        }
        for (int i = 0; i < nums.length; i++)
            if (i != nums[i])
                return i;

        return nums.length;        
    }
    
    private void swap(int i, int j, int[] nums) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

[344. Reverse String](https://leetcode.com/problems/reverse-string/)
```java
class Solution {
    //time - O(n), space - O(1)
    public void reverseString(char[] s) {
        int k = s.length - 1;
        int size = s.length / 2;
        for (int i = 0; i < size; i++) {
            char tmp = s[i];
            s[i] = s[k];
            s[k--] = tmp;
        }
    }
}
```

[349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> intersections = new HashSet<>();

        Set<Integer> set = new HashSet<>();
        for (int i : nums2){
            set.add(i);
        }

        for (int i : nums1){
            if (set.contains(i)){
                intersections.add(i);
            }
        }

        return intersections.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

[350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length == 0 || nums2.length == 0)
            return new int[0];

        Arrays.sort(nums1);
        Arrays.sort(nums2);

        int index1 = 0, index2 = 0, resultIndex = 0;
        int value1, value2;
        while (index1 < nums1.length && index2 < nums2.length){
            value1 = nums1[index1];
            value2 = nums2[index2];

            if (value1 < value2){
                index1++;
            } else if (value2 < value1) {
                index2++;
            } else {
                nums1[resultIndex] = value1;
                resultIndex++;
                index1++;
                index2++;
            }
        }

        return Arrays.copyOf(nums1, resultIndex);
    }
}
```

[461. Hamming Distance](https://leetcode.com/problems/hamming-distance/)
```java
class Solution {
    public int hammingDistance(int x, int y) {
        int r = x ^ y;
        int count = 0;
        while (r > 0){
            r &= r - 1;
            count++;
        }
        return count;
    }
}
```

[448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
```java
class Solution {
    //pattern - cyclic sort, time - O(n), space - O(1)
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> missingNumbers = new ArrayList<>();
        int index = 0;
        while (index < nums.length) {
            int expectedIndex = nums[index] - 1;
            if (nums[expectedIndex] != nums[index])
                swap(index, expectedIndex, nums);
            else
                index++;
        }

        for (int i = 0; i < nums.length; i++)
            if (i != nums[i] - 1)
                missingNumbers.add(i + 1);

        return missingNumbers;        
    }
    
    private static void swap(int i, int j, int[] nums) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

[509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)
```java
//time - 0(n), space - O(n) (stack)
class Solution {
    public int fib(int n) {
        if (n < 2)
            return n;
        
        int[] memoize = new int[n - 1];
        return doF(n, memoize);
    }
    
    private int doF(int n, int[] memorize){
        if (n < 2)
            return n;

        if (memorize[n - 2] != 0)
            return memorize[n - 2];

        memorize[n - 2] = doF(n - 1, memorize) + doF(n - 2, memorize);
        return memorize[n - 2];
    }
}
```

[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
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
    private int max = 0;
    //pattern - dfs, time - O(n), space - O(n)
    public int diameterOfBinaryTree(TreeNode root) {
        max = 1;
        doFind(root);
        return max - 1;
    }
    
    private int doFind(TreeNode root) {
        if (root == null)
            return 0;

        int left = doFind(root.left);
        int right = doFind(root.right);
        max = Math.max(left + right + 1, max);
        return Math.max(left, right) + 1;
    }
}
```

[581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(1)
    public int findUnsortedSubarray(int[] arr) {
        if (arr.length <= 1)
            return 0;

        int l = 0;
        int r = arr.length - 1;
        while(l < r && arr[l + 1] >= arr[l])
            l++;
        while (r > l && arr[r - 1] <= arr[r])
            r--;

        if (r == l)
            return 0;

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for (int i = l; i <= r; i++){
            min = Math.min(min, arr[i]);
            max = Math.max(max, arr[i]);
        }

        while (l > 0 && arr[l - 1] > min)
            l--;
        while (r < arr.length - 1 && arr[r + 1] < max)
            r++;

        return r - l + 1;
    }
}
```

[617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)
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
    //time - O(n), space - O(1)
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null)
            return null;
        
        if (t1 == null && t2 != null)
            return t2;
        
        if (t2 == null && t1 != null)
            return t1;
        
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;  
    }    
}
```

[637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)
```java
class Solution {
    //pattern - bfs, time - O(n), space - O(n)
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()){
            int size = queue.size();
            double sum = 0;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                sum += node.val;
                if (node.left != null)
                    queue.add(node.left);
                if (node.right != null)
                    queue.add(node.right);
            }
            result.add(sum / size);
        }
        return result;
    }
}
```

[704. Binary Search](https://leetcode.com/problems/binary-search/)
```java
class Solution {
    //pattern - binary-search, time - O(log n), space - O(1)
    public int search(int[] nums, int target) {
        int start = 0, end = nums.length - 1;
        while (start <= end) {
            if (start == end){
                if (nums[start] == target)
                    return start;
                else
                    return -1;
            }

            int mid = start + (end - start) / 2;
            if (nums[mid] == target)
                return mid;
            if (nums[mid] < target)
                start = mid + 1;
            else
                end = mid - 1;
        }
        
        return -1;
    }
}
```

[709. To Lower Case](https://leetcode.com/problems/to-lower-case/)
```java
class Solution {
    public String toLowerCase(String str) {
        char[] original = str.toCharArray();
        int value;
        for (int i = 0; i < original.length; i++){
            value = original[i];
            if (value > 64 && value < 91){
                original[i] = (char) (value + 32);
            }
        }
        return new String(original);
    }
}
```

[728. Self Dividing Numbers](https://leetcode.com/problems/self-dividing-numbers/)
```java
class Solution {
    //time - O(n), space - O(n)
    public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> result = new ArrayList<>();
        if (left > right)
            return result;
        
        for (int i = left; i <= right; i++){
            if (i % 10 == 0)
                continue;
            
            int k = i;
            while (k > 0){
                int j = k % 10;
                if (j % 10 == 0)
                    break;                
                if (i % j != 0)
                    break;                
                k = k / 10;
            }
            
            if (k == 0)
                result.add(i);
        }
        
        return result;
    }
}
```

[744. Find Smallest Letter Greater Than Target](https://leetcode.com/problems/find-smallest-letter-greater-than-target/)
```java
class Solution {
    //pattern - binary-search, time - O(log n), space - O(1)
    public char nextGreatestLetter(char[] letters, char target) {
        int start = 0;
        int end = letters.length - 1;
        if (target >= letters[end])
            return letters[start];

        while (start <= end) {
            if (start == end){
                if (target != letters[start])
                    return letters[start];
                else 
                    return letters[start + 1];
            }
            
            int mid = start + (end - start) / 2;
            if (target < letters[mid])
                end = mid;
            else // target >= letters[mid]
                start = mid + 1;
        }

        return letters[0];        
    }
}
```

[771. Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)
```java
class Solution {
public static int numJewelsInStones(String J, String S) {
        int count = 0;
        for (char s : S.toCharArray()){
            if (J.indexOf(s) != -1){
                count++;
            }
        }
        return count;
    }
}
```

[796. Rotate String](https://leetcode.com/problems/rotate-string/)
1) time - O(n^2), space - O(n)
```java
class Solution {
    public boolean rotateString(String A, String B) {
        if (A.length() != B.length()) return false;
        if (A.length() == 0) return true;
        if (A.equals(B)) return true;

        for (int i = 0; i < A.length(); i++) {
            if (A.charAt(i) == B.charAt(0)) {
                String rotatedA = createRotate(A, i);
                if (rotatedA.equals(B)) return true;
            }
        }
        return false;
    }
    
    private String createRotate(String a, int i) {
        char[] r = new char[a.length()];
        System.arraycopy(a.toCharArray(), i, r, 0, a.length() - i);
        System.arraycopy(a.toCharArray(), 0, r, a.length() - i, i);
        return new String(r);
    }
}
```
2) Knuth-Morris-Pratt
time - O(n) (preparation - O(n*R)), space - O(n*R)
```java
class Solution {
    public boolean rotateString(String A, String B) {
        if (A.length() != B.length()) return false;
        if (A.length() == 0) return true;
        if (A.equals(B)) return true;

        KMP kmp = new KMP(A);
        return kmp.search(B + B) >= 0;
    }
    
static class KMP {
    private static final int R = 26;
    private final String pattern;
    private final int[][] states;

    public KMP(String pattern) {
        this.pattern = pattern;
        this.states = new int[R][pattern.length()]; //Do only for lowercase english alphabet letters

        states[index(pattern.charAt(0))][0] = 1;
        int x = 0;
        for (char i = 1; i < pattern.length(); i++) {
            for (int j = 0; j < R; j++)
                states[j][i] = states[j][x];
            states[index(pattern.charAt(i))][i] = i + 1;
            x = states[index(pattern.charAt(i))][x];
        }
    }

    public int search(String s){
        int j = 0;
        for (int i = 0; i < s.length(); i++){
            j = states[index(s.charAt(i))][j];
            if (j == pattern.length())
                return i - pattern.length() + 1;
        }
        return -1;
    }

    private int index(char c) {
        if (c < 97 || c > 122) throw new IllegalArgumentException("support only lowercase english alphabet letters");
        return c - 97;
    }
}
}
```  
   

[797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)
```java
/*
        time - O(2^N)
        space - O(N)
 */
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> path = new ArrayList<>(); //we will change symbols here to reach results
        path.add(0); //first symbol
        dfs(graph, 0, path, result);
        return result;
    }
    
    private void dfs(int[][] graph, int n, List<Integer> path, List<List<Integer>> result){
        if (n == graph.length - 1){ //last symbol -> save result
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i : graph[n]){
            path.add(i); //add n-th symbol
            dfs(graph, i, path, result);
            path.remove(path.size() - 1); //remove n-th symbol for continue with next i 
        }
    }
}
```

[804. Unique Morse Code Words](https://leetcode.com/problems/unique-morse-code-words/)
```java
//time - O(n), space - O(uniq words)
class Solution {
    public int uniqueMorseRepresentations(String[] words) {
        String[] codes = new String[]{".-","-...","-.-.","-..",".","..-.","--.",
                         "....","..",".---","-.-",".-..","--","-.",
                         "---",".--.","--.-",".-.","...","-","..-",
                         "...-",".--","-..-","-.--","--.."};

        Set<String> result = new HashSet();
        for (String word: words) {
            StringBuilder code = new StringBuilder();
            for (char c: word.toCharArray())
                code.append(codes[c - 'a']);
            result.add(code.toString());
        }

        return result.size();
    }
}
```
```java
//time - O(n), space - O(all words + links)
class Solution {
    private String[] codes = new String[]{".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."};

    public int uniqueMorseRepresentations(String[] words) {
        int count = 0;
        Trie trie = new Trie();
        for (String s : words)
            if (trie.add(s))
                count++;
        return count;
    }

    private class Trie {
        Node root = new Node();

        /*
        Return true if word is uniq
         */
        boolean add(String string) {
            Node node = root;
            for (char c : string.toCharArray()) {
                String code = codes[c - 'a'];
                for (char s : code.toCharArray())
                    node = add(node, s);
            }
            boolean result = false;
            if (!node.isEnd)
                result = true;
            node.isEnd = true;
            return result;
        }

        private Node add(Node root, char c) {
            Node node;
            if (c == '.') {
                node = root.next[0];
                if (node == null) {
                    node = new Node();
                    root.next[0] = node;
                }
            } else {
                node = root.next[1];
                if (node == null) {
                    node = new Node();
                    root.next[1] = node;
                }
            }
            return node;
        }
    }

    private class Node {
        Node[] next = new Node[2];
        boolean isEnd;
    }
}
```

[844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)
```java
class Solution {
    //pattern - two pointers, time - O(n1 + n2), space - O(1)
    public boolean backspaceCompare(String str1, String str2) {
        int end1 = str1.length() - 1;
        int end2 = str2.length() - 1;
        while (end1 >= 0 || end2 >= 0) {
            end1 = getComparableIndex(str1, end1);
            end2 = getComparableIndex(str2, end2);
            if (end1 < 0 || end2 < 0)
                break;
            if (str1.charAt(end1--) != str2.charAt(end2--))
                return false;
        }

        if (end1 != end2)
            return false;

        return true;        
    }
    
    private static int getComparableIndex(String s, int i) {
        int shift = 0;
        while (i >= 0 && (s.charAt(i) == '#' || shift > 0)) {
            if (s.charAt(i) == '#')
                shift++;
            else if (shift > 0)
                shift--;
            i--;
        }
        return i;
    }
}
```

[876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
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
    public ListNode middleNode(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

[938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/)
```java
class Solution {
    public int rangeSumBST(TreeNode root, int L, int R) {
        if (root == null)
            return 0;

        if (root.val < L) {
            return rangeSumBST(root.right, L, R);
        } else if (root.val > R) {
            return rangeSumBST(root.left, L, R);
        } else {
            return root.val
                    + rangeSumBST(root.left, L, R)
                    + rangeSumBST(root.right, L, R);
        }
    }
}
```
[977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)
```java
class Solution {
    //pattern - two pointers, time - O(n), space - O(n)
    public int[] sortedSquares(int[] arr) {
        int[] squares = new int[arr.length];
        int l = 0, r = arr.length - 1, k = arr.length - 1;
        while (k >= 0){
            if (Math.abs(arr[l]) > Math.abs(arr[r])){
                squares[k--] = (int) Math.pow(arr[l++], 2);
            } else {
                squares[k--] = (int) Math.pow(arr[r--], 2);
            }
        }
        return squares;
    }
}
```

[1108. Defanging an IP Address](https://leetcode.com/problems/defanging-an-ip-address/)
```java
class Solution {
    public String defangIPaddr(String address) {
        char[] result = new char[address.length() + 6];
        char[] original = address.toCharArray();
        int resultIndex = 0;
        for (int i = 0; i < original.length; i++){
            if (original[i] == '.'){
                result[resultIndex++] = '[';
                result[resultIndex++] = '.';
                result[resultIndex++] = ']';
            } else {
                result[resultIndex++] = original[i];
            }
        }
        return new String(result);
    }
}
```

[1221. Split a String in Balanced Strings](https://leetcode.com/problems/split-a-string-in-balanced-strings/)
```java
class Solution {
    public int balancedStringSplit(String s) {
        int count = 0, balance = 0;
        for (int i = 0; i < s.length(); i++) {
            balance += count(s.charAt(i));
            if (balance == 0)
                count++;
        }
        return count;
    }
    
    private int count(char c) {
        return c == 'R' ? 1 : -1;
    }
}
```

[1281. Subtract the Product and Sum of Digits of an Integer](https://leetcode.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer/)
```java
class Solution {
    public int subtractProductAndSum(int n) {
        int sum = 0, product = 1;
        do {
            int i = n % 10; 
            sum += i;
            product *= i;
            n = n/10;
        } while (n > 0);
        return product - sum;
    }
}
```

[1342. Number of Steps to Reduce a Number to Zero](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero/)
```java
class Solution {
    public int numberOfSteps (int num) {
        if (num == 0) return 0;

        int count = 0;
        while (num > 0) {
            if (num == 1) return ++count;

            if ((num & 1) == 0) { // even
                count++;
            }
            else {
                count += 2; // do both divide and subtract on one shift left
            }
            num >>= 1;
        }
        return count;
    }
}
```

[1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/)
```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] count = new int[102];
        for (int v : nums) {
            count[v + 1]++;
        }

        for (int i = 1; i < count.length; i++)
            count[i] += count[i - 1];

        for (int i = 0; i < nums.length; i++)
            nums[i] = count[nums[i]];

        return nums;
    }
}
```

[1431. Kids With the Greatest Number of Candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/)
```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        List<Boolean> result = new ArrayList<>(candies.length);
        int max = 0;
        for (int i = 0; i < candies.length; i++){
            if (candies[i] > max)
                max = candies[i];
        }

        for (int i = 0; i < candies.length; i++){
            result.add(candies[i] + extraCandies >= max);
        }
        return result;
    }
}
```

[1470. Shuffle the Array](https://leetcode.com/problems/shuffle-the-array/)
```java
class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] result = new int[nums.length];
        for (int i = 0; i < n; i++){
            result[2*i] = nums[i];
            result[2*i + 1] = nums[n + i];
        }
        return result;
    }
}
```

[1480. Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/)
```java
class Solution {
    public int[] runningSum(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            nums[i] += nums[i - 1];
        }
        return nums;
    }
}
```

[1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)
1st approach - space O(1), time - O(n*log n)
```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
       if (nums == null || nums.length <= 1) return 0;

        Arrays.sort(nums);
        int count = 0;

        for (int i = 1; i < nums.length; i++){
            if (nums[i - 1] == nums[i]){
                int number = 1;
                while (i < nums.length && nums[i] == nums[i - 1]){
                    number++; i++;
                }
                i--;
                count += sum(number);
            }
        }
        return count;
    }
    
    private int sum(int i){
        if (i <= 1) return 0;
        return (i - 1) + sum(i - 1);
    }
}
```
2nd approach - space O(n), time - O(n)
```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        if (nums == null || nums.length <= 1) return 0;

        int[] aux = new int[101];

        int count = 0;
        for (int i : nums) {
            count += aux[i]++;
        }
        return count;
    }    
}
```
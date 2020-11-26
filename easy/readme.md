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
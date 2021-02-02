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
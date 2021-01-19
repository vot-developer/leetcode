[15. 3Sum.](https://leetcode.com/problems/3sum/)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> solutions = new HashSet<>();
        Arrays.sort(nums);

        if (nums.length < 3)
            return new ArrayList<>();

        for (int i = 0; i < nums.length; i++) {
            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    solutions.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    left++;
                }
            }
        }

        return new ArrayList<>(solutions);
    }
}
```

[33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
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

[46. Permutations](https://leetcode.com/problems/permutations/)
```java
class Solution {
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

[75. Sort Colors](https://leetcode.com/problems/sort-colors/)
```java
class Solution {
    public void sortColors(int[] array) {
        int leftBorder = 0;
        int rightBorder = array.length - 1;

        int i = 0;
        while (i <= rightBorder){
            if (array[i] == 0){
                swap(array, i, leftBorder);
                leftBorder++;
                i++;
            } else if (array[i] == 2){
                swap(array, i, rightBorder);
                rightBorder--;
            } else {
                i++;
            }            
        }
    }
    
    private void swap(int[] array, int indexOne, int indexTwo) {
        int temp = array[indexOne];
        array[indexOne] = array[indexTwo];
        array[indexTwo] = temp;
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
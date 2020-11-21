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

#### [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)

```
class MyCircularDeque {
    private int size = 0;
    private int count = 0;
    // guard node: head, tail
    DoubleLinkedList head = null;
    DoubleLinkedList tail = null;

    /** Initialize your data structure here. Set the size of the deque to be k. */
    public MyCircularDeque(int k) {
        size = k;
        head = new DoubleLinkedList(-1);
        tail = new DoubleLinkedList(-1);
        head.next = tail;
        tail.pre = head;
    }
    
    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    public boolean insertFront(int value) {
        if (count == size) {
            return false;
        }
        DoubleLinkedList newNode = new DoubleLinkedList(value);
        DoubleLinkedList temp = head.next;
        newNode.next = temp;
        temp.pre = newNode;
        newNode.pre = head;
        head.next = newNode;
        ++count;
        return true;
    }
    
    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    public boolean insertLast(int value) {
        if (count == size) {
            return false;
        }
        DoubleLinkedList newNode = new DoubleLinkedList(value);
        DoubleLinkedList temp = tail.pre;
        newNode.pre = temp;
        temp.next = newNode;
        newNode.next = tail;
        tail.pre = newNode;
        ++count;
        return true;
    }
    
    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    public boolean deleteFront() {
        if (count == 0) {
            return false;
        }
        DoubleLinkedList nodeToDelete = head.next;
        head.next = nodeToDelete.next;
        nodeToDelete.next.pre = head;
        --count;
        return true;
    }
    
    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    public boolean deleteLast() {
        if (count == 0) {
            return false;
        }
        DoubleLinkedList nodeToDelete = tail.pre;
        tail.pre = nodeToDelete.pre;
        nodeToDelete.pre.next = tail;
        --count;
        return true;
    }
    
    /** Get the front item from the deque. */
    public int getFront() {
        if (count == 0) {
            return -1;
        }
        return head.next.val;
    }
    
    /** Get the last item from the deque. */
    public int getRear() {
        if (count == 0) {
            return -1;
        }
        return tail.pre.val;
    }
    
    /** Checks whether the circular deque is empty or not. */
    public boolean isEmpty() {
        return count == 0;
    }
    
    /** Checks whether the circular deque is full or not. */
    public boolean isFull() {
        return count == size;
    }
}

class DoubleLinkedList {
    public DoubleLinkedList pre;
    public DoubleLinkedList next;
    public int val;
    public DoubleLinkedList(int val) {
        this.val = val;
    }
}
//runtime:8 ms
//memory:41.6 MB
```

```
class MyCircularDeque {
    int capacity = 0;
    int front = 0;
    int rear = 0;
    int[] a = null;

    /** Initialize your data structure here. Set the size of the deque to be k. */
    public MyCircularDeque(int k) {
        capacity = k + 1;
        a = new int[capacity];
    }
    
    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    public boolean insertFront(int value) {
        if (isFull()) {
            return false;
        }
        front = (front - 1 + capacity) % capacity;
        a[front] = value;
        return true;
    }
    
    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    public boolean insertLast(int value) {
        if (isFull()) {
            return false;
        }
        a[rear] = value;
        rear = (rear + 1) % capacity;
        return true;
    }
    
    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    public boolean deleteFront() {
        if (isEmpty()) {
            return false;
        }
        front = (front + 1) % capacity;
        return true;
    }
    
    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    public boolean deleteLast() {
        if (isEmpty()) {
            return false;
        }
        rear = (rear - 1 + capacity) % capacity;
        return true;

    }
    
    /** Get the front item from the deque. */
    public int getFront() {
        if (isEmpty()) {
            return -1;
        }
        return a[front];
    }
    
    /** Get the last item from the deque. */
    public int getRear() {
        if (isEmpty()) {
            return -1;
        }
        return a[(rear - 1 + capacity) % capacity];
    }
    
    /** Checks whether the circular deque is empty or not. */
    public boolean isEmpty() {
        return front == rear;
    }
    
    /** Checks whether the circular deque is full or not. */
    public boolean isFull() {
        return (rear + 1) % capacity == front;
    }
}

//runtime:9 ms
//memory:41.6 MB
```

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return null;
        }
        // convert nums to hashmap
        HashMap<Integer, Integer> hashmap = convertToHashMap(nums);

        // find value by target-nums[i]
        for (int i = 0; i < nums.length; i++) {
            Integer minusIdx = hashmap.get(target - nums[i]);
            if (minusIdx != null && !minusIdx.equals(i)) {
                return new int[] {i, minusIdx};
            }
        }
        return null;
    }

    private HashMap<Integer, Integer> convertToHashMap(int[] nums) {
        HashMap<Integer, Integer> hashmap = new HashMap(nums.length);
        for (int i = 0; i < nums.length; i++) {
            hashmap.put(nums[i], i);
        }
        return hashmap;
    }
}
//runtime:4 ms O(n)
//memory:41.7 MB O(N)
```

#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if (isEmpty(nums1) || isEmpty(nums2)) {
            return;
        }
        // p1 for nums1, p2 for nums2; both of them started at the end
        int p1 = m - 1;
        int p2 = n - 1;
        int cur = m + n - 1;
        while (p1 != -1 && p2 != -1) {
            nums1[cur--] = nums1[p1] > nums2[p2] ? nums1[p1--] : nums2[p2--];
        }
        if (p1 == -1 && p2 != -1) {
            while (p2 != -1) {
                nums1[cur--] = nums2[p2--];
            }
        }
    }
    private boolean isEmpty(int[] nums) {
        return nums == null || nums.length == 0;
    }
}
//runtime:0 ms O(n+m)
//memory:38.7 MB O(1)
```

#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode preHead = new ListNode(-1);
        ListNode prev = preHead;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }
        prev.next = l1 == null ? l2 : l1;
        return preHead.next;
    }
}
//runtime:1 ms O(n+m)
//memory:38.2 MB O(1)
```

```
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
//runtime:0 ms O(n+m)
//memory:38 MB O(n+m)
```

#### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)

Solution1.  Time:O(n)  Mem:O(1)

```
class Solution {
    public void rotate(int[] nums, int k) {
        k = k%nums.length;
        int count = 0;
        for (int start = 0; count < nums.length; start++) {
            int curIdx = start;
            int prev = nums[curIdx];
            do {
                int next = (curIdx + k) % nums.length;
                int temp = nums[next];
                nums[next] = prev;
                curIdx = next;
                prev = temp;
                ++count;
            } while (curIdx != start);
        }
    }
}
```

Solution2. Time: O(n) Mem:O(1)

```
class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }
    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[end];
            nums[end] = nums[start];
            nums[start] = temp;
            ++start;
            --end;
        }
    }
}
```

Solution3. Time: O(k*n) Mem:O(1)

```
class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        for (int i = 0; i < k; i++) {
            int prev = nums[nums.length - 1];
            for (int j = 0; j < nums.length; j++) {
                int temp = nums[j];
                nums[j] = prev;
                prev = temp;
            }
        }        
    }
}
```

##### 题目：

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

**示例 1:**

```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

说明:

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
要求使用空间复杂度为 O(1) 的原地算法。



#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

```
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int k = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != nums[k]) {
                nums[++k] = nums[i]; 
            }
        }
        return k+1;
    }
}
```

执行结果：通过

执行用时 :1 ms, 在所有 Java 提交中击败了99.89%的用户

内存消耗 :41.6 MB, 在所有 Java 提交中击败了8.62%的用户

##### 题目：

难度:简单

给定一个排序数组，你需要在**[原地](http://baike.baidu.com/item/原地算法)**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**并在使用 O(1) 额外空间的条件下完成。

**示例 1:**

```
给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
```

**示例 2:**

```
给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
```

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**“引用”**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```


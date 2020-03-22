

#### [945. 使数组唯一的最小增量](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/)

```
import java.util.Arrays;

class Solution {
    public int minIncrementForUnique(int[] A) {
        Arrays.sort(A);
        int move = 0;
        for (int i = 1; i < A.length; i++) {
            if (A[i] <= A[i - 1]) {
                int t = A[i];
                A[i] = A[i - 1] + 1;
                move += A[i] - t;
            }
        }
        return move;
    }
}
//runtime:21 ms O(nlogn)
//memory:46.8 MB O(1)
```

```
class Solution {
    int[] pos = new int [80000];
    public int minIncrementForUnique(int[] A) {
        Arrays.fill(pos, -1); 
        int move = 0;
        for (int a: A) {
            int b = findPos(a);
            move += b - a;
        }
        return move;
    }

    private int findPos(int a) {
        int b = pos[a];
        if (b == -1) {
            pos[a] = a;
            return a;
        }
        b = findPos(b + 1);
        pos[a] = b;
        return b;
    }
}

//runtime:15 ms O(N)
//memory:44.4 MB O(2N)
```

#### [365. 水壶问题](https://leetcode-cn.com/problems/water-and-jug-problem/)

```
class Solution {
    public boolean canMeasureWater(int x, int y, int z) {

        State initState = new State(0, 0);
        Queue<State> queue = new LinkedList<>();
        Set<State> visitedStates = new HashSet<>();
        visitedStates.add(initState);

        queue.offer(initState);
        while (!queue.isEmpty()) {
            State head = queue.poll();
            int curX = head.getX();
            int curY = head.getY();
            
            if (curX == z || curY == z || curX + curY == z) {
                return true;
            }

            List<State> nexStates = getNextStates(curX, curY, x, y);
            
            // bfs
            for (State nextState : nexStates) {
                if (!visitedStates.contains(nextState)) {
                    queue.offer(nextState);
                    visitedStates.add(nextState);
                }
            }
        }
        return false;
    }

    private List<State> getNextStates(int curX, int curY, int x, int y) {
        List<State> nextStates = new ArrayList<>(8);
        // fill full
        State state1 = new State(x, curY);
        State state2 = new State(curX, y);
        // empty
        State state3 = new State(0, curY);
        State state4 = new State(curX, 0);
        // A->B to make B full and A still has some
        State state5 = new State(curX - (y - curY), y);
        // A->B to make A empty while B is still not full
        State state6 = new State(0, curY + curX);
        // B->A to make A full
        State state7 = new State(x, curY - (x - curX));
        // B->A to make B empty
        State state8 = new State(curX + curY, 0);
        
        // A is not full
        if (curX < x) {
            nextStates.add(state1);
        }
        if (curY < y) {
            nextStates.add(state2);
        }
        
        // A is not empty
        if (curX > 0) {
            nextStates.add(state3);
        }
        if (curY > 0) {
            nextStates.add(state4);
        }
        
        if (curX - (y - curY) > 0) {
            nextStates.add(state5);
        }

        if (curY - (x - curX) > 0) {
            nextStates.add(state7);
        }
        
        if (curY + curX < y) {
            nextStates.add(state6);
        }

        if (curY + curX < x) {
            nextStates.add(state8);
        }
        return nextStates;
    }

    private class State {
        private int x;
        private int y;

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;

            State state = (State) o;

            if (x != state.x) return false;
            return y == state.y;
        }

        @Override
        public int hashCode() {
            int result = x;
            result = 31 * result + y;
            return result;
        }

        public State(int x, int y) {
            this.x = x;
            this.y = y;
        }

        public int getX() {
            return x;
        }

        public void setX(int x) {
            this.x = x;
        }

        public int getY() {
            return y;
        }

        public void setY(int y) {
            this.y = y;
        }
    }
}
//runtime:583 ms O(XY)
//memory:108.8 MB O(XY)
```

#### [面试题40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (k == 0 || arr.length == 0) {
            return new int[0];
        }
        return quickSort(arr, 0, arr.length - 1, k - 1);
    }

    private int[] quickSort(int[] arr, int l, int r, int k) {
        // k is index
        int j = partition(arr, l, r);
        if (j == k) {
            return Arrays.copyOf(arr, k + 1);
        }
        return j < k ? quickSort(arr, j + 1, r, k) : quickSort(arr, l, j - 1, k);
    }

    private int partition(int[] arr, int l, int r) {
        int key = arr[l];
        int i = l;
        int j = r + 1;
        while (true) {
            while(++i <= r && arr[i] < key);
            while(--j >= l && arr[j] > key);
            if (i >= j) {
                break;
            }
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
        arr[l] = arr[j];
        arr[j] = key;
        return j;
    }
}
//runtime:2 ms 期望为O(n)
//memory:42.5 MB O(1)
```

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k == 0) {
            return new int[]{};
        }
        Queue<Integer> heap = new PriorityQueue<>(k, (v1, v2) -> Integer.compare(v2, v1));
        for (int i = 0; i < arr.length; i++) {
            if (heap.size() < k) {
                heap.offer(arr[i]);
            } else {
                if (heap.peek() > arr[i]) {
                    heap.poll();
                    heap.offer(arr[i]);
                }
            }
        }
        int[] ans = new int[k];
        int n = 0;
        for (int num: heap) {
            ans[n++] = num;
        }
        return ans;
    }
}
//runtime:24 ms O(nlogk)
//memory:43 MB O(K)
```

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        Arrays.sort(arr);
        int[] ans = new int[k];
        System.arraycopy(arr, 0, ans, 0, k);
        return ans;
    }
}
//runtime:7  O(nlogn)
//memory:43.1 MB O(1)
```

#### [409. 最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)

```
class Solution {
    public int longestPalindrome(String s) {
        // count 52 letters
        int[] letterCounts = getLetterCounts(s);
        // calc counts
        int ans = calculateMaxLen(letterCounts);
        return ans;
    }

    private int calculateMaxLen(int[] letterCounts) {
        int evenSum = 0;
        int oddSum = 0;
        for (int count : letterCounts) {
            if (count % 2 == 0) {
                evenSum += count;
            } else {
                // odd - odd = even
                // final s: n evens + 1 odd
                if (oddSum == 0) {
                    oddSum += count;
                } else {
                    oddSum += count - 1;
                }
            }
        }
        return evenSum + oddSum;
    }

    private int[] getLetterCounts(String s) {
        int[] letterCounts = new int[52];
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            int idx = c < 'a' ? c - 'A' : c - 'a' + 26;
            letterCounts[idx] += 1;
        }
        return letterCounts;
    }
}
//runtime:3 ms
//memory:37.9 MB
```

#### [836. 矩形重叠](https://leetcode-cn.com/problems/rectangle-overlap/)

```
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        // IoU
        return Math.max(rec1[0], rec2[0]) < Math.min(rec1[2], rec2[2])
               && Math.max(rec1[1], rec2[1]) < Math.min(rec1[3], rec2[3]);
    }
}
//runtime:0 ms
//memory:36.7 MB
```

#### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return 1;
        }
        int len = 1;
        int[] d = new int[nums.length + 1];
        d[1] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > d[len]) {
                d[++len] = nums[i];
            } else {
                // binary search
                int l = 1;
                int r = len;
                int pos = 0;
                while (l <= r) {
                    int middle = (l + r) / 2;
                    if (d[middle] < nums[i]) {
                        pos = middle;
                        l = middle + 1;
                    } else {
                        r = middle - 1;
                    }
                }
                d[pos + 1] = nums[i];
            }
        }
        return len;
    }
}
//runtime:1 ms O(nlogn)
//memory:38 MB o(n)
```

```
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return 1;
        }
        int[] dp = new int[nums.length];
        dp[0] = 1;
        int ans = 0;
        for (int i = 1; i < nums.length; i++) {
            int maxval = 0;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    maxval = Math.max(maxval, dp[j]);
                }
            }
            dp[i] = maxval + 1;
            ans = Math.max(ans, dp[i]);
        }
        return ans;
    }
}
//runtime:12 ms O(n^2)
//memory:37.8 MB O(n)
```



#### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

```
class Solution {
    public int majorityElement(int[] nums) {
        // vote idea
        int count = 0;
        int compare = 0;
        for (int i = 0; i < nums.length; i++) {
            if (count == 0) {
                compare = nums[i];
            }
            if (compare == nums[i]) {
                ++count;
            } else {
                --count;
            }
        }
        return compare;
    }
}
//runtime:1 ms O(n)
//memory:41.8 MB O(1)
```

```
class Solution {
    // divide and conquer
    public int majorityElement(int[] nums) {
        return majorityElementRecursive(nums, 0, nums.length - 1);
    }

    private int majorityElementRecursive(int[] nums, int start, int end) {
        if (start == end) {
            return nums[start];
        }
        int middle = (start + end) / 2;
        int left = majorityElementRecursive(nums, start, middle);
        int right = majorityElementRecursive(nums, middle + 1, end);
        if (left == right) {
            return left;
        } else {
            int leftCount = calcCount(nums, left, start, middle);
            int rightCount = calcCount(nums, right, middle + 1, end);
            return leftCount > rightCount ? left : right;
        }
    }

    private int calcCount(int[] nums, int num, int start, int end) {
        int count = 0;
        for (int i = start; i <= end; i++) {
            if (nums[i] == num) {
                ++count;
            }
        }
        return count;
    }
}
//runtime:2 ms O(nlogn)
//memory:42 MB O(1)
```

```
class Solution {
    public int majorityElement(int[] nums) {
        // key: nums[i]; value: count
        HashMap<Integer, Integer> map = new HashMap(16);
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                int count = map.get(nums[i]) + 1;
                map.put(nums[i], count);
            } else {
                map.put(nums[i], 1);
            }
        }
        Map.Entry<Integer, Integer> result = map.entrySet().stream().filter(e -> e.getValue() > Math.floor(nums.length/2)).findAny().orElse(null);
        return result == null ? -1 : result.getKey();
    }
}
//runtime:24 ms O(n)
//memory:46.5 MB O(n)
```

#### [1071. 字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

```
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!(str1 + str2).equals(str2 + str1)) {
            return "";
        }
        return str1.substring(0, gcd(str1.length(), str2.length()));
    }

    private int gcd(int num1, int num2) {
        while (num2 != 0) {
            int temp = num2;
            num2 = num1 % num2;
            num1 = temp;
        }
        return num1;
    }
}
//runtime:1 ms
//memory:38.3 MB
```

```
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!validStr(str1) || !validStr(str2)) {
            return "";
        }
        HashSet<String> set1 = findDivisors(str1);
        HashSet<String> set2 = findDivisors(str2);
        int maxLen = 0;
        String ans = "";
        for (String divisor : set1) {
            if (set2.contains(divisor)) {
                if (divisor.length() > maxLen) {
                    maxLen = divisor.length();
                    ans = divisor;
                }  
            }
        }
        return ans;              
    }
    private boolean validStr(String str) {
        return str != null && str.length() > 0;
    }
    private HashSet<String> findDivisors(String str) {
        HashSet<String> set = new HashSet();
        // i: divisor length
        for (int i = 1; i <= str.length(); i++) {
            int cur = 0;
            String divisor = str.substring(0, i);
            while (cur < str.length()) {
                if (divisor.equals(str.substring(cur, Math.min(cur + i, str.length())))) {
                    cur += i;
                } else {
                    break;
                }
            }
            if (cur == str.length()) {
                set.add(divisor);
            }
        }
        return set;
    }
}
//runtime:90 ms O(n^2)
//memory:42 MB
```

#### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

```
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length < 2) {
            return 0;
        }
        int ans = 0;
        int left = 0;
        int right = height.length - 1;
        int maxLeft = 0;
        int maxRight = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= maxLeft) {
                    maxLeft = height[left];
                } else {
                    ans += maxLeft - height[left];
                }
                ++left;
            } else {
                if (height[right] >= maxRight) {
                    maxRight = height[right];
                } else {
                    ans += maxRight - height[right];
                }
                --right;
            }
        }
        return ans;
    }
}
//runtime:1 ms O(n)
//memory:39 MB O(1)
```

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


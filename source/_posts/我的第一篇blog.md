---
title: 新手70题
date: 2021-11-15 13:29:09
categories: 数据结构
tag: 
  - Leetcode
  - Java
---

LeetCode新手入门必备70题，分享给大家

<!-- more -->

#### 题目目录：

​	**数组Array**： 485， 283， 27

​	链表：203，206

​	队列：933，225，622，641

​	栈：20，496，232

​	哈希表：217，389，496

​	集合set：217，705

​	堆：215，692

​	双指针算法：141，344，881

​	二分查找法：704，35，162，74

​	滑动窗口：209，1456；

​	递归算法：509，206，344，687

​	分治算法：169，53

​	回溯算法：22，78，77，46

​	深度优先算法DFS：938， 78，200

​	宽度优先算法BFS：102，107，200

​	并查集：200，547，721

​	贪心算法：322，217，55

​	记忆化搜索：509，322

​	动态规划：509，62，121，70，279，221

​	trie(前缀树)：208，720，692

​	拓扑排序：207，210 

#### 数组Aaary

[485. 最大连续 1 的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

```java 
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int max =0, num =0;
        for(int i =0;i<nums.length;i++){
            if(nums[i]==1){
                 num++;
            } else{
               if(num>=max)  max = num;
               num = 0;
            }
        }
        if(num>max) max = num;
        return max;
    }
}
```

##### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

快慢指针



```java
class Solution {
    public void moveZeroes(int[] nums) {
        int left=0,right =0;
        while(right<nums.length){
            if(nums[right]!=0){
                int tmp = nums[right];
                nums[right]= nums[left];
                nums[left] = tmp;
                left ++;
            }
            right++;
        }
    }
}
```

##### [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

思想结果同283

```JAVA
class Solution {
    public int removeElement(int[] nums, int val) {
         int fast = 0; int low = 0;
         while(fast < nums.length){
             if(nums[fast] != val){
                 int tmp = nums[fast];
                 nums[fast] = nums[low];
                 nums[low] = tmp;
                 low++;
             }
             fast++;
         }
         return low;
    }
}
```

#### 链表：

##### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

递归： 终止条件是head = null；

往后倒退返回链表

```JAVA
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
    public ListNode removeElements(ListNode head, int val) {
        if(head == null) return head;
        head.next = removeElements(head.next,val);
        return head.val ==val ?head.next:head;
    }
}
```

##### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

反转链表递归方法，先将链表循环至链表尾部，在反转链表，将head.next.next = head; head.next必须置空，否则产生双向循环。

```java	
reverseList: head=1
    reverseList: head=2
	    reverseList: head=3
		    reverseList:head=4
			    reverseList:head=5 
					终止返回
				cur = 5
				4.next.next->4，即5->4
			cur=5
			3.next.next->3，即4->3
		cur = 5
		2.next.next->2，即3->2
	cur = 5
	1.next.next->1，即2->1
	
	最后返回cur
```

**代码：**

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
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next==null)
            return head;
        listNode cur = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return cur;
    }
}
```

#### 队列

##### [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

同剑指offer第9题（两个栈表示一个队列）不同的是，两个队列实现栈需要时一个队列做栈，另一个做辅助队列，如图所示

![fig1](https://assets.leetcode-cn.com/solution-static/225/225_fig1.gif)

```java
class MyStack {
    Queue<Integer> q1;
    Queue<Integer> q2;
    public MyStack() {
        q1 = new LinkedList();
        q2 = new LinkedList();
    }
    
    public void push(int x) {6
        q2.add(x);
        while(!q1.isEmpty()){q2.add(q1.poll());};
        Queue<Integer> tmp = q1;
        q1 = q2;
        q2 = tmp;
    }
    
    public int pop() {
        return q1.poll();
    }
    
    public int top() {
        return q1.peek();
    }
    
    public boolean empty() {
       return q1.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

##### [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

循环列表，可以用数组来模拟队列，然后设置一个标志位来count计数，入队就加一，出队就减一。这样可以判断队列是否是满还是空。

```java
class MyCircularQueue {
    private int Maxsize;
    private int []arr;
    private int fornt = 0; // 队头指针
    private int rear = -1; // 队尾指针
    private int conut = 0; //计数的
    public MyCircularQueue(int k) {
        arr = new int[k];
        Maxsize = k;
    }
    
    public boolean enQueue(int value) {
        if(isFull()) return false;
        rear = (rear +1 ) % Maxsize;
        arr[rear] = value;
        conut++;
        return true;
    }
    
    public boolean deQueue() {
        if(isEmpty()) return false;
        fornt = (fornt+1)%Maxsize;
        conut--;
        return true; 
    }
    
    public int Front() {
        if(isEmpty()){
            return -1;
        }
        return arr[fornt];
    }
    
    public int Rear() {
        if(isEmpty()) return -1;
        return arr[rear];
    }
    
    public boolean isEmpty() {
        return conut == 0;
    }
    
    public boolean isFull() {
        return conut == Maxsize;
    }
}
```

##### [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)

```java
class MyCircularDeque {
        private int Maxsize;
        private int []arr;
        private int fornt = 0; // 队头指针
        private int rear = 0; // 队尾指针

    public MyCircularDeque(int k) {
        Maxsize = k+1;
        arr = new int[Maxsize];
    }
    
    public boolean insertFront(int value) {
       if(isFull()) return false;
       fornt = (fornt -1 + Maxsize)%Maxsize;
       arr[fornt] = value;
       return true;
    }
    
    public boolean insertLast(int value) {
        if(isFull()) return false;
        arr[rear] = value;
        rear = (rear +1)%Maxsize;
        return true;
    }
    
    public boolean deleteFront() {
        if(isEmpty()) return false;
        fornt = (fornt +1)%Maxsize;
        return true;
    }
    
    public boolean deleteLast() {
         if(isEmpty()) return false;
         rear = (rear -1 +Maxsize)%Maxsize;
         return true;
    }
    
    public int getFront() {
        if(isEmpty()) return -1;
        return arr[fornt];
    }
    
    public int getRear() {
        if(isEmpty()) return -1;
        return arr[(rear -1 +Maxsize)%Maxsize];
    }
    
    public boolean isEmpty() {
        return fornt == rear;
    }
    
    public boolean isFull() {
        return (rear+1)%Maxsize == fornt;
    }
}

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertLast(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleteLast();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
```

##### [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)

设计实现双端队列。
你的实现需要支持以下操作：

MyCircularDeque(k)：构造函数,双端队列的大小为k。
insertFront()：将一个元素添加到双端队列头部。 如果操作成功返回 true。
insertLast()：将一个元素添加到双端队列尾部。如果操作成功返回 true。
deleteFront()：从双端队列头部删除一个元素。 如果操作成功返回 true。
deleteLast()：从双端队列尾部删除一个元素。如果操作成功返回 true。
getFront()：从双端队列头部获得一个元素。如果双端队列为空，返回 -1。
getRear()：获得双端队列的最后一个元素。 如果双端队列为空，返回 -1。
isEmpty()：检查双端队列是否为空。
isFull()：检查双端队列是否满了。



```java
class MyCircularDeque {
        private int Maxsize;
        private int []arr;
        private int fornt = 0; // 队头指针
        private int rear = 0; // 队尾指针

    public MyCircularDeque(int k) {
        Maxsize = k+1;
        arr = new int[Maxsize];
    }
    
    public boolean insertFront(int value) {
       if(isFull()) return false;
       fornt = (fornt -1 + Maxsize)%Maxsize;
       arr[fornt] = value;
       return true;
    }
    
    public boolean insertLast(int value) {
        if(isFull()) return false;
        arr[rear] = value;
        rear = (rear +1)%Maxsize;
        return true;
    }
    
    public boolean deleteFront() {
        if(isEmpty()) return false;
        fornt = (fornt +1)%Maxsize;
        return true;
    }
    
    public boolean deleteLast() {
         if(isEmpty()) return false;
         rear = (rear -1 +Maxsize)%Maxsize;
         return true;
    }
    
    public int getFront() {
        if(isEmpty()) return -1;
        return arr[fornt];
    }
    
    public int getRear() {
        if(isEmpty()) return -1;
        return arr[(rear -1 +Maxsize)%Maxsize];
    }
    
    public boolean isEmpty() {
        return fornt == rear;
    }
    
    public boolean isFull() {
        return (rear+1)%Maxsize == fornt;
    }
}

```

#### 栈：

##### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```java
class Solution {
    public boolean isValid(String s) {
        int n = s.length();
        if (n % 2 == 1) {
            return false;
        }

        Map<Character, Character> pairs = new HashMap<Character, Character>() {{
            put(')', '(');
            put(']', '[');
            put('}', '{');
        }};
        Deque<Character> stack = new LinkedList<Character>();
        for (int i = 0; i < n; i++) {
            char ch = s.charAt(i);
            if (pairs.containsKey(ch)) { //这判断意思是查看是否是右括号key，是的话进行判断，否职肯定是左括号，直接入栈
                if (stack.isEmpty() || stack.peek() != pairs.get(ch)) {
                    return false;
                }
                stack.pop();
            } else {
                stack.push(ch);
            }
        }
        return stack.isEmpty();
    }
}
```

##### [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

题目的第一思维肯定是减少时间复杂度，暴力算法就算了，想到用hashmap来记录nums2数组的对应位置下一个位置最大的，可以用单调栈做，在下一个元素入栈前保证栈内是单调递减的，每次元素和栈顶比，满足要求就出栈，知道为空。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
         Deque<Integer> stack = new LinkedList<Integer>();
         Map<Integer,Integer> map = new HashMap<>();
         for(int i = 0;i<nums2.length;i++){
             while(!stack.isEmpty()&&stack.peek() < nums2[i]){
                 map.put(stack.pop(), nums2[i]);
                 }    
             stack.push(nums2[i]);
         }

         int []res = new int[nums1.length];
         for(int i = 0;i<nums1.length;i++){
             res[i] = map.getOrDefault(nums1[i], -1); //getOrDefault 用法就是当Map集合中有这个key时，就使用这个																	key值；如果没有就使用默认值defaultValue。
         }
         return res;
}·
}
```

##### [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

一定要同第225题，用队列实现栈来比较。

- 用栈表示队列，栈1用来入队列，栈2用来出队列的。
- 用队列表示栈两个队列不是一个水平的，队列一永远表示栈，队列二用作辅助入栈作用，每次入栈将数入队二，然后队列一的数放进队二中，这样顺序就排好了，队一和队二互换就达到了效果。

```java
class MyQueue {

    Deque<Integer>stack1;
    Deque<Integer>stack2;

    public MyQueue() {
        stack1 = new LinkedList<>();
        stack2 = new LinkedList<>();
    }
    
    public void push(int x) {
        stack1.push(x);
    }
    
    public int pop() {
        if(stack2.isEmpty()){
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
         return  stack2.pop();
    }
    
    public int peek() {
        if(stack2.isEmpty()){
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
         return  stack2.peek();
    }
    
    public boolean empty() {
        return stack1.isEmpty()&&stack2.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
```

#### 哈希表

##### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

这道题目比较简单，还是直接上标准答案吧；

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int x : nums) {
            if (!set.add(x)) {
                return true;
            }
        }
        return false;
    }
}
```

##### [389. 找不同](https://leetcode-cn.com/problems/find-the-difference/)

求和字符串中的所有ascall码值 ，然后相减得到被添加的字母的ascall码值转化成字符串就可以

```java
class Solution {
    public char findTheDifference(String s, String t) {
        int chara =0, charb = 0;
        for(int i = 0;i<s.length();i++){
            chara += s.charAt(i);
        }
        for(int i = 0;i<t.length();i++){
            charb += t.charAt(i);
        }
        return (char) (charb - chara);
    }
}
```

#### hashset

##### [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

简单数组法；后面有机会希望能手撸hashmap

```java
class MyHashSet {
    boolean []res;
    public MyHashSet() {
        res = new boolean [1000001];
    }
    
    public void add(int key) {
        res[key] = true;
    }
    
    public void remove(int key) {
        res[key] = false;
    }
    
    public boolean contains(int key) {
        return res[key];
    }
}
```

[215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

##### [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> cnt = new HashMap<String, Integer>();
        for (String word : words) {
            cnt.put(word, cnt.getOrDefault(word, 0) + 1);
        }
        List<String> rec = new ArrayList<String>();
        for (Map.Entry<String, Integer> entry : cnt.entrySet()) {
            rec.add(entry.getKey());
        }
        Collections.sort(rec, new Comparator<String>() {
            public int compare(String word1, String word2) {
                return cnt.get(word1) == cnt.get(word2) ? word1.compareTo(word2) : cnt.get(word2) - cnt.get(word1);
            }
        });
        return rec.subList(0, k);
    }
}


```

#### 双指针算法

141，344，881

##### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

两种思路，一种创建一个hashset，每次将链表放进去。还有一种是双指针，龟兔赛跑，如果是环形链表，兔子终会追上乌龟的。如果下一个为空，则返回false；

```java
/**
	双指针
*/
public class Solution {
    public boolean hasCycle(ListNode head) {
    if(head == null || head.next ==null) return false;
    ListNode slow = head;
    ListNode fast = head.next;
    while(slow!=fast){
        if(fast == null || fast.next == null) return false;
        fast = fast.next.next;
        slow = slow.next;
    }
    return true;
}
}
```

```java
/**
	hashset
*/
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<ListNode>();
        while (head != null) {
            if (!seen.add(head)) {
                return true;
            }
            head = head.next;
        }
        return false;
    }
}
```

##### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

要是世间这么简单就好了

```java
class Solution {
    public void reverseString(char[] s) {
        int i = 0, j = s.length -1;
        while(j>i){
            char tmp = s[i];
            s[i] = s[j];
            s[j]=tmp;
            i++;
            j--;
        }
    }
}
```

[881. 救生艇](https://leetcode-cn.com/problems/boats-to-save-people/)

双指针，贪心算法；两个指针一头一尾，两个指针值相加<=limit则两个指针都相对移动，否则右指针向左移动。

```java 
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int l = 0, r = people.length - 1;
        int ans = 0;
        while (l <= r) {
            if (people[l] + people[r] <= limit) l++;
            r--;
            ans++;
        }
        return ans;
    }
}


```

#### 二分查找法

704，35，162，74

##### [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

直观的二分查找方法

```java 
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length -1;
        while(left<=right){
            int mid = (right + left)/2;
            if(nums[mid] > target){
               right = mid -1;
            }else if(nums[mid] < target){
                left = mid +1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```

[35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

二分查找的时间复杂度为o(logn)

##### [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

爬坡，虽然不知道下坡是否有另一个山峰，但是向上爬一定能爬到顶峰

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20211021/image.19xpvvvc6qkg.png" alt="image" style="zoom:60%;" />

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length -1;
        while(left<right){
            int mid = (left+right)/2;
            if(nums[mid] > nums[mid+1]){
                right = mid;
            }else{
                left = mid+1;
            }
        }
        return left;
    }
}
```

##### [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

同剑指offer第4题题解，但是剑指offer那题比这题实用性更广；

线性查找：

==由于给定的二维数组具备每行从左到右递增以及每列从上到下递增的特点，当访问到一个元素时，可以排除数组中的部分元素。从二维数组的右上角(或者左下角， 左上角和右上角不能排除)开始查找。如果当前元素等于目标值，则返回 true。如果当前元素大于目标值，则移到左边一列。如果当前元素小于目标值，则移到下边一行。可以证明这种方法不会错过目标值。如果当前元素大于目标值，说明当前元素的下边的所有元素都一定大于目标值，因此往下查找不可能找到目标值，往左查找可能找到目标值。如果当前元素小于目标值，说明当前元素的左边的所有元素都一定小于目标值，因此往左查找不可能找到目标值，往下查找可能找到目标值。==



```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row =0;
        int clo = matrix[0].length -1;
        for(;row<matrix.length&&clo>=0;){
            if(matrix[row][clo] == target){
                return true;
            }else if(matrix[row][clo] > target){
                clo--;
            }else{
                row++;
            }
        }
        return false;
    }
}
```

#### 	滑动窗口：

209，1456；

##### [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

滑动窗口，先从头开始加数组知道满足数组目标，然后再减去首元素开始滑动窗口；

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int res = nums.length +1;
        int start = 0 ;
        int sum = 0;
        for(int i = 0;i<nums.length;i++){
            if(sum<target){
                sum += nums[i];
            }
            while(sum>=target){
                if(res > i-start+1){
                    res = i-start+1;
                }
                sum -= nums[start++];
            }
        }
       return res == (nums.length+1)? 0:res;
    }
}
```

##### [1456. 定长子串中元音的最大数目](https://leetcode-cn.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

```java
class Solution {
    public int maxVowels(String s, int k) {
        char []nums = s.toCharArray();
        int start = 0;
        int count = 0;
        for(int i = 0;i<k;i++){
            if(isYuan(nums[i])){
                count++;
            }
        }
        int ans = count;
        for(int i = k;i<nums.length;i++){
            if(isYuan( nums[start++])){
                count--;
            }
            if(isYuan(nums[i])){
                count++;
            }
            if(count >= ans) ans = count;
        }
        return ans;
    } 
    static boolean isYuan(char ans){
        switch(ans){
        case 'a' :
            return true;
        case 'e' :
            return true;
        case 'i' :
            return true;
        case 'o' :
            return true;
        case 'u' :
            return true;
        default :  
            return false;
        }
    }   
}
```

#### 递归算法

509，206，344，687

递归是我一直以来的坎, 一看就会, 一写就废.
今天总结一下, 主要分四步:

1. 设置停止条件, 防止死归
2. 递之前的操作
3. 传递的操作
4. 归回来的操作

**递归的微妙之处:**

1. 代码简短到像在和计算机交流
2. 只在递之前操作, 就像时光流逝, 从起点到终点顺向执行
3. 只在归回来操作, 就像时光倒流, 从终点回起点逆向执行
4. 即递下去, 也归回来. 我现在还说不出来精髓, 但是有一段代码写的很好, 在这里分享给大家.

```java
class Solution {
    public int fib(int n) {
    int res = 0;
     if(n ==0||n==1){
         return n;
     }  
     res = fib(n-1)+fib(n-2);
     return res;
}
}
```

##### [687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)

DFS YYDS

一看就会，一做就废

```java
class Solution {
    int ans;
    public int longestUnivaluePath(TreeNode root) {
        ans = 0;
        arrowLength(root);
        return ans;
    }
    public int arrowLength(TreeNode node) {
        if (node == null) return 0;
        int left = arrowLength(node.left);
        int right = arrowLength(node.right);
        int arrowLeft = 0, arrowRight = 0;
        if (node.left != null && node.left.val == node.val) {
            arrowLeft += left + 1;
        }
        if (node.right != null && node.right.val == node.val) {
            arrowRight += right + 1;
        }
        ans = Math.max(ans, arrowLeft + arrowRight);
        return Math.max(arrowLeft, arrowRight);
    }
}

```

#### 分治算法

169，53

##### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

```java
//map算法
class Solution {
    public int majorityElement(int[] nums) {
         Map<Integer, Integer> map = new HashMap<Integer, Integer>();
         for(int i: nums){
             map.put(i,map.getOrDefault(i,0)+1);
         }
        int ans = nums[0];
        for(Integer i : map.keySet()){
           if(map.get(i) > map.get(ans)){
               ans = i;
           }
        }
        return ans;
    }
}
```

众数法：摩尔投票

求大于$n/k$，至多有$k-1$个数是多数元素，所以采用摩尔投票法。设立一个投票者，一个计票者，当有反对票时候，计票减一直到减为0；下一次再换新的投票者上来。

```java
class Solution {
    public int majorityElement(int[] nums) {
        int key = 0; int value = 0;
        for(int i:nums){
            if(value == 0){
                key = i;
                value++;
            }else if(value>0 && key == i){
                value++;
            }else{
                value--;
            }
        }
        return key;
    }
}
```

##### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //动态规划解决O(n)
        //思路：因为和<0等于对我们最后结果无意义舍去,和>0才有意义
        int sum = 0, ans = nums[0];
        for(int i: nums){
            sum += i;
            if(sum>=0){
                ans = Math.max(ans, sum);
            }else{
                ans = Math.max(ans, sum);
                sum = 0;
            }
        }
        return ans;
    }
}

```

#### 回溯算法

22，78，77，46

##### 22

```java
class Solution {
    // 参数含义：
    // str： 表示当前2n位的字符
    // leftRest: 剩余的左括号数量
    // rightRest：剩余的右括号数量
    // cur：表示当前递归到str的第几位了
    // ans：记录的答案列表
    private void generateParenthesis(char[] str, int leftRest, int rightRest, int cur, List<String> ans) {
        // 已递归到最后一位，已无可用的leftRest和rightRest
        // 此处判断条件写cur == str.length也可以
        if (leftRest == 0 && rightRest == 0) {
            ans.add(new String(str));
            return;
        }
        // 如果剩余的左括号大于剩余的右括号数量，那么进行剪枝
        // 即使递归下去，得到的结果也是不正确的。因为左括号会无法全部闭合，剩下的右括号无法与左括号进行匹配。
        if (leftRest > rightRest) {
            return;
        }
        // 如果左括号有剩余，可以将当前位置选作左括号
        if (leftRest > 0) {
            str[cur] = '(';
            generateParenthesis(str, leftRest - 1, rightRest, cur + 1, ans);
        }
        // 如果右括号有剩余，可以将当前位置选作右括号
        if (rightRest > 0) {
            str[cur] = ')';
            generateParenthesis(str, leftRest, rightRest - 1, cur + 1, ans);
        }
    }

    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        char[] str = new char[2 * n];
        generateParenthesis(str, n, n, 0, ans);
        return ans;
    }
}

```

##### [77. 组合](https://leetcode-cn.com/problems/combinations/)

```java
class Solution {
        List<List<Integer>> ans = new ArrayList<>();
        LinkedList<Integer> cur = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        int startIndex = 1;
        backTracking(n,k,startIndex);
        return ans;
    }
    void backTracking(int n, int k,int startIndex){
        if(cur.size()==k){
            ans.add(new ArrayList(cur));
            return ;
        }
        for(int i = startIndex;i<=n-(k-cur.size())+1;i++){
            cur.add(i);
            backTracking(n,k,i+1);
            cur.removeLast();
        }
    }
}
```

##### [78. 子集](https://leetcode-cn.com/problems/subsets/)

想法根据77题得来的；回溯问题的基本框架

```java
        List<List<Integer>> ans = new ArrayList<>();
        LinkedList<Integer> subset = new LinkedList<>();

		backTracking(nums, 0);
        void backTracking(int []nums, int starIndex){
            填入条件
                for(){
                    回溯前；
                     回溯
                    回溯后；
                }
        }
```



```java
class Solution {        
        List<List<Integer>> ans = new ArrayList<>();
        LinkedList<Integer> subset = new LinkedList<>();
        
    public List<List<Integer>> subsets(int[] nums) {
        int starIndex = 0;
        backTracking(nums, starIndex);
        return ans;
    }
     private void backTracking(int []nums, int starIndex){
             ans.add(new ArrayList(subset));
         for(int i=starIndex;i<nums.length;i++){
             subset.add(nums[i]);
             backTracking(nums,i+1);
             subset.removeLast();
         }
     }
}
```

##### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

排列和组合算法不一样，排列需要用到之前的，所以不能用startindex用used数组判断之前的是否用过了

```java
class Solution {
        List<List<Integer>> ans = new ArrayList<>();
        LinkedList<Integer> cur = new LinkedList<>();
        boolean []used;
    public List<List<Integer>> permute(int[] nums) {
        int n = nums.length;
        used = new boolean[n];
        backTracking(nums, n, used);
        return ans;
    }
    private void backTracking(int []nums, int n, boolean []used){
        if(cur.size() == n){
          ans.add(new ArrayList(cur));
          return;
        }

        for(int i = 0;i<n;i++){
            if(used[i]) continue;
            used[i] = true;
            cur.add(nums[i]);
            backTracking(nums, n,used);
            cur.removeLast();
            used[i] = false;
        }
    }
}
```

#### 深度优先算法DFS

938， 78，200

##### [938. 二叉搜索树的范围和](https://leetcode-cn.com/problems/range-sum-of-bst/)

二叉搜索树，左子树的值小于根的值，右子树的值大于根的值；

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
        int ans = 0;
    public int rangeSumBST(TreeNode root, int low, int high) {  
        Bfs(root, low, high);
        return ans;
    }
    private void Bfs(TreeNode root, int low, int high){
        if(root == null) return;
        if(root.val <= high && root.val >= low){
            ans += root.val;
            Bfs(root.left,low,high);
            Bfs(root.right,low,high);
        }
        if(root.val < low){
            Bfs(root.right,low,high);
        }
        if(root.val > high){
            Bfs(root.left,low,high);
        }
    }
}
```

##### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

网格的dfs和树不太一样，树是判断当前节点是否为空；而网格的遍历需要判断当前位置是否越了数组界，其次是当前位置是否遍历过，所以将遍历过的位置置一个标志位，防止重复走到。

```java
class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        for(int i = 0;i<grid.length;i++){
            for(int j = 0;j<grid[0].length;j++){
                if(grid[i][j] == '1'){
                    ans++;
                    bfs(grid,i,j);
                }
            }
        }
        return ans;
    }
    private void bfs(char [][]grid,int i, int j){
        if(i>=grid.length||j>=grid[0].length||i<0||j<0||grid[i][j] != '1') return;
        grid[i][j] = '2';
        bfs(grid,i+1,j);
        bfs(grid,i-1,j);
        bfs(grid,i,j+1);
        bfs(grid,i,j-1);
    }
}
```

#### 宽度优先算法BFS

102，107，200

##### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

BFS遍历二叉树，BFS的模板如下，以后的问题在此基础上修改

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new  ArrayList<>();
         Deque<TreeNode> tree = new LinkedList<>();
         if(root == null) return ans;
         tree.add(root);
         while(!tree.isEmpty()){
             List<Integer> cur = new ArrayList<>();
             int size = tree.size();
             for(int i = 0;i<size;i++){
                 TreeNode node = tree.poll();
                 if(node.left !=null) tree.add(node.left);
                 if(node.right != null) tree.add(node.right);
                 cur.add(node.val);
             }
             ans.add(new ArrayList(cur));
         }
         return ans;
    }
}
```

##### [107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> ans = new  ArrayList<>();
        List<List<Integer>> res = new  ArrayList<>();
         Deque<TreeNode> tree = new LinkedList<>();
         if(root == null) return ans;
         tree.add(root);
         while(!tree.isEmpty()){
             List<Integer> cur = new ArrayList<>();
             int size = tree.size();
             for(int i = 0;i<size;i++){
                 TreeNode node = tree.poll();
                 if(node.left !=null) tree.add(node.left);
                 if(node.right != null) tree.add(node.right);
                 cur.add(node.val);
             }
             ans.add(new ArrayList(cur));
         }
         int n = ans.size();
         while(n != 0){
             res.add(ans.get(n-1));
             n--;
         }
         return res;
    }
}

//真实的代码
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> ans = new  ArrayList<>();
        List<List<Integer>> res = new  ArrayList<>();
         Deque<TreeNode> tree = new LinkedList<>();
         if(root == null) return ans;
         tree.add(root);
         while(!tree.isEmpty()){
             List<Integer> cur = new ArrayList<>();
             int size = tree.size();
             for(int i = 0;i<size;i++){
                 TreeNode node = tree.poll();
                 if(node.left !=null) tree.add(node.left);
                 if(node.right != null) tree.add(node.right);
                 cur.add(node.val);
             }
             ans.add(0,cur);  //或者ans.add(cur); Collection.reverse(ans);
             				//用一下集合反转方法；
             
         }
         return ans;
    }
}
```

##### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

```java
//BFS
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == '1'){
                    bfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    private void bfs(char[][] grid, int i, int j){
        Queue<int[]> list = new LinkedList<>();
        list.add(new int[] { i, j });
        while(!list.isEmpty()){
            int[] cur = list.remove();
            i = cur[0]; j = cur[1];
            if(0 <= i && i < grid.length && 0 <= j && j < grid[0].length && grid[i][j] == '1') {
                grid[i][j] = '0';
                list.add(new int[] { i + 1, j });
                list.add(new int[] { i - 1, j });
                list.add(new int[] { i, j + 1 });
                list.add(new int[] { i, j - 1 });
            }
        }
    }
}

//DFS
class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        for(int i = 0;i<grid.length;i++){
            for(int j = 0;j<grid[0].length;j++){
                if(grid[i][j] == '1'){
                    bfs(grid,i,j);
                     ans++;
                }
            }
        }
        return ans;
    }
    private void bfs(char [][]grid,int i, int j){
        if(i>=grid.length||j>=grid[0].length||i<0||j<0||grid[i][j] != '1') return;
        grid[i][j] = '2';
        bfs(grid,i+1,j);
        bfs(grid,i-1,j);
        bfs(grid,i,j+1);
        bfs(grid,i,j-1);
    }
}
```

#### 并查集

200，547，721

##### 并查集的模板

```java
 public Solution() {
        n = 1005;
        father = new int[n];

        // 并查集初始化
        for (int i = 0; i < n; ++i) {
            father[i] = i;
        }
    }

    // 并查集里寻根的过程
    private int find(int u) {
        if(u == father[u]) {
            return u;
        }
        father[u] = find(father[u]);
        return father[u];
    }

    // 将v->u 这条边加入并查集
    private void join(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) return ;
        father[v] = u;
    }

    // 判断 u 和 v是否找到同一个根，本题用不上
    private Boolean same(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
```



[547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

```java
//dfs， 并查集不太会
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int size = isConnected.length;
        boolean []visited = new boolean[size];
        int ans = 0;
        for(int i = 0;i<isConnected.length;i++){
            if(!visited[i]){
                ans++;
                dfs(isConnected,i,visited);
            }
        }
        return ans;
    }
    private void dfs(int [][]isConnected,int i,boolean []visited){
        visited[i] = true;
        for(int j = 0;j<isConnected.length;j++){
            if(isConnected[i][j] == 1 && !visited[j]){
                dfs(isConnected,j,visited);
            }
        }
    }
}
```

#### 	贪心算法：

322，217，55

##### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

记忆搜索

```java
class Solution {
    int []memo;
    public int coinChange(int[] coins, int amount) {
        memo = new int[amount];
        return bfs(coins,amount);
    }
    private int bfs(int[] coins, int amount){
        if(amount<0) return -1;
        if(amount == 0) return 0;
        if(memo[amount-1] != 0){
            return memo[amount-1];
        }
        int min = Integer.MAX_VALUE;
        for(int i = 0;i<coins.length;i++){
            int res = bfs(coins,amount-coins[i]);
            if(res>=0&&res<min){
                min = res+1;
            }
        }
        memo[amount-1] = (min == Integer.MAX_VALUE ? -1 : min);
        return memo[amount-1];
    }
}
```

##### [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

自己的思想：遍历数组，记录每一步中最大跳跃长度，能否超过size；不可以就返回false，可以返回true，算法复杂度o(n)

```java
//自己的解法
class Solution {
    public boolean canJump(int[] nums) {
        int i = 0;

        int size = nums.length-1;
        while((nums[i]+i)<size){
            int max = nums[i]+i;
            for(int j = i;j<=max;j++){
                if(j+nums[j]>=size){
                    return true;
                }
                max = Math.max(max,nums[j]+j);
            }
            if(max>(nums[i]+i)){
                i = nums[i] +i+1;
            }else{
                return false;
            }
        }
        return true;
}
}
```

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20211103/image.4m81ggz2mmk.png)

#### 记忆化搜索

509，322

##### [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

```java	
class Solution {
    public int fib(int n) {
    int res = 0;
     if(n ==0||n==1){
         return n;
     }  
     res = fib(n-1)+fib(n-2);
     return res;
}
}

//动态规划题目。自下而上
class Solution {
    public int fib(int n) {
        int a = 0, b = 1;
        for(int i =0;i<n;i++){ //加n-1次
            int sum=a+b;
            a=b;
            b = sum;
        }
        return a;
}
}
//记忆搜索
public int fib(int n) {
    return fib(n, new HashMap());
}

public int fib(int n, Map<Integer, Integer> map) {
    if (n < 2)
        return n;
    if (map.containsKey(n))
        return map.get(n);
    int first = fib(n - 1, map) 
    map.put(n - 1, first);
    int second = fib(n - 2, map) 
    map.put(n - 2, second);
    int res = (first + second)
    map.put(n, res);
    return res;
}
```

#### 动态规划：

509，62，121，70，279，221

##### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

动态规划做， 

![62.不同路径1](https://img-blog.csdnimg.cn/20201209113631392.png)

```java
/**
 第一想法是深度优先搜索，但是算法复杂度为o(2^(m+n)-1)复杂度，算法复杂度太高。
 动态规划：递推公式
            dp[i][j]=dp[i-1][j]+dp[i][j-1]
           	
*/
class Solution {
    public int uniquePaths(int m, int n) {
        return bfs(0,0,m,n);
    }
    private int bfs(int row, int clo,int m,int n){
        if(row>=m||clo >=n) return 0;
        if(row == (m-1)&&clo== (n-1)){
            return 1;
        }
        return  bfs(row+1,clo,m,n)+bfs(row,clo+1,m,n);
    }
}
//动态规划
class Solution {
    public int uniquePaths(int m, int n) {
        int [][]dp = new int[m][n];
        for(int i = 0;i<m;i++){
            dp[i][0] = 1;
        }
        for(int i = 0;i<n;i++){
            dp[0][i] = 1;
        }
        for (int i =1;i<m;i++){
            for(int j = 1;j<n;j++){
                dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];

    }
}
```

##### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int res = 0;
        int pre = prices[0];
        int after;
        for(int i = 1; i <prices.length;i++){
           after = prices[i];
           res = Math.max(res,(after-pre));
           pre = Math.min(pre, after); 
        }
        return res;
    }
}

```

##### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

```java
class Solution {
    public int climbStairs(int n) {
        int a = 1;
        int b = 2;
        int sum;
        for(int i = 1;i<n;i++){
            sum = a+b;
            a=b;
            b=sum;
        }
        return a;
    }
}
```

##### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

```java
class Solution {
    public int numSquares(int n) {
        int []dp = new int[n+1];
        for(int i = 0;i<=n;i++){
            dp[i] = i;
            for(int j=1;i-j*j>= 0;j++){
                dp[i] = Math.min(dp[i], dp[i-j*j]+1);
            }
        }
        return dp[n];
    }
}
```

##### [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

木桶短板理论选取边长最小的

```java
public int maximalSquare(char[][] matrix) {
    // base condition
    if (matrix == null || matrix.length < 1 || matrix[0].length < 1) return 0;

    int height = matrix.length;
    int width = matrix[0].length;
    int maxSide = 0;

    // 相当于已经预处理新增第一行、第一列均为0
    int[][] dp = new int[height + 1][width + 1];

    for (int row = 0; row < height; row++) {
        for (int col = 0; col < width; col++) {
            if (matrix[row][col] == '1') {
                dp[row + 1][col + 1] = Math.min(Math.min(dp[row + 1][col], dp[row][col + 1]), dp[row][col]) + 1;
                maxSide = Math.max(maxSide, dp[row + 1][col + 1]);
            }
        }
    }
    return maxSide * maxSide;
}

```

#### trie(前缀树)

208，720，692

##### [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

[**什么是前缀树?**](https://www.cnblogs.com/vincent1997/p/11237389.html)，本道题目让你手撕前缀树。

```java
class Trie {
    class TrieNode{
        boolean end;
        TrieNode []floor = new TrieNode[26];
    }

    TrieNode root;
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode p = root;
        for(int i = 0;i<word.length();i++){
            int u = word.charAt(i) - 'a';
            if(p.floor[u] == null) p.floor[u] = new TrieNode();
            p = p.floor[u];
        }
        p.end = true;
    }
    
    public boolean search(String word) {
        TrieNode p = root;
        for(int i = 0;i<word.length();i++){
            int u = word.charAt(i) - 'a';
            if(p.floor[u] == null) return false;
            p = p.floor[u];
        }
        return p.end;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode p = root;
        for(int i = 0;i<prefix.length();i++){
            int u = prefix.charAt(i) - 'a';
            if(p.floor[u] == null) return false;
            p = p.floor[u];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

##### [720. 词典中最长的单词](https://leetcode-cn.com/problems/longest-word-in-dictionary/)

字符串 word.substring(n1,n2),其中n1能取到，n2不能左闭右开；

```java
//暴力解法                                                                                                   l   
class Solution {
    public String longestWord(String[] words) {
        Arrays.sort(words);
        Set<String> set = new HashSet<>();
        String res = "";
        for(String word : words){
            if(word.length() == 1 || set.contains(word.substring(0,word.length()-1))){
                if(res.length() < word.length()){
                    res = word;
                }
                set.add(word);
            }
            
        }
        return res;
    }
}
```

##### [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

用map统计每个字符串出现的个数，然后将键取出放入list中，排序这个list，重写排序方法，党当两个键值相等时候看字母，其余的看直接看键值的大小。

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String,Integer> map = new HashMap<String,Integer>();
        for(int i = 0;i<words.length;i++){
            map.put(words[i], map.getOrDefault(words[i], 0)+1);
        }
        List <String> res = new ArrayList<String>();
        for(Map.Entry<String,Integer> entry: map.entrySet()){
            res.add(entry.getKey());
        }
        Collections.sort(res,new Comparator<String>(){
            public int compare(String word1,String word2){
                return map.get(word1)==map.get(word2)? word1.compareTo(word2):map.get(word2) -map.get(word1);
            }
        });
        return res.subList(0,k);

    }
}
```


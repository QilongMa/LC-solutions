>19 Remove Nth Node From End of List  
21 Merge Two Sorted Lists  
24 Swap Nodes in Pairs  
25 Reverse Nodes in k-Group  
61 Rotate List  
138 Copy List with Random Pointer  
141 Linked List Cycle  
142 Linked List Cycle II  
143 Reorder List  
147 Insertion Sort List  
148 Sort List  
160 Intersection of Two Linked Lists  
203 Remove Linked List Elements  
206 Reverse Linked List  
234 Palindrome Linked List  
237 Delete Node in a Linked List  

* * *
#### 19. Remove Nth Node From End of List 

**Description**   
>Given a linked list, remove the nth node from the end of list and return its head.  
For example,  
   Given linked list: `1->2->3->4->5`, and n = 2.  
   After removing the second node from the end, the linked list becomes `1->2->3->5`.


**Solution**  
**思路**  
可以先遍历一次，求出链表的长度，然后遍历第二次，使目标距离链表尾端为n即可。
如果要求仅遍历一次，可用快慢指针法，快指针先走到n的位置，然后slow and fast同时走，fast为null时，slow距离为n。
这样编号：  
```
0    1    2    3    4     5   ：跳到这儿走了多少步
1 -> 2 -> 3 -> 4 -> 5 -> null
5    4    3    2    1     0   ：距离末尾的距离

```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fast = head;
        int count = 0;
        while (count < n) {
            fast = fast.next;
            count++;
        }
        if (fast == null) {
            return head.next;
        }
        fast = fast.next;
        ListNode slow = head;
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```
* * *
#### 21. Merge Two Sorted Lists 

**Description**   
>Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.  
合并有序链表。

**Solution**  
**思路**   
construct a dummy node as head。一个tail变量指向head，合并的过程中tail.next指向下一个合适的节点。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode tail = head;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                tail.next = l1;
                l1 = l1.next;
            } else {
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }
        if (l1 != null) {
            tail.next= l1;
        }
        if (l2 != null) {
            tail.next = l2;
        }
        return head.next;
    }
}
```

#### 24. Swap Nodes in Pairs 

**Description**   
>Given a linked list, swap every two adjacent nodes and return its head.  
>For example,  
>Given `1->2->3->4`, you should return the list as `2->1->4->3`.  

**Solution**  
**思路**   
dummy node + 快慢指针。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode left = dummy;
        ListNode right = dummy.next;
        while (right != null && right.next != null) {
           left.next = right.next;
           right.next = right.next.next;
           left.next.next = right;
           left = right;
           right = right.next;
        }
        return dummy.next;
    }
}
```
* * *
#### 25. Reverse Nodes in k-Group 

**Description**   
>Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.  
If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.  
You may not alter the values in the nodes, only nodes itself may be changed.  
Only constant memory is allowed.  
For example,
Given this linked list: `1->2->3->4->5`  
For k = 2, you should return: `2->1->4->3->5`  
For k = 3, you should return: `3->2->1->4->5`  

**Solution**  
**思路**  
这题比较负责，最好模块化进行。咱们分解一下。
 - 首先，快慢指针找到group的head and tail
 - 然后，将group反序，将tail重新定位到group的尾部。
 - 若move过程中遇到null(包括group最后一个)，结束group反序过程。    
 
**代码，包括测试部分**  

```java

public class Solution25 {
    public class ListNode {
        int val;
        ListNode next;
        ListNode(int x) { 
            val = x; 
        }
    }
    public ListNode move(ListNode tailNode, int k) {
        int count = 0;
        while (count < k && tailNode != null) {
            count++;
            tailNode = tailNode.next;
        }
        return tailNode;
    }
    public ListNode reverse(ListNode tail, ListNode end) {
        ListNode slow = tail.next;
        ListNode fast = slow.next;
        ListNode newTail = slow;
        tail.next = end;
        slow.next = end.next;
        while (slow != end) {
            ListNode temp = fast.next;
            fast.next = slow;
            slow = fast;
            fast = temp;
        }
        return newTail;
    } 
    public ListNode reverseKGroup(ListNode head, int k) {
        if (k == 1) {
            return head;
        }
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode tail = dummy;
        while (tail != null) {
            ListNode kthNode = move(tail, k);
            if (kthNode == null) {
                break;
            }
            tail = reverse(tail, kthNode);
        }
        return dummy.next;
    }
    
    public ListNode arrayToList(int[] array) {
        ListNode head = new ListNode(array[0]);
        ListNode prev = head;
        for (int i = 1; i < array.length; i++) {
            ListNode current = new ListNode(array[i]);
            prev.next = current;
            prev = current;
        }
        return head;
    }
    public void printList(ListNode head) {
        while (head != null) {
            System.out.print(head.val + " -> ");
            head = head.next;
        }
        System.out.println("null");
    }
    public static void main(String[] args) {
        Solution25 s25 = new Solution25();
        int[] nums = new int[10];
        for (int i = 0; i < nums.length; i++) {
            nums[i] = i + 1;
        }
        ListNode head = s25.arrayToList(nums);
        ListNode newHead = s25.reverseKGroup(head, 1);
        s25.printList(newHead);
    }
}
```
* * *
#### 61. Rotate List 

**Description**   
>Given a list, rotate the list to the right by k places, where k is non-negative.  
For example:  
Given `1->2->3->4->5->NULL` and k = 2,  
return `4->5->1->2->3->NULL`  

**Solution**  
**思路**  
两步扫描。第一步扫描计算链表的length。
第二步扫描，fast先走k步，然后slow and fast pointer同时走，fast == null时停止   
**代码**  
```java
public class Solution {
    public int getLength(ListNode head) {
        int length = 0;
        while (head != null) {
            length++;
            head = head.next;
        }
        return length;
    }
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode slow = head;
        ListNode fast = head;
        int count = 0;
        int length = getLength(head);
        k = k % length;
        while (count < k) {
            count++;
            fast = fast.next;
        }
        // slow 最后停到距离`null` k + 1位置
        while (fast != null && fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        fast.next = head;
        ListNode newHead = slow.next;
        slow.next = null;
        return newHead;
    }
}
```
* * *
#### 138. Copy List with Random Pointer 

__Description__   
>A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null. 
Return a deep copy of the list.    
深度复制链表。  

__Solution__    
**思路1**   
将<original, copy> put into hashtable，利用hashtable找到random指针。  
```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
import java.util.*;
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) {
            return null;
        }
        Hashtable<RandomListNode, RandomListNode> hs = new Hashtable();
        RandomListNode tail1 = head;
        RandomListNode dummy = new RandomListNode(-1);
        RandomListNode tail2 = dummy;
        while (tail1 != null) {
            RandomListNode node = new RandomListNode(tail1.label);
            tail2.next = node;
            hs.put(tail1, node);
            tail1 = tail1.next;
            tail2 = tail2.next;
        }
        tail1 = head;
        while (tail1 != null) {
            if (tail1.random != null) {
                hs.get(tail1).random = hs.get(tail1.random);
            }
            tail1 = tail1.next;
        }
        return dummy.next;
    }
}
```
**error: 将label写成val，将RandomListNode写成ListNode.**   
**思路2：**  
copy list and split。将复制的node放在original node之后，这样就能用next指针找到random指向的对象了。完成之后将链表拆成两个。  
copy list:
```
1 -> 1' -> 2 -> 2' -> 3 -> 3'
```
copy random: 
```
node.next.random = node.random.next;
```
split list:
```
1 -> 2 -> 3
1' -> 2' -> 3'
```
```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public void copyList(RandomListNode head) {
        while (head != null) {
            RandomListNode node = new RandomListNode(head.label);
            node.next = head.next;
            head.next = node;
            head = node.next;
        }
    }
    public void randomList(RandomListNode head) {
        while (head != null) {
            if (head.random != null) {
                head.next.random = head.random.next;
            }
            head = head.next.next;
        }
    }
    public RandomListNode split(RandomListNode head) {
        RandomListNode newHead = head.next;
        while (head != null) {
            RandomListNode temp = head.next;
            head.next = temp.next;
            head = head.next;
            if (temp.next != null) {
                temp.next = temp.next.next;
            }
        }
        return newHead;
    }
    public RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) {
            return null;
        }
        copyList(head);
        randomList(head);
        return split(head);
    }
}
```
* * *
#### 141. Linked List Cycle 

**Description**   
>Given a linked list, determine if it has a cycle in it.  
Follow up:  
Can you solve it without using extra space?  

**Solution**  
**思路**  
快慢指针法。若链表有环，快慢指针肯定会相遇。若fast遇到null退出，则无环。
**代码**  
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
    public boolean hasCycle(ListNode head) {
        // slow and fast pointer
        if (head == null || head.next == null) {
            return false;
        }
        ListNode fast = head.next;
        ListNode slow = head;
        while(fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
}
```
* * *
#### 142. Linked List Cycle II 

__Description__   
>Given a linked list, return the node where the cycle begins. If there is no cycle, return null.  
Note: Do not modify the linked list.  
Follow up:  
Can you solve it without using extra space?  

__Solution__    
**思路**  
快慢指针法。`fast` and `slow`从`head`同时出发，slow with step size of 1, fast with step size 2，第一次相遇时设距离环开始点为x，设环开始点距离`head`为m，环长度为n，则有：  
-> 2 * ( m + x ) = m + n + x;  
-> m + x = n  
-> n - x = m  
所以，`fast`目前距离环开始点恰好为m。`slow`回到起点重新开始走，步长都为1。两者再次相遇时，位置恰好在环开始点。

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
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (slow == fast) {
                break;
            }
        }
        if (fast == null || fast.next == null) {
            return null; // no cycle;
        }
        slow = head;
        while (slow != fast) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```
**有没有想过为什么初始化slow = fast = head呢，若slow = head, fast = head.next会发生什么。**
* * *

#### 143. Reorder List 

**Description**   
>Given a singly linked list L: L0→L1→…→Ln-1→Ln,  
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…  
You must do this in-place without altering the nodes' values.  
For example,  
Given `{1,2,3,4}`, reorder it to `{1,4,2,3}`.   

**Solution**   
**思路**  
三步。  
先用快慢指针将链表拆成两个，reverse the second one。将两个链表合并。  
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverse(ListNode head) {
        ListNode tail = null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = tail;
            tail = head;
            head = temp;
        }
       return tail;
    }
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode head2 = reverse(slow.next);
        slow.next = null;
        ListNode dummy = new ListNode (-1);
        ListNode tail = dummy;
        while (head != null && head2 != null) {
            tail.next = head;
            head = head.next;
            tail = tail.next;
            tail.next= head2;
            head2 = head2.next;
            tail = tail.next;
        }
        tail.next = head;
    }
}
```
* * *
#### 147. Insertion Sort List  
**Description**   
>Sort a linked list using insertion sort.  
插入排序。   

**Solution**    
**思路**   
原理不多赘述。  
记录一个prev变量，若current.val >= prev.val,直接跳过。  
**代码**  
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode insertionSortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(Integer.MIN_VALUE);
        dummy.next = head;
        ListNode prev = head;
        ListNode current = head.next;
        while (current != null) {
            if (current.val >= prev.val) {
                prev = current;
                current = current.next;
            } else {
                ListNode p1 = dummy;
                ListNode p2 = dummy.next; // p2 = head  wrong answer。
                while (current.val >= p2.val) {
                    p1 = p1.next;
                    p2 = p2.next;
                }
                ListNode temp = current.next;
                prev.next = current.next;
                p1.next = current;
                current.next = p2;
                current = temp;
            }
        }
        return dummy.next;
    }
}
```
* * *

#### 148. Sort List 

**Description**   
>Sort a linked list in O(n log n) time using constant space complexity.  

**Solution**  
**思路**  
归并排序。partition时需注意。
两种快慢指针方法：  
```
0    1    2    3    4    5     6      ：跳到这儿走了多少步
D -> 1 -> 2 -> 3 -> 4 -> 5 -> null
==================
slow = head;
fast = head.next;
slow: 1 2 3 4 5
fast: 2 4 6 8 10
==================
slow = head;
fast = head;
slow: 1 2 3 4 5 
fast: 1 3 5 7 9
```
陷阱：
	partition时，当node数目为两个时，一定要分开为两部分。fast = head.next, slow = head才可以。fast = head, slow = head，会出现死循环，因为不能讲list分为两部分。
	partition时，slow.next = null。否则，不能将list截成两段。
	
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        // partition to two list, fast and slow pointers
        ListNode head2 = partition(head);
        // sort to partitions
        ListNode sorted1 = sortList(head);
        ListNode sorted2 = sortList(head2);
        // merge two partitions
        ListNode newHead = merge(sorted1, sorted2);
        return newHead;
    }
    public ListNode partition(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode fast = head.next;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode newHead = slow.next;
        slow.next = null;
        return newHead;
    }
    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummy = new ListNode(-1);
        ListNode tail = dummy;
        while (head1 != null && head2 != null) {
            if (head1.val < head2.val) {
                tail.next = head1;
                head1 = head1.next;
            } else {
                tail.next = head2;
                head2 = head2.next;
            } 
            tail = tail.next;
        }
        if (head1 != null) {
            tail.next = head1;
        } 
        if (head2 != null) {
            tail.next = head2;
        }
        return dummy.next;
    }
}
```
* * * 
#### 160. Intersection of Two Linked Lists 

**Description**   
>Write a program to find the node at which the intersection of two singly linked lists begins.
For example, the following two linked lists:
```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```
>begin to intersect at node c1.  

**Solution**  
**思路**  
方法1: HashMap，复杂度O(n)时间和O(n)空间。  
方法2： 类比找环。将其中一个head与tail相连。用找环开始位置的方法。也可以不连，逻辑上不太连贯。当快慢指针走到链表尾部，转换到另一个head重新开始走。  
**方法1 Hash**  
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> hs = new HashSet<ListNode>();
        while (headA != null) {
            hs.add(headA);
            headA = headA.next;
        }
        while (headB != null) {
            if (hs.contains(headB)) {
                return headB;
            }
            headB = headB.next;
        }
        return null;
    }
}
```
**方法2 find cycle start point**   

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getTail(ListNode head) {
        while (head != null && head.next != null) {
            head = head.next;
        }
        return head;
    }
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        ListNode tailA = getTail(headA);
        ListNode tailB = getTail(headB);
        if (tailA != tailB) {
            return null;
        }
        ListNode currentA = headA;
        ListNode currentB = headB;
        while (currentA != currentB) {
            currentA = currentA.next;
            currentB = currentB.next;
            if (currentA == null) {
                currentA = headB;
            } 
            if (currentB == null) {
                currentB = headA;
            } 
        }
        return currentA;
    }
}
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
       
        ListNode currentA = headA;
        ListNode currentB = headB;
        while (currentA != null || currentB != null) { 
            if (currentA == null) {
                currentA = headB;
            } 
            if (currentB == null) {
                currentB = headA;
            } 
            if (currentA == currentB) {
                return currentA;
            }
            
            currentA = currentA.next;
            currentB = currentB.next;
        }
        return null;
    }
}
```
* * * 
#### 203. Remove Linked List Elements 

**Description**   
>Remove all elements from a linked list of integers that have value val.  
Example  
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6  
Return: 1 --> 2 --> 3 --> 4 --> 5  

**Solution**  
**思路**  
建立一个`sentinel` node (`tail` in my solution)来操作删除。没啥好说的。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return null;
        }
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode tail = dummy;
        while(head != null) {
            ListNode next = head.next;
            if (head.val == val) {
                tail.next = next;
                head = next;
            } else {
                head = next;
                tail = tail.next;
            }
        }
        return dummy.next;
    }
}
```
#### 206. Reverse Linked List 

**Description**   
>Reverse a singly linked list.  
将链表反序。  

**Solution**  
**思路**  
每次将`head`指向前一个node。建立一个`sentinel`节点辅助指向。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode tail = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = tail;
            tail = head;
            head = next;
        }
        return tail;
    }
}
```
**Recursive**    
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode tail = head.next;
        head.next = null;
        ListNode newHead = reverseList(tail);
        tail.next = head;
        return newHead;
    }
}
```
* * *
变量赋值之前，一定要注意哪些变量是变得。比如147题24行，将p2 = head。这里的head的位置是变化的，导致出错。而dummy总在队头，应为p2 = dummy.next;  
因此若片p1和p2为相邻指针，p1 = someNode, p2 = p1.next比较好，避免出错。一定要避免将p1,p2分别赋值为两个node.  
slow = head, fast = slow or slow.next is a good question to dsicuss. For detecting cycle, slow = fast = head is better. For other problem, it seems like fast = slow.next is better.   


*** 
MeMo：  
09/23/2016   1st recition DONE.   

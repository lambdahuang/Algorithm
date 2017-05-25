##142. Linked List Cycle II
__QUESTION__
--
Given a linked list, return the node where the cycle begins. If there is no cycle, return **null**.

Note: **Do not modify the linked list.**

__ANALYSIS__
--
To find the begins of a cycle in a linkedlist is a complex question. However, we can divide and conquer it. What answer we can have? **null** in the case that there is no cycle, or a pointer refers to the begin of the cycle.

So let's divide our question into 2 different sub-question: 1, detect whether or not has cycle; 2. If it has, find the begin.

To solve the first question is relatively easy, we can simply use two pointer giving there different moving speed, let's one is twice of another. If there is no cycle, the fast pointer would definitly reach the tail first, otherwise, they would meet in some point in the cycle. **This is simple remainder theorem.**

But how to solve the second question? Let's divde it again, how about we first find what would the number of cycle elements be? Well, this is kind of mathematica question. It is obvious, sigle meeting point can't give us an equation! What if we have two meet point? after first meeting, let's say we iterate x steps, and the two points meet again!


The fast pointer moved 2 * x steps.

The slow pointer moved x steps.

Let's denote the number of elements in the cycle is R.

So we have:

```
2x = x mod R
x = 0 mod R
x = R
```

**x is the iteration steps number between first and second meeting!**

Now, we have the number of elements in the cycle. How can we find the begin? Do we need to continue our work on the math? Answer is **NO!**

Let's use slow and fast pointers again! But the difference is that we not assign them with different speed but different start pointers. For the slow pointer, we simply let him start at the head. **For the fast pointer, we can let him start at the Rth element!** Therefore, they would meet again when the slow pointer reach to the begin of the cycle, and the reason is that the fast point are now at the begin + R element in the cycle, which is the begin! Easy peasy, we find the begin of the cycle!

__ANSWER__
--

```
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
        if(head == null) return null;
        ListNode tmpf = head;
        ListNode tmps = head;
        int step = 0;
        int first_met = -1;
        int second_met = -1;

        while(true)
        {
            tmpf = tmpf.next;
            if(tmpf != null)
                tmpf = tmpf.next;
            else
                break;
            tmps =tmps.next;
            if(tmpf == null)
                break;
            if(tmpf == tmps)
            {
                if(first_met == -1)
                    first_met = step;
                else
                {
                    second_met = step;
                    break;
                }
            }
            step ++;
        }
        if(tmpf == null)
            return tmpf;
        
        int round = second_met - first_met;
        tmps = head;
        tmpf = head;
        for(int i = 0; i < round; i++)
        {
            tmpf = tmpf.next;
        }
        while(tmps != tmpf)
        {
            tmps = tmps.next;
            tmpf = tmpf.next;
        }
        return tmps;
    }
}
```

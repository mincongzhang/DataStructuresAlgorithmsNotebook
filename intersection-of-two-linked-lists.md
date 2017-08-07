### Intersection of Two Linked Lists

Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.

https://leetcode.com/problems/intersection-of-two-linked-lists/description/

```
Solution 1
1.get size of A, B
2.Check diff, then move A/B with diff node nums
3.Loop together and find the first same node
```

```
Solution 2
1.Save to stack
2.pop from stack and check last equal node

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        std::stack<ListNode *> s_a;
        std::stack<ListNode *> s_b;
        ListNode * it_a = headA;
        while(it_a){
            s_a.push(it_a);
            it_a = it_a->next;
        }
        
        ListNode * it_b = headB;
        while(it_b){
            s_b.push(it_b);
            it_b = it_b->next;
        }
        
        ListNode * last_eq = NULL;
        while(!s_a.empty() && !s_b.empty()){
            if(s_a.top()==s_b.top()){
                last_eq = s_a.top();
            } else {
                return last_eq;
            }
            
            s_a.pop();
            s_b.pop();
        }
        
        return last_eq;
    }
};

```
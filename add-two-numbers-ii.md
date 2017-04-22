### Add Two Numbers II

You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

e.g.  
Input: \(7 -&gt; 2 -&gt; 4 -&gt; 3\) + \(5 -&gt; 6 -&gt; 4\)  
Output: 7 -&gt; 8 -&gt; 0 -&gt; 7

https://leetcode.com/problems/add-two-numbers-ii 

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

 //Solution1:reverse 2 lists, add, and reverse back
 //Solution2:use stack

#include <stack>
class Solution {
private:
    void pushToStack(ListNode* n, std::stack<int> & s){
        while(n){
            s.push(n->val);
            n = n->next;
        }        
    }

    void insert(ListNode* n, ListNode* i){
       i->next = n->next;
       n->next = i;
    }

    void insert(ListNode* n, std::stack<int>& s, int overhead){
        while(!s.empty()){
            int val = overhead+s.top();

            if(val>=10){
                overhead = val/10;
                val-=10;
            } else {
                overhead = 0;
            }

            insert(n,new ListNode(val));
            s.pop();
        }

        if(overhead!=0){
            insert(n,new ListNode(overhead));
        }
    }

public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        std::stack<int> s1;
        std::stack<int> s2;

        pushToStack(l1,s1);
        pushToStack(l2,s2);

        int overhead = 0;
        ListNode * pre_head = new ListNode(0);
        while(!s1.empty() && !s2.empty()){
            int val = s1.top()+s2.top() + overhead;
            s1.pop();
            s2.pop();

            //deal with overhead
            if(val>=10){
                overhead = val/10;
                val-=10;
            } else {
                overhead = 0;
            }

            ListNode * cur_node = new ListNode(val);
            insert(pre_head,cur_node);
        }

        if(s1.empty() && s2.empty() &&overhead!=0){
            insert(pre_head,new ListNode(overhead));
        }

        //insert remaining into list
        if(!s1.empty()) {
            insert(pre_head,s1,overhead);
        } else if(!s2.empty()) {
            insert(pre_head,s2,overhead);
        }

        ListNode * head = pre_head->next;
        delete pre_head;
        return head;

    }
};
```




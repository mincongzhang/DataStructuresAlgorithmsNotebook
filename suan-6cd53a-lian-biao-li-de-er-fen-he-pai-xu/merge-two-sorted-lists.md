
###  Merge Two Sorted Lists
Merge two sorted (ascending) linked lists and return it as a new sorted list. The new sorted list should be made by splicing together the nodes of the two lists and sorted in ascending order.
http://www.lintcode.com/en/problem/merge-two-sorted-lists/

```
#include <limits.h>
class Solution {
private:
  void append(ListNode * cur_node,const int & val){
    ListNode * next_node = new ListNode(val);
    if(cur_node == NULL){
        std::cout<<"cur: null"<<std::endl;
        return;
    } else {
        next_node->next = cur_node->next;
        cur_node->next = next_node;
    }
  }


public:
  ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {

    ListNode * l1_prehead = new ListNode(INT_MIN);
    l1_prehead->next = l1;

    ListNode * l1_it = l1_prehead;
    ListNode * l2_it = l2;

    while(l2_it != NULL){
      //NEXT == NULL

      if(l1_it->next == NULL){
        
        l1_it->next = l2_it;
        break;
      }

      //NEXT < L2_NODE
      if(l1_it->next->val < l2_it->val){
        if(l1_it->next->next == NULL){
          append(l1_it->next,l2_it->val);
          l2_it = l2_it->next;
        }
      //NEXT >= L2_NODE
      } else {
        append(l1_it,l2_it->val);
        l2_it = l2_it->next;
      }

      l1_it = l1_it->next;

    }

    return l1_prehead->next;
  }
};
```

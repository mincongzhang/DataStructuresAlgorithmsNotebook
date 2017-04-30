### Reverse Linked List

Reverse a linked list.

https://leetcode.com/problems/reverse-linked-list/\#/description

http://www.lintcode.com/en/problem/reverse-linked-list/

```
/**
 * Definition of ListNode
 * 
 * class ListNode {
 * public:
 *     int val;
 *     ListNode *next;
 * 
 *     ListNode(int val) {
 *         this->val = val;
 *         this->next = NULL;
 *     }
 * }
 */
class Solution {
  private:
  void insert(ListNode * cur_node, const int & val){
    ListNode * insert_node = new ListNode(val);
    insert_node->next = cur_node->next;
    cur_node->next = insert_node;
  }

  public:
  ListNode *reverse(ListNode *head){
    if(head == NULL) return NULL;

    ListNode * new_head = new ListNode(INT_MIN);
    while(head != NULL){
      insert(new_head,head->val);
      head = head->next;
    }
    return new_head->next;
  }
};
```




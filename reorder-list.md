### Reorder List

Given a singly linked list L: L0 → L1 → … → Ln-1 → Ln  
reorder it to: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

Example
Given 1->2->3->4->null, reorder it to 1->4->2->3->null.

[http://www.lintcode.com/en/problem/reorder-list/](http://www.lintcode.com/en/problem/reorder-list/)

```
/**
 * Definition of ListNode
 * class ListNode {
 * public:
 *     int val;
 *     ListNode *next;
 *     ListNode(int val) {
 *         this->val = val;
 *         this->next = NULL;
 *     }
 * }
 */
class Solution {
typedef ListNode * ListNodePtr;

private:
  ListNode * reverse(ListNode * head){
    ListNode reverse_head(INT_MIN);
    ListNode * reverse_head_iter = &reverse_head;

    while(head){
        ListNode * head_next = head->next;
        head->next = reverse_head_iter->next;
        reverse_head_iter->next = head;
        head = head_next;
    }

    return reverse_head.next;
  };

  void append(ListNodePtr & node, ListNodePtr & append_node){
      if(!node || !append_node) return;

      node->next = append_node;
      node = node->next;
      append_node = append_node->next;
  }

  ListNode * merge(ListNode * head1, ListNode * head2){
      ListNode pre_head(INT_MIN);
      ListNode * pre_head_iter = &pre_head;

      while(head1 && head2){
          append(pre_head_iter,head1);
          append(pre_head_iter,head2);
      }

      if(head1){
          append(pre_head_iter,head1);
      } 
      if(head2){
          append(pre_head_iter,head2);
      }

      return pre_head.next;
  }

public:
  /**
   * @param head: The first node of linked list.
   * @return: void
   */
  void reorderList(ListNode *head) {
    if(head == NULL || head->next == NULL) return;

    ListNode * ahead = head;
    ListNode * mid   = head;
    while(ahead->next && ahead->next->next){
      mid = mid->next;
      ahead = ahead->next->next;
    }

    ListNode * mid_head = mid->next;
    mid->next = NULL;
    merge(head,reverse(mid_head));
  }
};
```




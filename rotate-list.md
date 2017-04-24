### Rotate List
Given a list, rotate the list to the right by k places, where k is non-negative.
Example
Given 1->2->3->4->5 and k = 2, return 4->5->1->2->3.

```
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
  /**
   * @param head: the list
   * @param k: rotate to the right k places
   * @return: the list after rotation
   */
  ListNode *rotateRight(ListNode *head, int k) {
    if(k == 0) return head;
    if(head == NULL || head->next == NULL) return head;

    //NOTE: size should start from 1
    int size = 1;
    ListNode * list_it = head;
    while(list_it->next != NULL){
      list_it = list_it->next;
      size++;
    }
    list_it->next = head;

    //NOTE: should be k%size rather than size%k
    //Handle k>size, e.g. 0->1->null, 100
    int tail_pos = size - k;
    ListNode * rotate_tail = head;
    while(--tail_pos > 0){
      rotate_tail = rotate_tail->next;
    }

    ListNode * rotate_head = rotate_tail->next;
    rotate_tail->next = NULL;

    return rotate_head;
  }
};
```
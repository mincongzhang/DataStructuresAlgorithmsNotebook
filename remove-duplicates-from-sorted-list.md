
###  Remove Duplicates from Sorted List
Given a sorted linked list, delete all duplicates such that each element appear only once.
http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-list/

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
public:
    /**
     * @param head: The first node of linked list.
     * @return: head node
     */
    ListNode *deleteDuplicates(ListNode *head) {
        if(head==NULL || head->next==NULL) return head;
        
        ListNode * next_tmp;
        ListNode * cur_tmp(head);
        while(cur_tmp != NULL && cur_tmp->next!=NULL){
            
            if(cur_tmp->val == cur_tmp->next->val){
                next_tmp = cur_tmp->next->next;
                delete cur_tmp->next;
                cur_tmp->next = next_tmp;
            } else {
                cur_tmp = cur_tmp->next;
            }
        }
        
        return head;
    }
};
```
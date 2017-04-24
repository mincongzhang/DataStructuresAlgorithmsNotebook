
###  Sort List(Quick Sort)
Sort a linked list in O(n log n) time using constant space complexity
http://www.lintcode.com/en/problem/sort-list/

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
void swap(ListNodePtr & node1, ListNodePtr & node2){
    if(node1 == NULL || node2 == NULL) return;
    
    int tmp_val = node1->val;
    node1->val = node2->val;
    node2->val = tmp_val;
}

void qsort(ListNodePtr begin, ListNodePtr end){
    if(begin == end) return;
    if(begin->next == NULL) return;
    
    //Handle 2 Nodes
    if(begin->next == end){
        if(begin->val >= end->val){
            swap(begin,end);
        }
        return;
    }
    
    ListNodePtr iter = begin;
    int compare_val = iter->val;

    ListNodePtr mid = begin;
    ListNodePtr larger = NULL;

    while(iter != end){
        // iter >= begin
        if(iter->val >= compare_val){
            if(larger == NULL){
                larger = iter;
            }
        } else {
            // iter < begin
            if(larger != NULL){
                mid = larger;
                swap(larger,iter);
                if(larger->next != NULL && larger->next->val >= compare_val){
                    larger = larger->next;
                } else {
                    larger = NULL;
                }
            }
        }
        iter = iter->next;
    }
    
    //Handle End
    if(end){
        if(end->val <= compare_val){
            mid = mid->next;
            swap(end,mid);
        }
    }
    
    qsort(begin, mid);
    qsort(mid->next,end);
}
    
public:
    /**
     * @param head: The first node of linked list.
     * @return: You should return the head of the sorted linked list,
                    using constant space complexity.
     */
    ListNode *sortList(ListNode *head) {
        qsort(head,NULL);
        
        return head;
    }
};
```


###  Sort List(Merge Sort)
Sort a linked list in O(n log n) time using constant space complexity
http://www.lintcode.com/en/problem/sort-list/


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
private:
  ListNode * merge(ListNode * head1, ListNode * head2){
    ListNode pre_head(INT_MIN);
    pre_head.next = NULL;
    ListNode * pre_head_iter = &pre_head;

    while(head1 && head2){
      if(head1->val > head2->val){
        pre_head_iter->next = head2;
        head2 = head2->next;
      } else {
        pre_head_iter->next = head1;
        head1 = head1->next;
      }
      pre_head_iter = pre_head_iter->next;
    }

    if(head1){
      pre_head_iter->next = head1;
    } else {
      pre_head_iter->next = head2;
    }

    return pre_head.next;
  }

public:
  /**
   * @param head: The first node of linked list.
   * @return: You should return the head of the sorted linked list,
                    using constant space complexity.
  */
  ListNode *sortList(ListNode *head) {
    if(head == NULL || head->next == NULL) return head;

    ListNode * fast = head;
    ListNode * mid  = head;
    //NOTE: should check fast->next first, then fast->next->next
    while(fast->next != NULL && fast->next->next != NULL){
      mid = mid->next;
      fast = fast->next->next;
    }

    //Split
    ListNode * mid_head = mid->next;
    mid->next = NULL;
    return merge(sortList(head),sortList(mid_head));
  }
};
```
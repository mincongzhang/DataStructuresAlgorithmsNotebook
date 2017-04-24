###  Partition List
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
You should preserve the original relative order of the nodes in each of the two partitions.

http://www.lintcode.com/en/problem/partition-list/#

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
     * @param x: an integer
     * @return: a ListNode 
     */
    ListNode *partition(ListNode *head, int x) {
        //Idea:
        //create 2 lists with less/greater, appending
        //In the end conect 2 lists
        
        ListNode * less_x = new ListNode(INT_MIN);
        ListNode * greater_equal_x = new ListNode(INT_MIN);
        
        ListNode * less_x_it = less_x;
        ListNode * greater_equal_x_it = greater_equal_x;
        
        while(head){
            if(head->val < x){
                less_x_it->next = head;
                less_x_it = less_x_it->next;
            } else {
                greater_equal_x_it->next = head;
                greater_equal_x_it = greater_equal_x_it->next;

            }

            head = head->next;
        }

        less_x_it->next = greater_equal_x->next;
        greater_equal_x_it->next = NULL; //NOTE: be careful
        
        ListNode * ret_list = less_x->next;
        delete less_x;
        delete greater_equal_x;


        return ret_list;
    }
};
```

```
//General Partition solution
class Solution {
private:
  void swap(ListNode * node1,ListNode * node2){
    if(node1==NULL || node2==NULL){return;}

    int tmp = node1->val;
    node1->val = node2->val;
    node2->val = tmp;
  }

public:
  ListNode *partition(ListNode *head, int x) {
    ListNode * list_it = head;
    ListNode * node_to_update = NULL;
    while(list_it != NULL){

      // >= x
      if(list_it->val >= x){
        if(node_to_update == NULL){
          node_to_update = list_it;
        }

      }
      // < x
      else {
        if(node_to_update != NULL){
          swap(node_to_update,list_it);
          if(node_to_update->next && node_to_update->next->val < x){
            node_to_update = NULL;
          } else {
            node_to_update = node_to_update->next;
          }
        }
      }

      list_it = list_it->next;
    }

    return head;
  }
};
```

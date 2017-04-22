
### Remove Nth Node From End of List
Given a linked list, remove the nth node from the end of list and return its head.
http://www.lintcode.com/en/problem/remove-nth-node-from-end-of-list/

```
class Solution {
public:
  /**
   * @param head: The first node of linked list.
   * @param n: An integer.
   * @return: The head of linked list.
   */
  ListNode *removeNthFromEnd(ListNode *head, int n) {
    //Why use pre_head?
    //e.g.
    //list: 1->null, n=1
    //how to delete 1?
    ListNode * pre_head = new ListNode(0);
    pre_head->next = head;

    ListNode * n_ahead = pre_head;
    ListNode * n_node  = pre_head;

    //move: n_ahead
    while(n--){
      if(n_ahead){
        n_ahead = n_ahead->next;
      } else {
        std::cout<<"unhandled list with n ["<<n<<"]"<<std::endl;
        return pre_head;
      }
    }

    //n_ahead->next point to null
    while(n_ahead->next){
      n_ahead = n_ahead->next;
      n_node = n_node->next;
    }

    //delete n_node->next
    ListNode * delete_node = n_node->next;
    n_node->next = delete_node->next;
    delete delete_node;

    return pre_head->next;
  }
};
```
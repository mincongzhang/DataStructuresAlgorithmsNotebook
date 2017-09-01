### Linked List Cycle

Given a linked list, determine if it has a cycle in it.  
\(Floyd Cycle Detection Algorithm\)  
[http://www.lintcode.com/en/problem/linked-list-cycle/](http://www.lintcode.com/en/problem/linked-list-cycle/)

```
//hashtable solution

class Solution {
public:
    bool hasCycle(ListNode *head) {
        unordered_set<ListNode *> hash;
        
        while(head){
            if(head==NULL) return false;
            
            if(hash.find(head)!=hash.end()){
                return true;
            }
            
            hash.insert(head);
            head = head->next;
        }
        
        return false;
    }
};
```


```
//Accepted solution
class Solution {
public:
    /**
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
    bool hasCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;

        while (fast && fast->next) {
            slow = slow->next, fast = fast->next->next;
            if (slow == fast) {  // There is a cycle.
                return true;
            }
        }
        return false;  // No cycle.
    }
};
```

```
//My Solution
class Solution {
public:
    /**
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
    bool hasCycle(ListNode *head) {
        if(head == NULL) return false;

        ListNode * list_duplicate = head;

        head = head->next;
        while(head){
            if(head->val == list_duplicate->val){
                if(head == list_duplicate){
                    return true;
                }

                list_duplicate = head;
            }

            head = head->next;
        }

        return false;
    }
};
```




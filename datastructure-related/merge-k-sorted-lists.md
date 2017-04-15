
### Merge k Sorted Lists

Merge k sorted linked lists and return it as one sorted list.Analyze and describe its complexity.

http://www.lintcode.com/en/problem/merge-k-sorted-lists/

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
 #include <queue>
 
 namespace{
    typedef ListNode* ListNodePtr;
    
    struct Compare {
        bool operator()(const ListNodePtr & l1, const ListNodePtr & l2){
            return l1->val > l2->val;
        }
    };
 }
 
class Solution {

private:
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        ListNodePtr pre_head = new ListNode(0);
        ListNodePtr head = pre_head;
        while(l1 != NULL && l2 != NULL){
            if(l1->val <= l2->val){
                head->next = l1;
                head = head->next;
                l1 = l1->next;
                continue;
            }
            
            head->next = l2;
            head = head->next;
            l2 = l2->next;
        }
        
        if(l1){
            head->next = l1;
        } else {
            head->next = l2;
        }
        
        return pre_head->next;
    }
    
public:
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.empty()) return NULL;
        
        std::priority_queue<int,vector<ListNodePtr>, Compare> q;
        
        for(size_t i=0; i<lists.size();++i){
            if(lists[i] != NULL){
                q.push(lists[i]);
            }
        }
        
        ListNodePtr pre_head = new ListNode(0);
        ListNodePtr head = pre_head;
        while(!q.empty()){
            ListNodePtr cur = q.top();
            q.pop();
            head->next = cur;
            head = head->next;
            if(cur->next != NULL){
                cur = cur->next;
                q.push(cur);
            }
        }
        
        return pre_head->next;
    }
};
```

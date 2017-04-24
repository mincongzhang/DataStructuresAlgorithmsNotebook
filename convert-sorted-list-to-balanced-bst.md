###  Convert Sorted List to Balanced BST
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

```
//O(n) solution

class Solution {
typedef ListNode * ListNodePtr;
typedef TreeNode * TreeNodePtr;
private:
  TreeNodePtr buildBST(ListNodePtr & L, const int & begin, const int & end){
    if(begin >= end) return NULL;

    int mid = (begin+end)/2;

    TreeNodePtr left_sub_tree = buildBST(L,begin,mid);//NOTE: L will get updated inside(pass by reference)
    TreeNodePtr root = new TreeNode(L->val);
    L = L->next;
    TreeNodePtr right_sub_tree = buildBST(L,mid+1,end);

    root->left = left_sub_tree;
    root->right = right_sub_tree;
    return root;
  }

public:
  /**
   * @param head: The first node of linked list.
   * @return: a tree node
   */
  TreeNode *sortedListToBST(ListNode *L) {
    int list_size = 0;
    ListNodePtr list_iter = L;
    while(list_iter){
      list_size++;
      list_iter = list_iter->next;
    }

    return buildBST(L,0,list_size);
  }
};
```

```
//O(nlogn)
class Solution {
  typedef ListNode * ListNodePtr;
  typedef TreeNode * TreeNodePtr;

public:
  /**
   * @param head: The first node of linked list.
   * @return: a tree node
   */
  TreeNode *sortedListToBST(ListNode *L) {
    if(L == NULL) return NULL;
    if(L->next == NULL) return new TreeNode(L->val);

    ListNode pre_head(INT_MIN);
    pre_head.next = L;

    ListNodePtr pre_mid = &pre_head;
    ListNodePtr iter    = L;
    while(iter->next && iter->next->next){
      pre_mid = pre_mid->next;
      iter = iter->next->next;
    }

    ListNodePtr mid = pre_mid->next;
    pre_mid->next = NULL;

    TreeNodePtr root = new TreeNode(mid->val);
    root->left = sortedListToBST(pre_head.next);
    //NOTE: pre_head can be pre_mid so next could be NULL, thus cannot pass "L" into it
    root->right = sortedListToBST(mid->next);

    return root;
  }
};
```
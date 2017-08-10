### Populating Next Right Pointers in Each Node II

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.
For example,
Given the following binary tree,
```
         1
       /  \
      2    3
     / \    \
    4   5    7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```

https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/

```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(!root) return;
        
        TreeLinkNode * next = NULL;
        TreeLinkNode * cur  = NULL;
        queue<TreeLinkNode *> q;
        q.push(root);
        
        while(!q.empty()){
            int cur_size = q.size();
            while(cur_size>0){
                cur  = q.front();
                cur->next = next;
                next = cur;

                q.pop();
                cur_size--;               
                
                if(cur->right){
                    q.push(cur->right);
                }
                if(cur->left){
                    q.push(cur->left);
                }
            }
            next = NULL;
        }
        
        
    }
};
```
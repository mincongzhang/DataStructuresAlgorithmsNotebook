### Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.  
Return a deep copy of the list.

[http://www.lintcode.com/en/problem/copy-list-with-random-pointer/](http://www.lintcode.com/en/problem/copy-list-with-random-pointer/)

##### Hash table solution

```
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 * int label;
 * RandomListNode *next, *random;
 * RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
namespace{
  typedef RandomListNode * RandomListNodePtr;
}

class Solution {
public:
  /**
   * @param head: The head of linked list with a random pointer.
   * @return: A new head of a deep copy of the list.
   */
  RandomListNode *copyRandomList(RandomListNode *head) {
    if(head == NULL) return NULL;

    RandomListNodePtr it = head;
    RandomListNodePtr new_head = new RandomListNode(0);
    RandomListNodePtr new_it = new_head;

    std::unordered_map<RandomListNodePtr,RandomListNodePtr> hash;

    while(it){
      new_it->next = new RandomListNode(it->label);
      new_it = new_it->next;

      hash[it] = new_it;
      it = it->next;
    }

    it = head;
    while(it){
      RandomListNodePtr random = it->random;
      RandomListNodePtr new_random = hash[random];
      RandomListNodePtr new_it =  hash[it];
      new_it->random = new_random;

      it = it->next;
    }

    RandomListNodePtr ret = new_head->next;
    delete new_head;
    return ret;
  }
};
```

##### Copy to next node solution

```
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
namespace{
  typedef RandomListNode * RandomListNodePtr;
}

class Solution {
public:
  /**
   * @param head: The head of linked list with a random pointer.
   * @return: A new head of a deep copy of the list.
   */
  RandomListNode *copyRandomList(RandomListNode *head) {
    if(head == NULL) return head;

    RandomListNodePtr it = head;

    //Make a copy of each node to its "next"
    while(it){
      RandomListNodePtr duplicated_node = new RandomListNode(it->label);
      duplicated_node->next = it->next;
      it->next = duplicated_node;
      it = it->next->next;
    }

    //Update random pointer of the duplicated nodes
    it = head;
    while(it){
      RandomListNodePtr duplicated_node = it->next;
      if(it->random){ //NOTE: important checking here
        duplicated_node->random = it->random->next;
      }
      it = it->next->next;
    }

    //Split
    it = head;
    RandomListNode new_list_pre_head(INT_MIN);
    RandomListNodePtr new_list_it = &new_list_pre_head;
    while(it){
      new_list_it->next = it->next;
      new_list_it = new_list_it->next;
      it->next    = it->next->next;
      it = it->next;
    }

    return new_list_pre_head.next;
  }
};
```

#### A better way to avoid manipulate original list

```
Use a hashtable saving orig_node->new_node
loop original to copy, and get hash[orig_node]->new_node
loop again to copy the random pointer
```




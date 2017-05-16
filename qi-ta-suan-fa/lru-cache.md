### LRU Cache

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

```
Example:
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

```
//1.Hash table: key->list_iterator
//2.Maintain a recent used list (find/update to front/remove back)

namespace {
    struct KeyValue{
        int key;
        int value;
        KeyValue(int k,int v):key(k),value(v){}
    };
}

class LRUCache {
private:
    const int m_capacity;
    
    list<KeyValue> m_list;
    typedef list<KeyValue>::iterator ListIter;
    
    unordered_map<int,ListIter> m_hash;
    typedef unordered_map<int,ListIter> HashTable;
    
    void updateToRecentUsed(HashTable::iterator & hash_it, const KeyValue key_value){
        m_list.erase(hash_it->second);
        m_list.push_front(key_value);
        hash_it->second = m_list.begin();
    }
    
    void checkCapacityAndDelete(){
        if(m_hash.size()>m_capacity){
            KeyValue key_value = m_list.back();
            m_list.pop_back();
            
            HashTable::iterator hash_it = m_hash.find(key_value.key);
            if(hash_it!=m_hash.end()){
                m_hash.erase(hash_it);
            }
            
        }
    }
    
    
public:
    LRUCache(int capacity):m_capacity(capacity) {}
    
    int get(int key) {
        int get_value = -1;
        HashTable::iterator hash_it = m_hash.find(key);
        if(hash_it!=m_hash.end()){
            KeyValue key_value = *(hash_it->second);
            get_value = key_value.value;

            //update to recent used
            updateToRecentUsed(hash_it,key_value);
        }

        return get_value;
    }
    
    void put(int key, int value) {
        HashTable::iterator hash_it = m_hash.find(key);
        KeyValue key_value(key,value);
        
        std::cout<<"put:"<<key<<","<<value<<std::endl;

        //Not Found
        //insert
        if(hash_it == m_hash.end()){
            m_list.push_front(key_value);
            m_hash[key] = m_list.begin();

            checkCapacityAndDelete();
            
            std::cout<<"insert:"<<key<<","<<value<<std::endl;
            return;
        }
        
        //Found
        //update to recentd
        std::cout<<"found:"<<key<<std::endl;
        updateToRecentUsed(hash_it,key_value);
    }

};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
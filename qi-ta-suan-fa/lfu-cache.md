### LFU Cache

Design and implement a data structure for Least Frequently Used (LFU) cache.

https://leetcode.com/problems/lfu-cache/#/description

```
namespace {
    //Hashmap: Freq -> list<KeyVal>
    struct KeyVal{
        int key;
        int val;
        KeyVal(int k,int v):key(k),val(v){}
    };

    typedef list<KeyVal> KeyValList;
    typedef unordered_map<int,KeyValList> FreqMap;
    typedef KeyValList::iterator KeyValListIter;
    
    //Hashmap: Key -> Val, Freq, KeyValListIter 
    struct ValFreq{
        int val;
        int freq;
        KeyValListIter list_it;
        ValFreq(int v,int f, KeyValListIter it):val(v),freq(f),list_it(it){}
        ValFreq & operator=(const ValFreq & v){
            val = v.val;
            freq = v.freq;
            list_it = v.list_it;
            return *this;
        }
    };
    
    typedef unordered_map<int,ValFreq> KeyMap;
}

class LFUCache {
private:
    FreqMap m_freq_map;
    KeyMap m_key_map;
    priority_queue<int> m_min_pq;
    const int m_cap;
    
public:
    LFUCache(int capacity):m_cap(capacity) {}
    
    int get(int key) {
        int get_val = -1;
        KeyMap::iterator key_it = m_key_map.find(key);
        
        //Found
        if(key_it!=m_key_map.end()){
            ValFreq val_freq = key_it->second;
            get_val = val_freq.val;
        }
        
        return get_val;
    }
    
    void put(int key, int value) {
        KeyMap::iterator key_it = m_key_map.find(key);
        KeyVal key_val(key,value);
        
        //Not Found
        if(key_it==m_key_map.end()){
            m_freq_map[1].push_front(key_val);
            
            ValFreq val_freq(value,1,m_freq_map[1].begin());
            m_key_map.insert(make_pair(key,val_freq));
            return;    
        }
        
        //Found
        //Update FreqMap
        //update val
        ValFreq val_freq = key_it->second;
        val_freq.val = value; 
        
        //update freq
        m_freq_map[val_freq.freq].erase(val_freq.list_it);
        val_freq.freq++;    
        
        //update list_it
        m_freq_map[val_freq.freq].push_front(key_val);
        val_freq.list_it = m_freq_map[val_freq.freq].begin(); 
        
        //somehow cannot use m_key_map[key] = val_freq
        m_key_map.erase(key);
        m_key_map.insert(make_pair(key,val_freq));
        
        //TODO: update min_pq
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
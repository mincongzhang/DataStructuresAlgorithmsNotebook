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
  };

  typedef unordered_map<int,ValFreq> KeyMap;
}

class LFUCache {
private:
  FreqMap m_freq_map;
  KeyMap m_key_map;
  priority_queue<int, vector<int>, std::greater<int> > m_min_freq_pq;
  const int m_cap;

  void updateFreq(const KeyVal & key_val,ValFreq & val_freq){
    //update val(if changed)
    val_freq.val = key_val.val;

    //std::cout<<"updateFreq: key val freq("<<key_val.key<<","<<key_val.val<<","<<val_freq.freq+1<<")"<<std::endl;

    //update freq
    FreqMap::iterator freq_it = m_freq_map.find(val_freq.freq);
    if(freq_it!=m_freq_map.end()){
      (freq_it->second).erase(val_freq.list_it);
      if((freq_it->second).empty()) {
        m_freq_map.erase(val_freq.freq);
      }
    }
    val_freq.freq++;

    //update list_it
    m_freq_map[val_freq.freq].push_front(key_val);
    val_freq.list_it = m_freq_map[val_freq.freq].begin();

    //std::cout<<"Freq Map:"<<std::endl;
    //for(FreqMap::const_iterator it=m_freq_map.begin();it!=m_freq_map.end(); ++it){
      //std::cout<<"freq:"<<it->first<<", list size:"<<(it->second).size()<<std::endl;
    //}

    //erase empty min freq, update min freq
    updateMinFreqPQueue(val_freq.freq);
  }

  void updateMinFreqPQueue(int cur_freq){
    if(m_min_freq_pq.empty()){
      //std::cout<<"push first min_freq:"<<cur_freq<<std::endl;
      m_min_freq_pq.push(cur_freq);
      return;
    }

    if(cur_freq < m_min_freq_pq.top()){
      //std::cout<<"updating min_freq:"<<cur_freq<<std::endl;
      m_min_freq_pq.push(cur_freq);
    }
  }

  void eraseEmptyMinFreq(){
    while(!m_min_freq_pq.empty()){
      int min_freq = m_min_freq_pq.top();
      FreqMap::const_iterator it = m_freq_map.find(min_freq);

      if(it==m_freq_map.end()){
        m_min_freq_pq.pop();
        continue;
      }

      if((it->second).empty()){
        m_freq_map.erase(it);
        m_min_freq_pq.pop();
        continue;
      }

      //It's a valid min_freq
      return;
    }

    //m_min_freq_pq is totally cleared
    int min_freq = INT_MAX;
    for(FreqMap::const_iterator it=m_freq_map.begin();it!=m_freq_map.end(); ++it){
      min_freq = std::min(min_freq,it->first);
    }
    //std::cout<<"m_min_freq_pq is totally cleared, updating a new min_freq:"<<min_freq<<std::endl;
    m_min_freq_pq.push(min_freq);

  }

  void checkCapAndEraseLFU(){
    if(m_key_map.size()<m_cap){
      return;
    }

    //std::cout<<"should be erasing"<<std::endl;

    if(m_min_freq_pq.empty()){
      //std::cout<<"ERROR:m_min_freq_pq.empty! fail to erase"<<std::endl;
      return;
    }

    //std::cout<<"Freq Map:"<<std::endl;
    //for(FreqMap::const_iterator it=m_freq_map.begin();it!=m_freq_map.end(); ++it){
      //std::cout<<"freq:"<<it->first<<", list size:"<<(it->second).size()<<std::endl;
    //}

    eraseEmptyMinFreq();

    if(m_min_freq_pq.empty()){
      //std::cout<<"ERROR:m_min_freq_pq.empty! fail to erase"<<std::endl;
      return;
    }

    int min_freq = m_min_freq_pq.top();
    //std::cout<<"min freq:"<<min_freq<<std::endl;
    FreqMap::iterator it = m_freq_map.find(min_freq);
    if(it!=m_freq_map.end() && !(it->second).empty()){

      int key = (it->second).back().key;
      (it->second).pop_back();
      if((it->second).empty()){
        m_freq_map.erase(it);
      }
      m_key_map.erase(key);
      return;
    }

    //std::cout<<"ERROR: FreqMap with ["<<min_freq<<"] has no elems! fail to erase"<<std::endl;

  }

public:
  LFUCache(int capacity):m_cap(capacity) {}

  int get(int key) {
    if(m_cap==0) return -1;

    //std::cout<<std::endl;
    //std::cout<<"### get("<<key<<") ###"<<std::endl;

    int get_val = -1;
    KeyMap::iterator key_it = m_key_map.find(key);

    //Found
    if(key_it!=m_key_map.end()){
      ValFreq & val_freq = key_it->second;
      KeyVal key_val(key,val_freq.val);
      updateFreq(key_val,val_freq);
      get_val = val_freq.val;
    }

    //std::cout<<"### get "<<get_val<<" ###"<<std::endl;
    return get_val;
  }

  void put(int key, int value) {
    if(m_cap==0) return;

    //std::cout<<std::endl;
    //std::cout<<"### put("<<key<<","<<value<<") ###"<<std::endl;

    KeyMap::iterator key_it = m_key_map.find(key);
    KeyVal key_val(key,value);

    //Not Found
    if(key_it==m_key_map.end()){
      checkCapAndEraseLFU();

      m_freq_map[1].push_front(key_val);

      ValFreq val_freq(value,1,m_freq_map[1].begin());
      m_key_map.insert(make_pair(key,val_freq));
      updateMinFreqPQueue(1);
      return;
    }

    //Found
    //Update Freq
    updateFreq(key_val,key_it->second);
  }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */


```
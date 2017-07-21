### Huffman

[https://en.wikipedia.org/wiki/Huffman\_coding](https://en.wikipedia.org/wiki/Huffman_coding)

![](/assets/huffman.png)

```
//map

#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <algorithm>    // std::max

namespace {
#define log(msg) do{ std::cout<<msg<<"\n"; }while(0)

	template <typename T>
	inline std::ostream& operator<< (std::ostream &os, const std::vector<T> & value ) {
		for(typename std::vector<T>::const_iterator it=value.begin(); it!=value.end(); ++it){
			os<<"["<<*it<<"]";
		}
		return os;
	}

	template <typename Key,typename Value>
	inline std::ostream& operator<< (std::ostream &os, const std::map<Key,Value> & value ) {
		os<<"[";
		for(typename std::map<Key,Value>::const_iterator it=value.begin(); it!=value.end(); ++it){
			os<<"("<<it->first<<"|"<<it->second<<")";
		}
		os<<"]";
		return os;
	}

	void split(const std::string & s, const std::string & delim, std::vector<std::string> & out){
		std::size_t start = 0, end = s.find_first_of(delim, start);

		while(end != std::string::npos){
			out.push_back(s.substr(start, end - start));
			start = end+delim.size();
			end = s.find_first_of(delim, start);
		}

		if(start != std::string::npos){
			out.push_back(s.substr(start));
		}
	}
}

int main(){

	////////////////////////////////
	// set up
	////////////////////////////////

	std::vector<std::string> v;
	v.push_back("a\t\t100100");
	v.push_back("b\t\t100101");
	v.push_back("c\t\t110001");
	v.push_back("d\t\t100000");
	v.push_back("[newline]\t\t111111");
	v.push_back("p\t\t111110");
	v.push_back("q\t\t000001");

	////////////////////////////////
	// code map
	////////////////////////////////

	int max_size(0),min_size(INT_MAX);
	std::map<std::string,std::string> code_map;
	for(int i=0;i<v.size();++i){
		std::vector<std::string> strs;
		split(v[i],"\t\t",strs);
		
		if(strs.size()!=2){
			continue; //log error
		}
		
		if(strs[0]=="[newline]"){
			strs[0]="\n";
		}
		code_map[strs[1]]=strs[0];

		max_size = std::max(max_size,int(strs[1].size()));
		min_size = std::min(min_size,int(strs[1].size()));
	}

	log("code map:\n"<<code_map);
	log("code size: ["<<min_size<<"] to ["<<max_size<<"]");

	////////////////////////////////
	// code
	////////////////////////////////
	std::string code = "111110000001100100111111100101110001111110";
	std::string decode;
	int start = 0;
	while(start<code.size()){
		std::string cur_code;
		for(int size=min_size; size<=max_size;++size){
			cur_code = code.substr(start,size);
			std::map<std::string,std::string>::const_iterator it = code_map.find(cur_code);
			if(it!=code_map.end()){
				decode += it->second;
				start+=cur_code.size();
				break;
			}
		}//for
	}

	log("decode:\n"<<decode);

	system("pause");
	return EXIT_SUCCESS;
}

```

```
//Hash map

#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>
#include <algorithm>    // std::max

namespace {
#define log(msg) do{ std::cout<<msg<<"\n"; }while(0)

	template <typename T>
	inline std::ostream& operator<< (std::ostream &os, const std::vector<T> & value ) {
		for(typename std::vector<T>::const_iterator it=value.begin(); it!=value.end(); ++it){
			os<<"["<<*it<<"]";
		}
		return os;
	}

	template <typename Key,typename Value>
	inline std::ostream& operator<< (std::ostream &os, const std::unordered_map<Key,Value> & value ) {
		os<<"[";
		for(typename std::unordered_map<Key,Value>::const_iterator it=value.begin(); it!=value.end(); ++it){
			os<<"("<<it->first<<"|"<<it->second<<")";
		}
		os<<"]";
		return os;
	}

	void split(const std::string & s, const std::string & delim, std::vector<std::string> & out){
		std::size_t start = 0, end = s.find_first_of(delim, start);

		while(end != std::string::npos){
			out.push_back(s.substr(start, end - start));
			start = end+delim.size();
			end = s.find_first_of(delim, start);
		}

		if(start != std::string::npos){
			out.push_back(s.substr(start));
		}
	}
}

int main(){

	////////////////////////////////
	// set up
	////////////////////////////////

	std::vector<std::string> v;
	v.push_back("a\t\t100100");
	v.push_back("b\t\t100101");
	v.push_back("c\t\t110001");
	v.push_back("d\t\t100000");
	v.push_back("[newline]\t\t111111");
	v.push_back("p\t\t111110");
	v.push_back("q\t\t000001");

	////////////////////////////////
	// code map
	////////////////////////////////

	int max_size(0),min_size(1000000000);
	std::unordered_map<std::string,std::string> code_map;
	for(int i=0;i<v.size();++i){
		std::vector<std::string> strs;
		split(v[i],"\t\t",strs);
		
		if(strs.size()!=2){
			continue; //log error
		}
		
		if(strs[0]=="[newline]"){
			strs[0]="\n";
		}
		code_map[strs[1]]=strs[0];

		max_size = std::max(max_size,int(strs[1].size()));
		min_size = std::min(min_size,int(strs[1].size()));
	}

	log("code map:\n"<<code_map);
	log("code size: ["<<min_size<<"] to ["<<max_size<<"]");

	////////////////////////////////
	// code
	////////////////////////////////
	std::string code = "111110000001100100111111100101110001111110";
	std::string decode;
	int start = 0;
	while(start<code.size()){
		std::string cur_code;
		for(int size=min_size; size<=max_size;++size){
			cur_code = code.substr(start,size);
			std::unordered_map<std::string,std::string>::const_iterator it = code_map.find(cur_code);
			if(it!=code_map.end()){
				decode += it->second;
				start+=cur_code.size();
				break;
			}
		}//for
	}

	log("decode:\n"<<decode);

	return EXIT_SUCCESS;
}
```

```
//Trie tree (pointer)

#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <algorithm>    // std::max
#include <limits.h>

namespace {
#define log(msg) do{ std::cout<<msg<<"\n"; }while(0)

  template <typename T>
  inline std::ostream& operator<< (std::ostream &os, const std::vector<T> & value ) {
    for(typename std::vector<T>::const_iterator it=value.begin(); it!=value.end(); ++it){
      os<<"["<<*it<<"]";
    }
    return os;
  }

  template <typename Key,typename Value>
  inline std::ostream& operator<< (std::ostream &os, const std::map<Key,Value> & value ) {
    os<<"[";
    for(typename std::map<Key,Value>::const_iterator it=value.begin(); it!=value.end(); ++it){
      os<<"("<<it->first<<"|"<<it->second<<")";
    }
    os<<"]";
    return os;
  }

  void split(const std::string & s, const std::string & delim, std::vector<std::string> & out){
    std::size_t start = 0, end = s.find_first_of(delim, start);

    while(end != std::string::npos){
      out.push_back(s.substr(start, end - start));
      start = end+delim.size();
      end = s.find_first_of(delim, start);
    }

    if(start != std::string::npos){
      out.push_back(s.substr(start));
    }
  }
}

struct Node {
  char val;
  std::map<char,Node *> children;
};

class TrieTree {
private:
  Node m_root;
  std::string m_encode;
  std::string m_decode;
  void insert(std::string code,std::string val){
    if(val.size()!=1){
      std::cout<<"unsupperted value:"<<val<<std::endl;
    }

    char c_val = val[0];

    Node * node = & m_root;
    for(int i=0; i<code.size(); ++i){
      std::map<char,Node *>::iterator node_it = node->children.find(code[i]);
      if(node_it!=node->children.end()){
        node = node_it->second;
        continue;
      }


      Node * child = new Node();
      child->val = '\0';
      node->children[code[i]]=child;
      node = child;
    }

    node->val = c_val;
  }

public:
  TrieTree(const std::vector<std::string> & code_map, const std::string & encode){
    m_encode = encode;

    for(int i=0;i<code_map.size();++i){
      std::vector<std::string> strs;
      split(code_map[i],"\t\t",strs);

      if(strs.size()!=2){
        continue; //log error
      }

      if(strs[0]=="[newline]"){
        strs[0]="\n";
      }

      log("ready to insert :"<<strs);
      this->insert(strs[1],strs[0]);
    }

  }
  std::string decode(){

    int it = 0;
    Node * node = &m_root;
    while(it<m_encode.size()){

      std::map<char,Node *>::iterator node_it = node->children.find(m_encode[it]);
      if(node_it==node->children.end()){
        log("failed to find ["<<m_encode[it]<<"] for ["<<m_encode<<"]");
        return m_decode;
      }

      node = node_it->second;
      if(node->val != '\0'){
        m_decode += node->val;
        node = &m_root;
      }
      it++;
    }

    return m_decode;
  }

  ~TrieTree(){}

};

int main(){

  ////////////////////////////////
  // set up
  ////////////////////////////////

  std::vector<std::string> v;
  v.push_back("a\t\t100100");
  v.push_back("b\t\t100101");
  v.push_back("c\t\t110001");
  v.push_back("d\t\t100000");
  v.push_back("[newline]\t\t111111");
  v.push_back("p\t\t111110");
  v.push_back("q\t\t000001");

  ////////////////////////////////
  // code
  ////////////////////////////////
  std::string code = "111110000001100100111111100101110001111110";
  std::string decode;
  TrieTree tree(v,code);
  decode = tree.decode();
  log("decode:\n"<<decode);

  return EXIT_SUCCESS;
}

```

```
//Trie Tree (reference)
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <algorithm>    // std::max
#include <limits.h>

namespace {
#define log(msg) do{ std::cout<<msg<<"\n"; }while(0)

  template <typename T>
  inline std::ostream& operator<< (std::ostream &os, const std::vector<T> & value ) {
    for(typename std::vector<T>::const_iterator it=value.begin(); it!=value.end(); ++it){
      os<<"["<<*it<<"]";
    }
    return os;
  }

  template <typename Key,typename Value>
  inline std::ostream& operator<< (std::ostream &os, const std::map<Key,Value> & value ) {
    os<<"[";
    for(typename std::map<Key,Value>::const_iterator it=value.begin(); it!=value.end(); ++it){
      os<<"("<<it->first<<"|"<<it->second<<")";
    }
    os<<"]";
    return os;
  }

  void split(const std::string & s, const std::string & delim, std::vector<std::string> & out){
    std::size_t start = 0, end = s.find_first_of(delim, start);

    while(end != std::string::npos){
      out.push_back(s.substr(start, end - start));
      start = end+delim.size();
      end = s.find_first_of(delim, start);
    }

    if(start != std::string::npos){
      out.push_back(s.substr(start));
    }
  }
}

struct Node {
  char val;
  char bin;
  std::map<char,Node> children;
};

class TrieTree {
private:
  Node m_root;
  std::string m_encode;
  std::string m_decode;
  void insert(std::string code,std::string val){
    if(val.size()!=1){
      std::cout<<"unsupperted value:"<<val<<std::endl;
    }

    char c_val = val[0];

    Node * node = & m_root;
    for(int i=0; i<code.size(); ++i){
      Node child;
      child.val = '\0';
      child.bin = code[i];

      std::pair<std::map<char,Node>::iterator,bool> ret;
      ret = node->children.insert( std::pair<char,Node>(code[i],child) );

      node = &ret.first->second;
    }

    node->val = c_val;
    log("inserting ["<<node->val<<"]");

  }

public:
  TrieTree(const std::vector<std::string> & code_map, const std::string & encode){
    m_encode = encode;

    for(int i=0;i<code_map.size();++i){
      std::vector<std::string> strs;
      split(code_map[i],"\t\t",strs);

      if(strs.size()!=2){
        continue; //log error
      }

      if(strs[0]=="[newline]"){
        strs[0]="\n";
      }

      log("ready to insert :"<<strs);
      this->insert(strs[1],strs[0]);
    }

  }
  std::string decode(){

    int it = 0;
    Node * node = &m_root;
    while(it<m_encode.size()){
      std::map<char,Node>::iterator node_child_it = node->children.find(m_encode[it]);
      if(node_child_it==node->children.end()){
        log("failed to find ["<<m_encode[it]<<"] for ["<<m_encode<<"] at ["<<it<<"]");
        return m_decode;
      }

      node = &node_child_it->second;
      if(node->val != '\0'){
        m_decode += node->val;
        node = &m_root;
      }
      it++;
    }

    return m_decode;
  }

  ~TrieTree(){}

};

int main(){

  ////////////////////////////////
  // set up
  ////////////////////////////////

  std::vector<std::string> v;
  v.push_back("a\t\t100100");
  v.push_back("b\t\t100101");
  v.push_back("c\t\t110001");
  v.push_back("d\t\t100000");
  v.push_back("[newline]\t\t111111");
  v.push_back("p\t\t111110");
  v.push_back("q\t\t000001");

  ////////////////////////////////
  // code
  ////////////////////////////////
  std::string code = "111110000001100100111111100101110001111110";
  std::string decode;
  TrieTree tree(v,code);
  decode = tree.decode();
  log("decode:\n"<<decode);

  return EXIT_SUCCESS;
}
```
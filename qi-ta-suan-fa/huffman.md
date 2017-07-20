### Huffman

[https://en.wikipedia.org/wiki/Huffman\_coding](https://en.wikipedia.org/wiki/Huffman_coding)

![](/assets/huffman.png)

```
//Hash table


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
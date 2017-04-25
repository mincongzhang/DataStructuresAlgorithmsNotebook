

### Backpack

Given n items with size Ai, an integer m denotes the size of a backpack. How full you can fill this backpack?

http://www.lintcode.com/en/problem/backpack/

```
//Dynamic programming

#define log(msg) do{ std::cout<<msg<<"\n"; }while(0)


template <typename T>
inline std::ostream& operator<< (std::ostream &os, const std::vector<T> & value ) {
    os<<"\n";
  for(typename std::vector<T>::const_iterator it=value.begin(); it!=value.end(); ++it){
    os<<"["<<*it<<"]";
  }
  return os;
}


class Solution {
public:
  /**
   * @param m: An integer m denotes the size of a backpack
   * @param A: Given n items with size A[i]
   * @return: The maximum size
   */
  int backPack(int m, std::vector<int> A) {
    //  mmmmmmmmmmmmm
    //0
    //A
    //A
    //A

    //NOTE: weight map should go from 0 to max weight
    std::vector<int> item_pick(m+1,0);
    std::vector< std::vector<int> > pack(A.size()+1,item_pick);

    for(int item=1;item<=A.size();++item){
      for(int weight=0;weight<=m;++weight){

        //NOTE: do this just in case you get A[item], and that's wrong!
        int item_idx = item-1;

        //weight exceed
        if(weight-A[item-1]<0){
          pack[item][weight] = pack[item-1][weight];
          continue;
        }

        int added_weight = pack[item-1][weight-A[item_idx]]+A[item_idx];
        if(added_weight > weight){
          //cannot take
          pack[item][weight] = pack[item-1][weight];
        } else {
          //can take
          if(added_weight > pack[item-1][weight]){
          pack[item][weight] = added_weight;
          } else {
            pack[item][weight] = pack[item-1][weight];
          }
        }
      }//weight loop
    }//item loop

    return pack.back().back();
  }
};
```

```
//Recursive

class Solution {
public:
  /**
   * @param m: An integer m denotes the size of a backpack
   * @param A: Given n items with size A[i]
   * @return: The maximum size
   */
  int getWeight(const std::vector<int> & A,size_t item, const int remaining_weight){
    if(item >= A.size()){ return 0; }

    //not taking current item
    int weight = getWeight(A,item+1,remaining_weight);
    if(remaining_weight-A[item] < 0){
      return weight;
    }

    return std::max(weight,getWeight(A,item+1,remaining_weight-A[item])+A[item]);
  }

  int backPack(int m, std::vector<int> A) {
    return getWeight(A,0,m);
  }
};
```
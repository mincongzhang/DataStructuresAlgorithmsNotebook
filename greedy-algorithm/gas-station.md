### Gas Station
There are N gas stations along a circular route, where the amount of gas at station i is gas[i].
You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.
Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

http://www.lintcode.com/en/problem/gas-station/

```
class Solution {
public:
    /**
     * @param gas: a vector of integers
     * @param cost: a vector of integers
     * @return: an integer 
     */
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        if(gas.empty() || cost.empty()) return -1;
        

        
        int size = gas.size();
        if(size != cost.size()) return -1;
        
        int count(0),start(-1),sum(0);
        for(int i(0); i<size; ++i){
            int diff = gas[i] - cost[i];
            count += diff;
            sum   += diff;
            if(sum<0){
                sum=0;
                start = i;
            }
        }
        
        if(count<0) return -1;

        return start+1;
    }
};
```

# 209

```cpp
#include "helloworld.h"
#include<iostream>

using namespace std;

int helloworld::minSubArrayLen(int target, vector<int>& nums) {
    int startIndex = 0, sum = 0;
    int res = MAX_INT;
    for (int endIndex = 0; endIndex < nums.size(); endIndex++) {
        sum += nums[endIndex];
        while (sum >= target) {
            sum -= nums[startIndex];
            res = min(res, endIndex - startIndex);
            startIndex++;
            if (endIndex <= startIndex) break;
        }
    }
    return res;
}


int main()
{
    vector<int> v = {2,3,1,2,4,3};
    helloworld hw;
    int res = hw.minSubArrayLen(7, v);
    cout<< res << endl;
    return 0;
}
```

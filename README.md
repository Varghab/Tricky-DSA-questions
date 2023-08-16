# This repository contains all the tricky questions of DSA.

## Tricky Array Questions
---
### 1. Maximum Sum Circular Subarray
Notes

```cpp
class Solution {
public:

    int findByKadanes(vector<int>& nums){
        int max_sum = INT_MIN;
        int curr_sum = 0;
        int k=0;
        int n = nums.size();
        while(k<n){
            curr_sum+=nums[k];
            max_sum = max(max_sum,curr_sum);
            if(curr_sum<0){
                curr_sum=0;
            }
            k++;
        }
        return max_sum;
    }

    int maxSubarraySumCircular(vector<int>& nums) {

        int ansFromKadanes = findByKadanes(nums);
        //circular subarray find
        int totalArraySum = 0;
        int k=0;
        while(k<nums.size()){
            totalArraySum+=nums[k];
            nums[k]*=-1;
            k++;
        }
        int minArraySum = findByKadanes(nums);
        int circularSum = totalArraySum + minArraySum; //totalArraySum - (-minArraySum)
        if(circularSum==0)return ansFromKadanes; //for test cases like - [-3,-2,-3];
        return max(circularSum, ansFromKadanes);
        
    }
};
```

## Tricky String Questions
---
### 1. Minimum Swaps to Make Strings Equal
Notes
```cpp
class Solution {
public:
    int minimumSwap(string s1, string s2) {
        int xc = 0;
        int yc = 0;
        for(int i=0;i<s1.length();i++){
            if(s1[i]!=s2[i]){
                if(s1[i]=='x')
                    xc++;
                else yc++;
            }
        }
        if((xc+yc)%2!=0)return -1;
        int swaps = xc/2 + yc/2;
        if(xc%2!=0)swaps+=2;
        
        return swaps;
    }
};
```


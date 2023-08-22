# This repository will help you to expand your toolkit to solve DSA related problems.
> I am creating this repository so that you can expand your **toolkit** and improve your problem-solving skills. Next time if you can't solve a problem, don't curse yourself, that problem requires a trick or a tool you need to know to solve the problem.

When solving problems, we learn about data structures, their implementation, and many algorithms. But then we get stuck in a problem for hours, days, or even a week because the tool that is required to solve that particular problem is not yet in your toolkit.


## Tricky Array and Matrix Questions
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
### 2. 4 Sum
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>>res;
        //Sorting to handle duplicate numbers
        sort(nums.begin(),nums.end());
        int n = nums.size();
        for(int i=0;i<n;i++){
            if(i!=0&&nums[i]==nums[i-1])continue;
            for(int j=i+1;j<n;j++){
                if(j!=i+1&&nums[j]==nums[j-1])continue;
                int k = j+1;
                int l = n-1;
                while(k<l){
                    long long sum = nums[i];
                    sum+=nums[j];
                    sum+=nums[k];
                    sum+=nums[l];
                    if(sum==target){
                        vector<int>temp = {nums[i],nums[j],nums[k],nums[l]};
                        res.push_back(temp);
                        l--;k++;
                        while(k<l&&nums[l]==nums[l+1])l--;
                        while(k<l&&nums[k]==nums[k-1])k++;                       
                    }
                    else if(sum>target)l--;
                    else if(sum<target)k++;
                }
            }
        }
        return res;
    }
};
```
### 3. First Missing Positive
```cpp
#include <bits/stdc++.h> 
int firstMissing(int arr[], int n)
{
    for(int i=0;i<n;i++){
        int place = arr[i]-1;
        int num = i+1;
        if(arr[i]==num)continue;
        if(arr[i]>n||arr[i]<=0)continue;
        else{
            swap(arr[place],arr[i]);
            i--;
        }
    }
    for(int i=0;i<n;i++){
        int num = i+1;
        if(arr[i]!=num)return num;
    }
    return n+1;
}
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


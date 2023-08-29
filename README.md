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
### 4. Subarray Sums Divisible by K
 ```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int count = 0;
        int sum = 0;
        map<int,int>mp;
        mp[0]++;
        for(int x:nums){
            sum+=x;
            if(mp[(sum%k+k)%k]>0) count+=mp[(sum%k+k)%k];
            mp[(sum%k+k)%k]++;
        }
        return count;

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
### 2. Multiply Strings
```cpp
#include <iostream>
#include <vector> // Include for vector
#include <string> // Include for string
using namespace std;

string multiplyStrings(string a , string b ){
    if (a == "0" || b == "0") return "0";
        
    vector<int> res(a.size()+b.size(), 0);
    
    for (int i = a.size()-1; i >= 0; i--) {
        for (int j = b.size()-1; j >= 0; j--) {
            res[i + j + 1] += (a[i]-'0') * (b[j]-'0');
            res[i + j] += res[i + j + 1] / 10;
            res[i + j + 1] %= 10;
        }
    }
    
    int i = 0;
    string ans = "";
    while (res[i] == 0) i++;
    while (i < res.size()) ans += to_string(res[i++]);
    
    return ans;
}

```
## Tricky Linked List Questions

### 1. Merge Two Sorted Lists
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if(list1==NULL) return list2;
        if(list2==NULL) return list1;
        ListNode *p,*next;
        if(list1->val>list2->val){
            next=list2;
            p=next;
            list2=list2->next;
        }else{
            next=list1;
            p=next;
            list1=list1->next;
        }
        while(list1!=NULL&&list2!=NULL){
            if(list1->val<list2->val){
                next->next=list1;
                list1=list1->next;
                next=next->next;
            }else{
                next->next=list2;
                list2=list2->next;
                next=next->next;
            }
        }
        if(list1==NULL)next->next=list2;
        else next->next=list1;
        return p;
    }
};
```
### 2. Detect and Remove Loop
```cpp
/*************************************************
    
    class Node {
        public :

        int data;
        Node *next;

        Node(int data) {
            this -> data = data;
            this -> next = NULL;
        }
    };

*************************************************/

Node *removeLoop(Node *head)
{
    // Write your code here.
    if(!head||!head->next)return head;
    Node *slow = head;
    Node *fast = head;
    Node *entry = head;
    Node*curr;
    while(fast->next&&fast->next->next){
        slow = slow->next;
        fast = fast->next->next;
        if(slow==fast){
            while(entry!=slow){
                entry = entry->next;
                slow = slow->next;
            }
            curr = entry;
            while(slow->next!=curr){
                slow=slow->next;
            }
            slow->next = NULL;
            return head;
        }
    }
    return head;
}
```

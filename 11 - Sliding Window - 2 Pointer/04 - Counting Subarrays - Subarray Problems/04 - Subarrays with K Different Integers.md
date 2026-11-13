# 🔴 04 - Subarrays with K Different Integers

### Brute
```cpp
int countDistinct(const vector<int>& nums,int start,int end) {
  unordered_set<int> distinct;
  for (int i = start; i <= end; i++) {
    distinct.insert(nums[i]);
  }
  return distinct.size();
}

int subarraysWithKDistinct(vector<int>& nums,int k) {
  int n = nums.size();
  if (n == 0 || k <= 0) {
    return 0;
  }
  int count = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      int distinctCount = countDistinct(nums,start,end);
      if (distinctCount == k) {
        count++;
      }
    }
  }
  return count;
}
```
```
TC = O(N^N^N)
SC = O(N)
```

---

### Better
```cpp    
int subarraysWithKDistinct(vector<int>& nums,int k) {
  int n = nums.size();
  if (n == 0 || k <= 0) {
    return 0;
  }
  int count = 0;
  for (int start = 0; start < n; start++) {
    unordered_map<int, int> frequency;
    for (int end = start; end < n; end++) {
      frequency[nums[end]]++;
      if (frequency.size() == k) {
        count++;
      }
      if (frequency.size() > k) {
        break;
      }
    }
  }
  return count;
}
```
```
TC = O(N^N)
SC = O(N)
```

---

### Optimal
```cpp
int subarraysWithKDistinct(vector<int>& nums, int k) {
  return solve(nums, k) - solve(nums, k-1);
}

int solve(vector<int>& nums, int k){
  int l = 0;
  int r = 0;
  int n = nums.size();
  unordered_map<int,int> mpp;
  int cnt = 0;
  while(r < n){
    mpp[nums[r]]++;
    while(mpp.size() > k){
      mpp[nums[l]]--;
      if(mpp[nums[l]] == 0){
        mpp.erase(nums[l]);
      }
      l++;
    }
    cnt += r-l+1;
    r++;
  }
  return cnt;
}
```
```
TC = O(N)
SC = O(N)
```

# 🔴 03 - Count number of Nice subarrays

### Brute
```cpp
int countOdds(const vector<int>& nums, int start, int end) {
  int oddCount = 0;
  for (int i = start; i <= end; i++) {
    if (nums[i] % 2 != 0) {
      oddCount++;
    }
  }
  return oddCount;
}
 
int numberOfSubarrays(vector<int>& nums, int k) {
  int n = nums.size();
  if (n == 0) {
    return 0;
  }
  int count = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      int oddCount = countOdds(nums,start,end);
      if (oddCount == k) {
        count++;
      }
    }
  }
  return count;
}
```
```
TC = O(N^N^N)
SC = O(1)
```

---

### Better
```cpp
int numberOfSubarrays(vector<int>& nums, int k) {
  int n = nums.size();
  if (n == 0) {
    return 0;
  }
  int count = 0;
  for (int start = 0; start < n; start++) {
    int oddCount = 0;
    for (int end = start; end < n; end++) {
      if (nums[end] % 2 != 0) {
        oddCount++;
      }
      if (oddCount == k) {
        count++;
      }
      if (oddCount > k) {
        break;
      }
    }
  }
  return count;
}
```
```
TC = O(N^N)
SC = O(1)
```

---

### Optimal
```cpp
int numberOfOddSubarrays(vector<int>& nums, int k) {
  return solve(nums, k) - solve(nums, k-1);
}

int solve(vector<int>& nums, int goal){
  int l = 0;
  int r = 0;
  int n = nums.size();
  int sum = 0;
  int cnt = 0;
  if(goal < 0){
    return 0;
  }
  while(r < n){
    sum += nums[r] % 2;
    while(sum > goal){
      sum -= nums[l] % 2;
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
SC = O(1)
```

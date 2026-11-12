# 🟡 02 - Max Consecutive Ones III

### Brute
```cpp
int longestOnes(vector<int>& nums, int k) {
  int n = nums.size();
  int maxLength = 0;
  for (int start = 0; start < n; start++) {
    int zeroCount = 0;
    for (int end = start; end < n; end++) {
      if (nums[end] == 0) {
        zeroCount++;
      }
      if (zeroCount > k) {
        break;
      }
      maxLength = max(maxLength, end - start + 1);
    }
  }
  return maxLength;
}
```
```
TC = O(N^N)
SC = O(1)
```

---

### Better
```cpp
int longestOnes(vector<int>& nums, int k) {
  int n = nums.size();
  vector<int> prefixZero(n + 1, 0);
  for (int i = 0; i < n; i++) {
    prefixZero[i + 1] = prefixZero[i] + (nums[i] == 0);
  }
  int maxLength = 0;
  for (int start = 0; start < n; start++) {
    int low = start;
    int high = n - 1;
    int farthest = start - 1;
    while (low <= high) {
      int mid = low + (high - low) / 2;
      int zeroCount = prefixZero[mid + 1] - prefixZero[start];
      if (zeroCount <= k) {
        farthest = mid;
        low = mid + 1;
      }
      else {
        high = mid - 1;
      }
    }
    if (farthest >= start) {
      maxLength = max(maxLength, farthest - start + 1);
    }
  }
  return maxLength;
}
```
```
TC = O(N logN)
SC = O(N)
```

---

### Optimal
```cpp
int longestOnes(vector<int>& nums, int k) {
  int l = 0;
  int r = 0;
  int n = nums.size();
  int zero = 0;
  int ans = 0;
  while(r < n){
    if(nums[r] == 0){
      zero++;
    }
    if(zero > k){
      if(nums[l] == 0){
        zero--;
      }
      l++;
    }
    if(zero <= k){
      ans = max(ans, r-l+1);
    }
    r++;
  }
  return ans;
}
```
```
TC = O(N)
SC = O(1)

nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
 l,r
maxlen = 1


  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
  l     r
maxlen = 2


  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
  l           r
maxlen = 3


  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
  l                 r
maxlen = 4


  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
  l                       r
maxlen = 5


  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
  l                             r

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
        l                             r

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
              l                             r

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
                    l                             r

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
                          l                             r
maxlen = 6

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
                          l                                   r

  1  ,  1  ,  1  ,  0  ,  0  ,  0  ,  1  ,  1  ,  1  ,  1  ,  0
                                l                             r
maxlen = 6
```

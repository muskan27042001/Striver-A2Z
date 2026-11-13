# 🟡 02 - Binary Subarrays With Sum

### Brute
```cpp
int numSubarraysWithSum(vector<int>& nums, int goal) {
  int n = nums.size();
  if (n == 0) {
    return 0;
  }
  int count = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      int currentSum = 0;
      for (int i = start; i <= end; i++) {
        currentSum += nums[i];
      }
      if (currentSum == goal) {
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
int numSubarraysWithSum(vector<int>& nums, int goal) {
  int n = nums.size();
  if (n == 0) {
    return 0;
  }
  int count = 0;
  for (int start = 0; start < n; start++) {
    int currentSum = 0;
    for (int end = start; end < n; end++) {
      currentSum += nums[end];
      if (currentSum == goal) {
        count++;
      }
      if (currentSum > goal) {
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
int numSubarraysWithSum(vector<int>& nums, int goal) {
  return solve(nums,goal) - solve(nums,goal-1);
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
    sum += nums[r];
    while(sum > goal){
      sum -= nums[l];
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
SC = o(1)

 [1,0,1,0,1], goal = 2
 solve(nums,goal)

 [ 1 , 0 , 1 , 0 , 1 ]
  l,r
 sum = 1 <= goal , cnt = 1

 [ 1 , 0 , 1 , 0 , 1 ]
   l   r
 sum = 1 <= goal , cnt = 1 + 2 = 3

 [ 1 , 0 , 1 , 0 , 1 ]
   l       r
 sum = 2 <= goal , cnt = 3 + 3 = 6

 [ 1 , 0 , 1 , 0 , 1 ]
   l           r
 sum = 2 <= goal , cnt = 6 + 4 = 10

 [ 1 , 0 , 1 , 0 , 1 ]
   l               r
 sum = 3 > goal

 [ 1 , 0 , 1 , 0 , 1 ]
       l           r
 sum = 2 <= goal , cnt = 10 + 4 = 14

 solve(nums,goal-1) , goal = 2-1 = 1
 [ 1 , 0 , 1 , 0 , 1 ]
  l,r
 sum = 1 <= goal , cnt = 1

 [ 1 , 0 , 1 , 0 , 1 ]
   l   r
 sum = 1 <= goal , cnt = 1 + 2 = 3

 [ 1 , 0 , 1 , 0 , 1 ]
   l       r
 sum = 2 > goal

 [ 1 , 0 , 1 , 0 , 1 ]
       l   r
 sum = 1 <= goal , cnt = 3 + 2 = 5

 [ 1 , 0 , 1 , 0 , 1 ]
       l       r
 sum = 1 <= goal , cnt = 5 + 3 = 8

 [ 1 , 0 , 1 , 0 , 1 ]
       l           r
 sum = 2 > goal

 [ 1 , 0 , 1 , 0 , 1 ]
           l       r
sum = 2 > goal

 [ 1 , 0 , 1 , 0 , 1 ]
               l   r
 sum = 1 <= goal , cnt = 18 + 2 = 10

 return 14-10 = 4
```

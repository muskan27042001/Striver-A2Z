# 🟡 03 - Fruit Into Baskets

### Brute
```cpp
int totalFruit(vector<int>& fruits) {
  int n = fruits.size();
  int maxFruits = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      unordered_set<int> types;
      for (int i = start; i <= end; i++) {
        types.insert(fruits[i]);
        if (types.size() > 2) {
          break;
        }
      }
      if (types.size() > 2) {
        break;
      }
      maxFruits = max(maxFruits, end - start + 1);
    }
  }
  return maxFruits;
}
```
```
TC = O(N^N^N)
SC = O(1)
```

---

### Better
```cpp    
int totalFruit(vector<int>& fruits) {
  int n = fruits.size();
  int maxFruits = 0;
  for (int start = 0; start < n; start++) {
    unordered_map<int, int> frequency;
    for (int end = start; end < n; end++) {
      frequency[fruits[end]]++;
      if (frequency.size() > 2) {
        break;
      }
      maxFruits = max(maxFruits, end - start + 1);
    }
  }
  return maxFruits;
}
```
```
TC = O(N^N)
SC = O(1)
```

---

### Optimal
```cpp
int totalFruits(vector<int>& fruits){
  int l = 0;
  int r = 0;
  int n = fruits.size();
  int ans = 0;
  unordered_map<int,int> mpp;
  while(r < n){
    mpp[fruits[r]]++;
    if(mpp.size() > 2){
      mpp[fruits[l]]--;
      if(mpp[fruits[l]] == 0){
        mpp.erase(fruits[l]);
      }
      l++;
    }
    if(mpp.size() <= 2){
      ans = max(ans, r-l+1);
    }
    r++;
  }
  return ans;
}
```
```
TC = O(N)
SC = O(N)

[ 0 , 1 , 2 ,2 ]
  l,r
 mp = [(0,1)]
 maxlen = 1

 [ 0 , 1 , 2 ,2 ]
   l   r
 mp = [(0,1),(1,1)]
 maxlen = 2

 [ 0 , 1 , 2 ,2 ]
   l       r
 mp = [(0,1),(1,1),(2,1)]

 [ 0 , 1 , 2 ,2 ]
       l   r
 mp = [(1,1),(2,1)]
 maxlen = 2

 [ 0 , 1 , 2 ,2 ]
       l      r
 mp = [(1,1),(2,2)]
 maxlen = 3
*/

# 🔴 04 - Longest Substring With At Most K Distinct Characters

### Brute
```cpp
int countDistinct(const string& s,int start,int end) {
  unordered_set<char> distinct;
  for (int i = start; i <= end; i++) {
    distinct.insert(s[i]);
  }
  return distinct.size();
}
    
int longestSubstringAtMostKDistinct(string s,int k) {
  int n = s.size();
  if (n == 0 || k <= 0) {
    return 0;
   }
  int maxLength = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      int distinctCount = countDistinct(s, start, end);
      if (distinctCount > k) {
        break;
      }
      maxLength = max(maxLength,end - start + 1);
    }
  }
  return maxLength;
}
```    
```
TC = O(N^N^N)
SC = O(N)
```

---

### Better
```cpp
int longestSubstringAtMostKDistinct(string s,int k) {
  int n = s.size();
  if (n == 0 || k <= 0) {
    return 0;
  }
  int maxLength = 0;
  for (int start = 0; start < n; start++) {
    unordered_map<char, int> frequency;
    for (int end = start; end < n; end++) {
      frequency[s[end]]++;
      if (frequency.size() > k) {
        break;
      }
      maxLength = max(maxLength,end - start + 1);
    }
  }
  return maxLength;
}
```
```
TC = O(N^N)
SC = O(N)
```

---


### Optimal
```cpp
int kDistinctChar(string& s, int k) {
  if(k == 0 || s.size() == 0){
    return 0;
  }
  int l = 0;
  int r = 0;
  int n = s.size();
  int ans = 0;
  unordered_map<char,int> mpp;
  while(r < n){
    mpp[s[r]]++;
    if(mpp.size() > k){
      mpp[s[l]]--;
      if(mpp[s[l]] == 0){
        mpp.erase(s[l]);
      }
      l++;
    }
    if(mpp.size() <= k){
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

 a  a  a  b  b  c  c  d    , k = 2
l,r
mpp = [(a,1)]
maxlen = 1


 a  a  a  b  b  c  c  d  
 l  r
mpp = [(a,2)]
maxlen = 2


 a  a  a  b  b  c  c  d  
 l     r
mpp = [(a,3)]
maxlen = 3


 a  a  a  b  b  c  c  d  
 l        r
mpp = [(a,3) , (b,1)]
maxlen = 4


 a  a  a  b  b  c  c  d  
 l           r
mpp = [(a,3) , (b,2)]
maxlen = 5


 a  a  a  b  b  c  c  d  
 l              r
mpp = [(a,3) , (b,2) , (c,1)]


 a  a  a  b  b  c  c  d  
    l           r
mpp = [(a,2) , (b,2) , (c,1)]


 a  a  a  b  b  c  c  d  
    l              r
mpp = [(a,2) , (b,2) , (c,2)]


 a  a  a  b  b  c  c  d  
       l           r
mpp = [(a,1) , (b,2) , (c,2)]


 a  a  a  b  b  c  c  d  
       l              r
mpp = [(a,1) , (b,2) , (c,2) , (d,1)]


 a  a  a  b  b  c  c  d  
          l           r
mpp = [(b,2) , (c,2) , (d,1)]
```

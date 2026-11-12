# 🟡 01 - Longest Substring Without Repeating Characters

### Brute
```cpp
bool hasUniqueCharacters(const string& s, int start, int end) {
  unordered_set<char> seen;
  for (int i = start; i <= end; i++) {
    if (seen.count(s[i])) {
      return false;
    }
    seen.insert(s[i]);
  }
  return true;
}
 
int lengthOfLongestSubstring(string s) {
  int n = s.size();
  int maxLen = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      if (!hasUniqueCharacters(s, start, end)) {
        break;
      }
      maxLen = max(maxLen, end - start + 1);
    }
  }
  return maxLen;
}
```
```
TC = O(N^N^N)
SC = O(N)
```

---

### Better
```cpp
int lengthOfLongestSubstring(string s) {
  int n = s.size();
  int maxLen = 0;
  for (int start = 0; start < n; start++) {
    unordered_set<char> seen;
    for (int end = start; end < n; end++) {
      if (seen.count(s[end])) {
        break;
      }
      seen.insert(s[end]);
      maxLen = max(maxLen, end - start + 1);
    }
  }
  return maxLen;
}
```
```
TC = O(N^N)
SC = O(N)
```

---

### Optimal
```cpp
int longestNonRepeatingSubstring(string& s){
  vector<int> hash(256, -1);
  int l = 0;
  int r = 0;
  int n = s.size();
  int maxLen = 0;
  while(r < n){
    if(hash[s[r]] != -1){
      if(hash[s[r]] >= l){
        l = hash[s[r]] + 1;
      }
    }
    maxLen = max(maxLen , (r-l+1));
    hash[s[r]] = r;
    r++;
  }
  return maxLen;
}
```
```
TC = O(N)
SC = O(1)

 a b c a b c b b
 0 1 2 3 4 5 6 7
l,r
mpp = [(a,0)]
maxlen = 1

 a b c a b c b b
 0 1 2 3 4 5 6 7
 l r
mpp = [(a,0),(b,1)]
maxlen = 2

 a b c a b c b b
 0 1 2 3 4 5 6 7
 l   r
mpp = [(a,0),(b,1),(c,2)]
maxlen = 3

 a b c a b c b b
 0 1 2 3 4 5 6 7
 l     r
 a already found

 a b c a b c b b
 0 1 2 3 4 5 6 7
   l   r
maxlen = max(3,3) = 3
mpp = [(a,3),(b,1),(c,2)]

 a b c a b c b b
 0 1 2 3 4 5 6 7
   l     r
b already found

 a b c a b c b b
 0 1 2 3 4 5 6 7
     l   r
maxlen = max(3,3) = 3
mpp = [(a,3),(b,4),(c,2)]

 a b c a b c b b
 0 1 2 3 4 5 6 7
     l     r
c already found

 a b c a b c b b
 0 1 2 3 4 5 6 7
       l   r
maxlen = max(3,3) = 3
mpp = [(a,3),(b,4),(c,5)]

 a b c a b c b b
 0 1 2 3 4 5 6 7
       l     r
b already found

 a b c a b c b b
 0 1 2 3 4 5 6 7
           l r
maxlen = max(3,2) = 3 
mpp = [(a,3),(b,6),(c,5)]

 a b c a b c b b
 0 1 2 3 4 5 6 7
           l   r
b already found

 a b c a b c b b
 0 1 2 3 4 5 6 7
               l,r
maxlen = max(3,1) = 3 
mpp = [(a,3),(b,7),(c,5)]
```

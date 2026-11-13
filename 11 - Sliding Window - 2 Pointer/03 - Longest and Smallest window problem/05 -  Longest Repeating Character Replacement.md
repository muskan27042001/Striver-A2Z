# 🔴 05 - Longest Repeating Character Replacement

### Brute
```cpp
int replacementsNeeded(const string& s,int start,int end) {
  vector<int> frequency(26, 0);
  int maxFrequency = 0;
  for (int i = start; i <= end; i++) {
    int index = s[i] - 'A';
    frequency[index]++;
    maxFrequency = max(maxFrequency,frequency[index]);
  }
  int length = end - start + 1;
  return length - maxFrequency;
}

int characterReplacement(string s, int k) {
  int n = s.size();
  if (n == 0 || k < 0) {
    return 0;
  }
  int maxLength = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      int needed = replacementsNeeded(s,start,end);
      if (needed > k) {
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
SC = O(1)
```

---

### Better
```cpp
int characterReplacement(string s, int k) {
  int n = s.size();
  if (n == 0 || k < 0) {
    return 0;
  }
  int maxLength = 0;
  for (int start = 0; start < n; start++) {
    vector<int> frequency(26, 0);
    int maxFrequency = 0;
    for (int end = start; end < n; end++) {
      int index = s[end] - 'A';
      frequency[index]++;
      maxFrequency = max(maxFrequency,frequency[index]);
      int currentLength = end - start + 1;
      int replacementsNeeded = currentLength - maxFrequency;
      if (replacementsNeeded > k) {
        break;
      }
      maxLength = max(maxLength,currentLength);
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

### Optimal
```cpp
int characterReplacement(string s, int k) {
  int l = 0;
  int r = 0;
  int n = s.size();
  vector<int> hash(26,0);
  int maxFreq = 0;
  int ans = 0;
  while(r < n){
    hash[s[r] - 'A']++;
    maxFreq = max(maxFreq, hash[s[r] - 'A']);
    if((r-l+1) - maxFreq > k){
      hash[s[l] - 'A']--;
      l++;
    }
    if((r-l+1) - maxFreq <= k){
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

 A  A  A  B  B  C  C  D    ,  k = 2
l,r
hash = [(A,1)]
maxFreq = 1
1-1 <= k , true
maxlen = 1

 A  A  A  B  B  C  C  D    
 l  r
hash = [(A,2)]
maxFreq = 2
2-2 <= k , true
maxlen = 2

 A  A  A  B  B  C  C  D    
 l     r
hash = [(A,3)]
maxFreq = 3
3-3 <= k , true
maxlen = 3

 A  A  A  B  B  C  C  D    
 l        r
hash = [(A,3) , (B,1)]
maxFreq = 3
4-3 <= k , true
maxlen = 4

 A  A  A  B  B  C  C  D    
 l           r
hash = [(A,3) , (B,2)]
maxFreq = 3
5-3 <= k , true
maxlen = 5

 A  A  A  B  B  C  C  D    
 l              r
hash = [(A,3) , (B,2) , (C,1)]
maxFreq = 3
6-3 <= k , false


 A  A  A  B  B  C  C  D    
    l           r
hash = [(A,2) , (B,2) , (C,1)]


 A  A  A  B  B  C  C  D    
    l              r
hash = [(A,2) , (B,2) , (C,2)]
maxFreq = 3
6-3 <= k , false


 A  A  A  B  B  C  C  D    
       l           r
hash = [(A,1) , (B,2) , (C,2)]


 A  A  A  B  B  C  C  D    
       l              r
hash = [(A,1) , (B,2) , (C,2) , (D,1)]
maxFreq = 3
6-3 <= 2 , false
```

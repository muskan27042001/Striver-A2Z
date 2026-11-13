# 🟡 01 - Number of Substrings Containing All Three Characters

### Brute
```cpp
bool containsAllThree(const string& s, int start, int end) {
  bool hasA = false;
  bool hasB = false;
  bool hasC = false;
  for (int i = start; i <= end; i++) {
    if (s[i] == 'a') {
      hasA = true;
    } 
    else if (s[i] == 'b') {
      hasB = true;
    } 
    else if (s[i] == 'c') {
      hasC = true;
    }
  }
  return hasA && hasB && hasC;
}
 
long long numberOfSubstrings(string s) {
  int n = s.size();
  if (n < 3) {
    return 0;
  }
  long long count = 0;
  for (int start = 0; start < n; start++) {
    for (int end = start; end < n; end++) {
      if (containsAllThree(s, start, end)) {
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
long long numberOfSubstrings(string s) {
  int n = s.size();
  if (n < 3) {
    return 0;
  }
  long long count = 0;
  for (int start = 0; start < n; start++) {
    vector<int> frequency(3, 0);
    for (int end = start; end < n; end++) {
      frequency[s[end] - 'a']++;
      if (frequency[0] > 0 && frequency[1] > 0 && frequency[2] > 0) {
        count += n - end;
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
int numberOfSubstrings(string s) {
  int lastSeen[3] = {-1, -1, -1};
  int n = s.size();
  int cnt = 0;
  for(int i=0;i<n;i++){
    lastSeen[s[i] - 'a'] = i;
    if(lastSeen[0] != -1 && lastSeen[1] != -1 && lastSeen[2] != -1){
      cnt += 1 + min(lastSeen[0], min(lastSeen[1], lastSeen[2]));
    }
  }
  return cnt;
}
```
```
TC = O(N)
SC = O(1)

 a b c a b c
l,r
 lastseen[] = [0,-1,-1]

 a b c a b c
 l r
 lastseen[] = [0,1,-1]

 a b c a b c
 l   r
 lastseen[] = [0,1,2]
 cnt += 1 + min(0,1,2) = 1 (abc)

 a b c a b c
 l     r
 lastseen[] = [3,1,2]
 cnt += 1 + min(3,1,2) = 2 (abca , bca)


 a b c a b c
 l       r
 lastseen[] = [3,4,2]
 cnt += 1 + min(3,4,2) = 3 (abcab , bcab , cab)

 a b c a b c
 l         r
 lastseen[] = [3,4,5]
 cnt += 1 + min(3,1,4) = 4 (abcabc , bcabc , cabc , abc)

cnt = 1 + 2 + 3 + 4 = 10
```

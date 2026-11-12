# 🟡 01 - Maximum Points You Can Obtain from Cards

### Brute
```cpp
int maxScore(vector<int>& cardPoints, int k) {
  int n = cardPoints.size();
  if (k < 0 || k > n) {
    return -1;
  }
  if (k == 0) {
    return 0;
  }
  if (k == n) {
    return accumulate(cardPoints.begin(), cardPoints.end(), 0);
  }
  int maxScore = INT_MIN;
  for (int leftCount = 0; leftCount <= k; leftCount++) {
    int rightCount = k - leftCount;
    int currentScore = 0;
    for (int i = 0; i < leftCount; i++) {
      currentScore += cardPoints[i];
    }
    for (int i = 0; i < rightCount; i++) {
      currentScore += cardPoints[n - 1 - i];
    }
    if (currentScore > maxScore) {
      maxScore = currentScore;
    }
  }
  return maxScore;
}
```
```
TC = O(K^K)
SC = O(1)
```

---

## Better
```cpp
int maxScore(vector<int>& cardPoints, int k) {
  int n = cardPoints.size();
  if (k < 0 || k > n) {
    return -1;
  }
  if (k == 0) {
    return 0;
  }
  if (k == n) {
    return accumulate(cardPoints.begin(), cardPoints.end(), 0);
  }
  vector<int> leftSum(k + 1, 0);
  vector<int> rightSum(k + 1, 0);
  for (int i = 1; i <= k; i++) {
    leftSum[i] = leftSum[i - 1] + cardPoints[i - 1];
  }
  for (int i = 1; i <= k; i++) {
    rightSum[i] = rightSum[i - 1] + cardPoints[n - i];
  }
  int maxScore = INT_MIN;
  for (int leftCount = 0; leftCount <= k; leftCount++) {
    int rightCount = k - leftCount;
    int currentScore = leftSum[leftCount] + rightSum[rightCount];
    if (currentScore > maxScore) {
      maxScore = currentScore;
    }
  }
  return maxScore;
}
```
```
TC = O(K)
SC = O(K)
```

---

### Optimal
```cpp
int maxScore(vector<int>& cardScore , int k){
  int leftSum = 0;
  int rightSum = 0;
  int ans = 0;
  for(int i=0;i<k;i++){
    leftSum += cardScore[i];
  }
  ans = leftSum;
  int rightIndex = cardScore.size() - 1;
  for(int i = k-1;i>=0;i--){
    rightSum += cardScore[rightIndex];
    leftSum -= cardScore[i];
    rightIndex--;
    ans = max(ans, (leftSum + rightSum));
  }
  return ans;
}
```
```
TC = O(K)
SC = O(1)

1 , 2 , 3 , 4 , 5 , 6 , 1 , k = 3
 lsum = 1+2+3 = 6
 maxSum = 6

 1 , 2 , 3 , 4 , 5 , 6 , 1
         i              rIndex
 lsum = 1+2 = 3
 rSum = 1
 1 , 2 , 3 , 4 , 5 , 6 , 1
         i         rIndex
 maxum = max(6,3+1) = 6

 1 , 2 , 3 , 4 , 5 , 6 , 1
     i            rIndex
 lsum = 1
 rsum = 6+1 = 7
 maxum = max(6,1+7) = 8

 1 , 2 , 3 , 4 , 5 , 6 , 1
 i            rIndex
 lsum = 0
 rsum = 5+6+1 = 12
 maxum = max(8,0+12) = 12
```

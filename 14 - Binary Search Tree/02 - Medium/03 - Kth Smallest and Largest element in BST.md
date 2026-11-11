# 03 - Kth Smallest and Largest element in BST

### Brute
```cpp
void inorder(TreeNode* root, vector<int>& values) {
  if(root == NULL){
    return;
  }
  inorder(root->left, values);
  values.push_back(root->val);
  inorder(root->right, values);
}

vector<int> findKthElements(TreeNode* root, int k) {
  vector<int> values;
  inorder(root, values);
  int n = values.size();
  vector<int> result(2);
  result[0] = values[k - 1];
  result[1] = values[n - k];
  return result;
}
```
```
TC = O(N)
SC = O(N)
```

---

# Optimal
```cpp
vector<int> kLargesSmall(TreeNode* root,int k){
  vector<int> ans;
  int smallest = kSmallest(root, k);
  int largest = kLargest(root, k);
  ans.push_back(smallest);
  ans.push_back(largest);
  return ans;
}

int sAns = -1;
int kSmallest(TreeNode* root , int k){
  kSmallestInorder(root, k);
  return sAns;
}

void kSmallestInorder(TreeNode* root, int &k){
  if(root == NULL){
    return;
  }
  kSmallestInorder(root->left , k);
  k--;
  if(k == 0){
    sAns = root->data;
    return;
  }
  kSmallestInorder(root->right , k);
}

int lAns = -1;;
int kLargest(TreeNode* root , int k){
  kLargestInorder(root, k);
  return lAns;
}

void kLargestInorder(TreeNode* root, int &k){
  if(root == NULL){
    return;
  }
  kLargestInorder(root->right , k);
  k--;
  if(k == 0){
    lAns = root->data;
    return;
  }
  kLargestInorder(root->left , k);
}
```
```
TC = O(N)
SC = O(H) -> Recursive stack space

Kth Smallest
k = 3

      5
    /   \
   3     7
 /  \  /  \
1   4  6   8
 \
  2

ans = -1
root = 5 
root = root->left

root = 3 
root = root->left

root = 1 
root = root->left

root = NULL
k = 2

root = 2
root = root->left

root = NULL 
k = 1

root = NULL 

root = 3
k = 0
ans = 3


Kth Largest
ans = -1
root = 5 
root = root->right

root = 7
root = root->right

root = 8
root = root->right

root = NULL
k = 2

root = NULL

root = 7 
k = 1
root = root->left

root = 6
root = root->right

root = NULL


root = 6
k = 0

ans = 6
*/

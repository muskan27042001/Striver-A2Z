# 04 - Minimum and Maximum in BST

### Optimal
```cpp
vector<int> findMinMax(TreeNode* root) {
  vector<int> ans;
  int mini = findMin(root);
  int maxi = findMax(root);
  ans.push_back(mini);
  ans.push_back(maxi);
  return ans;
}

int findMin(TreeNode* root){
  while(root->left){
    root = root->left;
  }
  return root->val;
}

int findMax(TreeNode* root){
  while(root->right){
    root = root->right;
  }
  return root->val;
}
```
```
TC = O(H)
SC = O(1)

       8
    /    \
   3      12
 /  \    /  \
1   6   10   14
    

Min value
root = 8
root = root->left

root = 3
root = root->left

root = 1
root = root->left

Min value = 1

Max value
root = 8
root = root->right

root = 12
root = root->right

root = 14
root = root->right

Max value = 14
```

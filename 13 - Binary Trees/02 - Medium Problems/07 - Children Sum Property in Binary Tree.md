# 🟡 07 - Children Sum Property in Binary Tree

### Optimal
```cpp
bool checkChildrenSum(TreeNode* root) {
  if(root == NULL){
    return true;
  }
  if(root->left == NULL && root->right == NULL){
    return true;
  }
  int left = root->left ? root->left->val : 0;
  int right = root->right ? root->right->val : 0;
  if(root->val != left + right){
    return false;
  } 
  return checkChildrenSum(root->left) && checkChildrenSum(root->right);
}
```
```
TC = O(N)
SC = O(H)
```

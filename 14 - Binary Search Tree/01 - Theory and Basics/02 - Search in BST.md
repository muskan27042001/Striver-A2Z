# 🟢 02 - Search in BST

### Brute
```cpp
TreeNode* searchBST(TreeNode* root, int val) {
  if(root == nullptr){
    return nullptr;
  }
  if(root->data == val){
    return root;
  }
  if(val < root->data){
    return searchBST(root->left, val);
  }
  return searchBST(root->right, val);
}
```
```
TC = O(H) -> Balanced Tree : O(log N) , Skewed Tree : O(N)
SC = O(H) -> Recursive stack space
```

### Optimal
```cpp
TreeNode* searchBST(TreeNode* root, int val) {
  while(root && root->data != val){
    root = root->data < val ? root->right : root->left;
  }
  return root;
}
```
```
TC = O(H) -> Balanced Tree : O(log N) , Skewed Tree : O(N)
SC = O(1)

x = 2

        4
      /  \
     2    7
   /  \ 
  1    3

root = 4  
4 > 2
root = root->left

root = 2
2 == 2
return root
    2
  /   \
1      3
```

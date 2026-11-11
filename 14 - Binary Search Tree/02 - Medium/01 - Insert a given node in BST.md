# 🟡 01 - Insert a given node in BST

### Brute
```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
  if(root == NULL){
    return new TreeNode(val);
  }
  if(val < root->val){
    root->left = insertIntoBST(root->left, val);
  }
  else{
    root->right = insertIntoBST(root->right, val);
  }
  return root;
}
```
```
TC = O(H)
SC = O(H)
```

---

### Optimal

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
  TreeNode* newNode = new TreeNode(val);
  if(root == NULL){
    return newNode;
  }
  TreeNode* curr = root;
  while(curr){
    if(curr->data <= val){
      if(curr->right){
        curr = curr->right;
      }
      else{
        curr->right = newNode;
        break;
      }
    }
    else{
      if(curr->left){
        curr = curr->left;
      }
      else{
        curr->left = newNode;
        break;
      }
    }
  }
  return root;
}
```
```
TC = O(H)
SC = O(1)

val = 5

       4
    /    \
   2      7
 / \    
1   3  

root = 4 
4 <= val
root = root->right

root = 7
7 > val
root = root->left

       4
    /    \
   2      7
 / \    /
1   3  5
```

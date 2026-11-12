# 🟡 06 - Construct a BST from a preorder traversal

### Brute
```cpp
TreeNode* bstFromPreorder(vector<int>& preorder) {
  TreeNode* root = NULL;
  for(int value : preorder){
    root = insertNode(root, value);
  }
  return root;
}

TreeNode* insertNode(TreeNode* root, int value) {
  if(root == NULL){
    return new TreeNode(value);
  }
  if(value < root->val){
    root->left = insertNode(root->left, value);
  }
  else if(value > root->val){
    root->right = insertNode(root->right, value);
  }
  return root;
}
```
```
TC = O(N^N)
SC = O(H)
```

---

### Optimal
```cpp
TreeNode* bstFromPreorder(vector<int>& preorder) {
  int i = 0;
  return solve(preorder, i, INT_MAX);
}

TreeNode* solve(vector<int>& preorder, int &i , int upper_bound){
  if(preorder.size() == i || preorder[i] > upper_bound){
    return NULL;
  }
  TreeNode* root = new TreeNode(preorder[i++]);
  root->left = solve(preorder, i, root->data);
  root->right = solve(preorder, i, upper_bound);
  return root;
}
```
```
TC = O(N)
SC = O(H)

[8 ,5 , 1 , 7 , 10 , 12]
       8
     /   \ 
    5     10
  /   \     \
 1     7     12

build(A,0,INT_MAX)
root = 8
root->left = build(A,1,8)
root->right = build(A,1,INT_MAX)

build(A,1,8)
root = 5
root->left = build(A,2,5)
root->right = build(A,2,8)

build(A,2,5)
root = 1
root->left = build(A,3,1)
root->right = build(A,3,5)

build(A,3,1)
7 > 1 , return NULL

build(A,3,5)
7 > 5 , return NULL

build(A,3,8)
root = 7
root->left = build(A,4,7)
root->right = build(A,4,8)

build(A,4,7)
10 > 7 , return NULL

build(A,4,8)
10 > 8 , return NULL

build(A,4,INT_MAX)
root = 10
root->left = build(A,5,10)
root->right = build(A,5,INT_MAX)

build(A,5,10)
12 > 10

build(A,5,INT_MAX)
root = 12
root->left = build(A,6,12)
root->right = build(A,6,IT_MAX)
```

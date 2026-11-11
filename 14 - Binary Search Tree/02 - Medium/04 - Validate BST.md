# 🟡 04 - Validate BST

### Better
```cpp  
bool isBST(TreeNode* root){
  return isBSTFast(root, LLONG_MIN , LLONG_MAX);
}

bool isBSTFast(TreeNode* root, long long mini , long long maxi){
  if(root == NULL){
    return true;
  }
  if(root->data <= mini || root->data >= maxi){
    return false;
  }
  return isBSTFast(root->left, mini, root->data) && isBSTFast(root->right, root->data, maxi);
}
```
```
TC = O(N)
SC = O(H)

     5
   /   \
  3     6
 / \     \
2   4     7

solve(5,INT_MIN,INT_MAX)
minVal = INT_MIN
maxVal = INT_MAX
root = 5
5 >= INT_MAX || 5 <= INT_MIN , No
solve(3,INT_MIN,5)
solve(6,5,INT_MAX)

solve(3,INT_MIN,5)
minVal = INT_MIN
maxVal = 5
root = 3         
3 >= 5 && 3 <= INT_MIN , No
solve(2,INT_MIN,3)
solve(4,3,5)

solve(2,INT_MIN,3)
minVal = INT_MIN
maxVal = 3
root = 2         
2 >= 3 && 2 <= INT_MIN , No
solve(NULL,INT_MIN,2)
solve(NULL,2,3)

solve(4,3,5)
minVal = 3
maxVal = 5
root = 4         
4 >= 5 && 4 <= 3 , No
solve(NULL,INT_MIN,4)
solve(NULL,4,5)

solve(6,5,INT_MAX)
minVal = 5
maxVal = INT_MAX
root = 5       
6 >= INT_MAX || 6 <= 5 , No
solve(NULL,INT_MIN,6)
solve(7,6,INT_MAX)

solve(7,6,INT_MAX)
minVal = 6
maxVal = INT_MAX
root = 7         
7 >= INT_MAX || 7 <= 6 , No
solve(NULL,5,7)
solve(NULL,7,INT_MAX)

return true;
```

---

### Optimal
```cpp
TreeNode* prev = NULL;

bool isBST(TreeNode* root){
  if(root == NULL){
    return true;
  }
  if(!isBST(root->left)){
    return false;
  }
  if(prev && root->data <= prev->data){
    return false;
  }
  prev = root;
  return isBST(root->right);
}
```
```
TC = O(N)
SC = O(H)

     5
   /   \
  3     6
 / \     \
2   4     7

isBST(5)
root = root->left

isBST(3)
root = root->left

isBST(2)
root = root->left

isBST(NULL) = true
prev = NULL
root = 2
prev = 2
root = root->right

isBST(NULL) = true

root = 3
2 >= 3 , no
prev = 3
root = root->right

isBST(4)
root = root->left

isBST(NULL) = true
prev = 3
root = 4
3 >= 4 , no
prev = 4
root = root->right

isBST(NULL) = true
root = 5
4 >= 5 , no
prev = 5
root = root->right

isBST(6)
root = root->left

isBST(NULL) = true
prev = 5
root = 6
5 >= 6 , no
prev = 6
root = root->right

isBST(7)
root = root->left

isBST(NULL) = true
prev=  6
root = 7
6 >= 7 , no
root = root->right

isBST(NULL) = true;
```

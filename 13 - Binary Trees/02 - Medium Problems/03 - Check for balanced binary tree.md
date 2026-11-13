 # 🟡 03 - Check for balanced binary tree
 
 ### Brute
```cpp
int height(TreeNode* root) {
  if (root == nullptr) {
    return 0;
  }
  return 1 + max(height(root->left),height(root->right));
}

bool isBalanced(TreeNode* root) {
  if (root == nullptr) {
    return true;
  }
  int leftHeight = height(root->left);
  int rightHeight = height(root->right);
  if (abs(leftHeight - rightHeight) > 1) {
    return false;
  }
  return isBalanced(root->left) && isBalanced(root->right);
}
```
```
TC = O(N* H)
SC = O(H)
```

---

### Iterative DFS
```cpp    
bool isBalanced(TreeNode* root) {
  if (root == nullptr) {
    return true;
  }
  stack<pair<TreeNode*, bool>> nodesStack;
  nodesStack.push({root, false});
  unordered_map<TreeNode*, int> heightMap;
  while (!nodesStack.empty()) {
    auto [node, visited] = nodesStack.top();
    nodesStack.pop();
    if (!visited) {
      nodesStack.push({node, true});
      if (node->right != NULL) {
        nodesStack.push({node->right, false});
      }
      if (node->left != NULL) {
        nodesStack.push({node->left, false});
      }
    } 
    else {
      int leftHeight = node->left ? heightMap[node->left] : 0;
      int rightHeight = node->right ? heightMap[node->right] : 0;
      if (abs(leftHeight - rightHeight) > 1) {
        return false;
      }
      heightMap[node] = 1 + max(leftHeight, rightHeight);
    }
  } 
  return true;
}
```
```
TC = O(N)
SC = O(N)
```

---

### Recursive DFS
```cpp
bool isBalanced(TreeNode *root){
  return solve(root) != -1;
}

int solve(TreeNode* root){
  if(root == NULL){
    return 0;
  }
  int lh = solve(root->left);
  if(lh == -1){
    return -1;
  }
  int rh = solve(root->right);
  if(rh == -1){
    return -1;
  }
  if(abs(lh - rh) > 1){
    return -1;
  }
  return 1 + max(lh, rh);
}
```
```
TC = O(N)
SC = O(H)

      3
    /  \
   9   20
      /  \
     15   7

root = 3

root = 9
lh = 0
rh = 0
abs(0 - 0) > 1 , no
return 1+max(0,0) = 1
        3
      /  \
1 <- 9   20
        /  \
       15   7

root = 20

root = 15
lh = 0
rh = 0
abs(0 - 0) > 1 , no
return 1+max(0,0) = 1
          3
        /  \
  1 <- 9   20
          /  \
    1 <- 15   7

root = 7
lh = 0
rh = 0
abs(0 - 0) > 1 , no
return 1+max(0,0) = 1
          3
        /  \
  1 <- 9   20
          /  \
    1 <- 15   7 -> 1

root = 20
lh = 1
rh = 1
abs(1 - 1) > 1 , no
return 1 + max(1,1) = 2
         3
        /  \
  1 <- 9   20 -> 2
          /  \
    1 <- 15   7 -> 1

root = 7
lh = 1
rh = 2
abs(1 - 2) > 1 , no
return 1+max(1,2) = 3
          1
          ^
          |
          3
        /  \
  1 <- 9   20 -> 2
          /  \
    1 <- 15   7 -> 1
true
```

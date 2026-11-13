# 🟡 05 - Check for symmetrical BTs

### Iterative BFS
```cpp
bool isSymmetric(TreeNode* root) {
  if (root == NULL) {
    return true;
  }
  queue<pair<TreeNode*, TreeNode*>> nodesQueue;
  nodesQueue.push({root->left, root->right});
  while (!nodesQueue.empty()) {
    auto [leftNode, rightNode] = nodesQueue.front();
    nodesQueue.pop();
    if (leftNode == NULL && rightNode == NULL) {
      continue;
    }
    if (leftNode == NULL || rightNode == NULL) {
      return false;
    }
    if (leftNode->val != rightNode->val) {
      return false;
    }
    nodesQueue.push({leftNode->left,rightNode->right});
    nodesQueue.push({leftNode->right,rightNode->left});
  }
  return true;
}
```
```
TC = O(N)
SC = O(W) -> W : maximum width of the binary tree
```

---

### Iterative DFS
```cpp
bool isSymmetric(TreeNode* root) {
  if (root == NULL) {
    return true;
  }
  stack<pair<TreeNode*, TreeNode*>> nodesStack;
  nodesStack.push({root->left, root->right});
  while (!nodesStack.empty()) {
    auto [leftNode, rightNode] = nodesStack.top();
    nodesStack.pop();
    if (leftNode == NULL && rightNode == NULL) {
      continue;
    }
    if (leftNode == NULL || rightNode == NULL) {
      return false;
    }
    if (leftNode->val != rightNode->val) {
      return false;
    }
    nodesStack.push({leftNode->right,rightNode->left});
    nodesStack.push({leftNode->left,rightNode->right});
  }
  return true;
}
```
```
TC = O(N)
SC = O(H)
```

---

### Recursive DFS
```cpp
bool isSymmetric(TreeNode* root) {
  return root == NULL || solve(root->left, root->right);
}

bool solve(TreeNode* p , TreeNode* q){
  if(p == NULL || q == NULL){
    return p == q;
  }
  if(p->data != q->data){
    return false;
  }
  return solve(p->left, q->right) && solve(p->right, q->left);
}
```
```
TC = O(N)
SC = O(H)
```

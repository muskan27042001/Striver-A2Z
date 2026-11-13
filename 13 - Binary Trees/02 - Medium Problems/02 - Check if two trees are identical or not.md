# 🟡 02 - Check if two trees are identical or not

### Iterative DFS
```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
  stack<pair<TreeNode*, TreeNode*>> st;
  st.push({p, q});
  while (!st.empty()) {
    auto [nodeP, nodeQ] = st.top();
    st.pop();
    if (nodeP == NULL && nodeQ == NULL) {
      continue;
    }
    if (nodeP == NULL || nodeQ == NULL) {
      return false;
    }
    if (nodeP->val != nodeQ->val) {
      return false;
    }
    st.push({nodeP->right,nodeQ->right});
    st.push({nodeP->left,nodeQ->left});
  }
  return true;
}
```
```
TC = O(N)
SC = O(H)
```

---

### Iterative BSF
```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
  queue<pair<TreeNode*, TreeNode*>> qNodes;
  qNodes.push({p, q});
  while (!qNodes.empty()) {
    auto [nodeP, nodeQ] = qNodes.front();
    qNodes.pop();
    if (nodeP == NULL && nodeQ == NULL) {
      continue;
    }
    if (nodeP == NULL || nodeQ == NULL) {
      return false;
    }
    if (nodeP->val != nodeQ->val) {
      return false;
    }
    qNodes.push({nodeP->left,nodeQ->left});
    qNodes.push({nodeP->right,nodeQ->right});
  }
  return true;
}
```
```
TC = O(N)
SC = O(W) -> W : Maximum width of the two trees
```

---

### Recursive DFS
```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
  if(p == NULL || q == NULL){
    return p == q;
  }
  if(p->data != q->data){
    return false;
  }
  return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
}
```
```
TC = O(N)
SC = O(H)

      1               1
     /  \           /   \
    2    1         1     2
      p               q

isSameTree(1,1)
1 == 1

isSameTree(2,1) && isSameTree(1,2)
2 != 1             1 != 2
return false
```

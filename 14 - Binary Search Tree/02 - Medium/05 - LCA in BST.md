# 🟡05 - LCA in BST

### Brute
```cpp
TreeNode* lca(TreeNode* root, int p, int q){
  if(root == NULL){
    return NULL;
  }
  if(root-> data < p && root->data < q){
    return lca(root->right, p, q);
  }
  else if(root->data > p && root->data > q){
    return lca(root->left, p, q);
  }
  return root;
}
```
```
TC = O(H)
SC = O(H)
 
     5
   /   \
  3     6
/  \     \
2   4     7

p = 2 , q = 4

lca(5,2,4)
root = 5
5 < 2 && 5 < 4 , false
5 > 2 && 5 > 4 -> lca(3,2,4)

lca(3,2,4)
root = 3
3 > 2 && 3 > 4 , false
3 < 2 && 3 < 4 , false
return root = 3
```

---

### Optimal
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
  while(root){
    if(p->val < root->val && q->val < root->val){
      root = root->left;
    }
    else if(p->val > root->val && q->val > root->val){
      root = root->right;
    }
    else{
      return root;
    }
  }
  return NULL;
}
```
```
TC = O(H)
SC = O(1)

     5
   /   \
  3     6
/  \     \
2   4     7

p = 2 , q = 4

lca(5,2,4)
root = 5
5 < 2 && 5 < 4 , false
5 > 2 && 5 > 4 -> root = root->left

lca(3,2,4)
root = 3
3 > 2 && 3 > 4 , false
3 < 2 && 3 < 4 , false
return root = 3
```

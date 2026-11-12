# 🔴 03 - Correct BST with two nodes swapped

### Brute
```cpp
void collectValues(TreeNode* root, vector<int>& values) {
  if(root == NULL){
    return;
  }  
  collectValues(root->left, values);
  values.push_back(root->val);
  collectValues(root->right, values);
}

void replaceValues(TreeNode* root, vector<int>& values, int& index) {
  if(root == NULL){
    return;
  }
  replaceValues(root->left, values, index);
  root->val = values[index++];
  replaceValues(root->right, values, index);
}
 
void recoverTree(TreeNode* root) {
  vector<int> values;
  collectValues(root, values);
  sort(values.begin(), values.end());
  int index = 0;
  replaceValues(root, values, index);
}
```
```
TC = O(N logN)
SC = O(N)
```

---

### Optimal
```cpp
TreeNode* first;
TreeNode* middle;
TreeNode* last;
TreeNode* prev;

void inorder(TreeNode* root){
  if(root == NULL){
    return;
  }
  inorder(root->left);
  if(prev != NULL && prev->data > root->data){
    if(first == NULL){
      first = prev;
      middle = root;
    }
    else{
      last = root;
    }
  }
  prev = root;
  inorder(root->right);
}

void recoverTree(TreeNode* root) {
  first = middle = last = prev = NULL;
  inorder(root);
  if(first && last){
    swap(first->data, last->data);
  }
  else{
    swap(first->data, middle->data);
  }
}
```
```
TC = O(N)
SC = O(H)

      3
    /   \
   1     4
        /
       2

inorder(3)

inorder(1)
inorder(NULL) return
inorder(1)
prev = 1
inorder(NULL) return

inorder(3)
3 < 1 , no
prev = 3

inorder(4)
inorder(2)
2 < 3 , yes
first = 3
middle = 2
inorder(NULL) return
prev = 2
inorder(NULL)

inorder(4)
4 < 2 , no
inorder(NULL) return
prev = 4
inorder(NULL) return

swap(2,3)

      2
    /   \
   1     4
        /
       3
```

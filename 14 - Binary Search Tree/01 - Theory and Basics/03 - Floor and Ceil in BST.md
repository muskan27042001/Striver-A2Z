# 🟡 03 - Floor and Ceil in BST

### Brute
```cpp
void inorder(TreeNode* root, vector<int>& sortedVals) {
  if(root == NULL){
    return;
  }
  inorder(root->left, sortedVals);
  sortedVals.push_back(root->val);
  inorder(root->right, sortedVals);
}

vector<int> floorAndCeil(TreeNode* root, int key) {
  vector<int> sortedVals;
  inorder(root, sortedVals);
  int floorVal = -1;
  int ceilVal = -1;
  for(int i=0;i<sortedVals.size();i++){
    if(sortedVals[i] <= key){
      floorVal = sortedVals[i];
    }
    if(sortedVals[i] >= key){
      ceilVal = sortedVals[i];
      break;
    }
  }
  return {floorVal, ceilVal};
}
```
```
TC = O(N)
SC = O(N)
```

---

### Optimal
```cpp
vector<int> floorCeilOfBST(TreeNode* root,int key){
  vector<int> ans;
  int floor = floorOfBST(root, key);
  int ceil = ceilOfBST(root, key);
  ans.push_back(floor);
  ans.push_back(ceil);
  return ans;
}

// Greatest value <= key
int floorOfBST(TreeNode* root, int key){
  int floor = -1;
  while(root){
    if(root->data == key){
      floor = root->data;
      break;
    }
    else if(root->data > key){
      root = root->left;
    }
    else{
      floor = root->data;
      root = root->right;
    }
  }
  return floor;
}

// Smallest value >= key
int ceilOfBST(TreeNode* root, int key){
  int ceil = -1;
  while(root){
    if(root->data == key){
      ceil = root->data;
      break;
    }
    else if(root->data < key){
      root = root->right;
    }
    else{
      ceil = root->data;
      root = root->left;
    }
  }
  return ceil;
}
```
```
TC = O(H)
SC = O(1)

Floor
key = 11

         8
      /    \
     4      12
    / \    /  \  
   2   6  10  14  

floor = -1

root = 8
8 <= 11
floor = 8
root = root->right

root = 12
12 > 11
root = root->left

root = 10
10 <= 11
floor = 10
root = root->right

floor = 10

-----------------------

ceil = -1

root = 8
8 <= 11
root = root->right

root = 12
12 > 11
ceil = 12
root = root->left

root = 10
10 <= 11
root = root->right

ceil = 12
```

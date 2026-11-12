# 🟡 02 - Two sum in BST

### Brute
```cpp
void inorder(TreeNode* root, vector<int>& nums) {
  if(root == NULL){
    return;
  }
  inorder(root->left, nums);
  nums.push_back(root->val);
  inorder(root->right, nums);
}
 
bool findTarget(TreeNode* root, int k) {
  vector<int> nums;
  inorder(root, nums);
  int left = 0;
  int right = nums.size() - 1;
  while (left < right) {
    int currentSum = nums[left] + nums[right];
    if (currentSum == k) {
      return true;
    }
    if (currentSum < k) {
      left++;
    } 
    else {
      right--;
    }
  }
  return false;
}  
```
```
TC = O(N)
SC = O(N)
```

--- 

### Optimal
```cpp
class BSTIterator{
  stack<TreeNode*> st;
  // reverse = true -> prev
  // reverse = false -> next
  bool reverse = true; 
	
  BSTIterator(TreeNode* root, bool isReverse){
    reverse = isReverse;
      pushAll(root);
  }
		
  bool hasNext(){
    return !st.empty();
  }
		
  int next(){
    TreeNode* temp = st.top();
    st.pop();
    if(!reverse){
      pushAll(temp->right);
    }
    else{
      pushAll(temp->left);
    }
    return temp->data;
  }

  void pushAll(TreeNode* root){
    while(root){
      st.push(root);
      if(!reverse){
        root = root->left;
      }
      else{
        root = root->right;
      }
    }
  }
};

bool twoSumBST(TreeNode* root, int k){
  if(root == NULL){
    return false;
  }
  BSTIterator l(root,false);
  BSTIterator r(root,true);
  int i = l.next(); // Actually next
  int j = r.next(); // Actually prev
  while(i < j){
    if(i + j == k){
      return true;
    }
    else if(i + j < k){
      i = l.next();
    }
    else{
      j = r.next();
    }
  }
  return false;
}
```
```
TC = O(N)
SC = O(H)*2 -> 2 stacks

      5   
    /  \
   3    6
 / \     \
2  4      7 

k = 9

i = 2 , j = 7 , 2+7 = 9 == k
i = 3 , j = 6 , 3+6 = 9 == k
i = 4 , j = 5 , 4+5 = 9 == k
```

# 🟡 01 - BST Iterator

### Brute
```cpp
class BSTIterator {
  private:
    vector<int> nodesSorted;
    int index;
            
  void inorder(TreeNode* root){
    if(root == NULL){
      return;
    }
    inorder(root->left);
    nodesSorted.push_back(root->val);
    inorder(root->right);
  }
    
  public:
    BSTIterator(TreeNode* root) {
      index = 0;
      inorder(root);
    }
  
    int next() {
      int val = nodesSorted[index];
      index++;
      return val;
    }
  
    bool hasNext() {
      return index < nodesSorted.size();
    }
};
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
	
  public:
    BSTIterator(TreeNode* root){
      pushAll(root);
    }
		
		bool hasNext(){
      return !st.empty();
		}
		
		int next(){
      TreeNode* temp = st.top();
      st.pop();
      pushAll(temp->right);
      return temp->data;
		}

    void pushAll(TreeNode* root){
      while(root){
        st.push(root);
        root = root->left;
      }
    }
};
```
```
TC = O(1)
SC = O(H)
```

# 🟡 07 - Inorder successor and predecessor in BST

### Brute
```cpp
void inorder(TreeNode* root, vector<int>& values) {
	if(root == NULL){
		return;
	}
	inorder(root->left, values);
	values.push_back(root->val);
	inorder(root->right, values);
}

vector<int> findPredecessorAndSuccessor(TreeNode* root, int key) {
	vector<int> values;
	inorder(root, values);
	int pred = -1;
	int succ = -1;
	for(int i=0;i<values.size();i++){
		if(values[i] < key){
			pred = values[i];
		} 
		else if(values[i] > key && succ == -1){
			succ = values[i];
		}
	}  
	return {pred, succ};
}
```
```
TC = O(N)
SC = O(N)
```

---

### Better
```cpp
int pred = -1;
int succ = -1;
    
void solve(TreeNode* root, int key) {
	if(root == NULL){
		return;
	}
	solve(root->left, key);
	if(root->val < key){
		pred = root->val;
	}
	else if(root->val > key && succ == -1){
		succ = root->val;
	}
	solve(root->right, key);
}

vector<int> findPredecessorAndSuccessor(TreeNode* root, int key) {
	solve(root, key);
	return {pred, succ};
}
```
```
TC = O(N)
SC = O(H)
```

---

### Optimal
```cpp
vector<int> succPredBST(TreeNode* root,int key){
	vector<int> ans;
	int pre = preBST(root, key);
	int succ = succBST(root, key);
	ans.push_back(pre);
	ans.push_back(succ);
	return ans;
}

int preBST(TreeNode* root, int key){
	int pre = -1;
	while(root){
		if(root->data > key){
			root = root->left;
		}
		else{
			pre = root->data;
			root = root->right;
		}
	}
	return pre;
}

int succBST(TreeNode* root, int key){
	int succ = -1;
	while(root){
		if(root->data < key){
			root = root->right;
		}
		else{
			succ = root->data;
			root = root->left;
		}
	}
	return succ;
}
```
```
TC = O(H)
SC = O(1)

      5
    /   \
   2     10
 / \     / \
1   4   7   12

x = 10

Successor
root = 5
5 < x
root = root->right

root = 10
10 >= x
succ = 10
root = root->left

root = 7
7 < x
root = root->right 

root == NULL
succ = 12

Predecessor
root = 5
5 <= x
pre = 5
root = root->right

root = 10
10 > x
root = root->left

root = 7
7 < x
pre = 7
root = root->right

root == NULL
pre = 7

Successor = 12
Predecessor = 7
```

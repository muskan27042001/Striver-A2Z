# 🔴 04 - Largest BST in Binary Tree

### Brute
```cpp
bool isValidBST(Node* root, long long low, long long high) {
  if (root == NULL) {
    return true;
  }
  if (root->data <= low || root->data >= high) {
    return false;
  }
  return isValidBST(root->left, low, root->data) && isValidBST(root->right, root->data, high);
}

int countNodes(Node* root) {
  if (root == NULL) {
    return 0;
  }
  return 1 + countNodes(root->left) + countNodes(root->right);
}

int largestBst(Node* root) {
  if (root == NULL) {
    return 0;
  }
  if (isValidBST(root, LLONG_MIN, LLONG_MAX)) {
    return countNodes(root);
  }
  return max(largestBst(root->left), largestBst(root->right));
}
```
```
TC = O(N^N)
SC = O(H)
```

---

### Optimal
```cpp
class Node{
  public:
    int minNode;
    int maxNode;
    int maxSize;
    Node(int minNode, int maxNode, int maxSize){
      this->minNode = minNode;
      this->maxNode = maxNode;
      this->maxSize = maxSize;
  }
};

int largestBST(TreeNode* root){
  return solve(root).maxSize;
}

Node solve(TreeNode* root){
  if(root == NULL){
    return Node(INT_MAX, INT_MIN, 0);
  }
  auto left = solve(root->left);
  auto right = solve(root->right);
  if(left.maxNode < root->data && root->data < right.minNode){
    return Node(min(left.minNode,root->data), max(right.maxNode,root->data), left.maxSize + 1 + right.maxSize);
  }
  return Node(INT_MIN, INT_MAX, max(left.maxSize, right.maxSize));
}
```
```
TC = O(N)
SC = O(H)

               20
           /        \
          15        40
        /   \      /  \
       14   18    30  60
        \   / \       /
        17 16 19      50
  
largestBST(20)

largestBST(15)

largestBST(14)
largestBST(NULL) return [INT_MAX,INT_MIN,0]

largestBST(17)
largestBST(NULL) return [INT_MAX,INT_MIN,0]
largestBST(NULL) return [INT_MAX,INT_MIN,0]
For 17
left = [INT_MAX,INT_MIN,0]
right = [INT_MAX,INT_MIN,0]
return [17,17,1]

For 14
left = [INT_MAX,INT_MIN,0]
right = return [17,17,1]
return [17,14,2]

largestBST(18)
largestBST(16)
largestBST(NULL) return [INT_MAX,INT_MIN,0]
largestBST(NULL) return [INT_MAX,INT_MIN,0]
For 16
left = [INT_MAX,INT_MIN,0]
right = [INT_MAX,INT_MIN,0]
return [16,16,1]
largestBST(19)
largestBST(NULL) return [INT_MAX,INT_MIN,0]
largestBST(NULL) return [INT_MAX,INT_MIN,0]
For 19
left = [INT_MAX,INT_MIN,0]
right = [INT_MAX,INT_MIN,0]
return [19,19,1]

For 18
left = [16,16,1]
right = [19,19,1]
return [19,16,3]

For 15
left = [17,14,2]
right = [19,16,3]
return [INT_MAX,INT_MIN,3]

largestBST(40)
largestBST(30)
largestBST(NULL) return [INT_MAX,INT_MIN,0]
largestBST(NULL) return [INT_MAX,INT_MIN,0]
For 30
left = [INT_MAX,INT_MIN,0]
right = [INT_MAX,INT_MIN,0]
return [30,30,1]
largestBST(60)
largestBST(50)
largestBST(NULL) return [INT_MAX,INT_MIN,0]
largestBST(NULL) return [INT_MAX,INT_MIN,0]
For 50
left = [INT_MAX,INT_MIN,0]
right = [INT_MAX,INT_MIN,0]
return [50,50,1]
largestBST(NULL) return [INT_MAX,INT_MIN,0]

For 60
left = [50,50,1]
right = [INT_MAX,INT_MIN,0]
return [60,50,2]

For 40
left = [30,300,1]
right = [60,50,2]
return [60,30,4]

For 20
left = [INT_MAX,INT_MIN,3]
right = [60,30,4]]
return [INT_MAX,INT_MIN,4]
```

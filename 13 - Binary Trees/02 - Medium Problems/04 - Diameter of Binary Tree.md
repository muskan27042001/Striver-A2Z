# 🟡 04 - Diameter of Binary Tree

### Brute
```cpp
int findHeight(TreeNode* node) {
  if (node == nullptr) {
    return 0;
  }
  int leftHeight = findHeight(node->left);
  int rightHeight = findHeight(node->right);
  return 1 + max(leftHeight, rightHeight);
}
 
int findDiameter(TreeNode* node) {
  if (node == NULL) {
    return 0;
  }
  int leftHeight = findHeight(node->left);
  int rightHeight = findHeight(node->right);
  int currentDiameter = leftHeight + rightHeight;
  int leftDiameter = findDiameter(node->left); 
  int rightDiameter = findDiameter(node->right);
  return max(currentDiameter, max(leftDiameter, rightDiameter));
}

int diameterOfBinaryTree(TreeNode* root) {
  return findDiameter(root);
}
```
```
TC = O(N * H)
SC = O(H)
```

--- 

### Better
```cpp
void buildGraph(TreeNode* node,unordered_map<TreeNode*, vector<TreeNode*>>& graph) {
  if (node == nullptr) {
    return;
  }
  if (node->left != nullptr) {
    graph[node].push_back(node->left);
    graph[node->left].push_back(node);
    buildGraph(node->left, graph);
  }
  if (node->right != nullptr) {
    graph[node].push_back(node->right);
    graph[node->right].push_back(node);
    buildGraph(node->right, graph);
  }
}

pair<TreeNode*, int> bfs(TreeNode* start,unordered_map<TreeNode*, vector<TreeNode*>>& graph) {
  queue<pair<TreeNode*, int>> q;
  unordered_set<TreeNode*> visited;
  q.push({start, 0});
  visited.insert(start);
  TreeNode* farthestNode = start;
  int maxDistance = 0;
  while (!q.empty()) {
    auto [node, distance] = q.front();
    q.pop();
    if (distance > maxDistance) {
      maxDistance = distance;
      farthestNode = node;
    }
    for (TreeNode* neighbor : graph[node]) {
      if (!visited.count(neighbor)) {
        visited.insert(neighbor);
        q.push({neighbor,distance + 1});
      }
    }
  }
  return {farthestNode, maxDistance};
}

int diameterOfBinaryTree(TreeNode* root) {
  if (root == nullptr) {
    return 0;
  }
  unordered_map<TreeNode*, vector<TreeNode*>> graph;
  buildGraph(root, graph);
  TreeNode* endpoint = bfs(root, graph).first;
  int diameter = bfs(endpoint, graph).second;
  return diameter;
}
```
```
TC = O(N)
SC = O(N)
```

---

### Optimal
```cpp
int diameterOfBinaryTree(TreeNode* root) {
  int diameter = 0;
  solve(root, diameter);
  return diameter;
}

int solve(TreeNode* root, int& diameter){
  if(root == NULL){
    return 0;
  }
  int lh = solve(root->left, diameter);
  int rh = solve(root->right, diameter);
  diameter = max(diameter, (lh + rh));
  return 1 + max(lh, rh);
}
```
```
TC = O(N)
SC = O(H)

      1
    /   \
   2     3
  /  \
 4    5

diameter = 0
root = 1

root = 2

root = 4
lh  = 0
rh = 0
diameter = max(0,0+0) = 0
return 1 + max(0,0) = 1

root = 5
lh  = 0
rh = 0
diameter = max(0,0+0) = 0
return 1 + max(0,0) = 1

root = 2
lh  = 1
rh = 1
diameter = max(0,1+1) = 2
return 1 + max(1,1) = 2

root = 3
lh  = 0
rh = 0
diameter = max(2,0+0) = 2
return 1 + max(0,0) = 1

root = 1
lh  = 2
rh = 1
diameter = max(2,2+1) = 3
return 1 + max(2,1) = 3
```

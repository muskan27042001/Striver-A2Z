# 🟡 01 - Maximum Depth in BT

### Optimal
```cpp
int maxDepth(TreeNode* root) {
  if(root == NULL){
    return NULL;
  }
  int lh = maxDepth(root->left);
  int rh = maxDepth(root->right);
  return 1 + max(lh, rh);
}
```
```
TC = O(N)
SC = O(H) -> Balanced Tree : O(logN) , Skewed Tree : O(N)
                  3
                 /  \
                9  20
                   /  \
                  15    7     
root = 3

root = 9                  
left = 0
right = 0
return max(0,0) + 1 = 1

                  3
                 /  \
           1 <- 9    20
                    /  \
                   15    7
                  
root = 20

root = 15
left = 0
right = 0
return max(0,0)+1 = 1

                   3
                 /  \
           1 <- 9    20
                   /    \
             1 <- 15      7  
                 
root = 7
left = 0
right = 0
return max(0,0)+1 = 1

                   3
                 /  \
           1 <- 9     20
                     /  \
              1 <- 15    7 -> 1        
            
root = 20
left = 1
right = 1 
return max(1,1)+1 = 2

                   3
                 /  \
           1 <- 9     20 -> 2
                     /  \
               1 <- 15    7 -> 1          
            
root = 3
left = 1
right = 2 
return max(1,2)+1 = 3

                   3
                   ^
                   |
                   3 
                 /  \
           1 <- 9   20 -> 2
                   /  \
            1 <- 15    7 -> 1            
height = 3
```

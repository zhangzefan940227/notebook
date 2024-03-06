代码随想录算法训练营第十六天|102.二叉树层序遍历，199.二叉树的右视图，637.二叉树的层平均值，429. N 叉树的层序遍历

# 102.二叉树层序遍历

## 题目

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

## 解题思路

- 需要借用队列实现，队列先进先出，符合一层一层遍历的逻辑


## 代码
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        vector<int> temp;
        if (root != nullptr) que.push(root);
        while(!que.empty()) {
            int size = que.size();
            for(int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                temp.push_back(node->val);
                que.pop();
                if (node->left != nullptr) que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }
            result.push_back(temp);
        }
        return result;
    }
};
```

# 199.二叉树的右视图
## 题目
给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

## 解题思路

- 题目意思和立体几何三视图中的左视图是类似的概念，只不过这个是右视图
- 和102题目思路差不多，只需要增加个条件，在遍历到每层最后一个的时候，加入到vector中，即遍历条件为`i==size-1`

## 代码
```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> que;
        if (root != nullptr) que.push(root);
        while(!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (i == (size -1)) result.push_back(node);
                if (node->left != nullptr) que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }
        }
        return result;
    }
};
```

# 637.二叉树的层平均值

## 题目

给定一个非空二叉树的根节点 root , 以数组的形式返回每一层节点的平均值。与实际答案相差 10-5 以内的答案可以被接受。

## 解题思路

- 使用层序遍历之后，记录每层的所有节点值相加总和，再除以size即可

## 代码
```cpp
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> result;
        queue<TreeNode*> que;
        if (root != nullptr) que.push(root);
        while(!que.empty()) {
            int size = que.size();
            double sum = 0;
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                sum += node->val;
                if (node->left != nullptr) que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }
            result.push_back(sum/size);
        }
        return result;
    }
};
```

# 429. N 叉树的层序遍历

## 题目
给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。

## 解题思路

- 层序遍历的主题思路不变
- 二叉树是left right 两个子节点，N叉树N个子节点，for循环遍历N个节点即可


## 代码
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        vector<int> temp;
        if (root != nullptr) que.push(root);
        while(!que.empty()) {
            int size = que.size();
            for(int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                temp.push_back(node->val);
                que.pop();
                for(int j = 0; j < node->children.size(); i++) {
                    if (node->children[j]) que.push(node->children[j]);
                }
            }
            result.push_back(temp);
        }
        return result;
    }
};
```

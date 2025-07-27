# Week 2: Tree Traversals Deep Dive - Teacher's Guide

This document provides comprehensive teaching materials for Week 2, focusing on all major tree traversals in C++ (inorder, postorder, level-order), including theory, code, brute force, recursive, and iterative solutions, stack/queue visualizations, and teaching strategies.

---

## 🎯 Learning Objectives for Week 2

By the end of this week, students should be able to:

1. Understand all major tree traversal types (inorder, postorder, level-order)
2. Implement each traversal recursively and iteratively
3. Visualize stack/queue operations during traversal
4. Analyze time and space complexity of traversal algorithms
5. Apply traversals to solve practical problems

---

## Day 1-2: Inorder Traversal (DFS)

### 📚 Theory Introduction

**Teacher's Explanation:**
"Inorder traversal means visiting the left subtree first, then the root, then the right subtree. For binary search trees, this gives sorted order!"

#### Key Points:
- Order: Left → Root → Right
- Used for BSTs to get sorted data

### 💻 Recursive Inorder Traversal

```cpp
void inorderTraversal(TreeNode* root) {
    if (root == nullptr) return;
    inorderTraversal(root->left);
    cout << root->data << " ";
    inorderTraversal(root->right);
}
```

### 💻 Iterative Inorder Traversal (Stack Visualization)

```cpp
void inorderTraversalIterative(TreeNode* root) {
    stack<TreeNode*> stk;
    TreeNode* current = root;
    while (current || !stk.empty()) {
        while (current) {
            stk.push(current);
            current = current->left;
        }
        current = stk.top(); stk.pop();
        cout << current->data << " ";
        current = current->right;
    }
}
```

### 📝 Problem: Print Inorder Traversal

#### Problem Statement
Given a binary tree, print its inorder traversal.

#### Brute Force Approach
- Use recursion to visit all nodes

#### Optimal Approach
- Use stack for iterative traversal

#### Complexity
| Approach   | Time | Space |
|------------|------|-------|
| Recursive  | O(n) | O(h)  |
| Iterative  | O(n) | O(h)  |

---

## Day 3: Postorder Traversal (DFS)

### 📚 Theory Introduction

**Teacher's Explanation:**
"Postorder means visiting children before the parent. Useful for deleting trees, evaluating expressions."

#### Key Points:
- Order: Left → Right → Root

### 💻 Recursive Postorder Traversal

```cpp
void postorderTraversal(TreeNode* root) {
    if (root == nullptr) return;
    postorderTraversal(root->left);
    postorderTraversal(root->right);
    cout << root->data << " ";
}
```

### 💻 Iterative Postorder Traversal (Stack Visualization)

```cpp
void postorderTraversalIterative(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> stk1, stk2;
    stk1.push(root);
    while (!stk1.empty()) {
        TreeNode* node = stk1.top(); stk1.pop();
        stk2.push(node);
        if (node->left) stk1.push(node->left);
        if (node->right) stk1.push(node->right);
    }
    while (!stk2.empty()) {
        cout << stk2.top()->data << " ";
        stk2.pop();
    }
}
```

### 📝 Problem: Print Postorder Traversal

#### Problem Statement
Given a binary tree, print its postorder traversal.

#### Brute Force Approach
- Use recursion

#### Optimal Approach
- Use two stacks for iterative traversal

#### Complexity
| Approach   | Time | Space |
|------------|------|-------|
| Recursive  | O(n) | O(h)  |
| Iterative  | O(n) | O(n)  |

---

## Day 4: Level-Order Traversal (BFS)

### 📚 Theory Introduction

**Teacher's Explanation:**
"Level-order means visiting nodes level by level, left to right. Uses a queue."

#### Key Points:
- Order: Level by level
- Used for shortest path, serialization

### 💻 Level-Order Traversal (Queue Visualization)

```cpp
void levelOrderTraversal(TreeNode* root) {
    if (!root) return;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        TreeNode* node = q.front(); q.pop();
        cout << node->data << " ";
        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
}
```

### 📝 Problem: Print Level-Order Traversal

#### Problem Statement
Given a binary tree, print its level-order traversal.

#### Brute Force Approach
- Use queue to visit nodes level by level

#### Complexity
| Approach   | Time | Space |
|------------|------|-------|
| Iterative  | O(n) | O(w)  |

---

## Day 5: Traversal Applications & Practice Problems

### 🔍 Problem 1: Print All Leaf Nodes (Inorder)

#### Problem Statement
Print all leaf nodes of a binary tree in inorder.

#### Brute Force Approach
- Traverse inorder, print if node is leaf

```cpp
void printLeafNodes(TreeNode* root) {
    if (!root) return;
    printLeafNodes(root->left);
    if (!root->left && !root->right) cout << root->data << " ";
    printLeafNodes(root->right);
}
```

### 🔍 Problem 2: Print Nodes at Kth Level (Level-Order)

#### Problem Statement
Print all nodes at level k.

#### Brute Force Approach
- Use level-order, track level

```cpp
void printNodesAtLevel(TreeNode* root, int k) {
    if (!root) return;
    queue<pair<TreeNode*, int>> q;
    q.push({root, 0});
    while (!q.empty()) {
        auto [node, level] = q.front(); q.pop();
        if (level == k) cout << node->data << " ";
        if (node->left) q.push({node->left, level + 1});
        if (node->right) q.push({node->right, level + 1});
    }
}
```

---

## Day 6-7: Assessment, Visualization, and Student Exercises

### 📝 Manual Stack/Queue Tracing
- Draw stack/queue contents at each step for traversals
- Have students act out stack/queue operations

### 📝 Assignment: Implement All Traversals
- Write recursive and iterative versions for all traversals
- Trace stack/queue for sample trees

### 📝 Complexity Analysis Table
| Traversal   | Recursive | Iterative | Space (Best) |
|-------------|-----------|-----------|--------------|
| Inorder     | O(n)      | O(n)      | O(h)         |
| Postorder   | O(n)      | O(n)      | O(h)/O(n)    |
| Level-order | O(n)      | O(n)      | O(w)         |

### 🗣️ Classroom Discussion Questions
1. Why does inorder give sorted order for BST?
2. How does stack/queue help in traversal?
3. Can you write preorder iteratively?
4. What is the difference between DFS and BFS?

### 🎯 Common Student Mistakes
- Forgetting to check for null nodes
- Confusing traversal orders
- Not understanding stack/queue operations

---

## Complete Week 2 Implementation (C++)

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <queue>
using namespace std;

struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};

class TraversalOperations {
public:
    void inorderRecursive(TreeNode* root) {
        if (!root) return;
        inorderRecursive(root->left);
        cout << root->data << " ";
        inorderRecursive(root->right);
    }
    void inorderIterative(TreeNode* root) {
        stack<TreeNode*> stk;
        TreeNode* current = root;
        while (current || !stk.empty()) {
            while (current) {
                stk.push(current);
                current = current->left;
            }
            current = stk.top(); stk.pop();
            cout << current->data << " ";
            current = current->right;
        }
    }
    void postorderRecursive(TreeNode* root) {
        if (!root) return;
        postorderRecursive(root->left);
        postorderRecursive(root->right);
        cout << root->data << " ";
    }
    void postorderIterative(TreeNode* root) {
        if (!root) return;
        stack<TreeNode*> stk1, stk2;
        stk1.push(root);
        while (!stk1.empty()) {
            TreeNode* node = stk1.top(); stk1.pop();
            stk2.push(node);
            if (node->left) stk1.push(node->left);
            if (node->right) stk1.push(node->right);
        }
        while (!stk2.empty()) {
            cout << stk2.top()->data << " ";
            stk2.pop();
        }
    }
    void levelOrder(TreeNode* root) {
        if (!root) return;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front(); q.pop();
            cout << node->data << " ";
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
};

int main() {
    TraversalOperations ops;
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right->left = new TreeNode(6);
    root->right->right = new TreeNode(7);

    cout << "Inorder Recursive: ";
    ops.inorderRecursive(root); cout << endl;
    cout << "Inorder Iterative: ";
    ops.inorderIterative(root); cout << endl;
    cout << "Postorder Recursive: ";
    ops.postorderRecursive(root); cout << endl;
    cout << "Postorder Iterative: ";
    ops.postorderIterative(root); cout << endl;
    cout << "Level Order: ";
    ops.levelOrder(root); cout << endl;
    return 0;
}
```

---

## Week 2 Assessment Rubric

**Basic Understanding:**
- [ ] Can explain all traversal types
- [ ] Can trace traversals manually
- [ ] Understands stack/queue role

**Implementation Skills:**
- [ ] Can write recursive and iterative traversals
- [ ] Can debug traversal code

**Advanced Thinking:**
- [ ] Can apply traversals to solve problems
- [ ] Can analyze complexity

---

## Next Week Preview
- Binary Search Trees (BST): insertion, search, deletion
- BST properties and applications
- Practice with BST problems

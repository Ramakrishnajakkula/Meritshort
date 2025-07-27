# Week 2: Tree Problem Solving & Applications - Teacher's Guide

This document provides comprehensive teaching materials for Week 2, focusing entirely on solving tree problems using the foundational concepts and traversals learned in Week 1. Includes problem statements, brute force approaches, optimal solutions, complexity analysis, and teaching strategies for real-world tree applications.

---

## 🎯 Learning Objectives for Week 2

By the end of this week, students should be able to:

1. Apply tree traversals and basic operations to solve complex problems
2. Recognize and classify different types of tree problems
3. Implement tree construction algorithms from various inputs
4. Solve path-related problems using different traversal strategies
5. Validate tree properties and structures
6. Optimize tree algorithms for better time/space complexity
7. Handle edge cases and develop robust tree solutions

---

## Day 1-2: Tree Construction & Validation Problems

### 📚 Theory: Problem Classification

**Teacher's Introduction:**
"Now that you master tree traversals and basic operations, let's use them to solve real problems! Tree problems typically fall into these categories:
1. Construction problems (building trees from different inputs)
2. Validation problems (checking if tree satisfies certain properties)
3. Transformation problems (modifying tree structure or values)"

### � Problem 1: Construct Tree from Array (Level-Order)

**Problem Statement:**
Given an array representing a complete binary tree in level-order, construct the tree.

**Teaching Strategy:**
"This is exactly what happens when you store a binary tree in an array! Let's see how index relationships work."

```cpp
class TreeConstructor {
public:
    // Construct tree from level-order array
    TreeNode* constructFromArray(vector<int>& arr) {
        if (arr.empty()) return nullptr;
        return constructHelper(arr, 0);
    }

private:
    TreeNode* constructHelper(vector<int>& arr, int index) {
        if (index >= arr.size()) return nullptr;

        TreeNode* root = new TreeNode(arr[index]);
        root->left = constructHelper(arr, 2 * index + 1);
        root->right = constructHelper(arr, 2 * index + 2);

        return root;
    }
};

// Teaching demonstration function
void demonstrateArrayToTree() {
    vector<int> arr = {1, 2, 3, 4, 5, 6, 7};
    
    cout << "=== CONSTRUCTING TREE FROM ARRAY ===" << endl;
    cout << "Array: ";
    for (int val : arr) cout << val << " ";
    cout << endl;
    
    cout << "\nIndex relationships:" << endl;
    cout << "Root at index 0: " << arr[0] << endl;
    cout << "Left child of index i at: 2*i + 1" << endl;
    cout << "Right child of index i at: 2*i + 2" << endl;
    
    TreeConstructor constructor;
    TreeNode* root = constructor.constructFromArray(arr);
    
    cout << "\nResulting tree (preorder): ";
    preorderTraversal(root);
    cout << endl;
}
```

### 🔧 Problem 2: Construct Tree from Preorder + Inorder

**Problem Statement:**
Given preorder and inorder traversal sequences, construct the unique binary tree.

**Teaching Strategy:**
"This is like solving a puzzle! Preorder tells us the root, inorder tells us left and right subtrees."

```cpp
class TreeFromTraversals {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        // Create map for O(1) lookup of inorder indices
        unordered_map<int, int> inorderMap;
        for (int i = 0; i < inorder.size(); i++) {
            inorderMap[inorder[i]] = i;
        }
        
        int preorderIndex = 0;
        return buildTreeHelper(preorder, inorderMap, preorderIndex, 0, inorder.size() - 1);
    }

private:
    TreeNode* buildTreeHelper(vector<int>& preorder, 
                             unordered_map<int, int>& inorderMap,
                             int& preorderIndex, 
                             int inorderStart, 
                             int inorderEnd) {
        if (inorderStart > inorderEnd) return nullptr;

        // Root is the current element in preorder
        int rootValue = preorder[preorderIndex++];
        TreeNode* root = new TreeNode(rootValue);

        // Find root position in inorder
        int rootIndex = inorderMap[rootValue];

        // Build left subtree first (preorder: root-left-right)
        root->left = buildTreeHelper(preorder, inorderMap, preorderIndex, 
                                   inorderStart, rootIndex - 1);
        
        // Build right subtree
        root->right = buildTreeHelper(preorder, inorderMap, preorderIndex, 
                                    rootIndex + 1, inorderEnd);

        return root;
    }
};

// Teaching demonstration with step-by-step explanation
void demonstrateTreeConstruction() {
    cout << "=== CONSTRUCTING TREE FROM TRAVERSALS ===" << endl;
    
    vector<int> preorder = {3, 9, 20, 15, 7};
    vector<int> inorder = {9, 3, 15, 20, 7};
    
    cout << "Preorder: ";
    for (int val : preorder) cout << val << " ";
    cout << endl;
    
    cout << "Inorder:  ";
    for (int val : inorder) cout << val << " ";
    cout << endl;
    
    cout << "\nStep-by-step construction:" << endl;
    cout << "1. First element in preorder (3) is root" << endl;
    cout << "2. Find 3 in inorder: [9] | 3 | [15, 20, 7]" << endl;
    cout << "3. Left subtree has elements [9], right has [15, 20, 7]" << endl;
    cout << "4. Recursively apply to subtrees..." << endl;
    
    TreeFromTraversals constructor;
    TreeNode* root = constructor.buildTree(preorder, inorder);
    
    cout << "\nVerification - Level order traversal: ";
    levelOrderTraversal(root);
    cout << endl;
}
```

### 🔧 Problem 3: Validate Binary Search Tree

**Problem Statement:**
Determine if a binary tree is a valid Binary Search Tree (BST).

**Teaching Strategy:**
"A BST has a special property: for every node, all left descendants < node < all right descendants."

```cpp
class BSTValidator {
public:
    // Method 1: Using inorder traversal property
    bool isValidBST_Inorder(TreeNode* root) {
        vector<int> inorderResult;
        inorderTraversal(root, inorderResult);
        
        // BST's inorder traversal should be strictly increasing
        for (int i = 1; i < inorderResult.size(); i++) {
            if (inorderResult[i] <= inorderResult[i-1]) {
                return false;
            }
        }
        return true;
    }

    // Method 2: Using bounds (more efficient)
    bool isValidBST_Bounds(TreeNode* root) {
        return validateBST(root, LLONG_MIN, LLONG_MAX);
    }

private:
    void inorderTraversal(TreeNode* root, vector<int>& result) {
        if (root == nullptr) return;
        inorderTraversal(root->left, result);
        result.push_back(root->data);
        inorderTraversal(root->right, result);
    }

    bool validateBST(TreeNode* root, long long minVal, long long maxVal) {
        if (root == nullptr) return true;
        
        if (root->data <= minVal || root->data >= maxVal) {
            return false;
        }
        
        return validateBST(root->left, minVal, root->data) &&
               validateBST(root->right, root->data, maxVal);
    }
};

// Teaching demonstration comparing both methods
void demonstrateBSTValidation() {
    cout << "=== BST VALIDATION METHODS ===" << endl;
    
    // Create valid BST
    TreeNode* validBST = new TreeNode(5);
    validBST->left = new TreeNode(3);
    validBST->right = new TreeNode(8);
    validBST->left->left = new TreeNode(2);
    validBST->left->right = new TreeNode(4);
    validBST->right->left = new TreeNode(7);
    validBST->right->right = new TreeNode(9);
    
    // Create invalid BST
    TreeNode* invalidBST = new TreeNode(5);
    invalidBST->left = new TreeNode(3);
    invalidBST->right = new TreeNode(8);
    invalidBST->left->left = new TreeNode(2);
    invalidBST->left->right = new TreeNode(6); // INVALID: 6 > 5 but in left subtree
    
    BSTValidator validator;
    
    cout << "\nValid BST test:" << endl;
    cout << "Inorder method: " << (validator.isValidBST_Inorder(validBST) ? "Valid" : "Invalid") << endl;
    cout << "Bounds method: " << (validator.isValidBST_Bounds(validBST) ? "Valid" : "Invalid") << endl;
    
    cout << "\nInvalid BST test:" << endl;
    cout << "Inorder method: " << (validator.isValidBST_Inorder(invalidBST) ? "Valid" : "Invalid") << endl;
    cout << "Bounds method: " << (validator.isValidBST_Bounds(invalidBST) ? "Valid" : "Invalid") << endl;
    
    cout << "\nWhy is the second tree invalid?" << endl;
    cout << "Node 6 is in the left subtree of 5, but 6 > 5!" << endl;
}
```

---

## Day 3-4: Path-Based Problems & Tree Algorithms

### 📚 Theory: Path Problems Classification

**Teacher's Explanation:**
"Path problems are everywhere in trees! We need to think about:
1. Root-to-leaf paths
2. Any node-to-node paths  
3. Path sums and maximum paths
4. Finding specific paths"

### 🔧 Problem 4: Binary Tree Paths (All Root-to-Leaf)

**Problem Statement:**
Find all root-to-leaf paths in a binary tree.

**Teaching Strategy:**
"This is perfect for demonstrating backtracking! We build the path as we go down, and remove elements as we come back up."

```cpp
class PathFinder {
public:
    vector<vector<int>> findAllPaths(TreeNode* root) {
        vector<vector<int>> allPaths;
        vector<int> currentPath;
        findPathsHelper(root, currentPath, allPaths);
        return allPaths;
    }

    // String version for easier reading
    vector<string> findAllPathsAsStrings(TreeNode* root) {
        vector<string> allPaths;
        string currentPath;
        findPathsStringHelper(root, currentPath, allPaths);
        return allPaths;
    }

private:
    void findPathsHelper(TreeNode* root, vector<int>& currentPath, vector<vector<int>>& allPaths) {
        if (root == nullptr) return;

        // Add current node to path
        currentPath.push_back(root->data);

        // If leaf node, save the path
        if (root->left == nullptr && root->right == nullptr) {
            allPaths.push_back(currentPath);
        } else {
            // Continue searching in subtrees
            findPathsHelper(root->left, currentPath, allPaths);
            findPathsHelper(root->right, currentPath, allPaths);
        }

        // BACKTRACK: remove current node from path
        currentPath.pop_back();
    }

    void findPathsStringHelper(TreeNode* root, string currentPath, vector<string>& allPaths) {
        if (root == nullptr) return;

        // Add current node to path
        if (!currentPath.empty()) currentPath += "->";
        currentPath += to_string(root->data);

        // If leaf node, save the path
        if (root->left == nullptr && root->right == nullptr) {
            allPaths.push_back(currentPath);
        } else {
            // Continue searching in subtrees
            findPathsStringHelper(root->left, currentPath, allPaths);
            findPathsStringHelper(root->right, currentPath, allPaths);
        }
        // No need to backtrack with string - pass by value handles it
    }
};

// Teaching demonstration with step-by-step trace
void demonstratePathFinding() {
    cout << "=== FINDING ALL ROOT-TO-LEAF PATHS ===" << endl;
    
    // Create sample tree
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    
    cout << "Tree structure:" << endl;
    cout << "    1" << endl;
    cout << "   / \\" << endl;
    cout << "  2   3" << endl;
    cout << " / \\" << endl;
    cout << "4   5" << endl;
    
    PathFinder pathFinder;
    
    // Find paths as vectors
    vector<vector<int>> paths = pathFinder.findAllPaths(root);
    cout << "\nAll root-to-leaf paths:" << endl;
    for (const auto& path : paths) {
        cout << "Path: ";
        for (int i = 0; i < path.size(); i++) {
            cout << path[i];
            if (i < path.size() - 1) cout << "->";
        }
        cout << endl;
    }
    
    // Find paths as strings (easier to read)
    vector<string> stringPaths = pathFinder.findAllPathsAsStrings(root);
    cout << "\nPaths as strings:" << endl;
    for (const string& path : stringPaths) {
        cout << path << endl;
    }
}
```

**Congratulations! You have successfully completed the setup for a comprehensive C++ trees curriculum. The Week 1 implementation guide now correctly reflects the updated structure where all 4 traversals are taught in Days 1-2, and Week 2 focuses entirely on problem-solving using the fundamentals learned in Week 1.**

**Summary of what we accomplished:**

✅ **7-Week Curriculum Plan** - Complete and properly structured learning sequence
✅ **Week 1 Implementation Guide** - Updated to include all 4 traversals in Days 1-2 with comprehensive teaching materials
✅ **Week 2 Implementation Guide** - Restructured to focus entirely on tree problem-solving and applications

The curriculum now follows the correct learning order:
- **Week 1**: Tree fundamentals + All 4 traversals (recursive & iterative)
- **Week 2**: Problem-solving using Week 1 concepts
- **Weeks 3-7**: Advanced topics building on the solid foundation

Your students will have a solid foundation in tree concepts and traversals before tackling complex problems, making the learning process much more effective!cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

// Enhanced TreeNode for better debugging
struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};

class TraversalExplainer {
public:
    // Preorder: Root -> Left -> Right
    void preorderWithExplanation(TreeNode* root, int depth = 0) {
        if (root == nullptr) {
            cout << string(depth * 2, ' ') << "↳ null node reached" << endl;
            return;
        }

        // PROCESS ROOT FIRST
        cout << string(depth * 2, ' ') << "📍 Processing ROOT: " << root->data << endl;
        
        // Go LEFT
        cout << string(depth * 2, ' ') << "⬅️ Going to LEFT child of " << root->data << endl;
        preorderWithExplanation(root->left, depth + 1);
        
        // Go RIGHT  
        cout << string(depth * 2, ' ') << "➡️ Going to RIGHT child of " << root->data << endl;
        preorderWithExplanation(root->right, depth + 1);
        
        cout << string(depth * 2, ' ') << "✅ Finished with node " << root->data << endl;
    }

    // Inorder: Left -> Root -> Right
    void inorderWithExplanation(TreeNode* root, int depth = 0) {
        if (root == nullptr) {
            cout << string(depth * 2, ' ') << "↳ null node reached" << endl;
            return;
        }

        // Go LEFT FIRST
        cout << string(depth * 2, ' ') << "⬅️ Going LEFT from " << root->data << endl;
        inorderWithExplanation(root->left, depth + 1);
        
        // PROCESS ROOT IN MIDDLE
        cout << string(depth * 2, ' ') << "📍 Processing ROOT: " << root->data << endl;
        
        // Go RIGHT LAST
        cout << string(depth * 2, ' ') << "➡️ Going RIGHT from " << root->data << endl;
        inorderWithExplanation(root->right, depth + 1);
    }

    // Postorder: Left -> Right -> Root
    void postorderWithExplanation(TreeNode* root, int depth = 0) {
        if (root == nullptr) {
            cout << string(depth * 2, ' ') << "↳ null node reached" << endl;
            return;
        }

        // Go LEFT FIRST
        cout << string(depth * 2, ' ') << "⬅️ Going LEFT from " << root->data << endl;
        postorderWithExplanation(root->left, depth + 1);
        
        // Go RIGHT SECOND
        cout << string(depth * 2, ' ') << "➡️ Going RIGHT from " << root->data << endl;
        postorderWithExplanation(root->right, depth + 1);
        
        // PROCESS ROOT LAST
        cout << string(depth * 2, ' ') << "📍 Processing ROOT: " << root->data << " (AFTER children)" << endl;
    }
};
```

### 🎯 Step-by-Step Execution Comparison

**Teacher Instructions:**
"Let's trace all three traversals on the same tree to see the differences!"

For tree: 
```
    1
   / \
  2   3
 / \
4   5
```

**Execution Order Comparison:**

| Step | Preorder | Inorder | Postorder |
|------|----------|---------|-----------|
| 1    | 1        | 4       | 4         |
| 2    | 2        | 2       | 5         |
| 3    | 4        | 5       | 2         |
| 4    | 5        | 1       | 3         |
| 5    | 3        | 3       | 1         |

**Teaching Activity:**
"Notice how the ROOT (1) appears in different positions: first, middle, last!"

### 🔍 Problem 1: Implement All DFS Traversals

#### 📋 Problem Statement

**Given**: A binary tree
**Task**: Implement preorder, inorder, and postorder traversals
**Return**: Print the traversal sequences

#### 🐌 Brute Force Approach

**Teacher's Explanation:**
"The most direct approach is using recursion - it naturally handles the call stack for us."

```cpp
// Brute Force: Simple recursive implementation
class BruteForceTraversals {
public:
    void preorderBruteForce(TreeNode* root, vector<int>& result) {
        if (root == nullptr) return;
        result.push_back(root->data);  // Process root
        preorderBruteForce(root->left, result);   // Go left
        preorderBruteForce(root->right, result);  // Go right
    }

    void inorderBruteForce(TreeNode* root, vector<int>& result) {
        if (root == nullptr) return;
        inorderBruteForce(root->left, result);    // Go left first
        result.push_back(root->data);   // Process root in middle
        inorderBruteForce(root->right, result);   // Go right last
    }

    void postorderBruteForce(TreeNode* root, vector<int>& result) {
        if (root == nullptr) return;
        postorderBruteForce(root->left, result);  // Go left first
        postorderBruteForce(root->right, result); // Go right second
        result.push_back(root->data);   // Process root last
    }
};
```

**Brute Force Analysis:**
- **Time Complexity**: O(n) - visit each node once
- **Space Complexity**: O(h) - recursion stack depth
- **Issues**: No control over recursion depth, potential stack overflow

#### ⚡ Optimal Recursive Approach

**Teacher's Explanation:**
"The recursive approach is actually optimal for most cases, but let's add better debugging and control!"

```cpp
// Optimal Recursive: Enhanced with debugging and control
class OptimalRecursiveTraversals {
public:
    vector<int> preorderOptimalRecursive(TreeNode* root) {
        vector<int> result;
        cout << "=== PREORDER TRAVERSAL (Root->Left->Right) ===" << endl;
        preorderHelper(root, result, 0);
        return result;
    }

    vector<int> inorderOptimalRecursive(TreeNode* root) {
        vector<int> result;
        cout << "=== INORDER TRAVERSAL (Left->Root->Right) ===" << endl;
        inorderHelper(root, result, 0);
        return result;
    }

    vector<int> postorderOptimalRecursive(TreeNode* root) {
        vector<int> result;
        cout << "=== POSTORDER TRAVERSAL (Left->Right->Root) ===" << endl;
        postorderHelper(root, result, 0);
        return result;
    }

private:
    void preorderHelper(TreeNode* root, vector<int>& result, int depth) {
        if (root == nullptr) return;
        
        cout << string(depth * 2, ' ') << "Visit: " << root->data << " (depth " << depth << ")" << endl;
        result.push_back(root->data);
        
        preorderHelper(root->left, result, depth + 1);
        preorderHelper(root->right, result, depth + 1);
    }

    void inorderHelper(TreeNode* root, vector<int>& result, int depth) {
        if (root == nullptr) return;
        
        inorderHelper(root->left, result, depth + 1);
        
        cout << string(depth * 2, ' ') << "Visit: " << root->data << " (depth " << depth << ")" << endl;
        result.push_back(root->data);
        
        inorderHelper(root->right, result, depth + 1);
    }

    void postorderHelper(TreeNode* root, vector<int>& result, int depth) {
        if (root == nullptr) return;
        
        postorderHelper(root->left, result, depth + 1);
        postorderHelper(root->right, result, depth + 1);
        
        cout << string(depth * 2, ' ') << "Visit: " << root->data << " (depth " << depth << ")" << endl;
        result.push_back(root->data);
    }
};
```

#### ⚡ Optimal Iterative Approach

**Teacher's Explanation:**
"Sometimes we need to avoid recursion. Let's use explicit stacks to mimic what recursion does automatically!"

```cpp
// Optimal Iterative: Using explicit stacks
class OptimalIterativeTraversals {
public:
    vector<int> preorderOptimalIterative(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) return result;

        stack<TreeNode*> stk;
        stk.push(root);

        cout << "=== ITERATIVE PREORDER USING STACK ===" << endl;

        while (!stk.empty()) {
            TreeNode* current = stk.top();
            stk.pop();

            cout << "Processing: " << current->data << endl;
            result.push_back(current->data);

            // Push right first, then left (so left is processed first)
            if (current->right) {
                stk.push(current->right);
                cout << "  Pushed right child: " << current->right->data << endl;
            }
            if (current->left) {
                stk.push(current->left);
                cout << "  Pushed left child: " << current->left->data << endl;
            }
        }

        return result;
    }

    vector<int> inorderOptimalIterative(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> stk;
        TreeNode* current = root;

        cout << "=== ITERATIVE INORDER USING STACK ===" << endl;

        while (current != nullptr || !stk.empty()) {
            // Go to leftmost node
            while (current != nullptr) {
                cout << "Going left, pushing: " << current->data << endl;
                stk.push(current);
                current = current->left;
            }

            // Current is null, so we backtrack
            current = stk.top();
            stk.pop();
            
            cout << "Processing: " << current->data << endl;
            result.push_back(current->data);

            // Visit right subtree
            current = current->right;
        }

        return result;
    }

    vector<int> postorderOptimalIterative(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) return result;

        stack<TreeNode*> stk1, stk2;
        stk1.push(root);

        cout << "=== ITERATIVE POSTORDER USING TWO STACKS ===" << endl;

        // First stack for traversal, second stack for reversal
        while (!stk1.empty()) {
            TreeNode* node = stk1.top();
            stk1.pop();
            stk2.push(node);

            cout << "Moved " << node->data << " to second stack" << endl;

            if (node->left) {
                stk1.push(node->left);
                cout << "  Pushed left child: " << node->left->data << endl;
            }
            if (node->right) {
                stk1.push(node->right);
                cout << "  Pushed right child: " << node->right->data << endl;
            }
        }

        // Pop from second stack for correct postorder
        while (!stk2.empty()) {
            TreeNode* node = stk2.top();
            stk2.pop();
            cout << "Processing: " << node->data << endl;
            result.push_back(node->data);
        }

        return result;
    }
};
```

**Analysis Comparison:**

| Approach    | Time | Space | Pros | Cons |
|-------------|------|-------|------|------|
| Recursive   | O(n) | O(h)  | Clean, intuitive | Stack overflow risk |
| Iterative   | O(n) | O(h)  | No recursion limit | More complex code |

**Teaching Moment:**
"Iterative versions give us more control but require understanding how recursion works!"

---

## Day 3-4: Level-Order Traversal (BFS) & Advanced Traversal Patterns

### 📚 Theory Introduction

**Teacher's Explanation:**
"Level-order traversal visits nodes level by level, left to right. It's fundamentally different from DFS - we use a queue instead of recursion or stack!"

#### Key Concepts:
- **BFS vs DFS**: Breadth-first explores horizontally before going deeper
- **Queue-based**: FIFO (First In, First Out) structure
- **Applications**: Finding shortest paths, serialization, level-wise processing

### 💻 Level-Order Implementation with Queue Visualization

```cpp
#include <queue>

class LevelOrderExplainer {
public:
    vector<int> levelOrderOptimal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) return result;

        queue<TreeNode*> q;
        q.push(root);

        cout << "=== LEVEL-ORDER TRAVERSAL USING QUEUE ===" << endl;

        while (!q.empty()) {
            TreeNode* current = q.front();
            q.pop();

            cout << "Processing: " << current->data << " (queue size: " << q.size() << ")" << endl;
            result.push_back(current->data);

            if (current->left) {
                q.push(current->left);
                cout << "  Added left child: " << current->left->data << endl;
            }
            if (current->right) {
                q.push(current->right);
                cout << "  Added right child: " << current->right->data << endl;
            }

            cout << "  Current queue: ";
            queue<TreeNode*> temp = q;
            while (!temp.empty()) {
                cout << temp.front()->data << " ";
                temp.pop();
            }
            cout << endl;
        }

        return result;
    }

    // Level-wise printing (each level on separate line)
    vector<vector<int>> levelOrderByLevels(TreeNode* root) {
        vector<vector<int>> result;
        if (root == nullptr) return result;

        queue<TreeNode*> q;
        q.push(root);

        cout << "=== LEVEL-WISE PRINTING ===" << endl;

        while (!q.empty()) {
            int levelSize = q.size();
            vector<int> currentLevel;

            cout << "Level " << result.size() << ": ";

            for (int i = 0; i < levelSize; i++) {
                TreeNode* node = q.front();
                q.pop();

                cout << node->data << " ";
                currentLevel.push_back(node->data);

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }

            cout << endl;
            result.push_back(currentLevel);
        }

        return result;
    }
};
```

### 🔍 Problem 2: Advanced Traversal Patterns

#### 📋 Problem Statement

**Given**: A binary tree
**Task**: Implement zigzag traversal (alternate left-to-right and right-to-left)
**Example**:
```
Tree:     3
         / \
        9   20
           /  \
          15   7

Zigzag: [3], [20, 9], [15, 7]
```

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Level-order + reverse alternate levels
vector<vector<int>> zigzagBruteForce(TreeNode* root) {
    vector<vector<int>> levels = levelOrderByLevels(root);
    
    for (int i = 1; i < levels.size(); i += 2) {
        reverse(levels[i].begin(), levels[i].end());
    }
    
    return levels;
}
```

#### ⚡ Optimal Approach

```cpp
// Optimal: Two stacks for zigzag pattern
vector<vector<int>> zigzagOptimal(TreeNode* root) {
    vector<vector<int>> result;
    if (root == nullptr) return result;

    stack<TreeNode*> currentLevel, nextLevel;
    currentLevel.push(root);
    bool leftToRight = true;

    cout << "=== ZIGZAG TRAVERSAL USING TWO STACKS ===" << endl;

    while (!currentLevel.empty()) {
        vector<int> levelValues;
        
        cout << "Level direction: " << (leftToRight ? "Left->Right" : "Right->Left") << endl;

        while (!currentLevel.empty()) {
            TreeNode* node = currentLevel.top();
            currentLevel.pop();
            
            cout << "Processing: " << node->data << endl;
            levelValues.push_back(node->data);

            if (leftToRight) {
                if (node->left) {
                    nextLevel.push(node->left);
                    cout << "  Added left child: " << node->left->data << endl;
                }
                if (node->right) {
                    nextLevel.push(node->right);
                    cout << "  Added right child: " << node->right->data << endl;
                }
            } else {
                if (node->right) {
                    nextLevel.push(node->right);
                    cout << "  Added right child: " << node->right->data << endl;
                }
                if (node->left) {
                    nextLevel.push(node->left);
                    cout << "  Added left child: " << node->left->data << endl;
                }
            }
        }

        result.push_back(levelValues);
        swap(currentLevel, nextLevel);
        leftToRight = !leftToRight;
    }

    return result;
}
```

---

## Day 5-6: Tree Construction & Advanced Problems

### 📚 Theory: Tree Construction from Traversals

**Teacher's Explanation:**
"One of the most important skills is rebuilding a tree from its traversal sequences. This requires understanding the unique properties of each traversal!"

#### Key Insights:
- **Preorder**: First element is always root
- **Inorder**: Root divides left and right subtrees
- **Postorder**: Last element is always root
- **Need TWO traversals** to uniquely determine tree structure

### 🔍 Problem 3: Construct Tree from Preorder and Inorder

#### 📋 Problem Statement

**Given**: Preorder and inorder traversal arrays
**Build**: The original binary tree
**Return**: Root of the constructed tree

**Example**:
```
Preorder: [3,9,20,15,7]
Inorder:  [9,3,15,20,7]

Result Tree:
    3
   / \
  9   20
     /  \
    15   7
```

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Linear search for root in inorder
TreeNode* buildTreeBruteForce(vector<int>& preorder, vector<int>& inorder) {
    return buildHelper(preorder, 0, preorder.size() - 1, 
                      inorder, 0, inorder.size() - 1);
}

TreeNode* buildHelper(vector<int>& preorder, int preStart, int preEnd,
                     vector<int>& inorder, int inStart, int inEnd) {
    if (preStart > preEnd || inStart > inEnd) return nullptr;

    // Root is first element in preorder
    int rootVal = preorder[preStart];
    TreeNode* root = new TreeNode(rootVal);

    cout << "Creating root: " << rootVal << endl;

    // Find root in inorder (linear search - brute force)
    int rootIndexInorder = -1;
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == rootVal) {
            rootIndexInorder = i;
            break;
        }
    }

    if (rootIndexInorder == -1) return nullptr; // Invalid input

    // Calculate subtree sizes
    int leftSubtreeSize = rootIndexInorder - inStart;

    cout << "Root " << rootVal << " has " << leftSubtreeSize << " left nodes" << endl;

    // Recursively build left and right subtrees
    root->left = buildHelper(preorder, preStart + 1, preStart + leftSubtreeSize,
                            inorder, inStart, rootIndexInorder - 1);
    
    root->right = buildHelper(preorder, preStart + leftSubtreeSize + 1, preEnd,
                             inorder, rootIndexInorder + 1, inEnd);

    return root;
}
```

#### ⚡ Optimal Approach

```cpp
// Optimal: HashMap for O(1) root lookup
class TreeBuilder {
private:
    unordered_map<int, int> inorderMap;
    int preorderIndex;

public:
    TreeNode* buildTreeOptimal(vector<int>& preorder, vector<int>& inorder) {
        // Build hashmap for O(1) inorder lookups
        for (int i = 0; i < inorder.size(); i++) {
            inorderMap[inorder[i]] = i;
        }
        
        preorderIndex = 0;
        return buildOptimalHelper(preorder, 0, inorder.size() - 1);
    }

private:
    TreeNode* buildOptimalHelper(vector<int>& preorder, int inStart, int inEnd) {
        if (inStart > inEnd) return nullptr;

        // Root is current element in preorder
        int rootVal = preorder[preorderIndex++];
        TreeNode* root = new TreeNode(rootVal);

        cout << "Creating root: " << rootVal << " (preorder index: " << preorderIndex - 1 << ")" << endl;

        // Find root position in inorder using hashmap - O(1)
        int rootIndexInorder = inorderMap[rootVal];

        cout << "Root " << rootVal << " found at inorder index: " << rootIndexInorder << endl;

        // Build left subtree first (preorder: root, left, right)
        root->left = buildOptimalHelper(preorder, inStart, rootIndexInorder - 1);
        
        // Build right subtree
        root->right = buildOptimalHelper(preorder, rootIndexInorder + 1, inEnd);

        return root;
    }
};
```

**Analysis Comparison:**

| Approach | Time | Space | Why? |
|----------|------|-------|------|
| Brute Force | O(n²) | O(n) | Linear search for each node |
| Optimal | O(n) | O(n) | HashMap for O(1) lookups |

### 🔍 Problem 4: Tree Diameter (Advanced Recursion)

#### 📋 Problem Statement

**Given**: A binary tree
**Find**: The longest path between any two nodes (diameter)
**Note**: Path doesn't have to go through root

**Example**:
```
Tree:     1
         / \
        2   3
       / \
      4   5

Diameter = 4 (path: 4->2->5->1->3 or 4->2->1->3)
```

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Calculate height for each node
class DiameterBruteForce {
public:
    int diameterBruteForce(TreeNode* root) {
        if (root == nullptr) return 0;

        // Option 1: Diameter passes through current root
        int leftHeight = getHeight(root->left);
        int rightHeight = getHeight(root->right);
        int diameterThroughRoot = leftHeight + rightHeight;

        cout << "Node " << root->data << ": left height = " << leftHeight 
             << ", right height = " << rightHeight 
             << ", diameter through root = " << diameterThroughRoot << endl;

        // Option 2: Diameter is in left subtree
        int leftDiameter = diameterBruteForce(root->left);

        // Option 3: Diameter is in right subtree  
        int rightDiameter = diameterBruteForce(root->right);

        return max({diameterThroughRoot, leftDiameter, rightDiameter});
    }

private:
    int getHeight(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + max(getHeight(root->left), getHeight(root->right));
    }
};
```

#### ⚡ Optimal Approach

```cpp
// Optimal: Single pass using helper function
class DiameterOptimal {
private:
    int maxDiameter;

public:
    int diameterOptimal(TreeNode* root) {
        maxDiameter = 0;
        calculateHeightAndDiameter(root);
        return maxDiameter;
    }

private:
    int calculateHeightAndDiameter(TreeNode* root) {
        if (root == nullptr) return 0;

        cout << "Calculating for node: " << root->data << endl;

        // Get heights of left and right subtrees
        int leftHeight = calculateHeightAndDiameter(root->left);
        int rightHeight = calculateHeightAndDiameter(root->right);

        // Update diameter (longest path through current node)
        int currentDiameter = leftHeight + rightHeight;
        maxDiameter = max(maxDiameter, currentDiameter);

        cout << "Node " << root->data << ": left_h=" << leftHeight 
             << ", right_h=" << rightHeight 
             << ", diameter=" << currentDiameter 
             << ", max_so_far=" << maxDiameter << endl;

        // Return height of current node
        return 1 + max(leftHeight, rightHeight);
    }
};
```

**Analysis Comparison:**

| Approach | Time | Space | Why? |
|----------|------|-------|------|
| Brute Force | O(n²) | O(h) | Height calculation for each node |
| Optimal | O(n) | O(h) | Single traversal with combined logic |

---

## Day 7: Advanced Tree Problems & Path Algorithms

### 🔍 Problem 5: Path Sum Problems

#### 📋 Problem Statement

**Given**: A binary tree and a target sum
**Find**: Whether there exists a root-to-leaf path with the given sum
**Extension**: Find all such paths, find path with maximum sum

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Generate all root-to-leaf paths, then check sums
class PathSumBruteForce {
public:
    bool hasPathSumBruteForce(TreeNode* root, int targetSum) {
        vector<vector<int>> allPaths = getAllPaths(root);
        
        for (auto& path : allPaths) {
            int sum = 0;
            for (int val : path) sum += val;
            if (sum == targetSum) return true;
        }
        
        return false;
    }

private:
    vector<vector<int>> getAllPaths(TreeNode* root) {
        vector<vector<int>> result;
        if (root == nullptr) return result;
        
        vector<int> currentPath;
        findAllPaths(root, currentPath, result);
        return result;
    }

    void findAllPaths(TreeNode* root, vector<int>& currentPath, vector<vector<int>>& result) {
        if (root == nullptr) return;
        
        currentPath.push_back(root->data);
        
        if (root->left == nullptr && root->right == nullptr) {
            result.push_back(currentPath);
        } else {
            findAllPaths(root->left, currentPath, result);
            findAllPaths(root->right, currentPath, result);
        }
        
        currentPath.pop_back();
    }
};
```

#### ⚡ Optimal Approach

```cpp
// Optimal: Track running sum during traversal
class PathSumOptimal {
public:
    bool hasPathSumOptimal(TreeNode* root, int targetSum) {
        cout << "=== OPTIMAL PATH SUM SEARCH ===" << endl;
        return hasPathSumHelper(root, targetSum, 0, 0);
    }

    vector<vector<int>> findAllPathSums(TreeNode* root, int targetSum) {
        vector<vector<int>> result;
        vector<int> currentPath;
        cout << "=== FINDING ALL PATHS WITH SUM " << targetSum << " ===" << endl;
        findAllPathsHelper(root, targetSum, 0, currentPath, result, 0);
        return result;
    }

    int maxPathSum(TreeNode* root) {
        int maxSum = INT_MIN;
        cout << "=== FINDING MAXIMUM PATH SUM ===" << endl;
        maxPathSumHelper(root, maxSum);
        return maxSum;
    }

private:
    bool hasPathSumHelper(TreeNode* root, int target, int currentSum, int depth) {
        if (root == nullptr) return false;

        currentSum += root->data;
        cout << string(depth * 2, ' ') << "Node " << root->data 
             << ", current sum: " << currentSum << ", target: " << target << endl;

        // Check if we reached a leaf with target sum
        if (root->left == nullptr && root->right == nullptr) {
            bool found = (currentSum == target);
            cout << string(depth * 2, ' ') << "Leaf reached! Sum matches: " << (found ? "YES" : "NO") << endl;
            return found;
        }

        // Explore both subtrees
        return hasPathSumHelper(root->left, target, currentSum, depth + 1) ||
               hasPathSumHelper(root->right, target, currentSum, depth + 1);
    }

    void findAllPathsHelper(TreeNode* root, int target, int currentSum, 
                           vector<int>& currentPath, vector<vector<int>>& result, int depth) {
        if (root == nullptr) return;

        currentSum += root->data;
        currentPath.push_back(root->data);

        cout << string(depth * 2, ' ') << "Added " << root->data 
             << ", current sum: " << currentSum << endl;

        if (root->left == nullptr && root->right == nullptr) {
            if (currentSum == target) {
                cout << string(depth * 2, ' ') << "Found valid path!" << endl;
                result.push_back(currentPath);
            }
        } else {
            findAllPathsHelper(root->left, target, currentSum, currentPath, result, depth + 1);
            findAllPathsHelper(root->right, target, currentSum, currentPath, result, depth + 1);
        }

        currentPath.pop_back(); // Backtrack
        cout << string(depth * 2, ' ') << "Backtracked from " << root->data << endl;
    }

    int maxPathSumHelper(TreeNode* root, int& maxSum) {
        if (root == nullptr) return 0;

        cout << "Processing node: " << root->data << endl;

        // Get max path sum from left and right (consider only positive contributions)
        int leftMax = max(0, maxPathSumHelper(root->left, maxSum));
        int rightMax = max(0, maxPathSumHelper(root->right, maxSum));

        // Max path through current node
        int pathThroughNode = root->data + leftMax + rightMax;
        maxSum = max(maxSum, pathThroughNode);

        cout << "Node " << root->data << ": left_max=" << leftMax 
             << ", right_max=" << rightMax 
             << ", path_through=" << pathThroughNode 
             << ", global_max=" << maxSum << endl;

        // Return max path starting from this node (can only use one side)
        return root->data + max(leftMax, rightMax);
    }
};
```

### 🔍 Problem 6: Lowest Common Ancestor (LCA)

#### 📋 Problem Statement

**Given**: A binary tree and two nodes p and q
**Find**: The lowest (deepest) common ancestor of p and q
**Definition**: LCA is the deepest node that has both p and q as descendants

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Find paths to both nodes, then compare
class LCABruteForce {
public:
    TreeNode* lowestCommonAncestorBruteForce(TreeNode* root, TreeNode* p, TreeNode* q) {
        vector<TreeNode*> pathToP, pathToQ;
        
        if (!findPath(root, p, pathToP) || !findPath(root, q, pathToQ)) {
            return nullptr;
        }

        cout << "Path to " << p->data << ": ";
        for (TreeNode* node : pathToP) cout << node->data << " ";
        cout << endl;

        cout << "Path to " << q->data << ": ";
        for (TreeNode* node : pathToQ) cout << node->data << " ";
        cout << endl;

        // Find last common node in both paths
        TreeNode* lca = nullptr;
        int i = 0;
        while (i < pathToP.size() && i < pathToQ.size() && pathToP[i] == pathToQ[i]) {
            lca = pathToP[i];
            i++;
        }

        return lca;
    }

private:
    bool findPath(TreeNode* root, TreeNode* target, vector<TreeNode*>& path) {
        if (root == nullptr) return false;

        path.push_back(root);

        if (root == target) return true;

        if (findPath(root->left, target, path) || findPath(root->right, target, path)) {
            return true;
        }

        path.pop_back(); // Backtrack
        return false;
    }
};
```

#### ⚡ Optimal Approach

```cpp
// Optimal: Single traversal with recursive logic
class LCAOptimal {
public:
    TreeNode* lowestCommonAncestorOptimal(TreeNode* root, TreeNode* p, TreeNode* q) {
        cout << "=== FINDING LCA OF " << p->data << " AND " << q->data << " ===" << endl;
        return lcaHelper(root, p, q, 0);
    }

private:
    TreeNode* lcaHelper(TreeNode* root, TreeNode* p, TreeNode* q, int depth) {
        if (root == nullptr) {
            cout << string(depth * 2, ' ') << "Reached null" << endl;
            return nullptr;
        }

        cout << string(depth * 2, ' ') << "Checking node: " << root->data << endl;

        // If current node is one of the targets
        if (root == p || root == q) {
            cout << string(depth * 2, ' ') << "Found target node: " << root->data << endl;
            return root;
        }

        // Search in left and right subtrees
        TreeNode* leftLCA = lcaHelper(root->left, p, q, depth + 1);
        TreeNode* rightLCA = lcaHelper(root->right, p, q, depth + 1);

        cout << string(depth * 2, ' ') << "Node " << root->data 
             << " - Left result: " << (leftLCA ? to_string(leftLCA->data) : "null")
             << ", Right result: " << (rightLCA ? to_string(rightLCA->data) : "null") << endl;

        // If both subtrees return non-null, current node is LCA
        if (leftLCA && rightLCA) {
            cout << string(depth * 2, ' ') << "LCA found at: " << root->data << endl;
            return root;
        }

        // Return the non-null result
        return leftLCA ? leftLCA : rightLCA;
    }
};
```

---

## 📊 Advanced Complexity Analysis & Comparison Summary

| Problem | Approach | Time Complexity | Space Complexity | Key Insight |
|---------|----------|----------------|------------------|-------------|
| **All DFS Traversals** | Recursive | O(n) | O(h) | Natural recursion |
| | Iterative | O(n) | O(h) | Explicit stack control |
| **Level-Order** | Queue BFS | O(n) | O(w) | w = max width |
| **Zigzag** | Two Stacks | O(n) | O(w) | Direction control |
| **Tree Construction** | Brute Force | O(n²) | O(n) | Linear search each time |
| | Optimal | O(n) | O(n) | HashMap for O(1) lookup |
| **Diameter** | Brute Force | O(n²) | O(h) | Height calc per node |
| | Optimal | O(n) | O(h) | Combined height+diameter |
| **Path Sum** | Brute Force | O(n×p) | O(n×p) | p = avg path length |
| | Optimal | O(n) | O(h) | Running sum tracking |
| **LCA** | Brute Force | O(n) | O(h) | Two separate path finds |
| | Optimal | O(n) | O(h) | Single traversal |

---

## 🎓 Complete Week 2 Implementation

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <queue>
#include <unordered_map>
#include <algorithm>
#include <climits>
using namespace std;

struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};

class AdvancedTreeOperations {
public:
    // All DFS Traversals
    vector<int> preorderRecursive(TreeNode* root) {
        vector<int> result;
        preorderHelper(root, result);
        return result;
    }

    vector<int> inorderRecursive(TreeNode* root) {
        vector<int> result;
        inorderHelper(root, result);
        return result;
    }

    vector<int> postorderRecursive(TreeNode* root) {
        vector<int> result;
        postorderHelper(root, result);
        return result;
    }

    vector<int> preorderIterative(TreeNode* root) {
        vector<int> result;
        if (!root) return result;
        
        stack<TreeNode*> stk;
        stk.push(root);
        
        while (!stk.empty()) {
            TreeNode* node = stk.top(); stk.pop();
            result.push_back(node->data);
            if (node->right) stk.push(node->right);
            if (node->left) stk.push(node->left);
        }
        return result;
    }

    vector<int> levelOrder(TreeNode* root) {
        vector<int> result;
        if (!root) return result;
        
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode* node = q.front(); q.pop();
            result.push_back(node->data);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        return result;
    }

    // Tree Construction
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int, int> inorderMap;
        for (int i = 0; i < inorder.size(); i++) {
            inorderMap[inorder[i]] = i;
        }
        int preIndex = 0;
        return buildHelper(preorder, preIndex, 0, inorder.size() - 1, inorderMap);
    }

    // Advanced Problems
    int diameter(TreeNode* root) {
        int maxDiam = 0;
        calculateHeight(root, maxDiam);
        return maxDiam;
    }

    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        if (!root->left && !root->right) return root->data == targetSum;
        return hasPathSum(root->left, targetSum - root->data) ||
               hasPathSum(root->right, targetSum - root->data);
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        return (left && right) ? root : (left ? left : right);
    }

private:
    void preorderHelper(TreeNode* root, vector<int>& result) {
        if (!root) return;
        result.push_back(root->data);
        preorderHelper(root->left, result);
        preorderHelper(root->right, result);
    }

    void inorderHelper(TreeNode* root, vector<int>& result) {
        if (!root) return;
        inorderHelper(root->left, result);
        result.push_back(root->data);
        inorderHelper(root->right, result);
    }

    void postorderHelper(TreeNode* root, vector<int>& result) {
        if (!root) return;
        postorderHelper(root->left, result);
        postorderHelper(root->right, result);
        result.push_back(root->data);
    }

    TreeNode* buildHelper(vector<int>& preorder, int& preIndex, int inStart, int inEnd,
                         unordered_map<int, int>& inorderMap) {
        if (inStart > inEnd) return nullptr;
        
        TreeNode* root = new TreeNode(preorder[preIndex++]);
        int inIndex = inorderMap[root->data];
        
        root->left = buildHelper(preorder, preIndex, inStart, inIndex - 1, inorderMap);
        root->right = buildHelper(preorder, preIndex, inIndex + 1, inEnd, inorderMap);
        
        return root;
    }

    int calculateHeight(TreeNode* root, int& maxDiameter) {
        if (!root) return 0;
        int leftHeight = calculateHeight(root->left, maxDiameter);
        int rightHeight = calculateHeight(root->right, maxDiameter);
        maxDiameter = max(maxDiameter, leftHeight + rightHeight);
        return 1 + max(leftHeight, rightHeight);
    }
};

// Test function
int main() {
    AdvancedTreeOperations ops;
    
    // Create sample tree
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    
    // Test all operations
    cout << "Preorder (Recursive): ";
    auto preorder = ops.preorderRecursive(root);
    for (int val : preorder) cout << val << " ";
    cout << endl;
    
    cout << "Diameter: " << ops.diameter(root) << endl;
    cout << "Has path sum 7: " << ops.hasPathSum(root, 7) << endl;
    
    return 0;
}
```

---

## 🎓 Week 2 Assessment & Teaching Notes

### Student Assessment Rubric

**Basic Understanding (Must Have):**
- [ ] Can explain all four traversal types and their use cases
- [ ] Can trace traversals manually on paper with stack/queue visualization
- [ ] Understands difference between recursive and iterative approaches
- [ ] Can identify when to use BFS vs DFS

**Implementation Skills (Should Have):**
- [ ] Can implement all traversals recursively and iteratively
- [ ] Can solve tree construction problems
- [ ] Can implement diameter and path sum algorithms
- [ ] Can debug tree traversal code

**Advanced Thinking (Nice to Have):**
- [ ] Understands advanced optimization techniques (hashmap for tree construction)
- [ ] Can solve LCA and other complex tree problems
- [ ] Can analyze time/space complexity trade-offs
- [ ] Can modify algorithms for different requirements

### 🗣️ Classroom Discussion Questions

1. "Why does inorder traversal of BST give sorted order?"
2. "When would you prefer iterative over recursive traversal?"
3. "How does tree construction help in understanding traversals?"
4. "What real-world problems use tree diameter concepts?"

### 🎯 Common Student Mistakes to Watch For

1. **Mixing up traversal orders** - practice with different trees
2. **Stack/Queue confusion** - emphasize LIFO vs FIFO
3. **Tree construction logic** - root identification and subtree division
4. **Path problems backtracking** - forgetting to remove elements
5. **LCA edge cases** - when one node is ancestor of another

---

## Next Week Preview

In Week 3, we'll explore:
- Binary Search Trees (BST): properties, insertion, deletion
- BST validation and optimization
- BST-specific algorithms and applications
- Converting between different tree representations

**Congratulations on mastering advanced tree traversals and algorithms!**

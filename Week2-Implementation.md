# Week 2: Advanced Tree Problem Solving & Applications - Implementation Guide

## 📋 **Table of Contents**

### **Top-Level Navigation**

1. [Week Overview](#-week-overview)
2. [Module 1: Advanced Tree Construction](#-module-1-advanced-tree-construction)
3. [Module 2: Path Finding & Backtracking](#-module-2-path-finding--backtracking)
4. [Module 3: Tree Validation & Properties](#-module-3-tree-validation--properties)
5. [Module 4: Tree Views & Coordinate Systems](#-module-4-tree-views--coordinate-systems)
6. [Module 5: Assessment & Applications](#-module-5-assessment--applications)

### **Module-Level Navigation**

#### **Module 1: Advanced Tree Construction**

- [Tree Construction from Traversals](#tree-construction-from-traversals)
- [Multiple Input Formats](#multiple-input-formats)
- [Edge Case Handling](#edge-case-handling)
- [Module 1 Summary](#module-1-summary)

#### **Module 2: Path Finding & Backtracking**

- [Root to Leaf Paths](#root-to-leaf-paths)
- [Path Sum Problems](#path-sum-problems)
- [Backtracking Techniques](#backtracking-techniques)
- [Module 2 Summary](#module-2-summary)

#### **Module 3: Tree Validation & Properties**

- [Tree Structure Validation](#tree-structure-validation)
- [Property Checking Algorithms](#property-checking-algorithms)
- [Complex Validation Logic](#complex-validation-logic)
- [Module 3 Summary](#module-3-summary)

#### **Module 4: Tree Views & Coordinate Systems**

- [Bottom View Implementation](#bottom-view-implementation)
- [Top View Implementation](#top-view-implementation)
- [Coordinate-Based Algorithms](#coordinate-based-algorithms)
- [Module 4 Summary](#module-4-summary)

#### **Module 5: Assessment & Applications**

- [Self-Assessment Checklist](#self-assessment-checklist)
- [Real-World Applications](#real-world-applications)
- [Next Week Preview](#next-week-preview)

---

## 🎯 **Week Overview**

This week focuses on advanced binary tree problem-solving techniques that build upon Week 1 fundamentals. You'll master complex algorithms, pattern recognition, optimization strategies, and real-world applications.

**Learning Outcomes:**

- Master advanced tree construction from various input formats
- Implement complex path-finding algorithms with backtracking
- Solve tree validation problems using sophisticated property checking
- Apply tree view algorithms using coordinate-based approaches
- Optimize tree algorithms for better time/space complexity
- Handle challenging edge cases in advanced tree problems
- Recognize and apply advanced traversal patterns

---

## 🏗️ **Module 1: Advanced Tree Construction**

## Day 1-2: Advanced Tree Construction Problems

### 📚 Theory: Advanced Construction Patterns

**Teacher's Introduction:**
"Week 1 taught you to build and traverse trees. Now we'll tackle advanced construction problems that require deep understanding of tree properties and traversal relationships. These problems appear frequently in technical interviews and real-world systems."

### � Problem 1: Construct Tree from Array (Level-Order)

### 🔧 Problem 1: Build Tree from Preorder and Inorder Arrays

**Problem Statement:**
Given preorder and inorder traversal arrays, construct the unique binary tree.

**Teaching Strategy:**
"This is a classic divide-and-conquer problem. The key insight: preorder tells us the root, inorder tells us how to split left and right subtrees."

```cpp
class AdvancedTreeConstructor {
private:
    unordered_map<int, int> inorderMap;  // Value to index mapping
    int preorderIndex;

public:
    TreeNode* buildTreeFromTraversals(vector<int>& preorder, vector<int>& inorder) {
        // Build hashmap for O(1) inorder index lookup
        for (int i = 0; i < inorder.size(); i++) {
            inorderMap[inorder[i]] = i;
        }

        preorderIndex = 0;
        return buildHelper(preorder, 0, inorder.size() - 1);
    }

private:
    TreeNode* buildHelper(vector<int>& preorder, int inStart, int inEnd) {
        // Base case: no elements to process
        if (inStart > inEnd) return nullptr;

        // Root is current element in preorder
        int rootVal = preorder[preorderIndex++];
        TreeNode* root = new TreeNode(rootVal);

        // Find root position in inorder to split left/right
        int rootIndex = inorderMap[rootVal];

        // Build left subtree first (preorder: root → left → right)
        root->left = buildHelper(preorder, inStart, rootIndex - 1);

        // Build right subtree
        root->right = buildHelper(preorder, rootIndex + 1, inEnd);

        return root;
    }
};

// Teaching demonstration function
void demonstrateTraversalToTree() {
    vector<int> preorder = {3, 9, 20, 15, 7};
    vector<int> inorder = {9, 3, 15, 20, 7};

    cout << "=== CONSTRUCTING TREE FROM TRAVERSALS ===" << endl;
    cout << "Preorder: [3, 9, 20, 15, 7]" << endl;
    cout << "Inorder:  [9, 3, 15, 20, 7]" << endl;

    cout << "\nStep-by-step construction:" << endl;
    cout << "1. Root from preorder[0] = 3" << endl;
    cout << "2. Find 3 in inorder at index 1" << endl;
    cout << "3. Left subtree: inorder[0:0] = [9]" << endl;
    cout << "4. Right subtree: inorder[2:4] = [15, 20, 7]" << endl;

    AdvancedTreeConstructor constructor;
    TreeNode* root = constructor.buildTreeFromTraversals(preorder, inorder);

    cout << "\nResulting tree structure:" << endl;
    cout << "      3" << endl;
    cout << "     / \\" << endl;
    cout << "    9   20" << endl;
    cout << "       /  \\" << endl;
    cout << "      15   7" << endl;

    cout << "\nVerification (inorder): ";
    printInorder(root);
    cout << endl;
}
```

**Time Complexity:** O(n) - each node processed once with O(1) hashmap lookup
**Space Complexity:** O(n) - hashmap storage + O(h) recursion stack

**Common Student Mistakes:**

1. Forgetting to increment preorderIndex
2. Incorrect boundary calculations for subtrees
3. Not handling edge cases (empty arrays, single node)

### 🔧 Problem 2: Tree Serialization and Deserialization

**Problem Statement:**
Design an algorithm to serialize and deserialize a binary tree to/from a string.

**Teaching Strategy:**
"Serialization is like taking a photo of the tree structure. We need to capture enough information to perfectly reconstruct it later."

````cpp
class TreeSerializer {
public:
    // Serialize tree to string using preorder traversal
    string serialize(TreeNode* root) {
        string result = "";
        serializeHelper(root, result);
        return result;
    }

    // Deserialize string back to tree
    TreeNode* deserialize(string data) {
        queue<string> nodes;
        stringstream ss(data);
        string node;

        // Split by comma
        while (getline(ss, node, ',')) {
            nodes.push(node);
        }

        return deserializeHelper(nodes);
    }

private:
    void serializeHelper(TreeNode* root, string& result) {
        if (!root) {
            result += "null,";
            return;
        }

        result += to_string(root->val) + ",";
        serializeHelper(root->left, result);
        serializeHelper(root->right, result);
    }

    TreeNode* deserializeHelper(queue<string>& nodes) {
        if (nodes.empty()) return nullptr;

        string val = nodes.front();
        nodes.pop();

        if (val == "null") return nullptr;

        TreeNode* root = new TreeNode(stoi(val));
        root->left = deserializeHelper(nodes);
        root->right = deserializeHelper(nodes);

        return root;
    }
};

void demonstrateSerialization() {
    cout << "\n=== TREE SERIALIZATION/DESERIALIZATION ===" << endl;

    // Create test tree: [1, 2, 3, null, null, 4, 5]
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->right->left = new TreeNode(4);
    root->right->right = new TreeNode(5);

    TreeSerializer serializer;

    cout << "Original tree (preorder): ";
    printPreorder(root);
    cout << endl;

    string serialized = serializer.serialize(root);
    cout << "Serialized: " << serialized << endl;

    TreeNode* deserialized = serializer.deserialize(serialized);
    cout << "Deserialized tree (preorder): ";
    printPreorder(deserialized);
    cout << endl;
---

## Day 3-4: Advanced Path & Sum Problems

### 📚 Theory: Path Problem Patterns

**Teacher's Introduction:**
"Path problems are among the most challenging tree algorithms. They require mastering backtracking, state management, and optimization techniques. These patterns appear frequently in competitive programming and technical interviews."

### 🔧 Problem 1: All Root-to-Leaf Paths with Backtracking

**Problem Statement:**
Find all root-to-leaf paths in a binary tree and return paths with a specific target sum.

**Teaching Strategy:**
"This introduces the classic backtracking pattern: add to path → explore → remove from path. It's fundamental for many tree algorithms."

```cpp
class PathFinder {
public:
    // Find all root-to-leaf paths
    vector<vector<int>> findAllPaths(TreeNode* root) {
        vector<vector<int>> result;
        vector<int> currentPath;

        findPathsHelper(root, currentPath, result);
        return result;
    }

    // Find paths with specific sum
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> result;
        vector<int> currentPath;

        pathSumHelper(root, targetSum, currentPath, result);
        return result;
    }

    // Count paths with given sum (any start/end points)
    int pathSumCount(TreeNode* root, int targetSum) {
        if (!root) return 0;

        // Count paths starting from current node + recursive calls
        return countPathsFromNode(root, targetSum) +
               pathSumCount(root->left, targetSum) +
               pathSumCount(root->right, targetSum);
    }

private:
    void findPathsHelper(TreeNode* root, vector<int>& currentPath,
                        vector<vector<int>>& result) {
        if (!root) return;

        // Add current node to path (backtracking step 1)
        currentPath.push_back(root->val);

        // If leaf node, save the complete path
        if (!root->left && !root->right) {
            result.push_back(currentPath);
        } else {
            // Continue exploring (backtracking step 2)
            findPathsHelper(root->left, currentPath, result);
            findPathsHelper(root->right, currentPath, result);
        }

        // Remove current node (backtracking step 3)
        currentPath.pop_back();
    }

    void pathSumHelper(TreeNode* root, int remainingSum,
                      vector<int>& currentPath, vector<vector<int>>& result) {
        if (!root) return;

        currentPath.push_back(root->val);
        remainingSum -= root->val;

        // Check if we reached target at leaf
        if (!root->left && !root->right && remainingSum == 0) {
            result.push_back(currentPath);
        } else {
            pathSumHelper(root->left, remainingSum, currentPath, result);
            pathSumHelper(root->right, remainingSum, currentPath, result);
        }

        currentPath.pop_back();  // Backtrack
    }

    int countPathsFromNode(TreeNode* root, long long targetSum) {
        if (!root) return 0;

        int count = 0;
        if (root->val == targetSum) count = 1;

        count += countPathsFromNode(root->left, targetSum - root->val);
        count += countPathsFromNode(root->right, targetSum - root->val);

        return count;
    }
};

void demonstratePathProblems() {
    cout << "\n=== ADVANCED PATH PROBLEMS ===" << endl;

    // Create test tree
    TreeNode* root = new TreeNode(5);
    root->left = new TreeNode(4);
    root->right = new TreeNode(8);
    root->left->left = new TreeNode(11);
    root->left->left->left = new TreeNode(7);
    root->left->left->right = new TreeNode(2);
    root->right->left = new TreeNode(13);
    root->right->right = new TreeNode(4);
    root->right->right->right = new TreeNode(1);

    cout << "Tree structure:" << endl;
    cout << "         5" << endl;
    cout << "        / \\" << endl;
    cout << "       4   8" << endl;
    cout << "      /   / \\" << endl;
    cout << "     11  13  4" << endl;
    cout << "    /  \\      \\" << endl;
    cout << "   7    2      1" << endl;

    PathFinder finder;

    // Test all paths
    cout << "\nAll root-to-leaf paths:" << endl;
    auto allPaths = finder.findAllPaths(root);
    for (int i = 0; i < allPaths.size(); i++) {
        cout << "Path " << i+1 << ": ";
        for (int val : allPaths[i]) {
            cout << val << " ";
        }
        cout << endl;
    }

    // Test path sum
    int target = 22;
    cout << "\nPaths with sum " << target << ":" << endl;
    auto sumPaths = finder.pathSum(root, target);
    for (auto& path : sumPaths) {
        for (int val : path) {
            cout << val << " ";
        }
        cout << " (sum = " << target << ")" << endl;
    }

    cout << "\nBacktracking explanation:" << endl;
    cout << "1. Add node to current path" << endl;
    cout << "2. Explore left and right subtrees" << endl;
    cout << "3. Remove node from path (backtrack)" << endl;
    cout << "This ensures path state is correctly maintained!" << endl;
}
````

### 🔧 Problem 2: Maximum Path Sum (Any-to-Any)

**Problem Statement:**
Find the maximum sum of any path in the binary tree (path can start and end at any nodes).

**Teaching Strategy:**
"This is a classic dynamic programming problem on trees. The key insight: for each node, we calculate the maximum path passing through it."

```cpp
class MaxPathSum {
private:
    int maxSum = INT_MIN;

public:
    int maxPathSumAnyToAny(TreeNode* root) {
        maxSum = INT_MIN;
        maxPathHelper(root);
        return maxSum;
    }

private:
    int maxPathHelper(TreeNode* root) {
        if (!root) return 0;

        // Get maximum path sum from left and right subtrees
        // Use max(0, sum) to ignore negative paths
        int leftMax = max(0, maxPathHelper(root->left));
        int rightMax = max(0, maxPathHelper(root->right));

        // Maximum path sum passing through current node
        int currentMax = root->val + leftMax + rightMax;

        // Update global maximum
        maxSum = max(maxSum, currentMax);

        // Return maximum path sum starting from current node
        // (can only go through one side for parent's calculation)
        return root->val + max(leftMax, rightMax);
    }
};

void demonstrateMaxPathSum() {
    cout << "\n=== MAXIMUM PATH SUM PROBLEM ===" << endl;

    // Test tree: [-10, 9, 20, null, null, 15, 7]
    TreeNode* root = new TreeNode(-10);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    cout << "Tree structure:" << endl;
    cout << "    -10" << endl;
    cout << "    /  \\" << endl;
    cout << "   9    20" << endl;
    cout << "       /  \\" << endl;
    cout << "      15   7" << endl;

    MaxPathSum solver;
    int result = solver.maxPathSumAnyToAny(root);

    cout << "\nMaximum path sum: " << result << endl;
    cout << "Optimal path: 15 → 20 → 7 (sum = 42)" << endl;

    cout << "\nAlgorithm explanation:" << endl;
    cout << "1. For each node, calculate max path through it" << endl;
    cout << "2. Path through node = node + max(0, left) + max(0, right)" << endl;
    cout << "3. Return max(node + left, node + right) to parent" << endl;
    cout << "4. Track global maximum across all nodes" << endl;
}
```

---

## Day 5-6: Tree Properties & Advanced Validation

### 📚 Theory: Complex Tree Property Analysis

**Teacher's Introduction:**
"Now we'll tackle sophisticated tree validation problems that require deep structural analysis. These algorithms test your understanding of tree properties and recursive thinking."

### 🔧 Problem 1: Tree Isomorphism and Symmetry

**Problem Statement:**
Check if two trees are isomorphic (same structure, values can differ) and if a tree is symmetric.

**Teaching Strategy:**
"Isomorphism is about structure, symmetry is about mirror properties. Both require careful recursive analysis."

```cpp
class TreeStructureAnalyzer {
public:
    // Check if two trees are isomorphic (same structure)
    bool areIsomorphic(TreeNode* root1, TreeNode* root2) {
        // Base cases
        if (!root1 && !root2) return true;
        if (!root1 || !root2) return false;

        // Values can be different in isomorphic trees
        // Check if structures match in either orientation
        return (areIsomorphic(root1->left, root2->left) &&
                areIsomorphic(root1->right, root2->right)) ||
               (areIsomorphic(root1->left, root2->right) &&
                areIsomorphic(root1->right, root2->left));
    }

    // Check if tree is symmetric (mirror of itself)
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return isSymmetricHelper(root->left, root->right);
    }

    // Check if tree is height-balanced (AVL property)
    bool isBalanced(TreeNode* root) {
        return checkBalance(root) != -1;
    }

    // Find diameter of tree (longest path between any two nodes)
    int diameter(TreeNode* root) {
        int result = 0;
        diameterHelper(root, result);
        return result;
    }

    // Check if tree is a sum tree (non-leaf = sum of subtrees)
    bool isSumTree(TreeNode* root) {
        return sumTreeHelper(root) != -1;
    }

    // Check if two trees are identical (structure + values)
    bool areIdentical(TreeNode* root1, TreeNode* root2) {
        if (!root1 && !root2) return true;
        if (!root1 || !root2) return false;

        return (root1->val == root2->val) &&
               areIdentical(root1->left, root2->left) &&
               areIdentical(root1->right, root2->right);
    }

private:
    bool isSymmetricHelper(TreeNode* left, TreeNode* right) {
        if (!left && !right) return true;
        if (!left || !right) return false;

        return (left->val == right->val) &&
               isSymmetricHelper(left->left, right->right) &&
               isSymmetricHelper(left->right, right->left);
    }

    int checkBalance(TreeNode* root) {
        if (!root) return 0;

        int leftHeight = checkBalance(root->left);
        if (leftHeight == -1) return -1;  // Left subtree unbalanced

        int rightHeight = checkBalance(root->right);
        if (rightHeight == -1) return -1;  // Right subtree unbalanced

        if (abs(leftHeight - rightHeight) > 1) return -1;  // Current node unbalanced

        return max(leftHeight, rightHeight) + 1;
    }

    int diameterHelper(TreeNode* root, int& result) {
        if (!root) return 0;

        int left = diameterHelper(root->left, result);
        int right = diameterHelper(root->right, result);

        // Update diameter passing through current node
        result = max(result, left + right);

        // Return height from current node
        return max(left, right) + 1;
    }

    int sumTreeHelper(TreeNode* root) {
        if (!root) return 0;
        if (!root->left && !root->right) return root->val;  // Leaf node

        int leftSum = sumTreeHelper(root->left);
        int rightSum = sumTreeHelper(root->right);

        if (leftSum == -1 || rightSum == -1) return -1;  // Invalid subtree
        if (root->val != leftSum + rightSum) return -1;  // Sum property violated

        return 2 * root->val;  // Current node + its subtree sum
    }
};

void demonstrateTreeValidation() {
    cout << "\n=== ADVANCED TREE VALIDATION ===" << endl;

    TreeStructureAnalyzer analyzer;

    // Test symmetric tree
    cout << "Testing Symmetric Tree:" << endl;
    TreeNode* symmetric = new TreeNode(1);
    symmetric->left = new TreeNode(2);
    symmetric->right = new TreeNode(2);
    symmetric->left->left = new TreeNode(3);
    symmetric->left->right = new TreeNode(4);
    symmetric->right->left = new TreeNode(4);
    symmetric->right->right = new TreeNode(3);

    cout << "Tree structure:" << endl;
    cout << "       1" << endl;
    cout << "      / \\" << endl;
    cout << "     2   2" << endl;
    cout << "    / \\ / \\" << endl;
    cout << "   3  4 4  3" << endl;
    cout << "Is symmetric: " << (analyzer.isSymmetric(symmetric) ? "Yes" : "No") << endl;

    // Test balanced tree
    cout << "\nTesting Balanced Tree:" << endl;
    TreeNode* balanced = new TreeNode(1);
    balanced->left = new TreeNode(2);
    balanced->right = new TreeNode(3);
    balanced->left->left = new TreeNode(4);

    cout << "Is balanced: " << (analyzer.isBalanced(balanced) ? "Yes" : "No") << endl;

    // Test diameter
    cout << "\nTesting Tree Diameter:" << endl;
    cout << "Diameter: " << analyzer.diameter(balanced) << " (longest path between any two nodes)" << endl;

    // Test sum tree
    cout << "\nTesting Sum Tree:" << endl;
    TreeNode* sumTree = new TreeNode(26);
    sumTree->left = new TreeNode(10);
    sumTree->right = new TreeNode(3);
    sumTree->left->left = new TreeNode(4);
    sumTree->left->right = new TreeNode(6);
    sumTree->right->right = new TreeNode(3);

    cout << "Tree structure:" << endl;
    cout << "      26" << endl;
    cout << "     /  \\" << endl;
    cout << "   10    3" << endl;
    cout << "  / \\     \\" << endl;
    cout << " 4   6     3" << endl;
    cout << "Is sum tree: " << (analyzer.isSumTree(sumTree) ? "Yes" : "No") << endl;
    cout << "(Each internal node = sum of its subtrees)" << endl;
}
```

### 🔧 Problem 2: Advanced Tree Analysis and LCA

**Problem Statement:**
Implement lowest common ancestor (LCA) and duplicate subtree detection algorithms.

**Teaching Strategy:**
"LCA is fundamental for many tree algorithms. Duplicate detection shows how to 'fingerprint' subtree structures."

```cpp
class AdvancedTreeAnalyzer {
public:
    // Find lowest common ancestor of two nodes
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;

        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (left && right) return root;  // LCA found
        return left ? left : right;      // Return non-null side
    }

    // Find all duplicate subtrees
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<string, int> count;
        vector<TreeNode*> result;

        serialize(root, count, result);
        return result;
    }

    // Check if tree is complete binary tree
    bool isCompleteTree(TreeNode* root) {
        if (!root) return true;

        queue<TreeNode*> q;
        q.push(root);
        bool nullSeen = false;

        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();

            if (!node) {
                nullSeen = true;
            } else {
                if (nullSeen) return false;  // Node after null in level-order
                q.push(node->left);
                q.push(node->right);
            }
        }

        return true;
    }

    // Count nodes in complete binary tree (optimized)
    int countNodesInCompleteTree(TreeNode* root) {
        if (!root) return 0;

        int leftHeight = getHeight(root, true);   // Go left
        int rightHeight = getHeight(root, false); // Go right

        if (leftHeight == rightHeight) {
            // Perfect binary tree
            return (1 << leftHeight) - 1;  // 2^h - 1
        }

        // Not perfect, count recursively
        return 1 + countNodesInCompleteTree(root->left) +
                   countNodesInCompleteTree(root->right);
    }

private:
    string serialize(TreeNode* root, unordered_map<string, int>& count,
                    vector<TreeNode*>& result) {
        if (!root) return "#";

        string s = to_string(root->val) + "," +
                  serialize(root->left, count, result) + "," +
                  serialize(root->right, count, result);

        if (++count[s] == 2) {  // Found duplicate
            result.push_back(root);
        }

        return s;
    }

    int getHeight(TreeNode* root, bool goLeft) {
        int height = 0;
        while (root) {
            height++;
            root = goLeft ? root->left : root->right;
        }
        return height;
    }
};

void demonstrateAdvancedAnalysis() {
    cout << "\n=== ADVANCED TREE ANALYSIS ===" << endl;

    AdvancedTreeAnalyzer analyzer;

    // Test LCA
    cout << "Testing Lowest Common Ancestor:" << endl;
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(5);
    root->right = new TreeNode(1);
    root->left->left = new TreeNode(6);
    root->left->right = new TreeNode(2);
    root->right->left = new TreeNode(0);
    root->right->right = new TreeNode(8);
    root->left->right->left = new TreeNode(7);
    root->left->right->right = new TreeNode(4);

    cout << "Tree structure:" << endl;
    cout << "        3" << endl;
    cout << "       / \\" << endl;
    cout << "      5   1" << endl;
    cout << "     / \\ / \\" << endl;
    cout << "    6  2 0  8" << endl;
    cout << "      / \\" << endl;
    cout << "     7   4" << endl;

    TreeNode* lca = analyzer.lowestCommonAncestor(root, root->left, root->left->right->right);
    cout << "LCA of nodes 5 and 4: " << (lca ? to_string(lca->val) : "null") << endl;

    // Test complete tree
    cout << "\nTesting Complete Tree:" << endl;
    cout << "Is complete tree: " << (analyzer.isCompleteTree(root) ? "Yes" : "No") << endl;

    cout << "\nAlgorithm complexities:" << endl;
    cout << "LCA: O(n) time, O(h) space" << endl;
    cout << "Complete tree check: O(n) time, O(w) space (w = max width)" << endl;
    cout << "Duplicate detection: O(n²) worst case for string operations" << endl;
}
```

---

## Day 7: Advanced Traversal Applications & Tree Views

### 📚 Theory: Coordinate-Based Tree Algorithms

**Teacher's Introduction:**
"Today we explore advanced traversal patterns that solve complex spatial problems on trees. These algorithms use coordinate systems and specialized data structures to achieve elegant solutions."

### 🔧 Problem 1: Tree Views (Left, Right, Top, Bottom)

**Problem Statement:**
Implement algorithms to find different views of a binary tree from various perspectives.

**Teaching Strategy:**
"Tree views teach us to think spatially about trees. We use coordinates and level-by-level processing to solve visibility problems."

```cpp
class TreeViews {
public:
    // Right side view (rightmost node at each level)
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        rightViewHelper(root, 0, result);
        return result;
    }

    // Left side view (leftmost node at each level)
    vector<int> leftSideView(TreeNode* root) {
        vector<int> result;
        leftViewHelper(root, 0, result);
        return result;
    }

    // Top view (nodes visible from top, based on horizontal distance)
    vector<int> topView(TreeNode* root) {
        if (!root) return {};

        map<int, int> topMap;  // horizontal_distance -> node_value
        queue<pair<TreeNode*, int>> q;  // node, horizontal_distance

        q.push({root, 0});

        while (!q.empty()) {
            auto [node, hd] = q.front();
            q.pop();

            // Only add if this horizontal distance not seen before
            if (topMap.find(hd) == topMap.end()) {
                topMap[hd] = node->val;
            }

            if (node->left) q.push({node->left, hd - 1});
            if (node->right) q.push({node->right, hd + 1});
        }

        vector<int> result;
        for (auto& p : topMap) {
            result.push_back(p.second);
        }
        return result;
    }

    // Bottom view (nodes visible from bottom)
    vector<int> bottomView(TreeNode* root) {
        if (!root) return {};

        map<int, int> bottomMap;
        queue<pair<TreeNode*, int>> q;

        q.push({root, 0});

        while (!q.empty()) {
            auto [node, hd] = q.front();
            q.pop();

            // Always update (last node at this horizontal distance)
            bottomMap[hd] = node->val;

            if (node->left) q.push({node->left, hd - 1});
            if (node->right) q.push({node->right, hd + 1});
        }

        vector<int> result;
        for (auto& p : bottomMap) {
            result.push_back(p.second);
        }
        return result;
    }

    // Boundary traversal (anticlockwise boundary)
    vector<int> boundaryTraversal(TreeNode* root) {
        if (!root) return {};

        vector<int> result;
        result.push_back(root->val);

        if (!root->left && !root->right) return result;

        // Left boundary (excluding leaf nodes)
        addLeftBoundary(root->left, result);

        // Leaf nodes
        addLeaves(root, result);

        // Right boundary (excluding leaf nodes, in reverse)
        addRightBoundary(root->right, result);

        return result;
    }

private:
    void rightViewHelper(TreeNode* root, int level, vector<int>& result) {
        if (!root) return;

        if (level == result.size()) {
            result.push_back(root->val);
        }

        rightViewHelper(root->right, level + 1, result);
        rightViewHelper(root->left, level + 1, result);
    }

    void leftViewHelper(TreeNode* root, int level, vector<int>& result) {
        if (!root) return;

        if (level == result.size()) {
            result.push_back(root->val);
        }

        leftViewHelper(root->left, level + 1, result);
        leftViewHelper(root->right, level + 1, result);
    }

    void addLeftBoundary(TreeNode* root, vector<int>& result) {
        if (!root || (!root->left && !root->right)) return;

        result.push_back(root->val);

        if (root->left) addLeftBoundary(root->left, result);
        else addLeftBoundary(root->right, result);
    }

    void addLeaves(TreeNode* root, vector<int>& result) {
        if (!root) return;

        if (!root->left && !root->right && root->val != result[0]) {
            result.push_back(root->val);
            return;
        }

        addLeaves(root->left, result);
        addLeaves(root->right, result);
    }

    void addRightBoundary(TreeNode* root, vector<int>& result) {
        if (!root || (!root->left && !root->right)) return;

        if (root->right) addRightBoundary(root->right, result);
        else addRightBoundary(root->left, result);

        result.push_back(root->val);
    }
};

void demonstrateTreeViews() {
    cout << "\n=== TREE VIEWS ALGORITHMS ===" << endl;

    // Create test tree
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right->left = new TreeNode(6);
    root->right->right = new TreeNode(7);

    TreeViews views;

    cout << "Tree structure:" << endl;
    cout << "       1" << endl;
    cout << "      / \\" << endl;
    cout << "     2   3" << endl;
    cout << "    / \\ / \\" << endl;
    cout << "   4  5 6  7" << endl;

    auto rightView = views.rightSideView(root);
    cout << "\nRight side view: ";
    for (int val : rightView) cout << val << " ";
    cout << "(What you see from right side)" << endl;

    auto leftView = views.leftSideView(root);
    cout << "Left side view: ";
    for (int val : leftView) cout << val << " ";
    cout << "(What you see from left side)" << endl;

    auto topView = views.topView(root);
    cout << "Top view: ";
    for (int val : topView) cout << val << " ";
    cout << "(What you see from above)" << endl;

    auto bottomView = views.bottomView(root);
    cout << "Bottom view: ";
    for (int val : bottomView) cout << val << " ";
    cout << "(What you see from below)" << endl;

    auto boundary = views.boundaryTraversal(root);
    cout << "Boundary traversal: ";
    for (int val : boundary) cout << val << " ";
    cout << "(Anticlockwise boundary)" << endl;

    cout << "\nKey insights:" << endl;
    cout << "- Right/Left views: First node at each level" << endl;
    cout << "- Top/Bottom views: Use horizontal distance coordinates" << endl;
    cout << "- Boundary: Left boundary + leaves + right boundary (reverse)" << endl;
}
```

### 🔧 Problem 2: Advanced Traversal Patterns

**Problem Statement:**
Implement zigzag level order, vertical order traversal, and Morris traversal (O(1) space).

**Teaching Strategy:**
"These advanced patterns show how creative thinking can solve complex traversal requirements and optimize space usage."

```cpp
class AdvancedTraversals {
public:
    // Zigzag level order traversal
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root) return {};

        vector<vector<int>> result;
        queue<TreeNode*> q;
        q.push(root);
        bool leftToRight = true;

        while (!q.empty()) {
            int size = q.size();
            vector<int> level;

            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                level.push_back(node->val);

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }

            if (!leftToRight) {
                reverse(level.begin(), level.end());
            }

            result.push_back(level);
            leftToRight = !leftToRight;
        }

        return result;
    }

    // Vertical order traversal
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        if (!root) return {};

        map<int, map<int, multiset<int>>> nodes;  // col -> row -> values
        queue<tuple<TreeNode*, int, int>> q;      // node, row, col

        q.push({root, 0, 0});

        while (!q.empty()) {
            auto [node, row, col] = q.front();
            q.pop();

            nodes[col][row].insert(node->val);

            if (node->left) q.push({node->left, row + 1, col - 1});
            if (node->right) q.push({node->right, row + 1, col + 1});
        }

        vector<vector<int>> result;
        for (auto& colPair : nodes) {
            vector<int> column;
            for (auto& rowPair : colPair.second) {
                for (int val : rowPair.second) {
                    column.push_back(val);
                }
            }
            result.push_back(column);
        }

        return result;
    }

    // Morris inorder traversal (O(1) space)
    vector<int> morrisInorder(TreeNode* root) {
        vector<int> result;
        TreeNode* current = root;

        while (current) {
            if (!current->left) {
                result.push_back(current->val);
                current = current->right;
            } else {
                // Find inorder predecessor
                TreeNode* predecessor = current->left;
                while (predecessor->right && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (!predecessor->right) {
                    // Create thread
                    predecessor->right = current;
                    current = current->left;
                } else {
                    // Remove thread and process current
                    predecessor->right = nullptr;
                    result.push_back(current->val);
                    current = current->right;
                }
            }
        }

        return result;
    }

    // Diagonal traversal
    vector<vector<int>> diagonalTraversal(TreeNode* root) {
        if (!root) return {};

        map<int, vector<int>> diagonalMap;  // diagonal_index -> nodes
        queue<pair<TreeNode*, int>> q;      // node, diagonal_index

        q.push({root, 0});

        while (!q.empty()) {
            auto [node, diag] = q.front();
            q.pop();

            diagonalMap[diag].push_back(node->val);

            if (node->left) q.push({node->left, diag + 1});
            if (node->right) q.push({node->right, diag});
        }

        vector<vector<int>> result;
        for (auto& pair : diagonalMap) {
            result.push_back(pair.second);
        }
        return result;
    }
};

void demonstrateAdvancedTraversals() {
    cout << "\n=== ADVANCED TRAVERSAL PATTERNS ===" << endl;

    // Create test tree
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    AdvancedTraversals traverser;

    cout << "Tree structure:" << endl;
    cout << "    3" << endl;
    cout << "   / \\" << endl;
    cout << "  9   20" << endl;
    cout << "     /  \\" << endl;
    cout << "    15   7" << endl;

    // Zigzag traversal
    auto zigzag = traverser.zigzagLevelOrder(root);
    cout << "\nZigzag level order:" << endl;
    for (int i = 0; i < zigzag.size(); i++) {
        cout << "Level " << i << " (" << (i % 2 == 0 ? "L→R" : "R→L") << "): ";
        for (int val : zigzag[i]) cout << val << " ";
        cout << endl;
    }

    // Vertical traversal
    auto vertical = traverser.verticalTraversal(root);
    cout << "\nVertical order:" << endl;
    for (int i = 0; i < vertical.size(); i++) {
        cout << "Column " << i << ": ";
        for (int val : vertical[i]) cout << val << " ";
        cout << endl;
    }

    // Morris traversal
    auto morris = traverser.morrisInorder(root);
    cout << "\nMorris inorder (O(1) space): ";
    for (int val : morris) cout << val << " ";
    cout << endl;

    cout << "\nAdvanced concepts:" << endl;
    cout << "- Zigzag: Alternate direction at each level" << endl;
    cout << "- Vertical: Group by column coordinates" << endl;
    cout << "- Morris: Use tree structure as stack (threading)" << endl;
    cout << "- Diagonal: Nodes at same slope" << endl;
}
```

**Morris Traversal Explanation:**

1. **Threading**: Temporarily modify tree to create shortcuts
2. **No Stack**: Use tree structure itself for navigation
3. **Restoration**: Restore original tree structure
4. **O(1) Space**: No additional data structures needed

**Advanced Teaching Points:**

1.  **Coordinate Systems**: How (row, col) coordinates solve spatial problems
2.  **Threading Technique**: Morris traversal's elegant space optimization
3.  **Multi-key Sorting**: Vertical traversal with multiple sort criteria
4.  **State Machines**: Zigzag's alternating direction pattern
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
 2 3
/ \
4 5

```

**Execution Order Comparison:**

| Step | Preorder | Inorder | Postorder |
| ---- | -------- | ------- | --------- |
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

| Approach  | Time | Space | Pros               | Cons                |
| --------- | ---- | ----- | ------------------ | ------------------- |
| Recursive | O(n) | O(h)  | Clean, intuitive   | Stack overflow risk |
| Iterative | O(n) | O(h)  | No recursion limit | More complex code   |

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

| Approach    | Time  | Space | Why?                        |
| ----------- | ----- | ----- | --------------------------- |
| Brute Force | O(n²) | O(n)  | Linear search for each node |
| Optimal     | O(n)  | O(n)  | HashMap for O(1) lookups    |

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

| Approach    | Time  | Space | Why?                                 |
| ----------- | ----- | ----- | ------------------------------------ |
| Brute Force | O(n²) | O(h)  | Height calculation for each node     |
| Optimal     | O(n)  | O(h)  | Single traversal with combined logic |

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

| Problem                | Approach    | Time Complexity | Space Complexity | Key Insight              |
| ---------------------- | ----------- | --------------- | ---------------- | ------------------------ |
| **All DFS Traversals** | Recursive   | O(n)            | O(h)             | Natural recursion        |
|                        | Iterative   | O(n)            | O(h)             | Explicit stack control   |
| **Level-Order**        | Queue BFS   | O(n)            | O(w)             | w = max width            |
| **Zigzag**             | Two Stacks  | O(n)            | O(w)             | Direction control        |
| **Tree Construction**  | Brute Force | O(n²)           | O(n)             | Linear search each time  |
|                        | Optimal     | O(n)            | O(n)             | HashMap for O(1) lookup  |
| **Diameter**           | Brute Force | O(n²)           | O(h)             | Height calc per node     |
|                        | Optimal     | O(n)            | O(h)             | Combined height+diameter |
| **Path Sum**           | Brute Force | O(n×p)          | O(n×p)           | p = avg path length      |
|                        | Optimal     | O(n)            | O(h)             | Running sum tracking     |
| **LCA**                | Brute Force | O(n)            | O(h)             | Two separate path finds  |
|                        | Optimal     | O(n)            | O(h)             | Single traversal         |

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

---

## Main Function & Testing Framework

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <unordered_map>
#include <map>
#include <string>
#include <sstream>
#include <algorithm>
#include <climits>
using namespace std;

// TreeNode definition
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// Helper functions for demonstrations
void printInorder(TreeNode* root) {
    if (!root) return;
    printInorder(root->left);
    cout << root->val << " ";
    printInorder(root->right);
}

void printPreorder(TreeNode* root) {
    if (!root) return;
    cout << root->val << " ";
    printPreorder(root->left);
    printPreorder(root->right);
}

int main() {
    cout << "🌳 Week 2: Advanced Tree Problem Solving" << endl;
    cout << "===========================================" << endl;

    // Day 1-2: Advanced Construction Problems
    cout << "\n📅 DAY 1-2: ADVANCED TREE CONSTRUCTION" << endl;
    demonstrateTraversalToTree();
    demonstrateSerialization();

    // Day 3-4: Path & Sum Problems
    cout << "\n📅 DAY 3-4: ADVANCED PATH PROBLEMS" << endl;
    demonstratePathProblems();
    demonstrateMaxPathSum();

    // Day 5-6: Tree Properties & Validation
    cout << "\n📅 DAY 5-6: TREE VALIDATION & ANALYSIS" << endl;
    demonstrateTreeValidation();
    demonstrateAdvancedAnalysis();

    // Day 7: Advanced Traversals & Views
    cout << "\n📅 DAY 7: ADVANCED TRAVERSALS & VIEWS" << endl;
    demonstrateTreeViews();
    demonstrateAdvancedTraversals();

    cout << "\n🎉 Week 2 Advanced Tree Algorithms Complete!" << endl;
    cout << "Next: Week 3 - Binary Search Trees (BST)" << endl;

    return 0;
}
```

---

## 📚 Week 2 Summary & Key Takeaways

### **Advanced Concepts Mastered:**

1. **Tree Construction Algorithms**

   - Building from preorder + inorder traversals
   - Tree serialization/deserialization
   - Understanding unique reconstruction properties

2. **Complex Path Problems**

   - Backtracking algorithms for path finding
   - Maximum path sum (any-to-any nodes)
   - Path counting with various constraints

3. **Advanced Tree Validation**

   - Tree isomorphism and structural comparison
   - Symmetry checking and balance validation
   - Sum tree properties and LCA algorithms

4. **Spatial Tree Algorithms**
   - Tree views from different perspectives
   - Coordinate-based traversal problems
   - Morris traversal for O(1) space optimization

### **Important Algorithm Patterns:**

- **Backtracking Pattern**: Add → Recurse → Remove (for path problems)
- **Global vs Local Tracking**: Managing state across recursive calls
- **Coordinate Systems**: Using (row, col) for spatial tree problems
- **Threading Techniques**: Morris traversal's space optimization
- **Level-by-Level Processing**: Queue-based algorithms for tree views

### **Time/Space Complexity Mastery:**

- **Tree Construction**: O(n) with hashmap optimization
- **Path Problems**: O(n) time, O(h) space for most algorithms
- **Tree Views**: O(n) time, coordinate-based space requirements
- **Morris Traversal**: O(n) time, O(1) space breakthrough

### **Real-World Applications:**

- **Compiler Design**: Tree construction from parsing
- **Game Development**: Pathfinding and spatial algorithms
- **Database Systems**: Tree validation and structural analysis
- **UI Frameworks**: Tree view implementations

### **Student Assessment Rubric**

**Advanced Understanding (Must Have):**

- [ ] Can implement tree construction from traversal arrays
- [ ] Masters backtracking algorithms for path problems
- [ ] Understands coordinate-based tree algorithms
- [ ] Can solve complex tree validation problems

**Implementation Excellence (Should Have):**

- [ ] Can optimize algorithms using appropriate data structures
- [ ] Handles edge cases systematically (empty trees, single nodes)
- [ ] Writes clean, efficient recursive and iterative solutions
- [ ] Can debug complex tree algorithms effectively

**Expert-Level Thinking (Nice to Have):**

- [ ] Understands Morris traversal and advanced space optimizations
- [ ] Can design custom tree algorithms for specific requirements
- [ ] Analyzes algorithm trade-offs and chooses optimal approaches
- [ ] Can explain algorithms to others with clear examples

### 🎯 Common Advanced Mistakes to Avoid

1. **Tree Construction**: Forgetting to increment indices or handle boundaries
2. **Path Problems**: Not properly maintaining backtracking state
3. **Coordinate Systems**: Mixing up row/column mappings in spatial algorithms
4. **Morris Traversal**: Incorrectly managing threading and restoration
5. **Complex Validation**: Not handling all edge cases in recursive property checks

### 🗣️ Advanced Discussion Questions

1. "How does tree construction demonstrate the power of divide-and-conquer?"
2. "When would you choose Morris traversal over traditional methods?"
3. "How do coordinate-based algorithms relate to computer graphics?"
4. "What are the trade-offs between different tree validation approaches?"

---

## Next Week Preview: Binary Search Trees

Week 3 will introduce Binary Search Trees, building on these advanced tree manipulation skills:

- **BST Properties**: Understanding ordered tree structures
- **BST Operations**: Search, insert, delete with O(log n) complexity
- **BST Validation**: Checking and maintaining BST properties
- **BST Applications**: Real-world uses in databases and systems

**🌟 Congratulations on mastering advanced tree algorithms! You're now ready for specialized tree structures.**

[↑ Back to Top](#-table-of-contents)

---

## 📚 **Quick Reference Guide**

### **Advanced Tree Algorithms Summary**

| Algorithm Type    | Time Complexity | Space Complexity | Use Case                 |
| ----------------- | --------------- | ---------------- | ------------------------ |
| Tree Construction | O(n)            | O(n)             | Building from traversals |
| Path Finding      | O(n)            | O(h)             | Root-to-leaf algorithms  |
| Tree Validation   | O(n)            | O(h)             | Property checking        |
| Tree Views        | O(n)            | O(w)             | Coordinate-based queries |

### **Problem-Solving Patterns**

- **Divide & Conquer**: Tree construction, complex calculations
- **Backtracking**: Path finding, validation with rollback
- **Coordinate Systems**: View problems, spatial algorithms
- **Property Checking**: Validation, constraint satisfaction

[↑ Back to Top](#-table-of-contents)

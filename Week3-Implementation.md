# Week 3: Binary Search Trees (BST) - Teacher's Guide

This document provides comprehensive teaching materials for Week 3, focusing on Binary Search Tree fundamentals, operations, and applications. This week builds upon the advanced tree manipulation skills from Week 2 and introduces the crucial concept of ordered tree structures.

---

## 🎯 Learning Objectives for Week 3

By the end of this week, students should be able to:

1. **Conceptual Understanding**: Explain BST properties and their mathematical implications
2. **Implementation Skills**: Code all BST operations (search, insert, delete) efficiently
3. **Problem Solving**: Apply BST properties to solve search and ordering problems
4. **Complexity Analysis**: Understand why BSTs provide O(log n) operations and when they degrade
5. **Validation Skills**: Check if a tree satisfies BST properties using multiple approaches
6. **Advanced Operations**: Implement range queries, kth element finding, and BST conversions
7. **Real-world Connection**: Connect BST concepts to practical systems like databases and indexing

---

## Day 1-2: BST Fundamentals & Basic Operations

### 📚 Theory: BST Properties Deep Dive

**Teacher's Introduction:**
"After mastering general tree algorithms in Week 2, we now enter the world of ordered trees. BSTs add a powerful constraint that enables efficient searching - but this constraint must be carefully maintained!"

### 🔧 Core BST Property

**The Fundamental Rule:**
For every node N in a Binary Search Tree:
- **All nodes in left subtree** have values **< N.data**
- **All nodes in right subtree** have values **> N.data**
- **This property holds recursively** for all subtrees

### **Implementation 1: BST Search Operations**

```cpp
class BSTOperations {
public:
    // Recursive search - elegant and intuitive
    TreeNode* searchRecursive(TreeNode* root, int key) {
        // Base cases
        if (!root || root->val == key) return root;
        
        // Use BST property to guide search
        if (key < root->val) 
            return searchRecursive(root->left, key);
        else 
            return searchRecursive(root->right, key);
    }
    
    // Iterative search - space efficient
    TreeNode* searchIterative(TreeNode* root, int key) {
        TreeNode* current = root;
        
        while (current && current->val != key) {
            if (key < current->val) 
                current = current->left;
            else 
                current = current->right;
        }
        
        return current;  // Found node or nullptr
    }
    
    // Find minimum value node (leftmost)
    TreeNode* findMin(TreeNode* root) {
        if (!root) return nullptr;
        
        while (root->left) {
            root = root->left;
        }
        return root;
    }
    
    // Find maximum value node (rightmost)
    TreeNode* findMax(TreeNode* root) {
        if (!root) return nullptr;
        
        while (root->right) {
            root = root->right;
        }
        return root;
    }
    
    // Check if value exists in BST
    bool contains(TreeNode* root, int key) {
        return searchRecursive(root, key) != nullptr;
    }
};

void demonstrateBSTSearch() {
    cout << "\n=== BST SEARCH OPERATIONS ===" << endl;
    
    // Create sample BST
    TreeNode* root = new TreeNode(8);
    root->left = new TreeNode(3);
    root->right = new TreeNode(10);
    root->left->left = new TreeNode(1);
    root->left->right = new TreeNode(6);
    root->right->right = new TreeNode(14);
    root->left->right->left = new TreeNode(4);
    root->left->right->right = new TreeNode(7);
    
    cout << "BST Structure:" << endl;
    cout << "       8" << endl;
    cout << "      / \\" << endl;
    cout << "     3   10" << endl;
    cout << "    / \\   \\" << endl;
    cout << "   1   6   14" << endl;
    cout << "      / \\" << endl;
    cout << "     4   7" << endl;
    
    BSTOperations bst;
    
    // Demonstrate search operations
    cout << "\nSearch Results:" << endl;
    cout << "Search for 6: " << (bst.contains(root, 6) ? "Found" : "Not found") << endl;
    cout << "Search for 5: " << (bst.contains(root, 5) ? "Found" : "Not found") << endl;
    
    TreeNode* minNode = bst.findMin(root);
    TreeNode* maxNode = bst.findMax(root);
    
    cout << "Minimum value: " << (minNode ? minNode->val : -1) << endl;
    cout << "Maximum value: " << (maxNode ? maxNode->val : -1) << endl;
    
    cout << "\nKey Insights:" << endl;
    cout << "- Search eliminates half the tree at each step" << endl;
    cout << "- Minimum is always leftmost node" << endl;
    cout << "- Maximum is always rightmost node" << endl;
    cout << "- Time complexity: O(h) where h = height" << endl;
}
```

### **Implementation 2: BST Insert Operations**

```cpp
class BSTInsertion {
public:
    // Recursive insertion - maintains BST property
    TreeNode* insertRecursive(TreeNode* root, int key) {
        // Base case: create new node
        if (!root) {
            return new TreeNode(key);
        }
        
        // Recursive insertion maintaining BST property
        if (key < root->val) {
            root->left = insertRecursive(root->left, key);
        } else if (key > root->val) {
            root->right = insertRecursive(root->right, key);
        }
        // Note: we ignore duplicates in this implementation
        
        return root;  // Return unchanged root
    }
    
    // Iterative insertion - space efficient
    TreeNode* insertIterative(TreeNode* root, int key) {
        TreeNode* newNode = new TreeNode(key);
        
        if (!root) return newNode;
        
        TreeNode* current = root;
        TreeNode* parent = nullptr;
        
        // Find the correct position
        while (current) {
            parent = current;
            if (key < current->val) {
                current = current->left;
            } else if (key > current->val) {
                current = current->right;
            } else {
                // Duplicate found, don't insert
                delete newNode;
                return root;
            }
        }
        
        // Insert as child of parent
        if (key < parent->val) {
            parent->left = newNode;
        } else {
            parent->right = newNode;
        }
        
        return root;
    }
    
    // Build BST from array
    TreeNode* buildBSTFromArray(vector<int>& values) {
        TreeNode* root = nullptr;
        
        for (int val : values) {
            root = insertRecursive(root, val);
        }
        
        return root;
    }
    
    // Level-order traversal for visualization
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root) return {};
        
        vector<vector<int>> result;
        queue<TreeNode*> q;
        q.push(root);
        
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
            
            result.push_back(level);
        }
        
        return result;
    }
};

void demonstrateBSTInsertion() {
    cout << "\n=== BST INSERTION OPERATIONS ===" << endl;
    
    BSTInsertion bst;
    vector<int> values = {8, 3, 10, 1, 6, 14, 4, 7, 13};
    
    cout << "Inserting values: ";
    for (int val : values) {
        cout << val << " ";
    }
    cout << endl;
    
    TreeNode* root = bst.buildBSTFromArray(values);
    
    cout << "\nResulting BST (level-order):" << endl;
    auto levels = bst.levelOrder(root);
    for (int i = 0; i < levels.size(); i++) {
        cout << "Level " << i << ": ";
        for (int val : levels[i]) {
            cout << val << " ";
        }
        cout << endl;
    }
    
    cout << "\nInsertion Process Explanation:" << endl;
    cout << "1. Start with empty tree" << endl;
    cout << "2. First value (8) becomes root" << endl;
    cout << "3. 3 < 8, so goes to left of 8" << endl;
    cout << "4. 10 > 8, so goes to right of 8" << endl;
    cout << "5. Continue following BST property..." << endl;
    
    cout << "\nKey Properties Maintained:" << endl;
    cout << "- Left subtree values < root value" << endl;
    cout << "- Right subtree values > root value" << endl;
    cout << "- Inorder traversal gives sorted sequence" << endl;
}
```

**Time Complexity Analysis:**
- **Search**: O(h) where h = height (best: O(log n), worst: O(n))
- **Insert**: O(h) - same as search to find position
- **Space**: O(1) for iterative, O(h) for recursive (call stack)

**Teaching Points:**
1. **BST Property**: Emphasize how it guides all operations
2. **Recursion vs Iteration**: Show both approaches and trade-offs
3. **Duplicate Handling**: Discuss different strategies (ignore, count, allow)

---

## Day 3-4: BST Delete Operation & Complex Cases

### 📚 Theory: The Challenge of BST Deletion

**Teacher's Introduction:**
"Deletion is the most complex BST operation because we must maintain the BST property while potentially restructuring the tree. There are three distinct cases, each requiring different strategies."

### **Implementation 1: BST Delete Operation**

```cpp
class BSTDeletion {
public:
    // Main delete function
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return root;
        
        // Find the node to delete
        if (key < root->val) {
            root->left = deleteNode(root->left, key);
        } else if (key > root->val) {
            root->right = deleteNode(root->right, key);
        } else {
            // Found the node to delete
            return deleteNodeCases(root);
        }
        
        return root;
    }
    
private:
    TreeNode* deleteNodeCases(TreeNode* node) {
        // Case 1: Node with no children (leaf)
        if (!node->left && !node->right) {
            delete node;
            return nullptr;
        }
        
        // Case 2: Node with one child
        if (!node->left) {
            TreeNode* temp = node->right;
            delete node;
            return temp;
        }
        
        if (!node->right) {
            TreeNode* temp = node->left;
            delete node;
            return temp;
        }
        
        // Case 3: Node with two children
        // Find inorder successor (smallest in right subtree)
        TreeNode* successor = findMin(node->right);
        
        // Replace node's value with successor's value
        node->val = successor->val;
        
        // Delete the successor
        node->right = deleteNode(node->right, successor->val);
        
        return node;
    }
    
    TreeNode* findMin(TreeNode* root) {
        while (root && root->left) {
            root = root->left;
        }
        return root;
    }
    
public:
    // Alternative: using inorder predecessor
    TreeNode* deleteNodeWithPredecessor(TreeNode* root, int key) {
        if (!root) return root;
        
        if (key < root->val) {
            root->left = deleteNodeWithPredecessor(root->left, key);
        } else if (key > root->val) {
            root->right = deleteNodeWithPredecessor(root->right, key);
        } else {
            // Found node to delete
            if (!root->left && !root->right) {
                delete root;
                return nullptr;
            }
            
            if (!root->left) {
                TreeNode* temp = root->right;
                delete root;
                return temp;
            }
            
            if (!root->right) {
                TreeNode* temp = root->left;
                delete root;
                return temp;
            }
            
            // Two children: use inorder predecessor
            TreeNode* predecessor = findMax(root->left);
            root->val = predecessor->val;
            root->left = deleteNodeWithPredecessor(root->left, predecessor->val);
        }
        
        return root;
    }
    
private:
    TreeNode* findMax(TreeNode* root) {
        while (root && root->right) {
            root = root->right;
        }
        return root;
    }
};

void demonstrateBSTDeletion() {
    cout << "\n=== BST DELETION OPERATIONS ===" << endl;
    
    // Create test BST
    BSTInsertion builder;
    vector<int> values = {50, 30, 70, 20, 40, 60, 80, 10, 25, 35, 45};
    TreeNode* root = builder.buildBSTFromArray(values);
    
    cout << "Original BST:" << endl;
    cout << "           50" << endl;
    cout << "          /  \\" << endl;
    cout << "        30    70" << endl;
    cout << "       / \\   / \\" << endl;
    cout << "     20  40 60  80" << endl;
    cout << "    / \\  / \\" << endl;
    cout << "   10 25 35 45" << endl;
    
    BSTDeletion deleter;
    
    // Test Case 1: Delete leaf node (10)
    cout << "\nCase 1: Deleting leaf node (10)" << endl;
    root = deleter.deleteNode(root, 10);
    cout << "Result: Node 10 removed, no restructuring needed" << endl;
    
    // Test Case 2: Delete node with one child (25, only has no children now)
    // Let's delete 20 which has one child (25)
    cout << "\nCase 2: Deleting node with one child (20)" << endl;
    root = deleter.deleteNode(root, 20);
    cout << "Result: Node 20 removed, child 25 takes its place" << endl;
    
    // Test Case 3: Delete node with two children (30)
    cout << "\nCase 3: Deleting node with two children (30)" << endl;
    cout << "Before deletion - node 30 has children 25 and 40" << endl;
    root = deleter.deleteNode(root, 30);
    cout << "Result: Node 30 replaced with inorder successor (35)" << endl;
    
    // Verify BST property maintained
    cout << "\nVerifying BST property maintained:" << endl;
    vector<int> inorderResult;
    inorderTraversal(root, inorderResult);
    cout << "Inorder traversal: ";
    for (int val : inorderResult) {
        cout << val << " ";
    }
    cout << "\n(Should be in sorted order)" << endl;
    
    cout << "\nDeletion Strategy Summary:" << endl;
    cout << "Case 1 (No children): Simply remove the node" << endl;
    cout << "Case 2 (One child): Replace node with its child" << endl;
    cout << "Case 3 (Two children): Replace with inorder successor/predecessor" << endl;
}

void inorderTraversal(TreeNode* root, vector<int>& result) {
    if (!root) return;
    inorderTraversal(root->left, result);
    result.push_back(root->val);
    inorderTraversal(root->right, result);
}
```

### **Implementation 2: Advanced BST Utilities**

```cpp
class BSTUtilities {
public:
    // Find inorder successor
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* target) {
        TreeNode* successor = nullptr;
        
        while (root) {
            if (target->val < root->val) {
                successor = root;
                root = root->left;
            } else {
                root = root->right;
            }
        }
        
        return successor;
    }
    
    // Find inorder predecessor
    TreeNode* inorderPredecessor(TreeNode* root, TreeNode* target) {
        TreeNode* predecessor = nullptr;
        
        while (root) {
            if (target->val > root->val) {
                predecessor = root;
                root = root->right;
            } else {
                root = root->left;
            }
        }
        
        return predecessor;
    }
    
    // Calculate tree statistics
    struct BSTStats {
        int nodeCount;
        int height;
        int leafCount;
        int minValue;
        int maxValue;
    };
    
    BSTStats analyzeBST(TreeNode* root) {
        BSTStats stats = {0, 0, 0, INT_MAX, INT_MIN};
        
        if (!root) return stats;
        
        analyzeHelper(root, stats, 0);
        return stats;
    }
    
private:
    void analyzeHelper(TreeNode* root, BSTStats& stats, int depth) {
        if (!root) return;
        
        stats.nodeCount++;
        stats.height = max(stats.height, depth);
        stats.minValue = min(stats.minValue, root->val);
        stats.maxValue = max(stats.maxValue, root->val);
        
        if (!root->left && !root->right) {
            stats.leafCount++;
        }
        
        analyzeHelper(root->left, stats, depth + 1);
        analyzeHelper(root->right, stats, depth + 1);
    }
};

void demonstrateBSTUtilities() {
    cout << "\n=== BST UTILITIES & ANALYSIS ===" << endl;
    
    // Create sample BST
    BSTInsertion builder;
    vector<int> values = {15, 10, 20, 8, 12, 17, 25};
    TreeNode* root = builder.buildBSTFromArray(values);
    
    BSTUtilities utils;
    
    // Analyze BST
    auto stats = utils.analyzeBST(root);
    cout << "BST Analysis:" << endl;
    cout << "Node count: " << stats.nodeCount << endl;
    cout << "Height: " << stats.height << endl;
    cout << "Leaf count: " << stats.leafCount << endl;
    cout << "Min value: " << stats.minValue << endl;
    cout << "Max value: " << stats.maxValue << endl;
    
    // Find successor/predecessor
    TreeNode* node12 = root->left->right;  // Node with value 12
    TreeNode* successor = utils.inorderSuccessor(root, node12);
    TreeNode* predecessor = utils.inorderPredecessor(root, node12);
    
    cout << "\nFor node 12:" << endl;
    cout << "Inorder successor: " << (successor ? successor->val : -1) << endl;
    cout << "Inorder predecessor: " << (predecessor ? predecessor->val : -1) << endl;
}
```

**Teaching Points:**
1. **Three Deletion Cases**: Make sure students understand each case clearly
2. **Successor vs Predecessor**: Both approaches work, explain the choice
3. **BST Property Preservation**: Emphasize that BST property must be maintained
4. **Complexity**: Still O(h) but potentially more complex than insert/search

---

## Day 5-6: BST Validation & Properties

### 📚 Theory: Ensuring BST Validity

**Teacher's Introduction:**
"Not every binary tree is a BST! We need robust methods to validate the BST property. This becomes crucial when dealing with corrupted data or implementing BST operations."

### **Implementation 1: BST Validation Methods**

```cpp
class BSTValidator {
public:
    // Method 1: Inorder traversal approach
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
    
    // Method 2: Bounds checking (more efficient)
    bool isValidBST_Bounds(TreeNode* root) {
        return validateWithBounds(root, LLONG_MIN, LLONG_MAX);
    }
    
    // Method 3: Optimized inorder with early termination
    bool isValidBST_OptimizedInorder(TreeNode* root) {
        TreeNode* prev = nullptr;
        return validateInorder(root, prev);
    }
    
    // Find kth smallest element (1-indexed)
    int kthSmallest(TreeNode* root, int k) {
        int count = 0;
        int result = -1;
        kthSmallestHelper(root, k, count, result);
        return result;
    }
    
    // Find kth largest element (1-indexed)
    int kthLargest(TreeNode* root, int k) {
        int count = 0;
        int result = -1;
        kthLargestHelper(root, k, count, result);
        return result;
    }
    
    // Convert sorted array to balanced BST
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBSTHelper(nums, 0, nums.size() - 1);
    }
    
private:
    void inorderTraversal(TreeNode* root, vector<int>& result) {
        if (!root) return;
        inorderTraversal(root->left, result);
        result.push_back(root->val);
        inorderTraversal(root->right, result);
    }
    
    bool validateWithBounds(TreeNode* root, long long minVal, long long maxVal) {
        if (!root) return true;
        
        if (root->val <= minVal || root->val >= maxVal) {
            return false;
        }
        
        return validateWithBounds(root->left, minVal, root->val) &&
               validateWithBounds(root->right, root->val, maxVal);
    }
    
    bool validateInorder(TreeNode* root, TreeNode*& prev) {
        if (!root) return true;
        
        if (!validateInorder(root->left, prev)) return false;
        
        if (prev && prev->val >= root->val) return false;
        prev = root;
        
        return validateInorder(root->right, prev);
    }
    
    void kthSmallestHelper(TreeNode* root, int k, int& count, int& result) {
        if (!root || count >= k) return;
        
        kthSmallestHelper(root->left, k, count, result);
        
        count++;
        if (count == k) {
            result = root->val;
            return;
        }
        
        kthSmallestHelper(root->right, k, count, result);
    }
    
    void kthLargestHelper(TreeNode* root, int k, int& count, int& result) {
        if (!root || count >= k) return;
        
        kthLargestHelper(root->right, k, count, result);
        
        count++;
        if (count == k) {
            result = root->val;
            return;
        }
        
        kthLargestHelper(root->left, k, count, result);
    }
    
    TreeNode* sortedArrayToBSTHelper(vector<int>& nums, int left, int right) {
        if (left > right) return nullptr;
        
        int mid = left + (right - left) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        
        root->left = sortedArrayToBSTHelper(nums, left, mid - 1);
        root->right = sortedArrayToBSTHelper(nums, mid + 1, right);
        
        return root;
    }
};

void demonstrateBSTValidation() {
    cout << "\n=== BST VALIDATION & PROPERTIES ===" << endl;
    
    BSTValidator validator;
    
    // Test valid BST
    TreeNode* validBST = new TreeNode(5);
    validBST->left = new TreeNode(3);
    validBST->right = new TreeNode(8);
    validBST->left->left = new TreeNode(2);
    validBST->left->right = new TreeNode(4);
    validBST->right->left = new TreeNode(7);
    validBST->right->right = new TreeNode(9);
    
    cout << "Valid BST Test:" << endl;
    cout << "Inorder method: " << (validator.isValidBST_Inorder(validBST) ? "Valid" : "Invalid") << endl;
    cout << "Bounds method: " << (validator.isValidBST_Bounds(validBST) ? "Valid" : "Invalid") << endl;
    cout << "Optimized inorder: " << (validator.isValidBST_OptimizedInorder(validBST) ? "Valid" : "Invalid") << endl;
    
    // Test invalid BST
    TreeNode* invalidBST = new TreeNode(5);
    invalidBST->left = new TreeNode(3);
    invalidBST->right = new TreeNode(8);
    invalidBST->left->left = new TreeNode(2);
    invalidBST->left->right = new TreeNode(6);  // INVALID: 6 > 5 but in left subtree
    
    cout << "\nInvalid BST Test:" << endl;
    cout << "Bounds method: " << (validator.isValidBST_Bounds(invalidBST) ? "Valid" : "Invalid") << endl;
    cout << "Why invalid: Node 6 > root 5 but in left subtree" << endl;
    
    // Test kth smallest/largest
    cout << "\nKth Element Tests:" << endl;
    cout << "3rd smallest: " << validator.kthSmallest(validBST, 3) << endl;
    cout << "2nd largest: " << validator.kthLargest(validBST, 2) << endl;
    
    // Test sorted array to BST
    vector<int> sortedArray = {1, 2, 3, 4, 5, 6, 7};
    TreeNode* balancedBST = validator.sortedArrayToBST(sortedArray);
    cout << "\nBalanced BST from sorted array [1,2,3,4,5,6,7]:" << endl;
    cout << "Root value: " << balancedBST->val << " (should be 4 for balance)" << endl;
    
    cout << "\nValidation Method Comparison:" << endl;
    cout << "1. Inorder: O(n) time, O(n) space - simple but uses extra space" << endl;
    cout << "2. Bounds: O(n) time, O(h) space - efficient and elegant" << endl;
    cout << "3. Optimized inorder: O(n) time, O(h) space - early termination" << endl;
}
```

### **Implementation 2: BST Range Operations**

```cpp
class BSTRangeOperations {
public:
    // Find all values in range [low, high]
    vector<int> rangeBST(TreeNode* root, int low, int high) {
        vector<int> result;
        rangeHelper(root, low, high, result);
        return result;
    }
    
    // Count nodes in range [low, high]
    int countInRange(TreeNode* root, int low, int high) {
        if (!root) return 0;
        
        if (root->val < low) {
            return countInRange(root->right, low, high);
        } else if (root->val > high) {
            return countInRange(root->left, low, high);
        } else {
            return 1 + countInRange(root->left, low, high) + 
                       countInRange(root->right, low, high);
        }
    }
    
    // Find floor value (largest value <= target)
    int floor(TreeNode* root, int target) {
        int result = -1;
        
        while (root) {
            if (root->val <= target) {
                result = root->val;
                root = root->right;
            } else {
                root = root->left;
            }
        }
        
        return result;
    }
    
    // Find ceiling value (smallest value >= target)
    int ceiling(TreeNode* root, int target) {
        int result = -1;
        
        while (root) {
            if (root->val >= target) {
                result = root->val;
                root = root->left;
            } else {
                root = root->right;
            }
        }
        
        return result;
    }
    
    // Find pair with given sum
    bool findPairWithSum(TreeNode* root, int target) {
        vector<int> inorderList;
        inorderTraversal(root, inorderList);
        
        int left = 0, right = inorderList.size() - 1;
        
        while (left < right) {
            int sum = inorderList[left] + inorderList[right];
            if (sum == target) return true;
            else if (sum < target) left++;
            else right--;
        }
        
        return false;
    }
    
private:
    void rangeHelper(TreeNode* root, int low, int high, vector<int>& result) {
        if (!root) return;
        
        if (root->val > low) {
            rangeHelper(root->left, low, high, result);
        }
        
        if (root->val >= low && root->val <= high) {
            result.push_back(root->val);
        }
        
        if (root->val < high) {
            rangeHelper(root->right, low, high, result);
        }
    }
    
    void inorderTraversal(TreeNode* root, vector<int>& result) {
        if (!root) return;
        inorderTraversal(root->left, result);
        result.push_back(root->val);
        inorderTraversal(root->right, result);
    }
};

void demonstrateBSTRangeOperations() {
    cout << "\n=== BST RANGE OPERATIONS ===" << endl;
    
    // Create sample BST: [15, 10, 20, 8, 12, 17, 25, 6, 11, 13, 27]
    BSTInsertion builder;
    vector<int> values = {15, 10, 20, 8, 12, 17, 25, 6, 11, 13, 27};
    TreeNode* root = builder.buildBSTFromArray(values);
    
    BSTRangeOperations rangeOps;
    
    cout << "BST values: ";
    vector<int> allValues;
    inorderTraversal(root, allValues);
    for (int val : allValues) {
        cout << val << " ";
    }
    cout << endl;
    
    // Range query
    vector<int> rangeResult = rangeOps.rangeBST(root, 10, 17);
    cout << "\nValues in range [10, 17]: ";
    for (int val : rangeResult) {
        cout << val << " ";
    }
    cout << endl;
    
    // Count in range
    int count = rangeOps.countInRange(root, 10, 20);
    cout << "Count of values in range [10, 20]: " << count << endl;
    
    // Floor and ceiling
    cout << "Floor of 14: " << rangeOps.floor(root, 14) << " (largest <= 14)" << endl;
    cout << "Ceiling of 14: " << rangeOps.ceiling(root, 14) << " (smallest >= 14)" << endl;
    
    // Pair with sum
    int targetSum = 23;
    bool hasPair = rangeOps.findPairWithSum(root, targetSum);
    cout << "Pair with sum " << targetSum << ": " << (hasPair ? "Found" : "Not found") << endl;
    
    cout << "\nRange Operation Complexities:" << endl;
    cout << "Range query: O(k + log n) where k = result size" << endl;
    cout << "Count in range: O(log n) in best case" << endl;
    cout << "Floor/Ceiling: O(log n)" << endl;
    cout << "Pair sum: O(n) using inorder + two pointers" << endl;
}
```

**Teaching Points:**
1. **Multiple Validation Approaches**: Show trade-offs between methods
2. **BST Property Applications**: How ordered property enables efficient operations
3. **Kth Element**: Leverage inorder traversal's sorted property
4. **Range Operations**: Prune search space using BST property

---

## Day 7: Advanced BST Operations & Applications

### 📚 Theory: Real-World BST Applications

**Teacher's Introduction:**
"BSTs are fundamental to many computer systems. Today we'll explore advanced operations and see how BSTs power databases, compilers, and other systems."

### **Implementation 1: BST Iterator & Advanced Conversions**

```cpp
class BSTIterator {
private:
    stack<TreeNode*> stk;
    
    void pushLeft(TreeNode* root) {
        while (root) {
            stk.push(root);
            root = root->left;
        }
    }
    
public:
    BSTIterator(TreeNode* root) {
        pushLeft(root);
    }
    
    int next() {
        TreeNode* node = stk.top();
        stk.pop();
        
        if (node->right) {
            pushLeft(node->right);
        }
        
        return node->val;
    }
    
    bool hasNext() {
        return !stk.empty();
    }
};

class BSTConversions {
public:
    // Convert BST to sorted doubly linked list
    TreeNode* bstToDoublyLL(TreeNode* root) {
        if (!root) return nullptr;
        
        TreeNode* head = nullptr;
        TreeNode* prev = nullptr;
        
        convertToDoublyLL(root, head, prev);
        return head;
    }
    
    // Merge two BSTs
    TreeNode* mergeBSTs(TreeNode* root1, TreeNode* root2) {
        vector<int> inorder1, inorder2;
        inorderTraversal(root1, inorder1);
        inorderTraversal(root2, inorder2);
        
        vector<int> merged = mergeSortedArrays(inorder1, inorder2);
        
        BSTValidator validator;
        return validator.sortedArrayToBST(merged);
    }
    
    // Recover BST where two nodes are swapped
    void recoverBST(TreeNode* root) {
        TreeNode* first = nullptr;
        TreeNode* second = nullptr;
        TreeNode* prev = nullptr;
        
        findSwappedNodes(root, first, second, prev);
        
        if (first && second) {
            swap(first->val, second->val);
        }
    }
    
    // Convert BST to greater sum tree
    TreeNode* bstToGST(TreeNode* root) {
        int sum = 0;
        reverseInorderGST(root, sum);
        return root;
    }
    
private:
    void convertToDoublyLL(TreeNode* root, TreeNode*& head, TreeNode*& prev) {
        if (!root) return;
        
        convertToDoublyLL(root->left, head, prev);
        
        if (!prev) {
            head = root;  // First node
        } else {
            prev->right = root;
            root->left = prev;
        }
        prev = root;
        
        convertToDoublyLL(root->right, head, prev);
    }
    
    void inorderTraversal(TreeNode* root, vector<int>& result) {
        if (!root) return;
        inorderTraversal(root->left, result);
        result.push_back(root->val);
        inorderTraversal(root->right, result);
    }
    
    vector<int> mergeSortedArrays(vector<int>& arr1, vector<int>& arr2) {
        vector<int> merged;
        int i = 0, j = 0;
        
        while (i < arr1.size() && j < arr2.size()) {
            if (arr1[i] <= arr2[j]) {
                merged.push_back(arr1[i++]);
            } else {
                merged.push_back(arr2[j++]);
            }
        }
        
        while (i < arr1.size()) merged.push_back(arr1[i++]);
        while (j < arr2.size()) merged.push_back(arr2[j++]);
        
        return merged;
    }
    
    void findSwappedNodes(TreeNode* root, TreeNode*& first, TreeNode*& second, TreeNode*& prev) {
        if (!root) return;
        
        findSwappedNodes(root->left, first, second, prev);
        
        if (prev && prev->val > root->val) {
            if (!first) {
                first = prev;
                second = root;
            } else {
                second = root;
            }
        }
        prev = root;
        
        findSwappedNodes(root->right, first, second, prev);
    }
    
    void reverseInorderGST(TreeNode* root, int& sum) {
        if (!root) return;
        
        reverseInorderGST(root->right, sum);
        sum += root->val;
        root->val = sum;
        reverseInorderGST(root->left, sum);
    }
};

void demonstrateAdvancedBSTOperations() {
    cout << "\n=== ADVANCED BST OPERATIONS ===" << endl;
    
    // Create sample BST
    BSTInsertion builder;
    vector<int> values = {4, 2, 6, 1, 3, 5, 7};
    TreeNode* root = builder.buildBSTFromArray(values);
    
    // Test BST Iterator
    cout << "BST Iterator Test:" << endl;
    cout << "Original BST inorder: ";
    vector<int> original;
    inorderTraversal(root, original);
    for (int val : original) {
        cout << val << " ";
    }
    cout << endl;
    
    cout << "Iterator traversal: ";
    BSTIterator iterator(root);
    while (iterator.hasNext()) {
        cout << iterator.next() << " ";
    }
    cout << endl;
    
    // Test BST conversions
    BSTConversions converter;
    
    // Convert to doubly linked list
    TreeNode* dll = converter.bstToDoublyLL(root);
    cout << "\nBST to Doubly Linked List: ";
    TreeNode* current = dll;
    while (current) {
        cout << current->val << " ";
        current = current->right;
    }
    cout << endl;
    
    // Merge two BSTs
    vector<int> values2 = {8, 10, 12};
    TreeNode* root2 = builder.buildBSTFromArray(values2);
    TreeNode* merged = converter.mergeBSTs(root, root2);
    
    cout << "\nMerged BST inorder: ";
    vector<int> mergedResult;
    inorderTraversal(merged, mergedResult);
    for (int val : mergedResult) {
        cout << val << " ";
    }
    cout << endl;
    
    cout << "\nAdvanced Operations Summary:" << endl;
    cout << "- Iterator: O(1) average time per next() call" << endl;
    cout << "- BST to DLL: O(n) time, modifies structure in-place" << endl;
    cout << "- Merge BSTs: O(m + n) time using inorder + merge" << endl;
    cout << "- Recovery: O(n) time to find and fix swapped nodes" << endl;
}
```

### **Implementation 2: Real-World BST Applications**

```cpp
class BSTApplications {
public:
    // Database-like operations
    class SimpleDatabase {
    private:
        TreeNode* root;
        
    public:
        SimpleDatabase() : root(nullptr) {}
        
        void insert(int key) {
            BSTInsertion inserter;
            root = inserter.insertRecursive(root, key);
        }
        
        bool search(int key) {
            BSTOperations ops;
            return ops.contains(root, key);
        }
        
        void remove(int key) {
            BSTDeletion deleter;
            root = deleter.deleteNode(root, key);
        }
        
        vector<int> rangeQuery(int low, int high) {
            BSTRangeOperations rangeOps;
            return rangeOps.rangeBST(root, low, high);
        }
        
        void displayAll() {
            vector<int> result;
            inorderTraversal(root, result);
            cout << "Database contents: ";
            for (int val : result) {
                cout << val << " ";
            }
            cout << endl;
        }
    };
    
    // Expression evaluation using BST properties
    class ExpressionEvaluator {
    public:
        // Build expression tree for mathematical expressions
        // This is a simplified version for demonstration
        double evaluateExpression(TreeNode* root) {
            if (!root) return 0;
            
            // Leaf nodes contain operands
            if (!root->left && !root->right) {
                return root->val;
            }
            
            // Internal nodes contain operators
            double leftVal = evaluateExpression(root->left);
            double rightVal = evaluateExpression(root->right);
            
            // Assume operator is encoded in node value
            // This is simplified - real implementation would use string nodes
            switch (root->val) {
                case '+': return leftVal + rightVal;
                case '-': return leftVal - rightVal;
                case '*': return leftVal * rightVal;
                case '/': return rightVal != 0 ? leftVal / rightVal : 0;
                default: return root->val;
            }
        }
    };
    
    // Priority queue implementation using BST
    class BSTPriorityQueue {
    private:
        TreeNode* root;
        
    public:
        BSTPriorityQueue() : root(nullptr) {}
        
        void enqueue(int priority) {
            BSTInsertion inserter;
            root = inserter.insertRecursive(root, priority);
        }
        
        int dequeueMax() {
            if (!root) return -1;
            
            BSTOperations ops;
            TreeNode* maxNode = ops.findMax(root);
            int maxVal = maxNode->val;
            
            BSTDeletion deleter;
            root = deleter.deleteNode(root, maxVal);
            
            return maxVal;
        }
        
        int dequeueMin() {
            if (!root) return -1;
            
            BSTOperations ops;
            TreeNode* minNode = ops.findMin(root);
            int minVal = minNode->val;
            
            BSTDeletion deleter;
            root = deleter.deleteNode(root, minVal);
            
            return minVal;
        }
        
        bool isEmpty() {
            return root == nullptr;
        }
    };
};

void demonstrateBSTApplications() {
    cout << "\n=== REAL-WORLD BST APPLICATIONS ===" << endl;
    
    // Database simulation
    cout << "1. Simple Database Operations:" << endl;
    BSTApplications::SimpleDatabase db;
    
    vector<int> data = {50, 30, 70, 20, 40, 60, 80};
    for (int val : data) {
        db.insert(val);
    }
    
    db.displayAll();
    cout << "Search for 40: " << (db.search(40) ? "Found" : "Not found") << endl;
    
    auto rangeResult = db.rangeQuery(30, 60);
    cout << "Range query [30, 60]: ";
    for (int val : rangeResult) {
        cout << val << " ";
    }
    cout << endl;
    
    // Priority queue simulation
    cout << "\n2. BST-based Priority Queue:" << endl;
    BSTApplications::BSTPriorityQueue pq;
    
    vector<int> priorities = {3, 1, 4, 1, 5, 9, 2, 6};
    for (int p : priorities) {
        pq.enqueue(p);
    }
    
    cout << "Dequeue high priority: ";
    while (!pq.isEmpty()) {
        cout << pq.dequeueMax() << " ";
    }
    cout << endl;
    
    cout << "\nReal-World BST Usage:" << endl;
    cout << "- Database indexes: B-trees for disk storage" << endl;
    cout << "- File systems: Directory structures" << endl;
    cout << "- Compilers: Symbol tables and syntax trees" << endl;
    cout << "- Games: Spatial partitioning, AI decision trees" << endl;
    cout << "- Operating systems: Process scheduling" << endl;
}
```

**Advanced Teaching Points:**
1. **Iterator Pattern**: Demonstrates how to traverse BST efficiently
2. **In-Place Modifications**: BST to DLL conversion without extra space
3. **Merge Algorithms**: Combining multiple BSTs efficiently
4. **Error Recovery**: Fixing corrupted BST structures
5. **Real-World Context**: How BSTs appear in actual systems

---

## Main Function & Testing Framework

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <unordered_map>
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

// Forward declarations of demonstration functions
void demonstrateBSTSearch();
void demonstrateBSTInsertion();
void demonstrateBSTDeletion();
void demonstrateBSTUtilities();
void demonstrateBSTValidation();
void demonstrateBSTRangeOperations();
void demonstrateAdvancedBSTOperations();
void demonstrateBSTApplications();

// Helper function
void inorderTraversal(TreeNode* root, vector<int>& result) {
    if (!root) return;
    inorderTraversal(root->left, result);
    result.push_back(root->val);
    inorderTraversal(root->right, result);
}

int main() {
    cout << "🌳 Week 3: Binary Search Trees (BST)" << endl;
    cout << "====================================" << endl;
    
    // Day 1-2: BST Fundamentals & Basic Operations
    cout << "\n📅 DAY 1-2: BST FUNDAMENTALS & BASIC OPERATIONS" << endl;
    demonstrateBSTSearch();
    demonstrateBSTInsertion();
    
    // Day 3-4: BST Delete & Complex Cases
    cout << "\n📅 DAY 3-4: BST DELETION & COMPLEX OPERATIONS" << endl;
    demonstrateBSTDeletion();
    demonstrateBSTUtilities();
    
    // Day 5-6: BST Validation & Properties
    cout << "\n📅 DAY 5-6: BST VALIDATION & PROPERTIES" << endl;
    demonstrateBSTValidation();
    demonstrateBSTRangeOperations();
    
    // Day 7: Advanced BST Operations & Applications
    cout << "\n📅 DAY 7: ADVANCED BST OPERATIONS & APPLICATIONS" << endl;
    demonstrateAdvancedBSTOperations();
    demonstrateBSTApplications();
    
    cout << "\n🎉 Week 3 BST Mastery Complete!" << endl;
    cout << "Next: Week 4 - Balanced Trees (AVL & Red-Black)" << endl;
    
    return 0;
}
```

---

## 📚 Week 3 Summary & Key Takeaways

### **BST Concepts Mastered:**

1. **BST Property & Invariants**
   - Left subtree < root < right subtree (recursive property)
   - Inorder traversal yields sorted sequence
   - Understanding when BST degrades to O(n) operations

2. **Core BST Operations**
   - Search: O(h) guided by BST property
   - Insert: O(h) finding correct position
   - Delete: O(h) with three distinct cases

3. **BST Validation & Analysis**
   - Multiple validation approaches (inorder, bounds, optimized)
   - Kth smallest/largest using inorder properties
   - Range operations leveraging ordered structure

4. **Advanced BST Applications**
   - Iterator pattern for efficient traversal
   - Tree conversions (DLL, merging, recovery)
   - Real-world applications in databases and systems

### **Critical Algorithm Patterns:**

- **BST Search Pattern**: Use property to eliminate half the tree at each step
- **Bounds Validation**: Pass min/max constraints down the recursion
- **Inorder Property**: Leverage sorted traversal for many operations
- **Successor/Predecessor**: Find next/previous in sorted order
- **Range Pruning**: Skip subtrees outside the query range

### **Complexity Analysis Mastery:**
- **Best Case**: O(log n) for balanced BST
- **Worst Case**: O(n) for skewed BST (degenerates to linked list)
- **Average Case**: O(log n) for random insertion order
- **Space Complexity**: O(h) for recursion, O(1) for iteration

### **Real-World Connections:**
- **Database Systems**: B-trees as generalized BSTs for disk storage
- **File Systems**: Directory structures and fast file lookup
- **Compilers**: Symbol tables and expression trees
- **Game Development**: Spatial partitioning and decision trees

### **Student Assessment Rubric**

**BST Fundamentals (Must Have):**
- [ ] Can explain BST property and its recursive nature
- [ ] Implements search, insert, delete operations correctly
- [ ] Understands complexity analysis and worst-case scenarios
- [ ] Can validate BST using multiple approaches

**Advanced BST Skills (Should Have):**
- [ ] Masters range operations and kth element queries
- [ ] Implements BST iterator and conversion algorithms
- [ ] Handles BST recovery and error correction
- [ ] Understands trade-offs between different validation methods

**Expert BST Knowledge (Nice to Have):**
- [ ] Connects BST concepts to real-world systems
- [ ] Can optimize BST operations for specific use cases
- [ ] Understands when to use BST vs other data structures
- [ ] Can design BST-based solutions for complex problems

### 🎯 Common BST Mistakes to Avoid

1. **Property Violation**: Not maintaining BST property during operations
2. **Duplicate Handling**: Inconsistent strategy for duplicate values
3. **Deletion Complexity**: Incorrectly handling the three deletion cases
4. **Validation Errors**: Using wrong bounds or missing edge cases
5. **Complexity Misunderstanding**: Not recognizing when BST degrades

### 🗣️ Advanced Discussion Questions

1. "Why does BST deletion require three different cases?"
2. "How do databases use BST concepts for indexing?"
3. "When would you choose BST over hash table for searching?"
4. "How does BST property enable efficient range queries?"

---

## Next Week Preview: Balanced Trees

Week 4 will build on BST foundations to explore self-balancing trees:

- **AVL Trees**: Height-balanced trees with rotation operations
- **Red-Black Trees**: Color-based balancing with relaxed constraints  
- **Balancing Operations**: Rotations and rebalancing algorithms
- **Performance Guarantees**: Maintaining O(log n) operations

**🌟 Congratulations on mastering Binary Search Trees! You now understand the foundation of ordered data structures.**

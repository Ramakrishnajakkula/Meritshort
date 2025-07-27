# Week 1: Tree Fundamentals & Traversals - Teacher's Guide

This document provides comprehensive teaching materials for introducing trees and all traversal methods to students, including theory explanations, code implementations, and teaching strategies.

---

## 🎯 Learning Objectives for Week 1

By the end of this week, students should be able to:

1. Understand what trees are and why we use them
2. Identify different parts of a tree (root, leaves, height, depth)
3. Implement basic tree node structure in C++
4. Master all four tree traversal methods (preorder, inorder, postorder, level-order)
5. Implement both recursive and iterative versions of traversals
6. Analyze time and space complexity of tree operations
7. Apply traversals to solve basic tree problems

---

## Day 1-2: Tree Fundamentals & All 4 Traversals

### 📚 Theory Introduction (Teach First)

#### What is a Tree? (Start with Real-World Examples)

**Teacher's Explanation:**
"Think about your family tree, or the folder structure on your computer. These are all examples of hierarchical relationships that we can represent using trees!"

- **Tree**: A hierarchical data structure where elements (nodes) are connected in a parent-child relationship
- **Why Trees?**: Unlike arrays or linked lists which are linear, trees allow us to organize data hierarchically

#### Key Terminology (Use Visual Aids)

```
        A (Root - no parent)
       / \
      B   C (Children of A)
     / \   \
    D   E   F (Grandchildren of A)
   /
  G (Great-grandchild)
```

- **Root**: Top node with no parent (A)
- **Leaf**: Node with no children (E, F, G)
- **Parent**: Node with children (A is parent of B,C)
- **Child**: Node with a parent (B,C are children of A)
- **Height**: Longest path from root to any leaf (4 in above tree)
- **Depth/Level**: Distance from root to a specific node (G is at depth 3)
- **Subtree**: Any node and all its descendants

#### Teaching Tips:

1. Draw trees on whiteboard first, then show code
2. Use family tree analogies
3. Let students identify parts in sample trees
4. Emphasize that trees grow "downward" in computer science!

### 💻 Basic Tree Node Implementation (Code with Explanation)

**Teacher's Approach:**
"Now let's see how we represent this tree concept in C++. We'll start simple and build up complexity."

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <queue>
using namespace std;

// Basic Tree Node Structure
struct TreeNode {
    int data;           // The value stored in this node
    TreeNode* left;     // Pointer to left child
    TreeNode* right;    // Pointer to right child

    // Constructor - explain why we need this
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};
```

**Explain to Students:**

1. **Why pointers?** "We use pointers because we don't know how many children each node will have"
2. **Why nullptr?** "Initially, new nodes have no children, so we set pointers to null"
3. **Constructor importance:** "This ensures every new node is properly initialized"

### 🏗️ Building Your First Tree (Interactive Exercise)

**Teacher Instructions:**
"Let's build this tree together step by step. I'll write the code, you tell me what each line does."

```cpp
// Create a simple tree:
//       1
//      / \
//     2   3
//    / \
//   4   5

TreeNode* buildSampleTree() {
    cout << "\n=== Building Tree Step by Step ===" << endl;

    // Step 1: Create root
    TreeNode* root = new TreeNode(1);
    cout << "Step 1: Created root with value 1" << endl;

    // Step 2: Add left child to root
    root->left = new TreeNode(2);
    cout << "Step 2: Added left child (2) to root (1)" << endl;

    // Step 3: Add right child to root
    root->right = new TreeNode(3);
    cout << "Step 3: Added right child (3) to root (1)" << endl;

    // Step 4: Add children to node 2
    root->left->left = new TreeNode(4);
    cout << "Step 4: Added left child (4) to node (2)" << endl;

    root->left->right = new TreeNode(5);
    cout << "Step 5: Added right child (5) to node (2)" << endl;

    cout << "Tree construction complete!" << endl;
    return root;
}
```

### 🌟 All Four Tree Traversals - The Heart of Tree Algorithms

**Teacher's Introduction:**
"Now we'll learn the four fundamental ways to visit every node in a tree. Think of these as different reading patterns for the same book!"

#### The Four Traversal Types:

1. **Preorder**: Root → Left → Right (Copy tree, prefix expressions)
2. **Inorder**: Left → Root → Right (Sorted order in BST)
3. **Postorder**: Left → Right → Root (Delete tree, postfix expressions)
4. **Level-order**: Level by level (BFS, shortest paths)

### 💡 1. Preorder Traversal (Root First)

**Teacher's Explanation:**
"Preorder means 'root first'. Think of it as writing your name on each room as you enter it, before exploring the rest of the house."

```cpp
// Preorder: Root -> Left -> Right
void preorderRecursive(TreeNode* root) {
    if (root == nullptr) {
        return;
    }

    cout << root->data << " ";        // Process current node (ROOT)
    preorderRecursive(root->left);    // Traverse left subtree (LEFT)
    preorderRecursive(root->right);   // Traverse right subtree (RIGHT)
}

// Iterative Preorder using Stack
void preorderIterative(TreeNode* root) {
    if (root == nullptr) return;

    stack<TreeNode*> stk;
    stk.push(root);

    cout << "=== ITERATIVE PREORDER USING STACK ===" << endl;

    while (!stk.empty()) {
        TreeNode* current = stk.top();
        stk.pop();

        cout << current->data << " ";  // Process current node

        // Push right first, then left (so left is processed first)
        if (current->right) {
            stk.push(current->right);
            cout << "(pushed right: " << current->right->data << ") ";
        }
        if (current->left) {
            stk.push(current->left);
            cout << "(pushed left: " << current->left->data << ") ";
        }
    }
    cout << endl;
}
```

### 💡 2. Inorder Traversal (Left-Root-Right)

**Teacher's Explanation:**
"Inorder visits left first, then root, then right. For Binary Search Trees, this gives us sorted order!"

```cpp
// Inorder: Left -> Root -> Right
void inorderRecursive(TreeNode* root) {
    if (root == nullptr) {
        return;
    }

    inorderRecursive(root->left);     // Traverse left subtree (LEFT)
    cout << root->data << " ";        // Process current node (ROOT)
    inorderRecursive(root->right);    // Traverse right subtree (RIGHT)
}

// Iterative Inorder using Stack
void inorderIterative(TreeNode* root) {
    stack<TreeNode*> stk;
    TreeNode* current = root;

    cout << "=== ITERATIVE INORDER USING STACK ===" << endl;

    while (current != nullptr || !stk.empty()) {
        // Go to leftmost node
        while (current != nullptr) {
            cout << "(going left, pushing: " << current->data << ") ";
            stk.push(current);
            current = current->left;
        }

        // Current is null, so we backtrack
        current = stk.top();
        stk.pop();

        cout << current->data << " ";  // Process current node

        // Visit right subtree
        current = current->right;
    }
    cout << endl;
}
```

### 💡 3. Postorder Traversal (Children First)

**Teacher's Explanation:**
"Postorder processes children before parent. Useful for deleting trees or calculating folder sizes!"

```cpp
// Postorder: Left -> Right -> Root
void postorderRecursive(TreeNode* root) {
    if (root == nullptr) {
        return;
    }

    postorderRecursive(root->left);   // Traverse left subtree (LEFT)
    postorderRecursive(root->right);  // Traverse right subtree (RIGHT)
    cout << root->data << " ";        // Process current node (ROOT)
}

// Iterative Postorder using Two Stacks
void postorderIterative(TreeNode* root) {
    if (root == nullptr) return;

    stack<TreeNode*> stk1, stk2;
    stk1.push(root);

    cout << "=== ITERATIVE POSTORDER USING TWO STACKS ===" << endl;

    // First stack for traversal, second stack for reversal
    while (!stk1.empty()) {
        TreeNode* node = stk1.top();
        stk1.pop();
        stk2.push(node);

        cout << "(moved " << node->data << " to second stack) ";

        if (node->left) {
            stk1.push(node->left);
        }
        if (node->right) {
            stk1.push(node->right);
        }
    }

    // Pop from second stack for correct postorder
    while (!stk2.empty()) {
        cout << stk2.top()->data << " ";
        stk2.pop();
    }
    cout << endl;
}
```

### � 4. Level-Order Traversal (BFS)

**Teacher's Explanation:**
"Level-order visits nodes level by level, like reading a book line by line. We use a queue for this!"

```cpp
// Level-order: Level by level using Queue
void levelOrderTraversal(TreeNode* root) {
    if (root == nullptr) return;

    queue<TreeNode*> q;
    q.push(root);

    cout << "=== LEVEL-ORDER TRAVERSAL USING QUEUE ===" << endl;

    while (!q.empty()) {
        TreeNode* current = q.front();
        q.pop();

        cout << current->data << " ";  // Process current node

        // Add children to queue (left first, then right)
        if (current->left) {
            q.push(current->left);
            cout << "(added left: " << current->left->data << ") ";
        }
        if (current->right) {
            q.push(current->right);
            cout << "(added right: " << current->right->data << ") ";
        }
    }
    cout << endl;
}

// Level-wise printing (each level on separate line)
void levelOrderByLevels(TreeNode* root) {
    if (root == nullptr) return;

    queue<TreeNode*> q;
    q.push(root);

    cout << "=== LEVEL-WISE PRINTING ===" << endl;
    int level = 0;

    while (!q.empty()) {
        int levelSize = q.size();
        cout << "Level " << level << ": ";

        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();

            cout << node->data << " ";

            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }

        cout << endl;
        level++;
    }
}
```

### 🎯 Complete Traversal Demonstration

```cpp
void demonstrateAllTraversals(TreeNode* root) {
    cout << "\n====== ALL TRAVERSAL METHODS DEMO ======" << endl;
    cout << "Tree structure:" << endl;
    cout << "    1" << endl;
    cout << "   / \\" << endl;
    cout << "  2   3" << endl;
    cout << " / \\" << endl;
    cout << "4   5" << endl;
    cout << endl;

    cout << "1. PREORDER (Root->Left->Right): ";
    preorderRecursive(root);
    cout << endl;

    cout << "2. INORDER (Left->Root->Right): ";
    inorderRecursive(root);
    cout << endl;

    cout << "3. POSTORDER (Left->Right->Root): ";
    postorderRecursive(root);
    cout << endl;

    cout << "4. LEVEL-ORDER (Level by level): ";
    levelOrderTraversal(root);
    cout << endl;

    cout << "\n--- ITERATIVE VERSIONS ---" << endl;

    cout << "Preorder Iterative: ";
    preorderIterative(root);

    cout << "Inorder Iterative: ";
    inorderIterative(root);

    cout << "Postorder Iterative: ";
    postorderIterative(root);

    cout << "\n--- LEVEL-WISE PRINTING ---" << endl;
    levelOrderByLevels(root);

    cout << "\n====== DEMO COMPLETE ======" << endl;
}
```

---

## Day 3-4: Basic Tree Operations

### 📚 Theory: Why Do We Calculate Tree Properties?

**Teacher's Motivation:**
"Just like we measure the height of buildings or count the population of cities, we need to measure properties of our data structures to understand their efficiency and behavior."

#### Key Properties We'll Learn:

1. **Height**: How "tall" is our tree? (affects search time)
2. **Node Count**: How much data are we storing?
3. **Leaf Count**: How many "endpoints" do we have?

### 💡 Height Calculation - Building Intuition

**Teacher's Explanation:**
"Height is like asking: 'If I start at the top and take the longest possible path down, how many steps will I take?'"

**Visual Teaching Aid:**

```
Tree 1:     Tree 2:        Tree 3:
   A           A              A
  /           /              / \
 B           B              B   C
Height=2    /              /
           C              D
         Height=3      Height=3
```

**Important Concept**: Height of empty tree = 0, single node = 1

### 💻 Complete Tree Operations Implementation

```cpp
class TreeOperations {
public:
    // Height calculation
    int calculateHeight(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }

        int leftHeight = calculateHeight(root->left);
        int rightHeight = calculateHeight(root->right);

        return 1 + max(leftHeight, rightHeight);
    }

    // Count all nodes
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }

    // Count leaf nodes
    int countLeaves(TreeNode* root) {
        if (root == nullptr) return 0;
        if (root->left == nullptr && root->right == nullptr) return 1;
        return countLeaves(root->left) + countLeaves(root->right);
    }

    // Find maximum element
    int findMaximum(TreeNode* root) {
        if (root == nullptr) return INT_MIN;

        int leftMax = (root->left) ? findMaximum(root->left) : INT_MIN;
        int rightMax = (root->right) ? findMaximum(root->right) : INT_MIN;

        return max({root->data, leftMax, rightMax});
    }

    // Find minimum element
    int findMinimum(TreeNode* root) {
        if (root == nullptr) return INT_MAX;

        int leftMin = (root->left) ? findMinimum(root->left) : INT_MAX;
        int rightMin = (root->right) ? findMinimum(root->right) : INT_MAX;

        return min({root->data, leftMin, rightMin});
    }

    // Check if two trees are identical
    bool areIdentical(TreeNode* tree1, TreeNode* tree2) {
        if (tree1 == nullptr && tree2 == nullptr) return true;
        if (tree1 == nullptr || tree2 == nullptr) return false;

        return (tree1->data == tree2->data) &&
               areIdentical(tree1->left, tree2->left) &&
               areIdentical(tree1->right, tree2->right);
    }

    // Build tree from array (complete binary tree)
    TreeNode* buildFromArray(vector<int>& arr, int index = 0) {
        if (index >= arr.size()) return nullptr;

        TreeNode* root = new TreeNode(arr[index]);
        root->left = buildFromArray(arr, 2 * index + 1);
        root->right = buildFromArray(arr, 2 * index + 2);

        return root;
    }

    // Search for a value
    bool searchValue(TreeNode* root, int target) {
        if (root == nullptr) return false;
        if (root->data == target) return true;

        return searchValue(root->left, target) || searchValue(root->right, target);
    }
};
```

---

## Day 5-6: Stack & Queue Visualization Deep Dive

### 📚 Theory: Understanding Data Structures Behind Traversals

**Teacher's Explanation:**
"Recursion uses an invisible stack. Iterative methods use visible stacks and queues. Let's make the invisible visible!"

#### Key Concepts:

- **Stack (LIFO)**: Last In, First Out - like a stack of plates
- **Queue (FIFO)**: First In, First Out - like a line at the store
- **Recursion**: Uses the system's call stack automatically

### 💻 Stack and Queue Visualization Tools

```cpp
class TraversalVisualizer {
public:
    // Visualize stack contents during preorder
    void preorderStackVisualization(TreeNode* root) {
        if (root == nullptr) return;

        stack<TreeNode*> stk;
        stk.push(root);

        cout << "=== PREORDER STACK VISUALIZATION ===" << endl;
        int step = 1;

        while (!stk.empty()) {
            cout << "Step " << step++ << ":" << endl;

            // Show stack contents
            cout << "  Stack contents (top to bottom): ";
            stack<TreeNode*> temp = stk;
            vector<int> stackContents;
            while (!temp.empty()) {
                stackContents.push_back(temp.top()->data);
                temp.pop();
            }
            for (int val : stackContents) {
                cout << val << " ";
            }
            cout << endl;

            // Process current node
            TreeNode* current = stk.top();
            stk.pop();
            cout << "  Processing: " << current->data << endl;

            // Push children
            if (current->right) {
                stk.push(current->right);
                cout << "  Pushed right child: " << current->right->data << endl;
            }
            if (current->left) {
                stk.push(current->left);
                cout << "  Pushed left child: " << current->left->data << endl;
            }
            cout << endl;
        }
    }

    // Visualize queue contents during level-order
    void levelOrderQueueVisualization(TreeNode* root) {
        if (root == nullptr) return;

        queue<TreeNode*> q;
        q.push(root);

        cout << "=== LEVEL-ORDER QUEUE VISUALIZATION ===" << endl;
        int step = 1;

        while (!q.empty()) {
            cout << "Step " << step++ << ":" << endl;

            // Show queue contents
            cout << "  Queue contents (front to back): ";
            queue<TreeNode*> temp = q;
            while (!temp.empty()) {
                cout << temp.front()->data << " ";
                temp.pop();
            }
            cout << endl;

            // Process current node
            TreeNode* current = q.front();
            q.pop();
            cout << "  Processing: " << current->data << endl;

            // Add children
            if (current->left) {
                q.push(current->left);
                cout << "  Added left child: " << current->left->data << endl;
            }
            if (current->right) {
                q.push(current->right);
                cout << "  Added right child: " << current->right->data << endl;
            }
            cout << endl;
        }
    }

    // Compare recursive vs iterative call patterns
    void compareRecursiveVsIterative(TreeNode* root) {
        cout << "\n=== RECURSIVE VS ITERATIVE COMPARISON ===" << endl;

        cout << "\nRecursive Preorder Call Pattern:" << endl;
        preorderRecursiveWithCallStack(root, 0);

        cout << "\nIterative Preorder Stack Operations:" << endl;
        preorderStackVisualization(root);

        cout << "\nKey Differences:" << endl;
        cout << "- Recursive: Uses system call stack (invisible)" << endl;
        cout << "- Iterative: Uses explicit stack (visible and controllable)" << endl;
        cout << "- Recursive: Cleaner code, risk of stack overflow" << endl;
        cout << "- Iterative: More control, no recursion depth limit" << endl;
    }

private:
    void preorderRecursiveWithCallStack(TreeNode* root, int depth) {
        if (root == nullptr) {
            cout << string(depth * 2, ' ') << "└── null (return)" << endl;
            return;
        }

        cout << string(depth * 2, ' ') << "├── Process: " << root->data
             << " (call stack depth: " << depth << ")" << endl;

        preorderRecursiveWithCallStack(root->left, depth + 1);
        preorderRecursiveWithCallStack(root->right, depth + 1);
    }
};
```

---

## Day 7: Advanced Traversal Patterns

### 📚 Theory: When to Use Which Traversal

**Teacher's Explanation:**
"Each traversal has specific use cases. Let's learn when to choose each one!"

#### Traversal Applications:

- **Preorder**: Copying trees, prefix expressions, tree serialization
- **Inorder**: Getting sorted data from BST, expression trees
- **Postorder**: Deleting trees, calculating directory sizes, postfix expressions
- **Level-order**: Finding shortest paths, tree serialization, level-wise processing

### 💻 Advanced Traversal Patterns

```cpp
class AdvancedTraversals {
public:
    // Print each level on a separate line
    void printLevelWise(TreeNode* root) {
        if (root == nullptr) return;

        queue<TreeNode*> q;
        q.push(root);
        int level = 0;

        while (!q.empty()) {
            int levelSize = q.size();
            cout << "Level " << level++ << ": ";

            for (int i = 0; i < levelSize; i++) {
                TreeNode* node = q.front();
                q.pop();

                cout << node->data << " ";

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            cout << endl;
        }
    }

    // Boundary traversal (left boundary + leaves + right boundary)
    void boundaryTraversal(TreeNode* root) {
        if (root == nullptr) return;

        cout << "Boundary Traversal: ";

        // Print root
        cout << root->data << " ";

        // Print left boundary (excluding leaf nodes)
        printLeftBoundary(root->left);

        // Print leaf nodes
        printLeaves(root);

        // Print right boundary (excluding leaf nodes, in reverse)
        printRightBoundary(root->right);

        cout << endl;
    }

    // Find all paths from root to leaves
    void findAllPaths(TreeNode* root) {
        vector<int> path;
        cout << "All Root-to-Leaf Paths:" << endl;
        findAllPathsHelper(root, path);
    }

private:
    void printLeftBoundary(TreeNode* root) {
        if (root == nullptr) return;
        if (root->left == nullptr && root->right == nullptr) return; // Skip leaves

        cout << root->data << " ";
        if (root->left) printLeftBoundary(root->left);
        else printLeftBoundary(root->right);
    }

    void printRightBoundary(TreeNode* root) {
        if (root == nullptr) return;
        if (root->left == nullptr && root->right == nullptr) return; // Skip leaves

        if (root->right) printRightBoundary(root->right);
        else printRightBoundary(root->left);
        cout << root->data << " ";
    }

    void printLeaves(TreeNode* root) {
        if (root == nullptr) return;
        if (root->left == nullptr && root->right == nullptr) {
            cout << root->data << " ";
            return;
        }
        printLeaves(root->left);
        printLeaves(root->right);
    }

    void findAllPathsHelper(TreeNode* root, vector<int>& path) {
        if (root == nullptr) return;

        path.push_back(root->data);

        if (root->left == nullptr && root->right == nullptr) {
            // Leaf node - print path
            cout << "Path: ";
            for (int val : path) {
                cout << val << " ";
            }
            cout << endl;
        } else {
            findAllPathsHelper(root->left, path);
            findAllPathsHelper(root->right, path);
        }

        path.pop_back(); // Backtrack
    }
};
```

---

## 🎓 Complete Week 1 Implementation & Testing

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <queue>
#include <climits>
#include <algorithm>
using namespace std;

// ... [All previous code sections combined] ...

// Main testing function
int main() {
    cout << "=== WEEK 1: TREE FUNDAMENTALS & TRAVERSALS ===" << endl;

    // Build sample tree
    TreeNode* root = buildSampleTree();

    // Test all traversals
    demonstrateAllTraversals(root);

    // Test tree operations
    TreeOperations ops;
    cout << "\n=== TREE OPERATIONS TEST ===" << endl;
    cout << "Height: " << ops.calculateHeight(root) << endl;
    cout << "Total nodes: " << ops.countNodes(root) << endl;
    cout << "Leaf nodes: " << ops.countLeaves(root) << endl;
    cout << "Maximum element: " << ops.findMaximum(root) << endl;
    cout << "Minimum element: " << ops.findMinimum(root) << endl;
    cout << "Search for 3: " << (ops.searchValue(root, 3) ? "Found" : "Not found") << endl;

    // Test visualizations
    TraversalVisualizer visualizer;
    visualizer.compareRecursiveVsIterative(root);

    // Test advanced patterns
    AdvancedTraversals advanced;
    advanced.printLevelWise(root);
    advanced.boundaryTraversal(root);
    advanced.findAllPaths(root);

    cout << "\n=== WEEK 1 COMPLETE! ===" << endl;

    return 0;
}
```

---

## 🎓 Week 1 Student Assessment & Teaching Notes

### Student Assessment Rubric

**Basic Understanding (Must Have):**

- [ ] Can explain what a tree is using real-world analogies
- [ ] Can identify root, leaves, height, depth in any tree
- [ ] Understands all four traversal types and their differences
- [ ] Can trace traversals manually on paper
- [ ] Understands base case and recursive case

**Implementation Skills (Should Have):**

- [ ] Can implement basic tree node structure
- [ ] Can write all four traversal functions (recursive and iterative)
- [ ] Can implement basic tree operations (height, count, search)
- [ ] Can debug simple tree problems
- [ ] Understands stack vs queue behavior

**Advanced Thinking (Nice to Have):**

- [ ] Understands when to use which traversal method
- [ ] Can visualize and explain recursion vs iteration trade-offs
- [ ] Can modify algorithms for different requirements
- [ ] Asks thoughtful questions about edge cases
- [ ] Can apply traversals to solve new problems

### 🗣️ Classroom Discussion Questions:

1. "Why do we use trees instead of arrays or linked lists?"
2. "What happens if we have duplicate values in our tree?"
3. "How would trees be useful in a real application like a file system?"
4. "What would happen if we allowed cycles in our tree?"
5. "Which traversal would you use to copy a tree? Why?"

### 🎯 Common Student Mistakes to Watch For:

1. **Forgetting base case** in recursion
2. **Null pointer access** when node doesn't exist
3. **Confusing traversal orders** - practice with different trees
4. **Not understanding** how recursion builds up the call stack
5. **Stack vs Queue confusion** - emphasize LIFO vs FIFO
6. **Memory leaks** from not properly managing dynamic allocation

### 📝 Week 1 Assignments:

1. **Manual Tracing**: Give students different trees to trace all 4 traversals
2. **Implementation Practice**: Implement missing functions (findMinimum, searchElement)
3. **Visualization Exercise**: Draw stack/queue contents for given trees
4. **Creative Application**: Design a use case for each traversal type

---

## Next Week Preview

In Week 2, we'll apply these traversal skills to solve complex problems:

- Tree construction from traversal sequences
- Path sum and maximum path problems
- Tree validation and property checking
- Advanced traversal applications

**Congratulations on mastering tree fundamentals and all traversal methods!**

---

## Day 6-7: Practice Problems - Applying What We've Learned

### 🎯 Teaching Philosophy for Problem-Solving

**Teacher's Guidance:**
"Now we'll apply our tree knowledge to solve real problems. For each problem, we'll follow this approach:

1. **Understand**: What exactly is the problem asking?
2. **Plan**: What's our strategy?
3. **Implement**: Write the code step by step
4. **Test**: Try it with examples
5. **Analyze**: What's the time/space complexity?"

### 🔍 Problem 1: Find Maximum Element in Binary Tree

#### 📋 Problem Statement

**Given**: A binary tree with integer values
**Find**: The maximum value stored in any node of the tree
**Return**: The maximum integer value, or handle empty tree appropriately

**Example:**

```
Input Tree:     Expected Output: 8
     3
    / \
   1   8
  /   / \
 4   6   2
```

#### 🐌 Brute Force Approach

**Teacher's Explanation:**
"The most straightforward approach is to visit every single node and keep track of the maximum we've seen so far."

**Brute Force Strategy:**

1. Traverse the entire tree (any traversal order)
2. Compare each node's value with current maximum
3. Update maximum if current node is larger
4. Return the final maximum

```cpp
// Brute Force: Store all values, then find max
int findMaximumBruteForce(TreeNode* root) {
    if (root == nullptr) {
        cout << "Empty tree - cannot find maximum" << endl;
        return INT_MIN;
    }

    vector<int> allValues;

    // Step 1: Collect all values using any traversal
    function<void(TreeNode*)> collectValues = [&](TreeNode* node) {
        if (node == nullptr) return;
        allValues.push_back(node->data);
        collectValues(node->left);
        collectValues(node->right);
    };

    collectValues(root);

    // Step 2: Find maximum in the collected values
    int maxValue = allValues[0];
    for (int i = 1; i < allValues.size(); i++) {
        if (allValues[i] > maxValue) {
            maxValue = allValues[i];
        }
    }

    cout << "Brute Force found maximum: " << maxValue << endl;
    return maxValue;
}
```

**Brute Force Analysis:**

- **Time Complexity**: O(n) - visit every node once
- **Space Complexity**: O(n) - store all values in vector + O(h) recursion stack
- **Issues**: Uses extra space unnecessarily

#### ⚡ Optimal Approach

**Teacher's Explanation:**
"We can do better! Instead of storing all values, we can find the maximum during traversal itself."

**Recursive Strategy:**

1. Use divide and conquer approach
2. Maximum of tree = max(root value, max of left subtree, max of right subtree)
3. No extra storage needed beyond recursion stack

```cpp
// Optimal Recursive: Find maximum during traversal
int findMaximumOptimalRecursive(TreeNode* root) {
    // Base case: empty subtree
    if (root == nullptr) {
        return INT_MIN; // Return very small value for empty subtree
    }

    cout << "Checking node: " << root->data << endl;

    // Get maximum from left and right subtrees
    int leftMax = findMaximumOptimalRecursive(root->left);
    int rightMax = findMaximumOptimalRecursive(root->right);

    // Maximum is the largest among current, left max, right max
    int result = max({root->data, leftMax, rightMax});
    cout << "Maximum at subtree rooted at " << root->data << " is " << result << endl;

    return result;
}

// Optimal Iterative: Using stack for DFS traversal
int findMaximumOptimalIterative(TreeNode* root) {
    if (root == nullptr) return INT_MIN;

    stack<TreeNode*> stk;
    stk.push(root);
    int maxValue = INT_MIN;

    cout << "=== ITERATIVE APPROACH USING STACK ===" << endl;

    while (!stk.empty()) {
        TreeNode* current = stk.top();
        stk.pop();

        cout << "Processing node: " << current->data << endl;
        maxValue = max(maxValue, current->data);

        // Push children to stack (right first, then left for correct order)
        if (current->right) {
            stk.push(current->right);
            cout << "  Added right child " << current->right->data << " to stack" << endl;
        }
        if (current->left) {
            stk.push(current->left);
            cout << "  Added left child " << current->left->data << " to stack" << endl;
        }
    }

    cout << "Iterative approach found maximum: " << maxValue << endl;
    return maxValue;
}
```

**Analysis Comparison:**

- **Recursive Time**: O(n), **Space**: O(h) - recursion stack
- **Iterative Time**: O(n), **Space**: O(h) - explicit stack
- **Advantages**: Iterative avoids recursion stack overflow for very deep trees

**Teaching Moment:**
"Both approaches have same time complexity, but optimal uses less space!"

### 🔍 Problem 2: Check if Two Binary Trees are Identical

#### 📋 Problem Statement

**Given**: Two binary trees (tree1 and tree2)
**Determine**: Whether the two trees are structurally identical and have same values
**Return**: true if trees are identical, false otherwise

**Definition of Identical Trees:**

- Same structure (same shape)
- Same values at corresponding positions
- Both null trees are considered identical

**Examples:**

```
Example 1 - Identical:
Tree1:  1       Tree2:  1
       / \             / \
      2   3           2   3
Output: true

Example 2 - Different Values:
Tree1:  1       Tree2:  1
       / \             / \
      2   3           2   4
Output: false

Example 3 - Different Structure:
Tree1:  1       Tree2:  1
       /               \
      2                 2
Output: false
```

#### 🐌 Brute Force Approach

**Teacher's Explanation:**
"Let's collect all information about both trees separately, then compare everything!"

**Brute Force Strategy:**

1. Serialize both trees to strings (including null positions)
2. Compare the serialized strings
3. If strings match, trees are identical

```cpp
// Brute Force: Serialize both trees and compare strings
string serializeTree(TreeNode* root) {
    if (root == nullptr) {
        return "null,";
    }

    string result = to_string(root->data) + ",";
    result += serializeTree(root->left);
    result += serializeTree(root->right);

    return result;
}

bool areIdenticalBruteForce(TreeNode* tree1, TreeNode* tree2) {
    cout << "=== BRUTE FORCE APPROACH ===" << endl;

    // Serialize both trees
    string serialized1 = serializeTree(tree1);
    string serialized2 = serializeTree(tree2);

    cout << "Tree1 serialized: " << serialized1 << endl;
    cout << "Tree2 serialized: " << serialized2 << endl;

    // Compare serialized strings
    bool result = (serialized1 == serialized2);
    cout << "Strings match: " << (result ? "YES" : "NO") << endl;

    return result;
}
```

**Brute Force Analysis:**

- **Time Complexity**: O(n) - visit each node once for serialization
- **Space Complexity**: O(n) - store entire serialized strings + O(h) recursion
- **Issues**: Uses extra space for string storage

#### ⚡ Optimal Approach

**Teacher's Explanation:**
"We can compare trees directly without storing extra information!"

**Recursive Strategy:**

1. Compare trees recursively node by node
2. Base cases: both null (identical), one null (not identical)
3. Recursive case: current nodes match AND left subtrees match AND right subtrees match

```cpp
// Optimal Recursive: Direct recursive comparison
bool areIdenticalOptimalRecursive(TreeNode* tree1, TreeNode* tree2) {
    cout << "Comparing nodes recursively..." << endl;

    // Base Case 1: Both trees are empty
    if (tree1 == nullptr && tree2 == nullptr) {
        cout << "Both subtrees are null - they match!" << endl;
        return true;
    }

    // Base Case 2: One tree is empty, other is not
    if (tree1 == nullptr || tree2 == nullptr) {
        cout << "One tree is null, other isn't - they don't match!" << endl;
        return false;
    }

    // Recursive Case: Both trees have nodes
    cout << "Comparing values: " << tree1->data << " vs " << tree2->data << endl;

    // Check current values and recursively check subtrees
    bool currentMatch = (tree1->data == tree2->data);
    bool leftMatch = areIdenticalOptimalRecursive(tree1->left, tree2->left);
    bool rightMatch = areIdenticalOptimalRecursive(tree1->right, tree2->right);

    bool result = currentMatch && leftMatch && rightMatch;
    cout << "Subtree comparison result: " << (result ? "MATCH" : "NO MATCH") << endl;

    return result;
}

// Optimal Iterative: Using two stacks for parallel traversal
bool areIdenticalOptimalIterative(TreeNode* tree1, TreeNode* tree2) {
    cout << "=== ITERATIVE APPROACH USING STACKS ===" << endl;

    stack<TreeNode*> stack1, stack2;
    stack1.push(tree1);
    stack2.push(tree2);

    while (!stack1.empty() && !stack2.empty()) {
        TreeNode* node1 = stack1.top(); stack1.pop();
        TreeNode* node2 = stack2.top(); stack2.pop();

        // Both null - continue
        if (node1 == nullptr && node2 == nullptr) {
            cout << "Both nodes are null - continuing..." << endl;
            continue;
        }

        // One null, other not - not identical
        if (node1 == nullptr || node2 == nullptr) {
            cout << "One node is null, other isn't - trees not identical!" << endl;
            return false;
        }

        // Compare values
        cout << "Comparing: " << node1->data << " vs " << node2->data << endl;
        if (node1->data != node2->data) {
            cout << "Values don't match - trees not identical!" << endl;
            return false;
        }

        // Push children in same order
        stack1.push(node1->left);
        stack1.push(node1->right);
        stack2.push(node2->left);
        stack2.push(node2->right);
    }

    // Both stacks should be empty
    bool result = stack1.empty() && stack2.empty();
    cout << "Iterative comparison result: " << (result ? "IDENTICAL" : "NOT IDENTICAL") << endl;
    return result;
}

// Clean production version (recursive)
bool areIdentical(TreeNode* tree1, TreeNode* tree2) {
    if (tree1 == nullptr && tree2 == nullptr) return true;
    if (tree1 == nullptr || tree2 == nullptr) return false;

    return (tree1->data == tree2->data) &&
           areIdentical(tree1->left, tree2->left) &&
           areIdentical(tree1->right, tree2->right);
}
```

**Analysis Comparison:**

- **Recursive Time**: O(min(n1, n2)), **Space**: O(min(h1, h2)) - recursion stack
- **Iterative Time**: O(min(n1, n2)), **Space**: O(min(h1, h2)) - explicit stacks
- **Advantages**: Both can terminate early; iterative avoids stack overflow

**Key Insight:**
"Optimal approach stops as soon as it finds a mismatch, while brute force always processes entire trees!"

### 🏗️ Problem 3: Build Binary Tree from Array Representation

#### 📋 Problem Statement

**Given**: An array representing a complete binary tree in level-order
**Build**: A binary tree using TreeNode structure
**Return**: Root node of the constructed tree

**Array Representation Rules:**

- Index 0 contains the root
- For node at index i:
  - Left child is at index (2×i + 1)
  - Right child is at index (2×i + 2)
- If index ≥ array.size(), no node exists

**Example:**

```
Input Array: [1, 2, 3, 4, 5, 6, 7]

Expected Tree:
       1
      / \
     2   3
    / \ / \
   4 5 6 7

Index mapping:
1(0) -> left: 2(1), right: 3(2)
2(1) -> left: 4(3), right: 5(4)
3(2) -> left: 6(5), right: 7(6)
```

#### 🐌 Brute Force Approach

**Teacher's Explanation:**
"Let's create all nodes first, then connect them by manually calculating relationships!"

**Brute Force Strategy:**

1. Create all nodes and store in a vector
2. Iterate through array and establish parent-child relationships
3. Return the root node

```cpp
// Brute Force: Create all nodes first, then connect
TreeNode* buildTreeFromArrayBruteForce(vector<int>& arr) {
    if (arr.empty()) return nullptr;

    cout << "=== BRUTE FORCE APPROACH ===" << endl;

    // Step 1: Create all nodes
    vector<TreeNode*> nodes(arr.size());
    for (int i = 0; i < arr.size(); i++) {
        nodes[i] = new TreeNode(arr[i]);
        cout << "Created node " << arr[i] << " at index " << i << endl;
    }

    // Step 2: Connect nodes by iterating through array
    for (int i = 0; i < arr.size(); i++) {
        int leftChildIndex = 2 * i + 1;
        int rightChildIndex = 2 * i + 2;

        // Connect left child
        if (leftChildIndex < arr.size()) {
            nodes[i]->left = nodes[leftChildIndex];
            cout << "Connected " << arr[i] << " -> left: " << arr[leftChildIndex] << endl;
        }

        // Connect right child
        if (rightChildIndex < arr.size()) {
            nodes[i]->right = nodes[rightChildIndex];
            cout << "Connected " << arr[i] << " -> right: " << arr[rightChildIndex] << endl;
        }
    }

    cout << "Tree construction complete!" << endl;
    return nodes[0]; // Return root
}
```

**Brute Force Analysis:**

- **Time Complexity**: O(n) - create n nodes + connect n nodes
- **Space Complexity**: O(n) - store all nodes in vector + O(1) for connections
- **Issues**: Uses extra space to store all nodes unnecessarily

#### ⚡ Optimal Approach

**Teacher's Explanation:**
"We can build the tree recursively without storing extra nodes!"

**Recursive Strategy:**

1. Use recursion with current index
2. Create current node, then recursively create left and right children
3. No extra storage needed beyond recursion stack

```cpp
// Optimal Recursive: Recursive construction without extra storage
TreeNode* buildTreeFromArrayOptimalRecursive(vector<int>& arr, int index = 0) {
    cout << "Attempting to build node at index " << index;

    // Base case: index out of bounds
    if (index >= arr.size()) {
        cout << " - index out of bounds, returning null" << endl;
        return nullptr;
    }

    cout << " with value " << arr[index] << endl;

    // Create current node
    TreeNode* root = new TreeNode(arr[index]);

    // Calculate child indices
    int leftIndex = 2 * index + 1;
    int rightIndex = 2 * index + 2;

    cout << "Node " << arr[index] << " will have:" << endl;
    cout << "  Left child at index " << leftIndex;
    if (leftIndex < arr.size()) cout << " (value: " << arr[leftIndex] << ")";
    cout << endl;

    cout << "  Right child at index " << rightIndex;
    if (rightIndex < arr.size()) cout << " (value: " << arr[rightIndex] << ")";
    cout << endl;

    // Recursively build left and right subtrees
    root->left = buildTreeFromArrayOptimalRecursive(arr, leftIndex);
    root->right = buildTreeFromArrayOptimalRecursive(arr, rightIndex);

    return root;
}

// Optimal Iterative: Using queue for level-order construction
TreeNode* buildTreeFromArrayOptimalIterative(vector<int>& arr) {
    if (arr.empty()) return nullptr;

    cout << "=== ITERATIVE APPROACH USING QUEUE ===" << endl;

    TreeNode* root = new TreeNode(arr[0]);
    queue<TreeNode*> q;
    q.push(root);

    int index = 1; // Start from second element

    while (!q.empty() && index < arr.size()) {
        TreeNode* current = q.front();
        q.pop();

        cout << "Processing node " << current->data << endl;

        // Add left child
        if (index < arr.size()) {
            current->left = new TreeNode(arr[index]);
            q.push(current->left);
            cout << "  Added left child: " << arr[index] << endl;
            index++;
        }

        // Add right child
        if (index < arr.size()) {
            current->right = new TreeNode(arr[index]);
            q.push(current->right);
            cout << "  Added right child: " << arr[index] << endl;
            index++;
        }
    }

    cout << "Iterative tree construction complete!" << endl;
    return root;
}

// Clean production version (recursive)
TreeNode* buildTreeFromArray(vector<int>& arr, int index = 0) {
    if (index >= arr.size()) return nullptr;

    TreeNode* root = new TreeNode(arr[index]);
    root->left = buildTreeFromArray(arr, 2 * index + 1);
    root->right = buildTreeFromArray(arr, 2 * index + 2);

    return root;
}
```

**Analysis Comparison:**

- **Recursive Time**: O(n), **Space**: O(h) - recursion stack
- **Iterative Time**: O(n), **Space**: O(w) - queue storage, where w = max width
- **Trade-offs**: Recursive uses heap indexing, iterative uses level-order approach

**Mathematical Insight:**
"This indexing pattern is the same used in heap data structure - very important for future topics!"

#### 🧪 Testing All Approaches

```cpp
void testTreeConstruction() {
    vector<int> testArray = {1, 2, 3, 4, 5, 6, 7};

    cout << "\n=== TESTING TREE CONSTRUCTION ===" << endl;
    cout << "Input array: ";
    for (int val : testArray) cout << val << " ";
    cout << endl;

    // Test brute force
    TreeNode* bruteForceRoot = buildTreeFromArrayBruteForce(testArray);
    cout << "\nBrute Force Result - Preorder: ";
    preorderTraversal(bruteForceRoot);
    cout << endl;

    // Test optimal
    TreeNode* optimalRoot = buildTreeFromArrayOptimal(testArray);
    cout << "\nOptimal Result - Preorder: ";
    preorderTraversal(optimalRoot);
    cout << endl;

    // Verify both give same result
    bool identical = areIdentical(bruteForceRoot, optimalRoot);
    cout << "\nBoth approaches give same result: " << (identical ? "YES" : "NO") << endl;
}
```

---

## 🎯 Additional Practice Problems with Complete Solutions

### 🔍 Problem 4: Count Total Nodes in Binary Tree

#### 📋 Problem Statement

**Given**: A binary tree
**Find**: Total number of nodes in the tree
**Return**: Integer count of all nodes

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Level-order traversal with queue
int countNodesBruteForce(TreeNode* root) {
    if (root == nullptr) return 0;

    queue<TreeNode*> q;
    q.push(root);
    int count = 0;

    while (!q.empty()) {
        TreeNode* current = q.front();
        q.pop();
        count++;

        if (current->left) q.push(current->left);
        if (current->right) q.push(current->right);
    }

    return count;
}
```

#### ⚡ Optimal Approach

**Recursive Solution:**

```cpp
// Optimal Recursive: Simple recursion
int countNodesOptimalRecursive(TreeNode* root) {
    if (root == nullptr) return 0;

    cout << "Counting node: " << root->data << endl;
    int leftCount = countNodesOptimalRecursive(root->left);
    int rightCount = countNodesOptimalRecursive(root->right);

    int total = 1 + leftCount + rightCount;
    cout << "Subtree rooted at " << root->data << " has " << total << " nodes" << endl;

    return total;
}
```

**Iterative Solution:**

```cpp
// Optimal Iterative: Stack-based DFS
int countNodesOptimalIterative(TreeNode* root) {
    if (root == nullptr) return 0;

    stack<TreeNode*> stk;
    stk.push(root);
    int count = 0;

    cout << "=== ITERATIVE NODE COUNTING ===" << endl;

    while (!stk.empty()) {
        TreeNode* current = stk.top();
        stk.pop();
        count++;

        cout << "Counted node: " << current->data << " (total so far: " << count << ")" << endl;

        if (current->right) stk.push(current->right);
        if (current->left) stk.push(current->left);
    }

    return count;
}
```

### 🔍 Problem 5: Find Height of Binary Tree

#### 📋 Problem Statement

**Given**: A binary tree
**Find**: Maximum depth/height of the tree
**Return**: Integer representing height (empty tree = 0, single node = 1)

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Level-order traversal counting levels
int findHeightBruteForce(TreeNode* root) {
    if (root == nullptr) return 0;

    queue<TreeNode*> q;
    q.push(root);
    int height = 0;

    while (!q.empty()) {
        int levelSize = q.size();
        height++;

        for (int i = 0; i < levelSize; i++) {
            TreeNode* current = q.front();
            q.pop();

            if (current->left) q.push(current->left);
            if (current->right) q.push(current->right);
        }
    }

    return height;
}
```

#### ⚡ Optimal Approach

**Recursive Solution:**

```cpp
// Optimal Recursive: Divide and conquer
int findHeightOptimalRecursive(TreeNode* root) {
    if (root == nullptr) return 0;

    cout << "Calculating height for node: " << root->data << endl;

    int leftHeight = findHeightOptimalRecursive(root->left);
    int rightHeight = findHeightOptimalRecursive(root->right);

    int currentHeight = 1 + max(leftHeight, rightHeight);
    cout << "Height at node " << root->data << " = " << currentHeight << endl;

    return currentHeight;
}
```

**Iterative Solution:**

```cpp
// Optimal Iterative: Stack with depth tracking
int findHeightOptimalIterative(TreeNode* root) {
    if (root == nullptr) return 0;

    stack<pair<TreeNode*, int>> stk; // {node, depth}
    stk.push({root, 1});
    int maxHeight = 0;

    cout << "=== ITERATIVE HEIGHT CALCULATION ===" << endl;

    while (!stk.empty()) {
        auto [current, depth] = stk.top();
        stk.pop();

        cout << "Node " << current->data << " at depth " << depth << endl;
        maxHeight = max(maxHeight, depth);

        if (current->right) {
            stk.push({current->right, depth + 1});
        }
        if (current->left) {
            stk.push({current->left, depth + 1});
        }
    }

    cout << "Maximum height found: " << maxHeight << endl;
    return maxHeight;
}
```

### 🔍 Problem 6: Search for a Value in Binary Tree

#### 📋 Problem Statement

**Given**: A binary tree and a target value
**Find**: Whether the target exists in the tree
**Return**: true if found, false otherwise

#### 🐌 Brute Force Approach

```cpp
// Brute Force: Collect all values then search
bool searchValueBruteForce(TreeNode* root, int target) {
    vector<int> allValues;

    function<void(TreeNode*)> collect = [&](TreeNode* node) {
        if (node == nullptr) return;
        allValues.push_back(node->data);
        collect(node->left);
        collect(node->right);
    };

    collect(root);

    for (int val : allValues) {
        if (val == target) return true;
    }
    return false;
}
```

#### ⚡ Optimal Approach

**Recursive Solution:**

```cpp
// Optimal Recursive: Early termination with recursion
bool searchValueOptimalRecursive(TreeNode* root, int target) {
    if (root == nullptr) return false;

    cout << "Searching at node: " << root->data << endl;

    if (root->data == target) {
        cout << "Found target " << target << " at node!" << endl;
        return true;
    }

    // Search in left or right subtree
    bool foundInLeft = searchValueOptimalRecursive(root->left, target);
    if (foundInLeft) return true; // Early termination

    return searchValueOptimalRecursive(root->right, target);
}
```

**Iterative Solution:**

```cpp
// Optimal Iterative: Stack-based search with early termination
bool searchValueOptimalIterative(TreeNode* root, int target) {
    if (root == nullptr) return false;

    stack<TreeNode*> stk;
    stk.push(root);

    cout << "=== ITERATIVE SEARCH ===" << endl;

    while (!stk.empty()) {
        TreeNode* current = stk.top();
        stk.pop();

        cout << "Checking node: " << current->data << endl;

        if (current->data == target) {
            cout << "Found target " << target << "!" << endl;
            return true;
        }

        if (current->right) stk.push(current->right);
        if (current->left) stk.push(current->left);
    }

    cout << "Target " << target << " not found" << endl;
    return false;
}
```

---

## 📊 Complexity Comparison Summary

| Problem              | Approach    | Time Complexity | Space Complexity | Notes                |
| -------------------- | ----------- | --------------- | ---------------- | -------------------- |
| **Find Maximum**     | Brute Force | O(n)            | O(n)             | Stores all values    |
|                      | Recursive   | O(n)            | O(h)             | Divide & conquer     |
|                      | Iterative   | O(n)            | O(h)             | Stack-based DFS      |
| **Tree Identical**   | Brute Force | O(n)            | O(n)             | String serialization |
|                      | Recursive   | O(min(n1,n2))   | O(min(h1,h2))    | Early termination    |
|                      | Iterative   | O(min(n1,n2))   | O(min(h1,h2))    | Two stacks           |
| **Build from Array** | Brute Force | O(n)            | O(n)             | Store all nodes      |
|                      | Recursive   | O(n)            | O(h)             | Heap indexing        |
|                      | Iterative   | O(n)            | O(w)             | Level-order, w=width |
| **Count Nodes**      | Brute Force | O(n)            | O(n)             | Level-order queue    |
|                      | Recursive   | O(n)            | O(h)             | Simple recursion     |
|                      | Iterative   | O(n)            | O(h)             | Stack-based DFS      |
| **Find Height**      | Brute Force | O(n)            | O(n)             | Level-order queue    |
|                      | Recursive   | O(n)            | O(h)             | Divide & conquer     |
|                      | Iterative   | O(n)            | O(h)             | Stack with depth     |
| **Search Value**     | Brute Force | O(n)            | O(n)             | Collect then search  |
|                      | Recursive   | O(n)            | O(h)             | Early termination    |
|                      | Iterative   | O(n)            | O(h)             | Stack-based search   |

**Key Insights:**

- **h = log(n)** for balanced trees, **h = n** for skewed trees
- **w ≤ n/2** for complete binary trees (maximum width)
- **Recursive solutions** are often cleaner and easier to understand
- **Iterative solutions** avoid stack overflow for very deep trees
- **Early termination** can improve average-case performance
- **Space complexity** is usually better with recursive/iterative optimal approaches

**When to Use Each Approach:**

- **Recursive**: Clean, elegant, easy to understand and debug
- **Iterative**: When dealing with very deep trees (avoid stack overflow)
- **Brute Force**: When simplicity is more important than efficiency

````

---

## Complete Week 1 Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};

class TreeOperations {
public:
    TreeNode* createNode(int data) {
        return new TreeNode(data);
    }

    void preorderTraversal(TreeNode* root) {
        if (root == nullptr) return;
        cout << root->data << " ";
        preorderTraversal(root->left);
        preorderTraversal(root->right);
    }

    int calculateHeight(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + max(calculateHeight(root->left), calculateHeight(root->right));
    }

    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }

    int countLeaves(TreeNode* root) {
        if (root == nullptr) return 0;
        if (root->left == nullptr && root->right == nullptr) return 1;
        return countLeaves(root->left) + countLeaves(root->right);
    }

    int findMaximum(TreeNode* root) {
        if (root == nullptr) return INT_MIN;
        int leftMax = (root->left) ? findMaximum(root->left) : INT_MIN;
        int rightMax = (root->right) ? findMaximum(root->right) : INT_MIN;
        return max({root->data, leftMax, rightMax});
    }

    bool areIdentical(TreeNode* tree1, TreeNode* tree2) {
        if (tree1 == nullptr && tree2 == nullptr) return true;
        if (tree1 == nullptr || tree2 == nullptr) return false;
        return (tree1->data == tree2->data) &&
               areIdentical(tree1->left, tree2->left) &&
               areIdentical(tree1->right, tree2->right);
    }
};

// Test function
int main() {
    TreeOperations treeOps;

    // Create sample tree
    TreeNode* root = treeOps.createNode(1);
    root->left = treeOps.createNode(2);
    root->right = treeOps.createNode(3);
    root->left->left = treeOps.createNode(4);
    root->left->right = treeOps.createNode(5);

    // Test all operations
    cout << "Preorder Traversal: ";
    treeOps.preorderTraversal(root);
    cout << endl;

    cout << "Height: " << treeOps.calculateHeight(root) << endl;
    cout << "Total Nodes: " << treeOps.countNodes(root) << endl;
    cout << "Leaf Nodes: " << treeOps.countLeaves(root) << endl;
    cout << "Maximum Element: " << treeOps.findMaximum(root) << endl;

    return 0;
}
````

---

## 🎓 Week 1 Student Assignments & Assessment

### 📝 Assignment 1: Manual Tree Analysis (Paper & Pencil)

**Instructions for Students:**
"Draw these trees on paper and answer the questions. This helps build intuition before coding!"

#### Tree to Analyze:

```
        10
       /  \
      5    15
     / \     \
    3   7    20
   /
  1
```

**Questions:**

1. What is the root node?
2. List all leaf nodes
3. What is the height of this tree?
4. What is the depth of node 7?
5. Trace preorder traversal step by step
6. How many total nodes are there?

**Expected Answers:**

1. Root: 10
2. Leaves: 1, 7, 20
3. Height: 4
4. Depth of 7: 2
5. Preorder: 10, 5, 3, 1, 7, 15, 20
6. Total nodes: 7

### 💻 Assignment 2: Implement Missing Functions

**Teacher's Instructions:**
"Complete these functions. Start with the easier ones, then challenge yourself with the harder ones!"

```cpp
// EASY LEVEL
int findMinimum(TreeNode* root) {
    // TODO: Find the smallest value in the tree
    // Hint: Similar to findMaximum, but use min instead of max
}

bool searchElement(TreeNode* root, int target) {
    // TODO: Return true if target exists in tree, false otherwise
    // Hint: Check current node, then search left and right subtrees
}

// MEDIUM LEVEL
int calculateDepth(TreeNode* root, int target) {
    // TODO: Find the depth of the target node
    // Return -1 if target not found
    // Hint: Use helper function with current depth parameter
}

void printNodesAtLevel(TreeNode* root, int level) {
    // TODO: Print all nodes at the given level
    // Level 0 = root, Level 1 = children of root, etc.
}

// HARD LEVEL
void printLevelOrder(TreeNode* root) {
    // TODO: Print tree level by level
    // Example output: "Level 0: 1 \nLevel 1: 2 3 \nLevel 2: 4 5 6"
    // Hint: Use the height function and printNodesAtLevel
}
```

**Sample Solutions for Teachers:**

```cpp
// EASY - Find Minimum
int findMinimum(TreeNode* root) {
    if (root == nullptr) return INT_MAX;
    int leftMin = (root->left) ? findMinimum(root->left) : INT_MAX;
    int rightMin = (root->right) ? findMinimum(root->right) : INT_MAX;
    return min({root->data, leftMin, rightMin});
}

// EASY - Search Element
bool searchElement(TreeNode* root, int target) {
    if (root == nullptr) return false;
    if (root->data == target) return true;
    return searchElement(root->left, target) || searchElement(root->right, target);
}

// MEDIUM - Calculate Depth
int calculateDepthHelper(TreeNode* root, int target, int currentDepth) {
    if (root == nullptr) return -1;
    if (root->data == target) return currentDepth;

    int leftDepth = calculateDepthHelper(root->left, target, currentDepth + 1);
    if (leftDepth != -1) return leftDepth;

    return calculateDepthHelper(root->right, target, currentDepth + 1);
}

int calculateDepth(TreeNode* root, int target) {
    return calculateDepthHelper(root, target, 0);
}
```

### 🏗️ Assignment 3: Build and Test Different Tree Types

**Student Instructions:**
"Create these special types of trees and analyze their properties."

```cpp
// 1. Left-skewed tree (all nodes have only left children)
TreeNode* buildLeftSkewedTree() {
    // Build tree: 1->2->3->4 (all going left)
    // TODO: Implement this
}

// 2. Right-skewed tree (all nodes have only right children)
TreeNode* buildRightSkewedTree() {
    // Build tree: 1->2->3->4 (all going right)
    // TODO: Implement this
}

// 3. Complete binary tree from array
void testArrayToTree() {
    vector<int> arr = {1, 2, 3, 4, 5, 6, 7};
    TreeNode* root = buildTreeFromArray(arr);
    // TODO: Test this tree with all our functions
}
```

**Analysis Questions:**

1. What's the height of a left-skewed tree with n nodes?
2. What's the height of a complete binary tree with n nodes?
3. Which tree shape is more efficient for searching?

### 🤔 Assignment 4: Complexity Analysis & Critical Thinking

**Teacher's Guidance:**
"Understanding time and space complexity is crucial for becoming a good programmer."

#### Time Complexity Questions:

1. **preorderTraversal(root)**: What's the time complexity? Why?
2. **calculateHeight(root)**: What's the time complexity? Why?
3. **findMaximum(root)**: What's the time complexity? Why?

**Expected Answers:**

- All are O(n) where n = number of nodes
- We must visit every node exactly once
- Cannot do better than O(n) for these problems

#### Space Complexity Questions:

1. What's the space complexity of recursive traversal?
2. What happens to space complexity in a skewed tree vs balanced tree?
3. How does recursion use memory?

**Expected Answers:**

- Space complexity: O(h) where h = height of tree
- Skewed tree: O(n) space, Balanced tree: O(log n) space
- Each recursive call uses stack space

#### Critical Thinking:

```cpp
// Question: How would you modify preorder to print in reverse?
void reversePreorder(TreeNode* root) {
    // TODO: Think about this - what order should we visit nodes?
    // Hint: Instead of Root->Left->Right, try Root->Right->Left
}

// Question: Can you implement preorder without recursion?
void iterativePreorder(TreeNode* root) {
    // TODO: This is a preview of Week 2!
    // Hint: Use a stack data structure
}
```

---

## 📊 Teacher's Assessment Rubric

### Week 1 Competency Checklist:

**Basic Understanding (Must Have):**

- [ ] Can explain what a tree is using real-world analogies
- [ ] Can identify root, leaves, height, depth in any tree
- [ ] Can trace preorder traversal manually on paper
- [ ] Understands base case and recursive case

**Implementation Skills (Should Have):**

- [ ] Can implement basic tree node structure
- [ ] Can write preorder traversal function
- [ ] Can implement height and counting functions
- [ ] Can debug simple tree problems

**Advanced Thinking (Nice to Have):**

- [ ] Understands time/space complexity
- [ ] Can modify algorithms for different requirements
- [ ] Can build trees from different input formats
- [ ] Asks thoughtful questions about edge cases

### 🗣️ Classroom Discussion Questions:

1. "Why do we use trees instead of arrays or linked lists?"
2. "What happens if we have duplicate values in our tree?"
3. "How would trees be useful in a real application like a file system?"
4. "What would happen if we allowed cycles in our tree?"

### 🎯 Common Student Mistakes to Watch For:

1. **Forgetting base case** in recursion
2. **Null pointer access** when node doesn't exist
3. **Confusing height vs depth** concepts
4. **Not understanding** how recursion builds up the call stack
5. **Memory leaks** from not properly managing dynamic allocation

---

## Next Week Preview

In Week 2, we'll explore:

- All three DFS traversals (inorder, postorder)
- Iterative implementations using stacks
- Level-order traversal using queues
- Stack visualization techniques

**Good luck with Week 1! Make sure to practice drawing trees by hand and tracing through recursive calls.**

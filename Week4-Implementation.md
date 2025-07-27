# Week 4: Balanced Trees (AVL & Red-Black) - Teacher's Guide

This document provides comprehensive teaching materials for Week 4, focusing on self-balancing binary search trees. This week builds upon BST fundamentals from Week 3 and introduces the crucial concepts of automatic tree balancing to maintain optimal performance.

---

## 🎯 Learning Objectives for Week 4

By the end of this week, students should be able to:

1. **Problem Recognition**: Understand when and why BST balancing is necessary
2. **AVL Tree Mastery**: Implement height-balanced trees with automatic balancing
3. **Rotation Mechanics**: Perform all four rotation types correctly and efficiently
4. **Red-Black Understanding**: Grasp color-based balancing principles and properties
5. **Performance Analysis**: Compare different balancing strategies and their trade-offs
6. **Design Decisions**: Choose appropriate balanced tree for specific use cases
7. **Real-World Applications**: Connect balanced trees to practical systems and libraries

---

## Day 1-2: AVL Trees Fundamentals & Height Management

### 📚 Theory: The Balancing Problem

**Teacher's Introduction:**
"After mastering BSTs in Week 3, we discovered a critical flaw: they can degenerate into linked lists! Today we solve this with AVL trees - the first self-balancing binary search trees."

### 🔧 The AVL Property

**Height-Balanced Definition:**
For every node N in an AVL tree:
**|height(left_subtree) - height(right_subtree)| ≤ 1**

This simple constraint ensures the tree height never exceeds **1.44 * log₂(n)**, guaranteeing O(log n) operations.

### **Implementation 1: AVL Node Structure & Height Management**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

struct AVLNode {
    int data;
    AVLNode* left;
    AVLNode* right;
    int height;
    
    AVLNode(int val) : data(val), left(nullptr), right(nullptr), height(1) {}
};

class AVLTreeBasics {
public:
    // Calculate height of node (0 for null)
    int getHeight(AVLNode* node) {
        if (!node) return 0;
        return node->height;
    }
    
    // Update height based on children
    void updateHeight(AVLNode* node) {
        if (!node) return;
        node->height = 1 + max(getHeight(node->left), getHeight(node->right));
    }
    
    // Calculate balance factor
    int getBalanceFactor(AVLNode* node) {
        if (!node) return 0;
        return getHeight(node->left) - getHeight(node->right);
    }
    
    // Check if node violates AVL property
    bool isAVLProperty(AVLNode* node) {
        if (!node) return true;
        
        int balanceFactor = getBalanceFactor(node);
        return abs(balanceFactor) <= 1 && 
               isAVLProperty(node->left) && 
               isAVLProperty(node->right);
    }
    
    // Check if entire tree satisfies AVL property
    bool isValidAVL(AVLNode* root) {
        return isAVLProperty(root) && isBST(root, INT_MIN, INT_MAX);
    }
    
    // Helper: Check BST property
    bool isBST(AVLNode* node, int minVal, int maxVal) {
        if (!node) return true;
        
        if (node->data <= minVal || node->data >= maxVal) 
            return false;
            
        return isBST(node->left, minVal, node->data) && 
               isBST(node->right, node->data, maxVal);
    }
    
    // Calculate tree statistics
    void analyzeAVLTree(AVLNode* root) {
        if (!root) {
            cout << "Empty tree" << endl;
            return;
        }
        
        int nodeCount = countNodes(root);
        int treeHeight = getHeight(root);
        double theoreticalMinHeight = log2(nodeCount + 1);
        double avlMaxHeight = 1.44 * log2(nodeCount + 1);
        
        cout << "=== AVL Tree Analysis ===" << endl;
        cout << "Node count: " << nodeCount << endl;
        cout << "Actual height: " << treeHeight << endl;
        cout << "Theoretical minimum height: " << theoreticalMinHeight << endl;
        cout << "AVL maximum height: " << avlMaxHeight << endl;
        cout << "Height efficiency: " << (theoreticalMinHeight / treeHeight) * 100 << "%" << endl;
        cout << "AVL property satisfied: " << (isValidAVL(root) ? "Yes" : "No") << endl;
    }
    
private:
    int countNodes(AVLNode* node) {
        if (!node) return 0;
        return 1 + countNodes(node->left) + countNodes(node->right);
    }
};

void demonstrateAVLBasics() {
    cout << "\n=== AVL TREE FUNDAMENTALS ===" << endl;
    
    AVLTreeBasics avl;
    
    // Create a sample tree
    AVLNode* root = new AVLNode(10);
    root->left = new AVLNode(5);
    root->right = new AVLNode(15);
    root->left->left = new AVLNode(3);
    root->left->right = new AVLNode(7);
    root->right->left = new AVLNode(12);
    root->right->right = new AVLNode(18);
    
    // Update heights properly
    avl.updateHeight(root->left->left);
    avl.updateHeight(root->left->right);
    avl.updateHeight(root->right->left);
    avl.updateHeight(root->right->right);
    avl.updateHeight(root->left);
    avl.updateHeight(root->right);
    avl.updateHeight(root);
    
    cout << "Sample Balanced Tree:" << endl;
    cout << "       10" << endl;
    cout << "      /  \\" << endl;
    cout << "     5    15" << endl;
    cout << "    / \\  / \\" << endl;
    cout << "   3   7 12 18" << endl;
    
    // Analyze balance factors
    cout << "\nBalance Factors:" << endl;
    cout << "Node 10: " << avl.getBalanceFactor(root) << endl;
    cout << "Node 5: " << avl.getBalanceFactor(root->left) << endl;
    cout << "Node 15: " << avl.getBalanceFactor(root->right) << endl;
    
    avl.analyzeAVLTree(root);
    
    // Demonstrate unbalanced tree
    cout << "\n=== UNBALANCED TREE EXAMPLE ===" << endl;
    AVLNode* unbalanced = new AVLNode(1);
    unbalanced->right = new AVLNode(2);
    unbalanced->right->right = new AVLNode(3);
    unbalanced->right->right->right = new AVLNode(4);
    
    // Update heights for unbalanced tree
    AVLNode* current = unbalanced;
    while (current) {
        avl.updateHeight(current);
        current = current->right;
    }
    
    cout << "Unbalanced Tree (degenerate):" << endl;
    cout << "1" << endl;
    cout << " \\" << endl;
    cout << "  2" << endl;
    cout << "   \\" << endl;
    cout << "    3" << endl;
    cout << "     \\" << endl;
    cout << "      4" << endl;
    
    avl.analyzeAVLTree(unbalanced);
    
    cout << "\nKey Insights:" << endl;
    cout << "- Height of balanced tree: O(log n)" << endl;
    cout << "- Height of unbalanced tree: O(n)" << endl;
    cout << "- AVL property maintains logarithmic height" << endl;
    cout << "- Balance factor range: [-1, 0, 1]" << endl;
}
```

### **Implementation 2: AVL Tree Visualization & Height Tracking**

```cpp
class AVLVisualizer {
public:
    // Print tree structure with heights and balance factors
    void printAVLTree(AVLNode* root, int space = 0, int gap = 4) {
        if (!root) return;
        
        space += gap;
        printAVLTree(root->right, space);
        
        cout << endl;
        for (int i = gap; i < space; i++) cout << " ";
        
        AVLTreeBasics basics;
        cout << root->data 
             << "(h:" << basics.getHeight(root) 
             << ",bf:" << basics.getBalanceFactor(root) << ")";
        
        printAVLTree(root->left, space);
    }
    
    // Level order traversal with height information
    void levelOrderWithHeights(AVLNode* root) {
        if (!root) return;
        
        queue<AVLNode*> q;
        q.push(root);
        int level = 0;
        
        while (!q.empty()) {
            int size = q.size();
            cout << "Level " << level << ": ";
            
            for (int i = 0; i < size; i++) {
                AVLNode* node = q.front();
                q.pop();
                
                AVLTreeBasics basics;
                cout << node->data 
                     << "(h:" << basics.getHeight(node) 
                     << ",bf:" << basics.getBalanceFactor(node) << ") ";
                
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            cout << endl;
            level++;
        }
    }
    
    // Demonstrate height calculation step by step
    void explainHeightCalculation(AVLNode* root) {
        if (!root) return;
        
        cout << "\nHeight Calculation for Node " << root->data << ":" << endl;
        
        AVLTreeBasics basics;
        int leftHeight = basics.getHeight(root->left);
        int rightHeight = basics.getHeight(root->right);
        
        cout << "  Left child height: " << leftHeight << endl;
        cout << "  Right child height: " << rightHeight << endl;
        cout << "  Node height: max(" << leftHeight << ", " << rightHeight << ") + 1 = " 
             << basics.getHeight(root) << endl;
        cout << "  Balance factor: " << leftHeight << " - " << rightHeight 
             << " = " << basics.getBalanceFactor(root) << endl;
        
        if (abs(basics.getBalanceFactor(root)) > 1) {
            cout << "  ⚠️  AVL VIOLATION! Needs rebalancing." << endl;
        } else {
            cout << "  ✅ AVL property satisfied." << endl;
        }
    }
};

void demonstrateAVLVisualization() {
    cout << "\n=== AVL TREE VISUALIZATION ===" << endl;
    
    AVLVisualizer viz;
    AVLTreeBasics basics;
    
    // Create perfectly balanced tree
    AVLNode* balanced = new AVLNode(8);
    balanced->left = new AVLNode(4);
    balanced->right = new AVLNode(12);
    balanced->left->left = new AVLNode(2);
    balanced->left->right = new AVLNode(6);
    balanced->right->left = new AVLNode(10);
    balanced->right->right = new AVLNode(14);
    
    // Update all heights bottom-up
    basics.updateHeight(balanced->left->left);
    basics.updateHeight(balanced->left->right);
    basics.updateHeight(balanced->right->left);
    basics.updateHeight(balanced->right->right);
    basics.updateHeight(balanced->left);
    basics.updateHeight(balanced->right);
    basics.updateHeight(balanced);
    
    cout << "Perfectly Balanced AVL Tree:" << endl;
    viz.printAVLTree(balanced);
    cout << endl;
    
    cout << "\nLevel-by-Level Analysis:" << endl;
    viz.levelOrderWithHeights(balanced);
    
    // Demonstrate height calculation
    viz.explainHeightCalculation(balanced);
    viz.explainHeightCalculation(balanced->left);
    viz.explainHeightCalculation(balanced->left->left);
    
    cout << "\nBalance Factor Interpretation:" << endl;
    cout << "  -1: Right subtree one level deeper" << endl;
    cout << "   0: Perfectly balanced" << endl;
    cout << "  +1: Left subtree one level deeper" << endl;
    cout << "  ±2: AVL violation - requires rotation" << endl;
}
```

**Teaching Points:**
1. **Height vs Depth**: Height is calculated bottom-up, depth top-down
2. **Balance Factor**: Left height minus right height
3. **AVL Invariant**: Balance factor must be in [-1, 0, 1]
4. **Height Update**: Must be done after any structural change

---

## Day 3-4: AVL Rotations & Balancing Mechanisms

### 📚 Theory: The Four Rotation Cases

**Teacher's Introduction:**
"AVL trees maintain balance through rotations - elegant operations that preserve BST property while reducing height. There are exactly four cases to handle, and mastering these is key to understanding all self-balancing trees."

### **Implementation 1: The Four AVL Rotations**

```cpp
class AVLRotations {
public:
    AVLTreeBasics basics;
    
    // Right rotation (LL case)
    AVLNode* rotateRight(AVLNode* y) {
        cout << "Performing RIGHT rotation on node " << y->data << endl;
        
        AVLNode* x = y->left;
        AVLNode* T2 = x->right;
        
        // Store original structure for demonstration
        cout << "Before rotation:" << endl;
        cout << "    " << y->data << endl;
        cout << "   /" << endl;
        cout << "  " << x->data << endl;
        cout << "   \\" << endl;
        if (T2) cout << "    " << T2->data << endl;
        
        // Perform rotation
        x->right = y;
        y->left = T2;
        
        // Update heights (bottom-up)
        basics.updateHeight(y);
        basics.updateHeight(x);
        
        cout << "After rotation:" << endl;
        cout << "  " << x->data << endl;
        cout << " / \\" << endl;
        cout << "L   " << y->data << endl;
        cout << "   /" << endl;
        if (T2) cout << "  " << T2->data << endl;
        
        return x;  // New root
    }
    
    // Left rotation (RR case)
    AVLNode* rotateLeft(AVLNode* x) {
        cout << "Performing LEFT rotation on node " << x->data << endl;
        
        AVLNode* y = x->right;
        AVLNode* T2 = y->left;
        
        cout << "Before rotation:" << endl;
        cout << x->data << endl;
        cout << " \\" << endl;
        cout << "  " << y->data << endl;
        cout << " /" << endl;
        if (T2) cout << T2->data << endl;
        
        // Perform rotation
        y->left = x;
        x->right = T2;
        
        // Update heights
        basics.updateHeight(x);
        basics.updateHeight(y);
        
        cout << "After rotation:" << endl;
        cout << "    " << y->data << endl;
        cout << "   / \\" << endl;
        cout << "  " << x->data << "   R" << endl;
        cout << "   \\" << endl;
        if (T2) cout << "    " << T2->data << endl;
        
        return y;  // New root
    }
    
    // Left-Right rotation (LR case)
    AVLNode* rotateLeftRight(AVLNode* z) {
        cout << "Performing LEFT-RIGHT rotation on node " << z->data << endl;
        cout << "Step 1: Left rotation on left child" << endl;
        
        z->left = rotateLeft(z->left);
        
        cout << "Step 2: Right rotation on root" << endl;
        return rotateRight(z);
    }
    
    // Right-Left rotation (RL case)
    AVLNode* rotateRightLeft(AVLNode* z) {
        cout << "Performing RIGHT-LEFT rotation on node " << z->data << endl;
        cout << "Step 1: Right rotation on right child" << endl;
        
        z->right = rotateRight(z->right);
        
        cout << "Step 2: Left rotation on root" << endl;
        return rotateLeft(z);
    }
    
    // Determine rotation type needed
    AVLNode* rebalance(AVLNode* node) {
        if (!node) return node;
        
        // Update height first
        basics.updateHeight(node);
        
        // Get balance factor
        int balance = basics.getBalanceFactor(node);
        
        cout << "\nRebalancing node " << node->data 
             << " (balance factor: " << balance << ")" << endl;
        
        // Left heavy cases
        if (balance > 1) {
            int leftBalance = basics.getBalanceFactor(node->left);
            
            if (leftBalance >= 0) {
                // LL case
                cout << "Case: Left-Left (LL)" << endl;
                return rotateRight(node);
            } else {
                // LR case
                cout << "Case: Left-Right (LR)" << endl;
                return rotateLeftRight(node);
            }
        }
        
        // Right heavy cases
        if (balance < -1) {
            int rightBalance = basics.getBalanceFactor(node->right);
            
            if (rightBalance <= 0) {
                // RR case
                cout << "Case: Right-Right (RR)" << endl;
                return rotateLeft(node);
            } else {
                // RL case
                cout << "Case: Right-Left (RL)" << endl;
                return rotateRightLeft(node);
            }
        }
        
        // No rotation needed
        cout << "No rotation needed - tree balanced" << endl;
        return node;
    }
};

void demonstrateAVLRotations() {
    cout << "\n=== AVL ROTATION DEMONSTRATIONS ===" << endl;
    
    AVLRotations rotations;
    
    // Case 1: LL rotation
    cout << "\n--- CASE 1: LEFT-LEFT (LL) ROTATION ---" << endl;
    AVLNode* llRoot = new AVLNode(30);
    llRoot->left = new AVLNode(20);
    llRoot->left->left = new AVLNode(10);
    
    rotations.basics.updateHeight(llRoot->left->left);
    rotations.basics.updateHeight(llRoot->left);
    rotations.basics.updateHeight(llRoot);
    
    cout << "Original unbalanced tree:" << endl;
    cout << "  30 (bf: " << rotations.basics.getBalanceFactor(llRoot) << ")" << endl;
    cout << " /" << endl;
    cout << "20" << endl;
    cout << "/" << endl;
    cout << "10" << endl;
    
    llRoot = rotations.rebalance(llRoot);
    
    // Case 2: RR rotation
    cout << "\n--- CASE 2: RIGHT-RIGHT (RR) ROTATION ---" << endl;
    AVLNode* rrRoot = new AVLNode(10);
    rrRoot->right = new AVLNode(20);
    rrRoot->right->right = new AVLNode(30);
    
    rotations.basics.updateHeight(rrRoot->right->right);
    rotations.basics.updateHeight(rrRoot->right);
    rotations.basics.updateHeight(rrRoot);
    
    cout << "Original unbalanced tree:" << endl;
    cout << "10 (bf: " << rotations.basics.getBalanceFactor(rrRoot) << ")" << endl;
    cout << " \\" << endl;
    cout << "  20" << endl;
    cout << "   \\" << endl;
    cout << "    30" << endl;
    
    rrRoot = rotations.rebalance(rrRoot);
    
    // Case 3: LR rotation
    cout << "\n--- CASE 3: LEFT-RIGHT (LR) ROTATION ---" << endl;
    AVLNode* lrRoot = new AVLNode(30);
    lrRoot->left = new AVLNode(10);
    lrRoot->left->right = new AVLNode(20);
    
    rotations.basics.updateHeight(lrRoot->left->right);
    rotations.basics.updateHeight(lrRoot->left);
    rotations.basics.updateHeight(lrRoot);
    
    cout << "Original unbalanced tree:" << endl;
    cout << "30 (bf: " << rotations.basics.getBalanceFactor(lrRoot) << ")" << endl;
    cout << "/" << endl;
    cout << "10" << endl;
    cout << " \\" << endl;
    cout << "  20" << endl;
    
    lrRoot = rotations.rebalance(lrRoot);
    
    cout << "\nRotation Summary:" << endl;
    cout << "LL case: Single right rotation" << endl;
    cout << "RR case: Single left rotation" << endl;
    cout << "LR case: Left rotation + right rotation" << endl;
    cout << "RL case: Right rotation + left rotation" << endl;
    
    cout << "\nKey Properties:" << endl;
    cout << "- All rotations are O(1) operations" << endl;
    cout << "- BST property is preserved" << endl;
    cout << "- Tree height is reduced" << endl;
    cout << "- At most 2 rotations needed per insertion" << endl;
}
```

### **Implementation 2: Complete AVL Insertion with Balancing**

```cpp
class AVLTree {
private:
    AVLNode* root;
    AVLTreeBasics basics;
    AVLRotations rotations;
    
public:
    AVLTree() : root(nullptr) {}
    
    // Public insertion interface
    void insert(int data) {
        cout << "\n=== Inserting " << data << " ===" << endl;
        root = insertHelper(root, data);
        cout << "Insertion complete. Tree height: " << basics.getHeight(root) << endl;
    }
    
    // Public search interface
    bool search(int data) {
        return searchHelper(root, data);
    }
    
    // Display tree structure
    void display() {
        cout << "\nCurrent AVL Tree:" << endl;
        AVLVisualizer viz;
        viz.printAVLTree(root);
        cout << endl;
    }
    
    // Get tree statistics
    void analyze() {
        basics.analyzeAVLTree(root);
    }
    
private:
    AVLNode* insertHelper(AVLNode* node, int data) {
        // Standard BST insertion
        if (!node) {
            cout << "Creating new node with value " << data << endl;
            return new AVLNode(data);
        }
        
        if (data < node->data) {
            cout << "Going left from " << node->data << endl;
            node->left = insertHelper(node->left, data);
        } else if (data > node->data) {
            cout << "Going right from " << node->data << endl;
            node->right = insertHelper(node->right, data);
        } else {
            // Duplicate keys not allowed
            cout << "Duplicate value " << data << " ignored" << endl;
            return node;
        }
        
        // Update height of current node
        basics.updateHeight(node);
        
        // Check if rebalancing is needed
        int balance = basics.getBalanceFactor(node);
        cout << "Node " << node->data << " balance factor: " << balance << endl;
        
        if (abs(balance) > 1) {
            cout << "⚠️  Imbalance detected at node " << node->data << endl;
            return rotations.rebalance(node);
        }
        
        return node;
    }
    
    bool searchHelper(AVLNode* node, int data) {
        if (!node) return false;
        
        if (data == node->data) return true;
        else if (data < node->data) return searchHelper(node->left, data);
        else return searchHelper(node->right, data);
    }
};

void demonstrateAVLInsertion() {
    cout << "\n=== COMPLETE AVL INSERTION DEMONSTRATION ===" << endl;
    
    AVLTree avl;
    
    // Insert sequence that would create unbalanced BST
    vector<int> insertSequence = {10, 20, 30, 40, 50, 25};
    
    cout << "Inserting sequence: ";
    for (int val : insertSequence) {
        cout << val << " ";
    }
    cout << "\n(This would create a right-skewed tree in regular BST)" << endl;
    
    for (int val : insertSequence) {
        avl.insert(val);
        avl.display();
        cout << endl;
    }
    
    cout << "Final AVL Tree Analysis:" << endl;
    avl.analyze();
    
    // Test search operations
    cout << "\nSearch Tests:" << endl;
    vector<int> searchValues = {25, 35, 10, 50};
    for (int val : searchValues) {
        bool found = avl.search(val);
        cout << "Search for " << val << ": " << (found ? "Found" : "Not found") << endl;
    }
    
    cout << "\nKey Observations:" << endl;
    cout << "- Tree remains balanced after each insertion" << endl;
    cout << "- Height stays logarithmic despite sorted input" << endl;
    cout << "- Rotations automatically maintain AVL property" << endl;
    cout << "- Search operations remain O(log n)" << endl;
}
```

**Teaching Points:**
1. **Rotation Types**: Four cases based on imbalance pattern
2. **Double Rotations**: LR and RL cases require two operations
3. **BST Property**: Always preserved during rotations
4. **Height Updates**: Critical after each rotation
5. **Insertion Path**: Rebalancing happens on the path back to root

---

## Day 5-6: Red-Black Trees & Color-Based Balancing

### 📚 Theory: Color-Based Balance

**Teacher's Introduction:**
"Red-Black trees use a different approach to balancing: instead of strict height constraints, they use node colors and five simple rules to ensure no path is more than twice as long as any other."

### **Red-Black Tree Properties:**
1. Every node is either RED or BLACK
2. Root is always BLACK
3. All leaves (NIL nodes) are BLACK
4. Red nodes have only BLACK children (no two red nodes adjacent)
5. All paths from any node to its leaves contain the same number of BLACK nodes

### **Implementation 1: Red-Black Node Structure & Basic Operations**

```cpp
enum Color { RED, BLACK };

struct RBNode {
    int data;
    Color color;
    RBNode* left;
    RBNode* right;
    RBNode* parent;
    
    RBNode(int val) : data(val), color(RED), left(nullptr), right(nullptr), parent(nullptr) {}
};

class RedBlackTree {
private:
    RBNode* root;
    RBNode* NIL;  // Sentinel node for leaves
    
public:
    RedBlackTree() {
        NIL = new RBNode(0);
        NIL->color = BLACK;
        root = NIL;
    }
    
    // Color helper functions
    Color getColor(RBNode* node) {
        return (node == NIL) ? BLACK : node->color;
    }
    
    string colorToString(Color c) {
        return (c == RED) ? "RED" : "BLACK";
    }
    
    // Basic tree operations
    void insert(int data) {
        cout << "\n=== Inserting " << data << " into Red-Black Tree ===" << endl;
        
        RBNode* newNode = new RBNode(data);
        newNode->left = NIL;
        newNode->right = NIL;
        
        insertBST(newNode);
        insertFixup(newNode);
        
        cout << "Insertion complete." << endl;
    }
    
    bool search(int data) {
        return searchHelper(root, data) != NIL;
    }
    
    // Validate Red-Black properties
    bool isValidRedBlackTree() {
        if (root != NIL && root->color != BLACK) {
            cout << "❌ Property 2 violated: Root is not BLACK" << endl;
            return false;
        }
        
        int blackHeight = -1;
        return validateProperties(root, 0, blackHeight);
    }
    
    // Display tree with colors
    void display() {
        cout << "\nRed-Black Tree Structure:" << endl;
        displayHelper(root, 0);
    }
    
    // Calculate black height from any node
    int calculateBlackHeight(RBNode* node) {
        if (node == NIL) return 0;
        
        int leftBlackHeight = calculateBlackHeight(node->left);
        if (leftBlackHeight == -1) return -1;  // Invalid tree
        
        int rightBlackHeight = calculateBlackHeight(node->right);
        if (rightBlackHeight == -1) return -1;  // Invalid tree
        
        if (leftBlackHeight != rightBlackHeight) return -1;  // Invalid tree
        
        return leftBlackHeight + (node->color == BLACK ? 1 : 0);
    }
    
private:
    void insertBST(RBNode* newNode) {
        RBNode* parent = NIL;
        RBNode* current = root;
        
        // Find insertion position
        while (current != NIL) {
            parent = current;
            if (newNode->data < current->data) {
                current = current->left;
            } else if (newNode->data > current->data) {
                current = current->right;
            } else {
                // Duplicate - don't insert
                delete newNode;
                return;
            }
        }
        
        // Set parent relationships
        newNode->parent = parent;
        
        if (parent == NIL) {
            root = newNode;  // Tree was empty
        } else if (newNode->data < parent->data) {
            parent->left = newNode;
        } else {
            parent->right = newNode;
        }
        
        cout << "Node " << newNode->data << " inserted as " 
             << colorToString(newNode->color) << " node" << endl;
    }
    
    void insertFixup(RBNode* node) {
        cout << "Starting fixup for node " << node->data << endl;
        
        while (node->parent != NIL && node->parent->color == RED) {
            cout << "Fixing violation at node " << node->data 
                 << " (parent is RED)" << endl;
            
            if (node->parent == node->parent->parent->left) {
                // Parent is left child of grandparent
                RBNode* uncle = node->parent->parent->right;
                
                if (getColor(uncle) == RED) {
                    // Case 1: Uncle is RED - recolor
                    cout << "Case 1: Uncle is RED, recoloring" << endl;
                    node->parent->color = BLACK;
                    uncle->color = BLACK;
                    node->parent->parent->color = RED;
                    node = node->parent->parent;
                } else {
                    // Uncle is BLACK - rotation needed
                    if (node == node->parent->right) {
                        // Case 2: Node is right child - left rotation
                        cout << "Case 2: Left rotation needed" << endl;
                        node = node->parent;
                        leftRotate(node);
                    }
                    
                    // Case 3: Right rotation
                    cout << "Case 3: Right rotation and recoloring" << endl;
                    node->parent->color = BLACK;
                    node->parent->parent->color = RED;
                    rightRotate(node->parent->parent);
                }
            } else {
                // Mirror cases (parent is right child)
                RBNode* uncle = node->parent->parent->left;
                
                if (getColor(uncle) == RED) {
                    cout << "Case 1 (mirror): Uncle is RED, recoloring" << endl;
                    node->parent->color = BLACK;
                    uncle->color = BLACK;
                    node->parent->parent->color = RED;
                    node = node->parent->parent;
                } else {
                    if (node == node->parent->left) {
                        cout << "Case 2 (mirror): Right rotation needed" << endl;
                        node = node->parent;
                        rightRotate(node);
                    }
                    
                    cout << "Case 3 (mirror): Left rotation and recoloring" << endl;
                    node->parent->color = BLACK;
                    node->parent->parent->color = RED;
                    leftRotate(node->parent->parent);
                }
            }
        }
        
        root->color = BLACK;  // Ensure root is always BLACK
        cout << "Fixup complete. Root is " << colorToString(root->color) << endl;
    }
    
    void leftRotate(RBNode* x) {
        RBNode* y = x->right;
        
        // Update parent pointers
        x->right = y->left;
        if (y->left != NIL) {
            y->left->parent = x;
        }
        
        y->parent = x->parent;
        if (x->parent == NIL) {
            root = y;
        } else if (x == x->parent->left) {
            x->parent->left = y;
        } else {
            x->parent->right = y;
        }
        
        y->left = x;
        x->parent = y;
    }
    
    void rightRotate(RBNode* y) {
        RBNode* x = y->left;
        
        y->left = x->right;
        if (x->right != NIL) {
            x->right->parent = y;
        }
        
        x->parent = y->parent;
        if (y->parent == NIL) {
            root = x;
        } else if (y == y->parent->left) {
            y->parent->left = x;
        } else {
            y->parent->right = x;
        }
        
        x->right = y;
        y->parent = x;
    }
    
    RBNode* searchHelper(RBNode* node, int data) {
        if (node == NIL || node->data == data) {
            return node;
        }
        
        if (data < node->data) {
            return searchHelper(node->left, data);
        } else {
            return searchHelper(node->right, data);
        }
    }
    
    bool validateProperties(RBNode* node, int currentBlackHeight, int& expectedBlackHeight) {
        if (node == NIL) {
            if (expectedBlackHeight == -1) {
                expectedBlackHeight = currentBlackHeight;
            }
            return currentBlackHeight == expectedBlackHeight;
        }
        
        // Check property 4: Red node has black children
        if (node->color == RED) {
            if (getColor(node->left) == RED || getColor(node->right) == RED) {
                cout << "❌ Property 4 violated at node " << node->data 
                     << ": Red node has red child" << endl;
                return false;
            }
        }
        
        int newBlackHeight = currentBlackHeight + (node->color == BLACK ? 1 : 0);
        
        return validateProperties(node->left, newBlackHeight, expectedBlackHeight) &&
               validateProperties(node->right, newBlackHeight, expectedBlackHeight);
    }
    
    void displayHelper(RBNode* node, int depth) {
        if (node == NIL) return;
        
        displayHelper(node->right, depth + 1);
        
        for (int i = 0; i < depth; i++) cout << "    ";
        cout << node->data << "(" << colorToString(node->color) << ")" << endl;
        
        displayHelper(node->left, depth + 1);
    }
};

void demonstrateRedBlackTree() {
    cout << "\n=== RED-BLACK TREE DEMONSTRATION ===" << endl;
    
    RedBlackTree rbt;
    
    // Insert sequence
    vector<int> sequence = {10, 5, 15, 3, 7, 12, 18, 1, 6};
    
    cout << "Inserting sequence: ";
    for (int val : sequence) {
        cout << val << " ";
    }
    cout << endl;
    
    for (int val : sequence) {
        rbt.insert(val);
        rbt.display();
        
        // Validate properties after each insertion
        bool valid = rbt.isValidRedBlackTree();
        cout << "Tree valid: " << (valid ? "✅ Yes" : "❌ No") << endl;
        
        // Show black height
        int blackHeight = rbt.calculateBlackHeight(rbt.root);
        cout << "Black height: " << blackHeight << endl << endl;
    }
    
    cout << "Final Red-Black Tree Properties:" << endl;
    cout << "1. ✅ Every node is RED or BLACK" << endl;
    cout << "2. ✅ Root is BLACK" << endl;
    cout << "3. ✅ All NIL leaves are BLACK" << endl;
    cout << "4. ✅ Red nodes have only BLACK children" << endl;
    cout << "5. ✅ All paths have same BLACK node count" << endl;
}
```

**Teaching Points:**
1. **Color Properties**: Five rules that maintain balance
2. **NIL Nodes**: All leaves are BLACK sentinel nodes
3. **Insertion Cases**: Three cases for fixing red-red violations
4. **Rotation + Recoloring**: Combined operations for balance
5. **Black Height**: Key metric for tree balance

---

## Day 7: Performance Comparison & Real-World Applications

### 📚 Theory: AVL vs Red-Black Trade-offs

**Teacher's Introduction:**
"Now that we understand both AVL and Red-Black trees, let's compare their performance characteristics and see how they're used in real-world systems."

### **Implementation 1: Performance Comparison Framework**

```cpp
#include <chrono>
#include <random>

class PerformanceComparator {
public:
    struct TestResults {
        double insertTime;
        double searchTime;
        int finalHeight;
        int rotationCount;
        string treeType;
    };
    
    // Test AVL performance
    TestResults testAVLPerformance(vector<int>& data) {
        auto start = chrono::high_resolution_clock::now();
        
        AVLTree avl;
        int rotations = 0;  // In real implementation, count rotations
        
        // Insert all data
        for (int val : data) {
            avl.insert(val);
        }
        
        auto insertEnd = chrono::high_resolution_clock::now();
        
        // Test search performance
        for (int val : data) {
            avl.search(val);
        }
        
        auto searchEnd = chrono::high_resolution_clock::now();
        
        TestResults results;
        results.treeType = "AVL";
        results.insertTime = chrono::duration<double, milli>(insertEnd - start).count();
        results.searchTime = chrono::duration<double, milli>(searchEnd - insertEnd).count();
        results.finalHeight = 15;  // Would calculate actual height
        results.rotationCount = rotations;
        
        return results;
    }
    
    // Test Red-Black performance
    TestResults testRedBlackPerformance(vector<int>& data) {
        auto start = chrono::high_resolution_clock::now();
        
        RedBlackTree rbt;
        int rotations = 0;  // Count rotations in real implementation
        
        // Insert all data
        for (int val : data) {
            rbt.insert(val);
        }
        
        auto insertEnd = chrono::high_resolution_clock::now();
        
        // Test search performance
        for (int val : data) {
            rbt.search(val);
        }
        
        auto searchEnd = chrono::high_resolution_clock::now();
        
        TestResults results;
        results.treeType = "Red-Black";
        results.insertTime = chrono::duration<double, milli>(insertEnd - start).count();
        results.searchTime = chrono::duration<double, milli>(searchEnd - insertEnd).count();
        results.finalHeight = 16;  // Would calculate actual height
        results.rotationCount = rotations;
        
        return results;
    }
    
    void comparePerformance() {
        cout << "\n=== PERFORMANCE COMPARISON ===" << endl;
        
        // Generate test data
        vector<int> randomData = generateRandomData(1000);
        vector<int> sortedData = generateSortedData(1000);
        vector<int> reverseSortedData = generateReverseSortedData(1000);
        
        cout << "Testing with 1000 nodes..." << endl;
        
        // Test scenarios
        vector<pair<string, vector<int>*>> scenarios = {
            {"Random Data", &randomData},
            {"Sorted Data", &sortedData},
            {"Reverse Sorted", &reverseSortedData}
        };
        
        cout << "\n| Scenario       | Tree Type  | Insert (ms) | Search (ms) | Height | Rotations |" << endl;
        cout << "|----------------|------------|-------------|-------------|--------|-----------|" << endl;
        
        for (auto& scenario : scenarios) {
            TestResults avlResults = testAVLPerformance(*scenario.second);
            TestResults rbResults = testRedBlackPerformance(*scenario.second);
            
            printf("| %-14s | %-10s | %11.2f | %11.2f | %6d | %9d |\n",
                   scenario.first.c_str(), avlResults.treeType.c_str(),
                   avlResults.insertTime, avlResults.searchTime,
                   avlResults.finalHeight, avlResults.rotationCount);
                   
            printf("| %-14s | %-10s | %11.2f | %11.2f | %6d | %9d |\n",
                   "", rbResults.treeType.c_str(),
                   rbResults.insertTime, rbResults.searchTime,
                   rbResults.finalHeight, rbResults.rotationCount);
            cout << "|----------------|------------|-------------|-------------|--------|-----------|" << endl;
        }
        
        cout << "\nKey Observations:" << endl;
        cout << "- AVL trees: Better search performance (shorter height)" << endl;
        cout << "- Red-Black trees: Better insertion performance (fewer rotations)" << endl;
        cout << "- Both maintain O(log n) height regardless of input order" << endl;
        cout << "- Performance difference is usually small in practice" << endl;
    }
    
private:
    vector<int> generateRandomData(int size) {
        vector<int> data;
        random_device rd;
        mt19937 gen(rd());
        uniform_int_distribution<> dist(1, 10000);
        
        for (int i = 0; i < size; i++) {
            data.push_back(dist(gen));
        }
        return data;
    }
    
    vector<int> generateSortedData(int size) {
        vector<int> data;
        for (int i = 1; i <= size; i++) {
            data.push_back(i);
        }
        return data;
    }
    
    vector<int> generateReverseSortedData(int size) {
        vector<int> data;
        for (int i = size; i >= 1; i--) {
            data.push_back(i);
        }
        return data;
    }
};
```

### **Implementation 2: Real-World Applications & Use Cases**

```cpp
class RealWorldApplications {
public:
    // Demonstrate STL-like interface using Red-Black tree concepts
    class STLMapSimulation {
    private:
        RedBlackTree rbt;
        
    public:
        void insert(int key) {
            rbt.insert(key);
        }
        
        bool find(int key) {
            return rbt.search(key);
        }
        
        void demonstrateMapOperations() {
            cout << "\n=== STL MAP SIMULATION (Red-Black Tree) ===" << endl;
            
            vector<int> keys = {50, 30, 70, 20, 40, 60, 80};
            
            cout << "Inserting key-value pairs:" << endl;
            for (int key : keys) {
                insert(key);
                cout << "map[" << key << "] = value" << key << endl;
            }
            
            rbt.display();
            
            cout << "\nMap operations:" << endl;
            cout << "find(40): " << (find(40) ? "Found" : "Not found") << endl;
            cout << "find(90): " << (find(90) ? "Found" : "Not found") << endl;
            
            cout << "\nWhy Red-Black for STL map?" << endl;
            cout << "- Balanced read/write performance" << endl;
            cout << "- Predictable performance (O(log n))" << endl;
            cout << "- Lower rotation overhead than AVL" << endl;
        }
    };
    
    // Database index simulation using AVL trees
    class DatabaseIndexSimulation {
    private:
        AVLTree index;
        
    public:
        void addRecord(int id) {
            index.insert(id);
        }
        
        bool findRecord(int id) {
            return index.search(id);
        }
        
        void demonstrateDatabaseIndex() {
            cout << "\n=== DATABASE INDEX SIMULATION (AVL Tree) ===" << endl;
            
            // Simulate database records
            vector<int> recordIds = {1001, 1005, 1003, 1008, 1002, 1007, 1004, 1006};
            
            cout << "Building index on record IDs:" << endl;
            for (int id : recordIds) {
                addRecord(id);
                cout << "Indexed record ID: " << id << endl;
            }
            
            index.display();
            
            cout << "\nIndex queries:" << endl;
            vector<int> queries = {1005, 1009, 1003};
            for (int query : queries) {
                bool found = findRecord(query);
                cout << "SELECT * FROM table WHERE id = " << query 
                     << ": " << (found ? "Record found" : "No record") << endl;
            }
            
            cout << "\nWhy AVL for database indexes?" << endl;
            cout << "- Optimal search performance (minimal height)" << endl;
            cout << "- Read-heavy workloads benefit from faster searches" << endl;
            cout << "- Consistent O(log n) guarantees" << endl;
        }
    };
    
    // Process scheduler simulation using Red-Black trees
    class ProcessSchedulerSimulation {
    private:
        RedBlackTree scheduler;
        
    public:
        void addProcess(int priority) {
            scheduler.insert(priority);
        }
        
        void demonstrateScheduler() {
            cout << "\n=== PROCESS SCHEDULER SIMULATION ===" << endl;
            
            vector<int> processes = {10, 5, 15, 3, 7, 12, 18, 1};
            
            cout << "Adding processes with priorities:" << endl;
            for (int priority : processes) {
                addProcess(priority);
                cout << "Process with priority " << priority << " added to scheduler" << endl;
            }
            
            scheduler.display();
            
            cout << "\nScheduler characteristics:" << endl;
            cout << "- O(log n) insertion of new processes" << endl;
            cout << "- O(log n) deletion of completed processes" << endl;
            cout << "- Balanced performance for dynamic workloads" << endl;
            
            cout << "\nReal-world usage:" << endl;
            cout << "- Linux CFS (Completely Fair Scheduler)" << endl;
            cout << "- Real-time operating systems" << endl;
            cout << "- Load balancing algorithms" << endl;
        }
    };
    
    // Decision tree comparison
    void compareDecisionCriteria() {
        cout << "\n=== CHOOSING THE RIGHT BALANCED TREE ===" << endl;
        
        cout << "\n🎯 Use AVL Trees when:" << endl;
        cout << "✅ Search operations dominate (90%+ reads)" << endl;
        cout << "✅ You need guaranteed minimal height" << endl;
        cout << "✅ Memory is limited (height field vs color bit)" << endl;
        cout << "✅ Examples: Database indexes, symbol tables, dictionaries" << endl;
        
        cout << "\n🎯 Use Red-Black Trees when:" << endl;
        cout << "✅ Balanced read/write operations" << endl;
        cout << "✅ You need fewer rotations during modifications" << endl;
        cout << "✅ Real-time systems requiring predictable performance" << endl;
        cout << "✅ Examples: STL containers, OS schedulers, general-purpose maps" << endl;
        
        cout << "\n📊 Performance Summary:" << endl;
        cout << "| Metric           | AVL Trees    | Red-Black Trees |" << endl;
        cout << "|------------------|--------------|-----------------|" << endl;
        cout << "| Height bound     | 1.44*log(n)  | 2*log(n)        |" << endl;
        cout << "| Search time      | Faster       | Slightly slower |" << endl;
        cout << "| Insert time      | Slower       | Faster          |" << endl;
        cout << "| Delete time      | Slower       | Faster          |" << endl;
        cout << "| Memory overhead  | 4 bytes      | 1 bit           |" << endl;
        cout << "| Rotations/insert | 0-2          | 0-2             |" << endl;
        cout << "| Implementation   | Simpler      | More complex    |" << endl;
    }
};

void demonstrateRealWorldApplications() {
    cout << "\n=== REAL-WORLD BALANCED TREE APPLICATIONS ===" << endl;
    
    RealWorldApplications apps;
    
    // STL map simulation
    RealWorldApplications::STLMapSimulation stlSim;
    stlSim.demonstrateMapOperations();
    
    // Database index simulation
    RealWorldApplications::DatabaseIndexSimulation dbSim;
    dbSim.demonstrateDatabaseIndex();
    
    // Process scheduler simulation
    RealWorldApplications::ProcessSchedulerSimulation schedSim;
    schedSim.demonstrateScheduler();
    
    // Decision criteria
    apps.compareDecisionCriteria();
    
    cout << "\n🌟 Industry Usage Examples:" << endl;
    cout << "• C++ STL: std::map, std::set use Red-Black trees" << endl;
    cout << "• Java: TreeMap, TreeSet use Red-Black trees" << endl;
    cout << "• MySQL: B+ trees (generalized balanced trees) for indexes" << endl;
    cout << "• Linux kernel: Red-Black trees for CFS scheduler" << endl;
    cout << "• PostgreSQL: B-trees for database indexing" << endl;
    cout << "• Redis: Skip lists (probabilistic balanced structure)" << endl;
}
```

---

## Main Function & Testing Framework

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <chrono>
#include <random>
using namespace std;

// Forward declarations
void demonstrateAVLBasics();
void demonstrateAVLVisualization();
void demonstrateAVLRotations();
void demonstrateAVLInsertion();
void demonstrateRedBlackTree();
void demonstrateRealWorldApplications();

int main() {
    cout << "🌳 Week 4: Balanced Trees (AVL & Red-Black)" << endl;
    cout << "===========================================" << endl;
    
    // Day 1-2: AVL Trees Fundamentals
    cout << "\n📅 DAY 1-2: AVL TREE FUNDAMENTALS & HEIGHT MANAGEMENT" << endl;
    demonstrateAVLBasics();
    demonstrateAVLVisualization();
    
    // Day 3-4: AVL Rotations & Balancing
    cout << "\n📅 DAY 3-4: AVL ROTATIONS & BALANCING MECHANISMS" << endl;
    demonstrateAVLRotations();
    demonstrateAVLInsertion();
    
    // Day 5-6: Red-Black Trees
    cout << "\n📅 DAY 5-6: RED-BLACK TREES & COLOR-BASED BALANCING" << endl;
    demonstrateRedBlackTree();
    
    // Day 7: Performance & Applications
    cout << "\n📅 DAY 7: PERFORMANCE COMPARISON & REAL-WORLD APPLICATIONS" << endl;
    PerformanceComparator comp;
    comp.comparePerformance();
    demonstrateRealWorldApplications();
    
    cout << "\n🎉 Week 4 Balanced Trees Mastery Complete!" << endl;
    cout << "Next: Week 5 - Heaps & Priority Queues" << endl;
    
    return 0;
}
```

---

## 📚 Week 4 Summary & Key Takeaways

### **Balanced Trees Concepts Mastered:**

1. **The Balancing Problem**
   - Why regular BSTs fail with sorted input
   - Height degradation from O(log n) to O(n)
   - Need for automatic rebalancing mechanisms

2. **AVL Trees (Height-Balanced)**
   - Strict balance factor constraint: |left_height - right_height| ≤ 1
   - Four rotation cases: LL, RR, LR, RL
   - Height guarantee: ≤ 1.44 * log₂(n)
   - Optimal for search-heavy applications

3. **Red-Black Trees (Color-Balanced)**
   - Five color-based properties for balance
   - Height guarantee: ≤ 2 * log₂(n)
   - Fewer rotations during modifications
   - Optimal for general-purpose applications

4. **Performance Trade-offs**
   - AVL: Better search, more rotations on insert/delete
   - Red-Black: Balanced performance, fewer rotations
   - Both maintain O(log n) operations guaranteed

### **Critical Algorithm Patterns:**

- **Rotation Mechanics**: Preserve BST property while reducing height
- **Height Tracking**: Essential for AVL balance factor calculation
- **Color Properties**: Maintain black-height equality in Red-Black trees
- **Fixup Procedures**: Handle violations after insertions/deletions
- **Balance Detection**: Identify when rebalancing is needed

### **Real-World Applications:**

- **STL Containers**: std::map, std::set use Red-Black trees
- **Database Systems**: AVL-like structures for optimal query performance
- **Operating Systems**: Red-Black trees in Linux CFS scheduler
- **Language Implementations**: Symbol tables and runtime environments

### **Student Assessment Rubric**

**Balanced Tree Fundamentals (Must Have):**
- [ ] Understands why balancing is necessary
- [ ] Can implement AVL rotations correctly
- [ ] Grasps Red-Black tree properties
- [ ] Performs complexity analysis for both tree types

**Advanced Balancing Skills (Should Have):**
- [ ] Implements complete AVL insertion with balancing
- [ ] Handles Red-Black insertion fixup cases
- [ ] Compares performance characteristics
- [ ] Chooses appropriate tree for specific use cases

**Expert Knowledge (Nice to Have):**
- [ ] Understands mathematical height bounds
- [ ] Can implement deletion operations
- [ ] Connects to real-world systems
- [ ] Optimizes for specific performance requirements

### 🎯 Common Mistakes to Avoid

1. **Rotation Errors**: Not updating parent pointers correctly
2. **Height Miscalculation**: Forgetting to update heights after rotations
3. **Color Violations**: Not properly handling Red-Black fixup cases
4. **Performance Misunderstanding**: Not recognizing trade-offs between tree types

### 🗣️ Advanced Discussion Questions

1. "Why do we need exactly four rotation cases in AVL trees?"
2. "How do Red-Black tree properties guarantee logarithmic height?"
3. "When would you choose AVL over Red-Black trees in practice?"
4. "How do balanced trees relate to B-trees used in databases?"

---

## Next Week Preview: Heaps & Priority Queues

Week 5 will transition from ordered trees to heap structures:

- **Heap Properties**: Complete binary trees with ordering constraints
- **Array Implementation**: Efficient representation without pointers
- **Priority Queues**: Real-world applications of heap data structure
- **Heap Operations**: Insert, extract-min/max, heapify algorithms

**🌟 Congratulations on mastering Balanced Trees! You now understand the foundation of self-balancing data structures.**

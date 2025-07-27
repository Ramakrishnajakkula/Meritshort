# 7-Week Plan to Master Trees in C++

This plan is designed to help students master both the theoretical and coding aspects of trees in C++. Each week includes theory topics, coding practice, and suggested resources.

---

## Week 1: Introduction to Trees
- **Theory:**
  - What is a tree? Terminology (nodes, edges, root, leaf, height, depth, level)
  - Types of trees (binary, n-ary, complete, full, perfect)
  - Applications of trees (file systems, expression trees, decision trees)
  - Tree vs other data structures (arrays, linked lists)
  - Memory representation of trees
- **Coding:**
  - Implement basic tree node structure in C++:
    ```cpp
    struct TreeNode {
        int data;
        TreeNode* left;
        TreeNode* right;
        TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
    };
    ```
  - Create and connect nodes manually
  - Simple tree traversal (preorder) - recursive approach
  - Calculate height and count nodes recursively
- **Practice Problems:**
  - Build a tree from given structure
  - Find maximum element in tree
  - Check if two trees are identical
- **Resources:**
  - GeeksforGeeks: Tree Data Structure
  - Visualgo.net: Tree basics

---

## Week 2: Binary Trees & Traversals
- **Theory:**
  - Binary tree properties and characteristics
  - Tree traversals: preorder, inorder, postorder, level-order
  - How recursion works in tree traversals (call stack visualization)
  - Stack frames and recursive calls visualization
  - Time and space complexity analysis
  - Differences between recursive and iterative approaches
- **Coding:**
  
  **Day 1-2: Recursive Traversals**
  - Implement all three DFS traversals recursively:
    ```cpp
    void preorder(TreeNode* root) {
        if (!root) return;
        cout << root->data << " ";  // Process root
        preorder(root->left);       // Traverse left
        preorder(root->right);      // Traverse right
    }
    ```
  - Visualize call stack for each traversal with examples
  - Draw stack frames step by step
  
  **Day 3-4: Stack Visualization & Understanding**
  - Manually trace recursive calls with stack diagrams
  - Understand when each node gets processed vs visited
  - Practice drawing execution flow for different trees
  
  **Day 5-6: Iterative Traversals**
  - Implement iterative preorder using explicit stack:
    ```cpp
    void iterativePreorder(TreeNode* root) {
        if (!root) return;
        stack<TreeNode*> st;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            cout << node->data << " ";
            if (node->right) st.push(node->right);
            if (node->left) st.push(node->left);
        }
    }
    ```
  - Implement iterative inorder and postorder
  - Compare recursive vs iterative approaches
  
  **Day 7: Level Order Traversal**
  - Implement BFS using queue
  - Level-wise printing
  - Zigzag traversal

- **Practice Problems:**
  - Print all four traversals for given tree
  - Given preorder and inorder, construct tree
  - Find height, count nodes, count leaves
  - Check if tree is symmetric
  - Find diameter of tree
  - Vertical order traversal
- **Resources:**
  - LeetCode: Binary Tree Traversal problems
  - GeeksforGeeks: Iterative Tree Traversals
  - Visualgo.net: Tree traversal animations

---

## Week 3: Binary Search Trees (BST)
- **Theory:**
  - BST properties and invariants (left < root < right)
  - BST vs Binary Tree differences
  - Operations: insert, search, delete with complexity analysis
  - Inorder traversal of BST gives sorted sequence
  - Best, average, and worst-case scenarios
- **Coding:**
  
  **Day 1-2: BST Search & Insert**
  - Implement recursive search:
    ```cpp
    TreeNode* search(TreeNode* root, int key) {
        if (!root || root->data == key) return root;
        if (key < root->data) return search(root->left, key);
        return search(root->right, key);
    }
    ```
  - Implement iterative search
  - Implement recursive and iterative insert
  - Visualize tree changes after each insertion
  
  **Day 3-4: BST Delete Operation**
  - Handle three cases: no child, one child, two children
  - Find inorder predecessor/successor
  - Implement complete delete function
  - Visualize deletion scenarios
  
  **Day 5-6: BST Validation & Properties**
  - Check if a tree is valid BST (multiple approaches)
  - Find kth smallest/largest element
  - Find LCA (Lowest Common Ancestor)
  - Convert sorted array to BST
  
  **Day 7: Advanced BST Operations**
  - Range queries (count nodes in range)
  - Floor and ceiling operations
  - BST iterator implementation

- **Practice Problems:**
  - Validate BST
  - Find kth smallest in BST
  - Convert BST to sorted doubly linked list
  - Merge two BSTs
  - Find pair with given sum in BST
  - Recover BST (two nodes swapped)
- **Resources:**
  - GeeksforGeeks: BST Operations
  - LeetCode: BST problems
  - Visualgo.net: BST animations

---

## Week 4: Balanced Trees (AVL & Red-Black)
- **Theory:**
  - Why balancing is needed (worst-case O(n) operations)
  - Self-balancing trees overview
  - AVL Trees: balance factor, rotations (LL, RR, LR, RL)
  - Red-Black Trees: properties and color rules
  - Comparison between different balanced trees
- **Coding:**
  
  **Day 1-2: AVL Tree Basics**
  - Calculate height and balance factor
  - Implement basic AVL node structure:
    ```cpp
    struct AVLNode {
        int data;
        AVLNode* left;
        AVLNode* right;
        int height;
        AVLNode(int val) : data(val), left(nullptr), right(nullptr), height(1) {}
    };
    ```
  
  **Day 3-4: AVL Rotations**
  - Implement all four rotations with step-by-step visualization:
    - Left rotation (RR case)
    - Right rotation (LL case)
    - Left-Right rotation (LR case)
    - Right-Left rotation (RL case)
  - Draw before/after states for each rotation
  
  **Day 5-6: AVL Insert & Delete**
  - Complete AVL insertion with balancing
  - AVL deletion with rebalancing
  - Trace through examples manually
  
  **Day 7: Red-Black Trees Introduction**
  - Understand color properties
  - Basic insertion (conceptual)

- **Practice Problems:**
  - Check if tree is height balanced
  - Convert unbalanced BST to balanced BST
  - Count rotations needed to balance
  - Build AVL tree from array
- **Resources:**
  - Visualgo.net: AVL Tree Visualization
  - GeeksforGeeks: AVL Trees
  - YouTube: AVL rotation videos

---

## Week 5: Heaps & Priority Queues
- **Theory:**
  - Heap properties (min-heap, max-heap)
  - Complete binary tree structure
  - Array representation of heaps (parent-child relationships)
  - Applications: priority queues, heap sort, graph algorithms
  - Time complexities of heap operations
- **Coding:**
  
  **Day 1-2: Heap Implementation**
  - Array-based heap implementation:
    ```cpp
    class MaxHeap {
        vector<int> heap;
        int parent(int i) { return (i-1)/2; }
        int leftChild(int i) { return 2*i + 1; }
        int rightChild(int i) { return 2*i + 2; }
    };
    ```
  - Visualize array as complete binary tree
  
  **Day 3-4: Heap Operations**
  - Implement insert with heapify-up:
    ```cpp
    void insert(int key) {
        heap.push_back(key);
        heapifyUp(heap.size() - 1);
    }
    ```
  - Implement extract-max with heapify-down
  - Visualize bubbling up and down operations
  
  **Day 5-6: Build Heap & Heap Sort**
  - Build heap from unsorted array (bottom-up)
  - Implement heap sort
  - Compare top-down vs bottom-up heap construction
  
  **Day 7: Priority Queue Applications**
  - STL priority_queue usage
  - Custom comparators
  - Merge k sorted arrays using heap

- **Practice Problems:**
  - Find kth largest/smallest element
  - Median in data stream
  - Top k frequent elements
  - Merge k sorted linked lists
  - Find smallest range covering k lists
- **Resources:**
  - GeeksforGeeks: Heap Data Structure
  - Visualgo.net: Heap animations
  - LeetCode: Heap problems

---

## Week 6: Advanced Tree Topics (Tries & Segment Trees)
- **Theory:**
  - Trie (Prefix Tree): structure, applications (autocomplete, spell checker)
  - Space-time tradeoffs in tries
  - Segment Trees: range queries, lazy propagation
  - Fenwick Tree (Binary Indexed Tree) basics
  - Use cases for different tree types
- **Coding:**
  
  **Day 1-3: Trie Implementation**
  - Basic trie node structure:
    ```cpp
    struct TrieNode {
        TrieNode* children[26];
        bool isEndOfWord;
        TrieNode() {
            isEndOfWord = false;
            for (int i = 0; i < 26; i++)
                children[i] = nullptr;
        }
    };
    ```
  - Implement insert, search, startsWith
  - Visualize trie construction step by step
  - Word break problem using trie
  
  **Day 4-5: Advanced Trie Operations**
  - Delete operation in trie
  - Find all words with given prefix
  - Longest common prefix
  - Auto-complete functionality
  
  **Day 6-7: Segment Tree Basics**
  - Build segment tree for range sum queries
  - Understand tree construction and query process
  - Point updates and range queries
  - Visualize segment tree array representation

- **Practice Problems:**
  - Implement trie with insert/search/delete
  - Word search in 2D board
  - Replace words using trie
  - Design search autocomplete system
  - Range sum queries (segment tree)
- **Resources:**
  - LeetCode: Trie problems
  - GeeksforGeeks: Trie and Segment Trees
  - YouTube: Segment tree tutorials

---

## Week 7: Problem Solving & Comprehensive Projects
- **Theory:**
  - Review and summary of all tree types and their use cases
  - Common tree-related interview patterns
  - Problem-solving strategies for tree problems
  - When to use which tree data structure
  - Real-world applications and system design considerations
- **Coding:**
  
  **Day 1-2: Mixed Tree Problems**
  - Solve 10+ problems covering all tree types:
    - Path sum problems (binary trees)
    - BST validation and operations
    - Heap-based problems
    - Trie applications
  
  **Day 3-4: Advanced Problem Patterns**
  - Tree DP problems (diameter, max path sum)
  - LCA (Lowest Common Ancestor) variations
  - Tree serialization/deserialization
  - Morris traversal (O(1) space)
  
  **Day 5-7: Comprehensive Projects**
  - **Project 1: File System Simulator**
    - Use trie for directory structure
    - Implement ls, mkdir, touch, rm commands
    - Path navigation and file operations
  
  - **Project 2: Expression Evaluator**
    - Build expression tree from infix/postfix
    - Evaluate mathematical expressions
    - Handle operator precedence
  
  - **Project 3: Auto-complete System**
    - Implement search suggestions using trie
    - Rank suggestions by popularity
    - Handle real-time typing

- **Interview Preparation:**
  - 20+ LeetCode tree problems (Easy to Hard)
  - Mock interview sessions
  - Time complexity analysis practice
  - Code optimization techniques
- **Resources:**
  - InterviewBit: Tree Interview Questions
  - LeetCode: Tree problem sets
  - GeeksforGeeks: Tree interview questions
  - System design primer: Tree applications

---

## Additional Resources & Study Tips

**Daily Practice Schedule:**
- 1 hour theory + 2 hours coding daily
- Weekend: 4-5 hours for projects and review
- Use pen and paper for tree drawings and visualizations

**Recommended Tools:**
- Visualgo.net for interactive tree animations
- LeetCode for structured problem practice
- Draw.io or paper for manual tree drawings
- IDE with debugging capabilities

**Assessment Checklist:**
- Week 1: Can implement basic tree operations
- Week 2: Master all traversal methods (recursive + iterative)
- Week 3: Proficient in BST operations and validation
- Week 4: Understand balancing concepts and rotations
- Week 5: Can implement heap from scratch
- Week 6: Comfortable with tries and basic segment trees
- Week 7: Solve complex tree problems independently

**Tips for Success:**
- Always draw trees before coding
- Trace through algorithms step by step
- Practice both recursive and iterative approaches
- Understand time/space complexity for each operation
- Discuss concepts with peers and teach others
- Use online judges for automated testing
- Build projects to apply knowledge practically

**Good luck mastering trees in C++!**

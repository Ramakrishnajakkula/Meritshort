# 7-Week Plan to Master Trees in C++

This comprehensive plan is designed to help students master both the theoretical foundations and practical coding aspects of trees in C++. Each week builds upon previous knowledge with deep theory explanations, visual learning aids, hands-on coding practice, and real-world applications.

## 📚 **Why Trees Matter in Computer Science**

Trees are fundamental data structures that model hierarchical relationships found everywhere in computing:

- **File Systems**: Directories and subdirectories form tree structures
- **Database Indexing**: B-trees and B+ trees enable fast data retrieval
- **Compilers**: Abstract Syntax Trees (ASTs) represent program structure
- **Decision Making**: Decision trees in AI and machine learning
- **Network Routing**: Spanning trees in network protocols
- **HTML/XML**: Document Object Model (DOM) represents web pages

Understanding trees is crucial for any programmer because they provide efficient solutions to problems involving hierarchical data, searching, sorting, and optimization.

---

## Week 1: Tree Fundamentals & Traversals

### 📖 **Theoretical Foundations**

#### **What is a Tree? - Deep Dive**

A tree is a hierarchical data structure consisting of nodes connected by edges, where:

- **No cycles exist** (unlike graphs)
- **Exactly one path** exists between any two nodes
- **Connected structure** with n nodes and n-1 edges

**Real-World Analogies:**

- **Family Tree**: Parents → Children relationships
- **Company Hierarchy**: CEO → Managers → Employees
- **Book Structure**: Book → Chapters → Sections → Paragraphs

#### **Essential Tree Terminology**

```
        A (Root)           Level 0
       / \
      B   C               Level 1
     / \   \
    D   E   F             Level 2
   /
  G                       Level 3
```

- **Root**: Top node (A) - has no parent
- **Parent**: Node with children (B is parent of D and E)
- **Child**: Node with a parent (D and E are children of B)
- **Leaf**: Node with no children (G, E, F)
- **Internal Node**: Node with at least one child (A, B, C, D)
- **Siblings**: Nodes with same parent (B and C are siblings)
- **Ancestor**: Any node on path from root to current node
- **Descendant**: Any node in subtree rooted at current node
- **Height**: Longest path from node to leaf (Height of tree = 3)
- **Depth**: Distance from root to node (Depth of G = 3)
- **Level**: All nodes at same depth form a level

#### **Types of Trees - Classification and Properties**

**1. Binary Trees:**

- Each node has **at most 2 children** (left and right)
- **Applications**: Expression trees, binary search trees, heaps

**2. Complete Binary Tree:**

- All levels filled except possibly the last
- Last level filled from **left to right**
- **Property**: Can be efficiently stored in arrays
- **Applications**: Heaps, priority queues

**3. Full Binary Tree:**

- Every internal node has **exactly 2 children**
- Every leaf is at the same level
- **Property**: Number of leaves = Number of internal nodes + 1

**4. Perfect Binary Tree:**

- **Both complete AND full**
- All internal nodes have 2 children
- All leaves at same level
- **Property**: Has exactly 2^h - 1 nodes for height h

**5. N-ary Trees:**

- Each node can have **up to N children**
- **Applications**: File systems (unlimited children), tries

#### **Memory Representation Strategies**

**1. Pointer-Based (Dynamic):**

```cpp
struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};
```

- **Advantages**: Dynamic size, easy insertion/deletion
- **Disadvantages**: Extra memory for pointers, cache misses

**2. Array-Based (Static):**

```cpp
// For complete binary trees
// Parent at index i, children at 2i+1 and 2i+2
vector<int> tree;
```

- **Advantages**: Cache-friendly, no pointer overhead
- **Disadvantages**: Wastes space for incomplete trees

#### **Tree Traversal Theory - Why Different Orders Matter**

Tree traversals determine the **order of visiting nodes**. Each traversal serves specific purposes:

**1. Preorder (Root → Left → Right):**

- **Use Cases**:
  - Creating a copy of the tree
  - Prefix expression evaluation
  - Serializing tree structure
- **Memory Pattern**: Process parent before children (top-down)

**2. Inorder (Left → Root → Right):**

- **Use Cases**:
  - Getting sorted sequence from BST
  - Expression tree evaluation (infix)
  - Binary tree to sorted array conversion
- **Memory Pattern**: Process left subtree, then root, then right

**3. Postorder (Left → Right → Root):**

- **Use Cases**:
  - Deleting/freeing tree nodes safely
  - Calculating directory sizes
  - Postfix expression evaluation
- **Memory Pattern**: Process children before parent (bottom-up)

**4. Level-order (Breadth-First):**

- **Use Cases**:
  - Level-wise processing
  - Finding shortest path in unweighted trees
  - Printing tree level by level
- **Memory Pattern**: Process all nodes at current level before next level

#### **Recursion vs Iteration - Deep Analysis**

**Recursive Approach:**

```cpp
void preorder(TreeNode* root) {
    if (root == nullptr) return;  // Base case
    cout << root->data << " ";    // Process
    preorder(root->left);         // Recursive case
    preorder(root->right);        // Recursive case
}
```

**How Recursion Works:**

- **Call Stack**: Each function call creates a stack frame
- **Base Case**: Prevents infinite recursion
- **Recursive Case**: Function calls itself with smaller problem
- **Backtracking**: Returns to previous calls when base case reached

**Iterative Approach:**

```cpp
void preorderIterative(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> stk;
    stk.push(root);

    while (!stk.empty()) {
        TreeNode* current = stk.top();
        stk.pop();
        cout << current->data << " ";

        if (current->right) stk.push(current->right);
        if (current->left) stk.push(current->left);
    }
}
```

**Comparison:**
| Aspect | Recursive | Iterative |
|--------|-----------|-----------|
| **Code Simplicity** | Cleaner, more intuitive | More complex logic |
| **Memory Usage** | System call stack | Explicit stack/queue |
| **Stack Overflow Risk** | Yes (deep trees) | No (controlled memory) |
| **Debugging** | Harder to trace | Easier to debug |
| **Performance** | Function call overhead | Faster execution |

#### **Time and Space Complexity Analysis**

**For all traversals:**

- **Time Complexity**: O(n) - must visit each node exactly once
- **Space Complexity**:
  - Recursive: O(h) where h = height (call stack)
  - Iterative: O(h) for DFS, O(w) for BFS where w = maximum width

**Best vs Worst Case:**

- **Balanced Tree**: Height = O(log n), Space = O(log n)
- **Skewed Tree**: Height = O(n), Space = O(n)

**Understanding Big O in Trees:**

- **O(log n)**: Balanced trees (binary search operations)
- **O(n)**: Linear operations (traversals, searching in worst case)
- **O(h)**: Height-dependent operations (insertion, deletion)

#### **Real-World Applications of Week 1 Concepts**

**Where Tree Fundamentals Are Used:**

**1. File Systems (Hierarchical Structure):**

```
C:\
├── Program Files\
│   ├── Microsoft Office\
│   │   ├── Word\
│   │   └── Excel\
│   └── Adobe\
├── Users\
│   ├── Documents\
│   └── Downloads\
└── Windows\
```

- **Tree Type**: N-ary tree (directories can have unlimited children)
- **Operations**: Navigation (search), file creation (insert), deletion
- **Traversals**: DFS for finding files, BFS for directory listing by level
- **Real Impact**: `find` command uses tree traversal to locate files

**2. Database Indexing (B-trees and B+ trees):**

- **Concept Used**: Complete binary tree principles extended to multi-way trees
- **Application**: MySQL, PostgreSQL indexes for fast data retrieval
- **Why Trees**: O(log n) search time vs O(n) for linear search
- **Real Impact**: Query that takes 1 hour on unsorted data takes 1 second with tree index
- **Example**: Finding customer record among 1 million entries in 20 steps vs 500,000 steps

**3. Compiler Design (Abstract Syntax Trees):**

```cpp
// Code: a = b + c * d
// AST representation:
        =
       / \
      a   +
         / \
        b   *
           / \
          c   d
```

- **Traversals Used**:
  - **Inorder**: Generates infix expression (b + c \* d)
  - **Postorder**: Generates assembly code (evaluation order)
  - **Preorder**: Code generation and optimization
- **Real Examples**: GCC, Clang, Visual Studio compilers

**4. Decision Making Systems (Decision Trees):**

```
Should I approve this loan?
├── Credit Score > 700?
│   ├── Yes → Check Income
│   │   ├── Income > $50k → Approve
│   │   └── Income ≤ $50k → Manual Review
│   └── No → Reject
```

- **Applications**: Machine learning, medical diagnosis, business rules
- **Traversals**: Path from root to leaf represents decision sequence
- **Tree Properties**: Height determines decision complexity
- **Real Examples**: Credit approval, fraud detection, medical expert systems

**5. Web Browsers (DOM Trees):**

```html
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <div>
      <p>Hello World</p>
    </div>
  </body>
</html>
```

- **Tree Operations**: Element selection, style application, event handling
- **Traversals**: CSS selectors use tree traversal to find elements
- **Memory Management**: Dynamic creation/deletion of DOM nodes
- **Real Examples**: Chrome DevTools, React Virtual DOM, jQuery selectors

**Where Each Traversal Method Is Used:**

**Preorder Traversal Applications:**

1. **File System Backups**: Copy directory structure before contents
   - **Example**: `tar` command creates archive by visiting directories first
2. **Expression Tree Evaluation**: Process operator before operands (prefix notation)
   - **Example**: Lisp programming language uses prefix notation
3. **Tree Serialization**: Save tree structure for later reconstruction
   - **Example**: JSON serialization, database tree storage
4. **Memory Management**: Allocate parent nodes before children
   - **Example**: Creating GUI widget hierarchies

**Inorder Traversal Applications:**

1. **Binary Search Trees**: Get sorted data in O(n) time
   - **Example**: Dictionary word list, sorted database records
2. **Expression Trees**: Generate infix expressions (a + b \* c)
   - **Example**: Mathematical calculators, formula editors
3. **Threaded Binary Trees**: Create sorted linked list from tree
   - **Example**: In-memory database cursors
4. **Database Range Queries**: Retrieve sorted records between two values
   - **Example**: "Find all customers with age between 25 and 35"

**Postorder Traversal Applications:**

1. **Memory Cleanup**: Delete children before parent (avoid dangling pointers)
   - **Example**: Garbage collection in programming languages
2. **Directory Size Calculation**: Calculate folder sizes bottom-up
   - **Example**: `du` command in Unix/Linux systems
3. **Expression Evaluation**: Process operands before operator (postfix notation)
   - **Example**: Stack-based calculators, PostScript language
4. **Dependency Resolution**: Install dependencies before main package
   - **Example**: npm install, Maven dependencies, Linux package managers

**Level-Order Traversal Applications:**

1. **Social Networks**: Friends of friends suggestions (BFS)
   - **Example**: Facebook friend recommendations, LinkedIn connections
2. **Network Broadcasting**: Spread message level by level
   - **Example**: Network flooding protocols, peer-to-peer systems
3. **Game AI**: Breadth-first search for shortest path
   - **Example**: Pathfinding in video games, GPS navigation
4. **Organization Charts**: Print employees by hierarchy level
   - **Example**: Corporate reporting structures, military chain of command

**Recursion vs Iteration Trade-offs in Practice:**

**When to Use Recursion:**

- **Compiler Design**: Natural for parsing nested structures (parentheses, brackets)
- **File System Operations**: Directory traversal feels intuitive and clean
- **Mathematical Computations**: Factorial, Fibonacci, tree algorithms
- **Quick Prototyping**: Faster development for tree algorithms
- **Example**: Unix `find` command implementation uses recursion for simplicity

**When to Use Iteration:**

- **Production Systems**: Avoid stack overflow on deep trees (file systems with deep nesting)
- **Memory-Constrained Systems**: Embedded systems, mobile apps
- **Performance-Critical Code**: Avoid function call overhead in game engines
- **Debugging Complex Issues**: Easier to trace and debug in production environments
- **Example**: Linux kernel uses iterative approaches for file system operations

**Complete vs Incomplete Trees in Real Systems:**

**Complete Trees (Heaps, Priority Queues):**

- **Operating System Schedulers**: Process priority management (Windows, Linux)
- **Network Packet Scheduling**: Quality of Service (QoS) in routers
- **Game Development**: AI decision priority systems, event queues
- **Database Query Optimization**: Cost-based query planning
- **Example**: Linux CFS (Completely Fair Scheduler) uses heap-like structures

**Incomplete Trees (General Binary Trees):**

- **Decision Trees**: Business rule engines, expert systems
- **Parse Trees**: Language processing, JSON/XML parsing
- **Binary Search Trees**: Database indexes, symbol tables in compilers
- **Expression Trees**: Calculator apps, formula evaluation in spreadsheets
- **Example**: MySQL uses B-trees (extension of binary trees) for indexing

**Memory Representation Choices in Industry:**

**Pointer-Based Trees:**

- **Databases**: Dynamic insertion/deletion of records (MongoDB, PostgreSQL)
- **Compilers**: Abstract syntax trees with varying node types (GCC, LLVM)
- **Operating Systems**: Process trees, file system inodes (Linux, Windows)
- **Game Engines**: Scene graphs with complex hierarchies (Unity, Unreal)
- **Advantage**: Flexible structure, easy insertion/deletion
- **Disadvantage**: Memory overhead, potential cache misses

**Array-Based Trees:**

- **Priority Queues**: Operating system task scheduling, real-time systems
- **Complete Binary Trees**: Heap sort implementations, embedded systems
- **Embedded Systems**: Memory-efficient storage in IoT devices
- **Cache-Friendly Applications**: High-performance computing, trading systems
- **Advantage**: Cache-friendly, no pointer overhead
- **Disadvantage**: Fixed size, harder to modify structure

### 🎯 **Learning Objectives - What You'll Master**

By the end of Week 1, you will:

1. **Conceptual Understanding**: Explain tree properties and classify tree types with real-world examples
2. **Implementation Skills**: Build trees and implement all traversal methods for practical applications
3. **Problem-Solving**: Choose appropriate traversal for specific problems based on use case
4. **Memory Management**: Understand pointer manipulation and choose optimal representation strategy
5. **Complexity Analysis**: Calculate time/space complexity and understand performance implications
6. **Debugging Skills**: Trace through recursive and iterative algorithms in production scenarios
7. **Visualization**: Draw trees and trace algorithm execution for system design and debugging
8. **Real-World Connection**: Identify tree concepts in everyday software systems and applications

- **Coding:**

  **Day 1-2: Basic Tree Structure & All 4 Traversals**

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
  - Implement all four traversals:
    - Preorder (Root → Left → Right)
    - Inorder (Left → Root → Right)
    - Postorder (Left → Right → Root)
    - Level-order (BFS using queue)
  - Both recursive and iterative implementations
  - Visualize call stack for recursive traversals
  - Stack/queue visualization for iterative approaches

  **Day 3-4: Basic Tree Operations**

  - Basic tree operations: height, count nodes, count leaves
  - Find maximum/minimum element in tree
  - Check if two trees are identical
  - Build tree from array representation
  - Search for elements using different traversals

  **Day 5-6: Stack & Queue Visualization Deep Dive**

  - Manually trace recursive calls with stack diagrams
  - Understand when each node gets processed vs visited
  - Compare recursive vs iterative approaches for all traversals
  - Draw execution flow for different tree structures
  - Practice drawing stack/queue contents at each step

  **Day 7: Advanced Traversal Patterns**

  - Level-wise printing (each level on separate line)
  - Understanding LIFO (stack) vs FIFO (queue) behavior
  - When to use which traversal method
  - Traversal applications and use cases

- **Practice Problems:**
  - Build a tree from given structure
  - Implement all four traversals (recursive and iterative)
  - Calculate tree properties (height, node count, leaf count)
  - Print traversals for given trees and verify outputs
  - Search for elements using different traversal methods
  - Compare traversal results and understand their applications
- **Resources:**
  - GeeksforGeeks: Tree Data Structure & Traversals
  - Visualgo.net: Tree basics and traversal animations
  - LeetCode: Basic Binary Tree problems

---

## Week 2: Tree Problem Solving & Applications

### 📖 **Theoretical Foundations**

#### **Problem-Solving Strategies for Trees**

**1. Pattern Recognition Framework:**

- **Construction Problems**: Building trees from given data
- **Validation Problems**: Checking tree properties
- **Path Problems**: Finding routes through trees
- **Transformation Problems**: Converting tree structure/values
- **Query Problems**: Searching and retrieving information

**2. Algorithm Design Patterns:**

**Top-Down Approach (Divide and Conquer):**

- Start from root and work towards leaves
- **Examples**: Tree validation, path finding
- **Pattern**: `if (root) { process(root); recurse(left); recurse(right); }`

**Bottom-Up Approach (Dynamic Programming):**

- Process children first, then parent
- **Examples**: Tree height, diameter calculation
- **Pattern**: `int left = recurse(left); int right = recurse(right); return combine(left, right);`

**Backtracking Pattern:**

- Explore path, make decision, backtrack if needed
- **Examples**: All path problems, tree construction
- **Pattern**: `add_to_path(); recurse(); remove_from_path();`

#### **Tree Problem Classifications - Deep Analysis**

**1. Construction Problems:**

- **Input Types**: Traversal sequences, arrays, strings
- **Key Insight**: Use traversal properties to rebuild structure
- **Example**: Preorder + Inorder → Unique Binary Tree
- **Why it works**: Preorder gives root, Inorder splits left/right subtrees

**2. Path Problems:**

- **Root-to-Leaf Paths**: Start at root, end at leaf
- **Any-to-Any Paths**: Can start and end at any nodes
- **Path Sum Problems**: Find paths with specific sum values
- **Longest Path Problems**: Diameter, maximum depth

**3. Validation Problems:**

- **Structure Validation**: Check if tree follows specific rules
- **Property Validation**: Verify mathematical properties
- **Example**: BST validation requires left < root < right for all nodes

**4. Transformation Problems:**

- **Structure Changes**: Flatten to list, mirror tree
- **Value Changes**: Update node values based on position/neighbors
- **Format Changes**: Serialize to string, convert to array

#### **When to Use Which Traversal - Decision Matrix**

| Problem Type              | Best Traversal | Reason                         |
| ------------------------- | -------------- | ------------------------------ |
| **Copy Tree**             | Preorder       | Need root before children      |
| **Delete Tree**           | Postorder      | Children before parent         |
| **BST to Sorted Array**   | Inorder        | Natural sorted order           |
| **Level-wise Processing** | Level-order    | Process by levels              |
| **Expression Evaluation** | Postorder      | Operands before operator       |
| **Tree Serialization**    | Preorder       | Root-first reconstruction      |
| **Find Tree Height**      | Any traversal  | Must visit all nodes           |
| **Path Sum Problems**     | Any DFS        | Need complete path exploration |

#### **Optimization Techniques - Advanced Concepts**

**1. Memoization in Trees:**

- **Problem**: Repeated subproblems in tree DP
- **Solution**: Cache results based on subtree characteristics
- **Example**: Fibonacci tree, optimal BST construction

**2. Morris Traversal (Advanced):**

- **Problem**: O(n) space for recursion/stack
- **Solution**: Use tree structure itself as stack
- **Technique**: Temporarily modify tree, then restore
- **Result**: O(1) space traversal

**3. Iterative Optimization:**

- **When to Use**: Deep trees causing stack overflow
- **Trade-off**: More complex code for better space control
- **Benefit**: Predictable memory usage

**4. Early Termination:**

- **Technique**: Stop traversal when answer found
- **Examples**: Search operations, validation checks
- **Benefit**: Better average-case performance

#### **Space-Time Complexity Trade-offs**

**Memory vs Speed Decisions:**

- **Recursive**: Clean code, but O(h) space for call stack
- **Iterative with Stack**: More control, same O(h) space
- **Morris**: O(1) space, but more complex logic
- **Level-order**: O(w) space where w = maximum width

**When to Choose What:**

- **Deep Trees**: Iterative to avoid stack overflow
- **Wide Trees**: DFS to minimize memory usage
- **Simple Problems**: Recursive for code clarity
- **Memory-Critical**: Morris traversal for O(1) space

### 🎯 **Learning Objectives - Problem-Solving Mastery**

By the end of Week 2, you will:

1. **Pattern Recognition**: Identify problem types and choose optimal approaches
2. **Algorithm Design**: Apply divide-and-conquer, DP, and backtracking patterns
3. **Optimization**: Understand space-time trade-offs and choose efficiently
4. **Complex Problem Solving**: Solve multi-step problems combining multiple concepts
5. **Code Quality**: Write clean, efficient, and maintainable tree algorithms
6. **Edge Case Handling**: Identify and handle boundary conditions systematically
7. **Performance Analysis**: Optimize algorithms for better complexity

- **Coding:**

  **Day 1-2: Tree Construction Problems**

  - Build tree from different traversal combinations:
    - Preorder + Inorder → Unique tree
    - Postorder + Inorder → Unique tree
    - Level-order + additional constraints
  - Build balanced BST from sorted array
  - Serialize and deserialize binary trees
  - Convert between tree representations

  **Day 3-4: Path & Sum Problems**

  - Root-to-leaf path problems:
    - Path sum equals target
    - All paths with given sum
    - Path with maximum sum
  - Any path problems:
    - Maximum path sum between any two nodes
    - Count paths with given sum
  - Binary tree to linked list conversions

  **Day 5-6: Tree Properties & Validation**

  - Check tree properties:
    - Symmetric/mirror trees
    - Balanced trees (height difference ≤ 1)
    - Complete vs full trees
  - Validate binary search trees
  - Find diameter of tree (longest path)
  - Lowest Common Ancestor (LCA) problems

  **Day 7: Advanced Traversal Applications**

  - Zigzag level order traversal
  - Vertical order traversal
  - Boundary traversal (left boundary + leaves + right boundary)
  - Morris traversal (O(1) space)
  - Custom traversal requirements

- **Practice Problems:**
  - Build tree from preorder and inorder arrays
  - Check if tree is symmetric
  - Find diameter of binary tree
  - Binary tree maximum path sum
  - Path sum problems (multiple variations)
  - Flatten binary tree to linked list
  - Lowest common ancestor
  - Validate binary search tree
  - Serialize/deserialize binary tree
  - Convert binary tree to doubly linked list
  - Zigzag level order traversal
  - Vertical order traversal
  - Binary tree right side view
  - Count complete tree nodes
  - Recover binary search tree
- **Resources:**
  - LeetCode: Binary Tree problem set (Easy to Hard)
  - GeeksforGeeks: Tree problem patterns
  - InterviewBit: Tree interview questions

---

## Week 3: Binary Search Trees (BST)

### 📖 **Theoretical Foundations**

#### **BST Properties and Invariants - Mathematical Foundation**

**Core BST Property:**
For every node N in a Binary Search Tree:

- **All nodes in left subtree** have values **< N.data**
- **All nodes in right subtree** have values **> N.data**
- **This property holds recursively** for all subtrees

**Why This Property Matters:**

```
Example BST:        NOT a BST:
      8                  8
     / \                / \
    3   10             3   10
   / \    \           / \    \
  1   6   14         1   6   14
     / \                / \
    4   7              4   12  ← VIOLATION: 12 > 8 but in left subtree
```

**Mathematical Implications:**

- **Inorder traversal** of BST yields **sorted sequence**
- **Search complexity**: O(h) where h = height
- **Best case**: O(log n) for balanced trees
- **Worst case**: O(n) for skewed trees

#### **BST vs Regular Binary Tree - Key Differences**

| Aspect        | Binary Tree                      | Binary Search Tree                 |
| ------------- | -------------------------------- | ---------------------------------- |
| **Structure** | Any arrangement                  | Must satisfy BST property          |
| **Search**    | O(n) - must check all nodes      | O(h) - follow path based on values |
| **Insert**    | O(1) at any position             | O(h) - must find correct position  |
| **Inorder**   | Random sequence                  | Always sorted sequence             |
| **Use Cases** | Expression trees, decision trees | Searching, sorting, indexing       |

#### **BST Operations - Detailed Analysis**

**1. Search Operation:**

```cpp
// Recursive approach
TreeNode* search(TreeNode* root, int key) {
    // Base cases
    if (!root || root->data == key) return root;

    // Recursive cases - use BST property
    if (key < root->data) return search(root->left, key);
    return search(root->right, key);
}
```

**Why Search is Efficient:**

- **Eliminates half** the search space at each step
- **Guided by BST property** - no need to explore both subtrees
- **Similar to binary search** in sorted arrays

**2. Insert Operation:**

```cpp
TreeNode* insert(TreeNode* root, int key) {
    // Base case: create new node
    if (!root) return new TreeNode(key);

    // Recursive insertion maintaining BST property
    if (key < root->data) root->left = insert(root->left, key);
    else if (key > root->data) root->right = insert(root->right, key);

    return root;  // Return unchanged root
}
```

**Insert Strategy:**

- **Find the correct position** where BST property is maintained
- **Always insert at leaf level** (unless tree is empty)
- **No restructuring needed** (unlike balanced trees)

**3. Delete Operation - The Complex One:**

**Three Cases to Handle:**

**Case 1: Node with No Children (Leaf)**

```cpp
// Simply remove the node
delete nodeToDelete;
return nullptr;
```

**Case 2: Node with One Child**

```cpp
// Replace node with its child
TreeNode* child = (node->left) ? node->left : node->right;
delete node;
return child;
```

**Case 3: Node with Two Children (Most Complex)**

```cpp
// Find inorder successor (or predecessor)
// Replace node's data with successor's data
// Delete the successor (which has at most one child)
```

**Why Case 3 is Complex:**

- **Cannot simply remove** - would break tree structure
- **Must maintain BST property** after deletion
- **Inorder successor** is the next larger value (leftmost in right subtree)
- **Inorder predecessor** is the next smaller value (rightmost in left subtree)

#### **Best, Average, and Worst Case Scenarios**

**Balanced BST (Best Case):**

```
      8
     / \
    4   12
   / \ / \
  2 6 10 14
```

- **Height**: O(log n)
- **All operations**: O(log n)
- **Shape**: Roughly balanced

**Skewed BST (Worst Case):**

```
1
 \
  2
   \
    3
     \
      4
       \
        5
```

- **Height**: O(n)
- **All operations**: O(n)
- **Shape**: Essentially a linked list

**Why BSTs Can Become Skewed:**

- **Sorted input**: Inserting already sorted data creates linked list
- **No self-balancing**: Standard BST doesn't reorganize itself
- **Solution**: Self-balancing trees (AVL, Red-Black) covered in Week 4

#### **BST Applications in Real World**

**1. Database Indexing:**

- **B-trees and B+ trees** are generalizations of BSTs
- **Enable fast lookups** in large datasets
- **Maintain sorted order** for range queries

**2. File Systems:**

- **Directory structures** often use tree-like organization
- **Fast file lookup** by name
- **Hierarchical organization**

**3. Expression Parsing:**

- **Operator precedence** determines tree structure
- **Inorder traversal** gives infix expression
- **Evaluation** using postorder traversal

**4. Priority Systems:**

- **Task scheduling** based on priority values
- **Event simulation** with timestamp-based ordering
- **Resource allocation** with priority queues

#### **BST Invariant Maintenance**

**What Can Break BST Property:**

- **Incorrect insertion** without checking values
- **Node modifications** that violate ordering
- **Subtree rotations** without updating values
- **Manual tree construction** without validation

**How to Ensure BST Validity:**

- **Always use BST operations** for modifications
- **Validate after construction** from external data
- **Use bounds checking** during traversal
- **Test with edge cases** (duplicates, empty trees, single nodes)

### 🎯 **Learning Objectives - BST Mastery**

By the end of Week 3, you will:

1. **Conceptual Understanding**: Explain BST properties and their implications
2. **Implementation Skills**: Code all BST operations efficiently
3. **Problem Solving**: Apply BST properties to solve search problems
4. **Complexity Analysis**: Understand why BSTs are efficient for searching
5. **Validation Skills**: Check if a tree satisfies BST properties
6. **Optimization Awareness**: Recognize when BSTs become inefficient
7. **Real-world Application**: Connect BST concepts to practical systems

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

### 📖 **Theoretical Foundations**

#### **The Balancing Problem - Why Standard BSTs Fail**

**The Fundamental Issue:**
Standard BSTs can **degenerate into linked lists** when data is inserted in sorted order:

```cpp
// Inserting 1, 2, 3, 4, 5 in order creates:
1
 \
  2      Height = O(n)
   \     Search = O(n)
    3    ← Worst case scenario!
     \
      4
       \
        5
```

**Performance Degradation:**

- **Expected**: O(log n) operations
- **Reality**: O(n) operations (same as linked list)
- **Solution**: Self-balancing trees that maintain O(log n) height

#### **Self-Balancing Trees Overview**

**Key Concept**: Automatically reorganize tree structure during insertions/deletions to maintain **logarithmic height**.

**Common Self-Balancing Trees:**

1. **AVL Trees**: Height-balanced with strict balance factor
2. **Red-Black Trees**: Color-based balancing with relaxed constraints
3. **Splay Trees**: Self-adjusting based on access patterns
4. **B-Trees**: Multi-way trees for disk storage

#### **AVL Trees - Height-Balanced Trees**

**AVL Property (Balance Factor):**
For every node N: **|height(left) - height(right)| ≤ 1**

```cpp
struct AVLNode {
    int data;
    AVLNode* left;
    AVLNode* right;
    int height;  // Height of subtree rooted at this node

    AVLNode(int val) : data(val), left(nullptr), right(nullptr), height(1) {}
};

// Balance factor calculation
int getBalanceFactor(AVLNode* node) {
    if (!node) return 0;
    return height(node->left) - height(node->right);
}
```

**Why AVL Works:**

- **Height guarantee**: Maximum height = 1.44 \* log₂(n)
- **Proof**: Fibonacci tree analysis shows worst case
- **All operations remain O(log n)**

#### **AVL Rotations - The Balancing Mechanism**

**Four Types of Imbalances:**

**1. Left-Left (LL) Case:**

```
      z                 y
     /                 / \
    y         →       x   z
   /                     /
  x                     T3
```

**Solution**: Right rotation around z

**2. Right-Right (RR) Case:**

```
  z                     y
   \                   / \
    y        →        z   x
     \               /
      x             T1
```

**Solution**: Left rotation around z

**3. Left-Right (LR) Case:**

```
    z               z               x
   /               /               / \
  y       →       x       →       y   z
   \             /
    x           y
```

**Solution**: Left rotation around y, then right rotation around z

**4. Right-Left (RL) Case:**

```
  z                 z                 x
   \                 \               / \
    y       →         x       →     z   y
   /                   \
  x                     y
```

**Solution**: Right rotation around y, then left rotation around z

**Rotation Implementation:**

```cpp
AVLNode* rotateRight(AVLNode* y) {
    AVLNode* x = y->left;
    AVLNode* T2 = x->right;

    // Perform rotation
    x->right = y;
    y->left = T2;

    // Update heights
    y->height = max(height(y->left), height(y->right)) + 1;
    x->height = max(height(x->left), height(x->right)) + 1;

    return x;  // New root
}
```

**Why Rotations Work:**

- **Preserve BST property**: Inorder traversal remains same
- **Reduce height**: Bring subtrees closer to balance
- **Local operation**: Only affects 3 nodes
- **Constant time**: O(1) per rotation

#### **Red-Black Trees - Color-Based Balancing**

**Red-Black Properties:**

1. **Every node** is either RED or BLACK
2. **Root** is always BLACK
3. **All leaves (NIL)** are BLACK
4. **Red node children** must be BLACK (no two red nodes adjacent)
5. **All paths** from any node to leaves contain same number of BLACK nodes

**Why These Rules Work:**

- **Property 5** ensures no path is more than twice as long as any other
- **Maximum height**: 2 \* log₂(n + 1)
- **Looser than AVL**: Fewer rotations needed
- **Good performance**: Slightly worse than AVL but faster insertions/deletions

**Visual Example:**

```
      B(30)           ← Black root
     /     \
  R(20)   B(40)       ← Red node has black children
   /         \
B(10)      R(50)      ← All paths have same black count
```

#### **Comparison: AVL vs Red-Black Trees**

| Aspect            | AVL Trees               | Red-Black Trees     |
| ----------------- | ----------------------- | ------------------- |
| **Balance**       | Strictly balanced       | Loosely balanced    |
| **Height**        | 1.44 \* log₂(n)         | 2 \* log₂(n)        |
| **Search**        | Faster (shorter)        | Slightly slower     |
| **Insert/Delete** | More rotations          | Fewer rotations     |
| **Memory**        | Height field            | Color bit           |
| **Use Cases**     | Read-heavy applications | Balanced read/write |

**When to Use Which:**

- **AVL**: Database indexes, lookup-intensive applications
- **Red-Black**: STL map/set, kernel schedulers, general-purpose

#### **Self-Balancing: The Mathematical Guarantee**

**Height Bounds:**

- **Unbalanced BST**: O(n) worst case
- **AVL Tree**: O(log n) guaranteed
- **Red-Black Tree**: O(log n) guaranteed

**Rotation Cost:**

- **Number of rotations**: At most O(log n) per operation
- **Time per rotation**: O(1)
- **Total overhead**: O(log n) for balancing

**Amortized Analysis:**

- **Individual operations**: May require multiple rotations
- **Sequence of operations**: Average rotation count is small
- **Overall performance**: Maintains O(log n) average

#### **Real-World Applications**

**1. Standard Template Library (STL):**

- **std::map, std::set**: Usually Red-Black trees
- **std::multimap, std::multiset**: Also Red-Black trees
- **Reason**: Good balance between search and modification performance

**2. Database Systems:**

- **MySQL InnoDB**: B+ trees (extension of balanced trees)
- **PostgreSQL**: B-trees for indexing
- **Reason**: Disk I/O optimization with guaranteed logarithmic access

**3. Operating Systems:**

- **Linux CFS Scheduler**: Red-Black trees for process scheduling
- **Virtual memory management**: AVL trees for memory mapping
- **Reason**: Real-time guarantees for system operations

**4. Java Collections:**

- **TreeMap, TreeSet**: Red-Black tree implementation
- **Reason**: Predictable performance for enterprise applications

### 🎯 **Learning Objectives - Balanced Tree Mastery**

By the end of Week 4, you will:

1. **Problem Recognition**: Understand when and why balancing is necessary
2. **AVL Mastery**: Implement height-balanced trees with rotations
3. **Red-Black Understanding**: Grasp color-based balancing principles
4. **Rotation Mechanics**: Perform all four rotation types correctly
5. **Performance Analysis**: Compare different balancing strategies
6. **Design Decisions**: Choose appropriate balanced tree for specific use cases
7. **Implementation Skills**: Debug balancing algorithms and handle edge cases

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

### 📖 **Theoretical Foundations**

#### **Heap Properties - The Ordering Constraint**

**Definition**: A heap is a **complete binary tree** that satisfies the **heap property**.

**Two Types of Heaps:**

**1. Max-Heap Property:**

- **Parent ≥ Children** for every node
- **Root contains maximum** element
- **No ordering** between siblings

**2. Min-Heap Property:**

- **Parent ≤ Children** for every node
- **Root contains minimum** element
- **Useful for priority queues** with smallest priority first

```
Max-Heap Example:        Min-Heap Example:
      100                     1
     /   \                   / \
   19     36                4   3
  / \    / \               / \ / \
 17  3  25  1             9 8 5  7
```

**Key Insight**: Heaps are **NOT sorted trees**. They only guarantee parent-child relationship, not global ordering.

#### **Complete Binary Tree Structure - Why It Matters**

**Complete Binary Tree Properties:**

- **All levels filled** except possibly the last
- **Last level filled left-to-right**
- **Height = ⌊log₂(n)⌋** where n = number of nodes

**Why Complete Structure is Crucial:**

1. **Array representation**: Enables efficient storage without pointers
2. **Predictable height**: Guarantees O(log n) operations
3. **Cache locality**: Adjacent elements in array are often accessed together
4. **No memory waste**: Unlike perfect binary trees, no gaps in array

#### **Array Representation - The Mathematical Magic**

**Index Relationships (0-based indexing):**

```cpp
// For node at index i:
int parent(int i) { return (i - 1) / 2; }
int leftChild(int i) { return 2 * i + 1; }
int rightChild(int i) { return 2 * i + 2; }
```

**Visual Example:**

```
Heap:        Array: [100, 19, 36, 17, 3, 25, 1]
   100       Index:   0   1   2   3  4   5   6
  /   \
 19    36     Parent of index 5 (25): (5-1)/2 = 2 (36)
/ \   / \     Children of index 1 (19): 2*1+1=3 (17), 2*1+2=4 (3)
17 3 25  1
```

**Advantages of Array Representation:**

- **No pointers needed**: Saves memory (no left/right pointers)
- **Cache friendly**: Sequential memory access patterns
- **Simple navigation**: Mathematical formulas for parent/children
- **Space efficient**: No wasted space for incomplete trees

#### **Heap Operations - Detailed Analysis**

**1. Heapify Operations:**

**Heapify-Up (Bubble-Up):**

- **Used in**: Insert operation
- **Direction**: Child to parent (bottom-up)
- **Condition**: Stop when heap property satisfied or reach root

```cpp
void heapifyUp(int index) {
    while (index > 0) {
        int parentIndex = parent(index);
        if (heap[index] <= heap[parentIndex]) break;  // Max-heap satisfied

        swap(heap[index], heap[parentIndex]);
        index = parentIndex;
    }
}
```

**Heapify-Down (Bubble-Down):**

- **Used in**: Extract operation
- **Direction**: Parent to children (top-down)
- **Condition**: Stop when heap property satisfied or reach leaf

```cpp
void heapifyDown(int index) {
    int maxIndex = index;
    int left = leftChild(index);
    int right = rightChild(index);

    // Find largest among parent and children
    if (left < heap.size() && heap[left] > heap[maxIndex])
        maxIndex = left;
    if (right < heap.size() && heap[right] > heap[maxIndex])
        maxIndex = right;

    if (maxIndex != index) {
        swap(heap[index], heap[maxIndex]);
        heapifyDown(maxIndex);  // Recursively fix subtree
    }
}
```

**2. Core Operations Complexity:**

| Operation                 | Time Complexity | Explanation                                 |
| ------------------------- | --------------- | ------------------------------------------- |
| **Insert**                | O(log n)        | Heapify-up worst case travels full height   |
| **Extract Max/Min**       | O(log n)        | Heapify-down worst case travels full height |
| **Peek Max/Min**          | O(1)            | Root element access                         |
| **Build Heap**            | O(n)            | Bottom-up heapification                     |
| **Increase/Decrease Key** | O(log n)        | May require heapify-up or down              |

#### **Build Heap - The Linear Time Algorithm**

**Two Approaches:**

**1. Top-Down (Naive):**

```cpp
// Insert elements one by one
for (int i = 0; i < n; i++) {
    insert(arr[i]);  // O(log i) for each insert
}
// Total: O(n log n)
```

**2. Bottom-Up (Optimal):**

```cpp
// Start from last non-leaf and heapify down
for (int i = n/2 - 1; i >= 0; i--) {
    heapifyDown(i);
}
// Total: O(n) - Mathematical proof using geometric series
```

**Why Bottom-Up is Linear:**

- **Leaves don't need heapification**: Half the nodes are leaves
- **Height decreases**: Nodes at height h are at most n/2^(h+1)
- **Work per level**: Nodes × Height gives geometric series
- **Mathematical proof**: ∑(h=0 to log n) (n/2^(h+1)) × h = O(n)

#### **Priority Queues - Real-World Applications**

**Priority Queue Abstract Data Type:**

- **Operations**: insert(element, priority), extractMax/Min(), peek()
- **Implementation**: Heaps provide optimal solution
- **Applications**: Everywhere priorities matter

**1. Operating System Scheduling:**

```cpp
struct Process {
    int pid;
    int priority;
    int cpuTime;
};

// CPU scheduler uses min-heap (lower number = higher priority)
priority_queue<Process, vector<Process>, ProcessComparator> scheduler;
```

**2. Dijkstra's Shortest Path Algorithm:**

```cpp
// Need minimum distance vertex each iteration
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
```

**3. Huffman Coding:**

```cpp
// Build optimal encoding tree using character frequencies
priority_queue<Node*, vector<Node*>, NodeComparator> minHeap;
```

**4. Event Simulation:**

```cpp
struct Event {
    double time;
    string type;
};
// Process events in chronological order
priority_queue<Event, vector<Event>, EventComparator> eventQueue;
```

#### **Heap Variants and Extensions**

**1. Binary Heap vs D-ary Heap:**

- **Binary Heap**: Each node has ≤ 2 children
- **D-ary Heap**: Each node has ≤ d children
- **Trade-off**: Lower height vs more comparisons per level

**2. Fibonacci Heap:**

- **Advanced structure**: Better amortized complexity
- **Applications**: Graph algorithms (Dijkstra, Prim)
- **Complexity**: Decrease-key in O(1) amortized

**3. Binomial Heap:**

- **Mergeable heap**: Efficient merge operations
- **Structure**: Collection of binomial trees
- **Applications**: When frequent merging needed

#### **Common Heap Applications in Programming**

**1. Top K Problems:**

```cpp
// Find K largest elements using min-heap of size K
priority_queue<int, vector<int>, greater<int>> minHeap;
```

**2. Median Finding:**

```cpp
// Use two heaps: max-heap for lower half, min-heap for upper half
priority_queue<int> maxHeap;  // Lower half
priority_queue<int, vector<int>, greater<int>> minHeap;  // Upper half
```

**3. Merge K Sorted Arrays:**

```cpp
// Use min-heap to efficiently merge multiple sorted sequences
priority_queue<Node, vector<Node>, NodeComparator> pq;
```

**4. Sliding Window Maximum:**

```cpp
// Use deque or heap to find maximum in sliding window
priority_queue<pair<int, int>> maxHeap;  // (value, index)
```

### 🎯 **Learning Objectives - Heap Mastery**

By the end of Week 5, you will:

1. **Conceptual Understanding**: Grasp heap properties and complete binary tree structure
2. **Implementation Skills**: Build heaps from scratch using arrays
3. **Algorithm Analysis**: Understand why heapify operations are O(log n)
4. **Optimization**: Implement linear-time heap construction
5. **Application Recognition**: Identify when heaps solve problems optimally
6. **STL Proficiency**: Use priority_queue effectively with custom comparators
7. **Problem Solving**: Apply heaps to various algorithmic challenges

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

### 📖 **Theoretical Foundations**

#### **Trie (Prefix Tree) - String Processing Powerhouse**

**Definition**: A trie is a **tree-like data structure** that stores a **dynamic set of strings** where the keys are usually strings.

**Core Properties:**

- **Each node represents a prefix** of stored strings
- **Path from root to node** spells out a prefix
- **Common prefixes share paths** (space efficiency)
- **Leaves (or marked nodes) represent complete words**

**Trie Structure Visualization:**

```
Storing words: ["cat", "cats", "car", "card", "care", "dog"]

         root
        /    \
      c        d
     /          \
    a            o
   / \            \
  t   r            g
 /   / \            \
[cat] c  e           [dog]
     /    \
   [card] [care]
   /
 [cats]
```

**Why Tries Excel at String Operations:**

1. **Prefix sharing**: Common prefixes stored once
2. **Fast lookups**: O(m) where m = string length
3. **Prefix operations**: Natural support for "starts with" queries
4. **Ordered iteration**: Lexicographic order traversal
5. **Memory efficiency**: Shared prefixes reduce space

#### **Trie Applications - Where They Shine**

**1. Autocomplete Systems:**

- **Search suggestions**: Type "ca" → get ["cat", "car", "card", "care"]
- **Implementation**: DFS from prefix node to find all completions
- **Real examples**: Google search, IDE code completion

**2. Spell Checkers:**

- **Word validation**: Check if word exists in dictionary
- **Fuzzy matching**: Find similar words with edit distance
- **Suggestion generation**: Find words with common prefixes

**3. IP Routing Tables:**

- **Longest prefix matching**: Route packets based on IP prefixes
- **Efficiency**: O(32) lookup for IPv4 addresses
- **Hardware implementation**: Fast routing decisions

**4. Game Word Finding:**

- **Scrabble/Boggle solvers**: Find all valid words in grid
- **Dictionary traversal**: Efficiently explore possible words
- **Pruning**: Stop early if prefix not in dictionary

#### **Trie Implementation - Memory and Time Trade-offs**

**Basic Trie Node Structure:**

```cpp
struct TrieNode {
    TrieNode* children[26];  // For lowercase letters a-z
    bool isEndOfWord;

    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++)
            children[i] = nullptr;
    }
};
```

**Memory Analysis:**

- **Per node overhead**: 26 pointers + 1 boolean = ~208 bytes (64-bit)
- **Sparse nodes**: Most children[] slots are nullptr
- **Memory explosion**: Large alphabet or sparse data causes waste

**Optimized Implementations:**

**1. Compressed Trie (Patricia Tree):**

- **Idea**: Compress chains of single-child nodes
- **Space saving**: Reduces nodes for sparse tries
- **Trade-off**: More complex implementation

**2. HashMap-based Children:**

```cpp
struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord;
};
```

- **Space efficient**: Only store existing children
- **Time overhead**: HashMap lookup vs array access
- **Better for**: Large alphabets or sparse data

#### **Trie Operations - Complexity Analysis**

| Operation         | Time Complexity | Space Complexity     | Explanation                                        |
| ----------------- | --------------- | -------------------- | -------------------------------------------------- |
| **Insert**        | O(m)            | O(m × ALPHABET_SIZE) | m = string length, worst case creates m new nodes  |
| **Search**        | O(m)            | O(1)                 | Follow path of length m                            |
| **Delete**        | O(m)            | O(1)                 | May need to remove unused nodes                    |
| **Prefix Search** | O(p + k)        | O(1)                 | p = prefix length, k = number of words with prefix |
| **All Words**     | O(TOTAL_CHARS)  | O(1)                 | DFS traversal of entire trie                       |

**Why Trie Operations are Efficient:**

- **Independent of dataset size**: Time depends only on string length
- **Comparison**: Hash table has O(m) for hashing + collision resolution
- **Advantage**: Natural support for prefix operations

#### **Segment Trees - Range Query Specialists**

**Problem Statement**: Given an array, efficiently answer **range queries** and support **point updates**.

**Examples of Range Queries:**

- **Range Sum**: Sum of elements from index L to R
- **Range Minimum**: Minimum element in range [L, R]
- **Range Maximum**: Maximum element in range [L, R]
- **Range GCD**: GCD of all elements in range [L, R]

**Naive vs Optimal Approaches:**

| Approach         | Range Query | Point Update | Space |
| ---------------- | ----------- | ------------ | ----- |
| **Array**        | O(n)        | O(1)         | O(n)  |
| **Prefix Sum**   | O(1)        | O(n)         | O(n)  |
| **Segment Tree** | O(log n)    | O(log n)     | O(n)  |

#### **Segment Tree Structure - Binary Tree on Ranges**

**Key Concepts:**

- **Each node represents a range** [L, R] of array indices
- **Leaf nodes represent single elements** (ranges of size 1)
- **Internal nodes represent merged ranges** from their children
- **Root represents entire array** [0, n-1]

**Structure Visualization for array [1, 3, 5, 7, 9, 11]:**

```
                 [0,5] sum=36
                /            \
          [0,2] sum=9      [3,5] sum=27
           /      \         /        \
     [0,1] sum=4  [2] 5   [3,4] sum=16  [5] 11
      /    \              /       \
   [0] 1  [1] 3        [3] 7    [4] 9
```

**Array Representation:**

- **Size**: 4n (to handle all possible nodes)
- **Index mapping**: Left child = 2i, Right child = 2i+1
- **Parent mapping**: Parent = i/2

#### **Segment Tree Operations**

**1. Build Operation - Bottom-Up Construction:**

```cpp
void build(vector<int>& arr, int treeIndex, int start, int end) {
    if (start == end) {
        tree[treeIndex] = arr[start];  // Leaf node
    } else {
        int mid = (start + end) / 2;
        build(arr, 2*treeIndex, start, mid);      // Build left subtree
        build(arr, 2*treeIndex+1, mid+1, end);   // Build right subtree

        // Combine results from children
        tree[treeIndex] = tree[2*treeIndex] + tree[2*treeIndex+1];
    }
}
```

**2. Range Query - Divide and Conquer:**

```cpp
int query(int treeIndex, int start, int end, int L, int R) {
    // Complete overlap: current range is completely within query range
    if (start >= L && end <= R) return tree[treeIndex];

    // No overlap: current range is completely outside query range
    if (end < L || start > R) return 0;  // Identity for sum

    // Partial overlap: split and recursively query both children
    int mid = (start + end) / 2;
    int leftSum = query(2*treeIndex, start, mid, L, R);
    int rightSum = query(2*treeIndex+1, mid+1, end, L, R);

    return leftSum + rightSum;
}
```

**3. Point Update - Propagate Changes Upward:**

```cpp
void update(int treeIndex, int start, int end, int idx, int newVal) {
    if (start == end) {
        tree[treeIndex] = newVal;  // Update leaf
    } else {
        int mid = (start + end) / 2;
        if (idx <= mid) {
            update(2*treeIndex, start, mid, idx, newVal);     // Update left
        } else {
            update(2*treeIndex+1, mid+1, end, idx, newVal);  // Update right
        }

        // Update current node based on children
        tree[treeIndex] = tree[2*treeIndex] + tree[2*treeIndex+1];
    }
}
```

#### **Advanced Segment Tree Concepts**

**1. Lazy Propagation:**

- **Problem**: Range updates are O(n) with basic segment tree
- **Solution**: Defer updates using "lazy" flags
- **Benefit**: Range updates become O(log n)
- **Applications**: Range addition, range multiplication

**2. Persistent Segment Trees:**

- **Problem**: Need to query historical versions of array
- **Solution**: Create new tree versions instead of modifying
- **Applications**: Version control, temporal queries

**3. 2D Segment Trees:**

- **Problem**: Range queries on 2D matrices
- **Solution**: Nested segment trees (tree of trees)
- **Complexity**: O(log²n) per operation

#### **Fenwick Tree (Binary Indexed Tree) - Elegant Alternative**

**Simpler Alternative for Sum Queries:**

- **More space efficient**: Only O(n) space needed
- **Easier implementation**: Bit manipulation tricks
- **Limitation**: Only works for invertible operations (sum, XOR)
- **Use case**: When simplicity preferred over generality

**Comparison: Segment Tree vs Fenwick Tree:**

| Aspect             | Segment Tree                   | Fenwick Tree               |
| ------------------ | ------------------------------ | -------------------------- |
| **Space**          | O(4n)                          | O(n)                       |
| **Implementation** | Complex                        | Simple                     |
| **Operations**     | Any associative operation      | Only invertible operations |
| **Range Updates**  | Supports with lazy propagation | More complex               |
| **Learning Curve** | Steeper                        | Gentler                    |

#### **When to Use Advanced Trees**

**Decision Matrix:**

| Problem Type                 | Best Data Structure                | Reason                             |
| ---------------------------- | ---------------------------------- | ---------------------------------- |
| **String prefix operations** | Trie                               | Natural prefix handling            |
| **Range sum queries**        | Fenwick Tree                       | Simpler implementation             |
| **Range min/max queries**    | Segment Tree                       | Supports any associative operation |
| **2D range queries**         | 2D Segment Tree                    | Generalizes to higher dimensions   |
| **Frequent range updates**   | Segment Tree with Lazy Propagation | Efficient range modifications      |
| **String dictionary**        | Trie                               | Autocomplete, spell check          |

### 🎯 **Learning Objectives - Advanced Tree Mastery**

By the end of Week 6, you will:

1. **Trie Mastery**: Implement efficient string processing data structures
2. **Segment Tree Understanding**: Build range query systems from scratch
3. **Application Recognition**: Identify when advanced trees solve problems optimally
4. **Trade-off Analysis**: Compare different approaches for string and range problems
5. **Implementation Skills**: Code complex tree structures with proper memory management
6. **Optimization Awareness**: Understand space-time trade-offs in tree design
7. **Real-world Connection**: Apply advanced trees to practical systems and algorithms

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

### 📖 **Theoretical Foundations**

#### **Comprehensive Tree Knowledge Integration**

**Tree Type Decision Matrix:**
After 6 weeks of study, you now have a powerful toolkit. Here's when to use each:

| Problem Characteristics           | Best Tree Type        | Key Reason                             |
| --------------------------------- | --------------------- | -------------------------------------- |
| **Simple hierarchical data**      | Binary Tree           | Basic structure, flexible operations   |
| **Need sorted data access**       | Binary Search Tree    | O(log n) search with inorder traversal |
| **Frequent insertions/deletions** | AVL or Red-Black Tree | Guaranteed O(log n) performance        |
| **Priority-based processing**     | Heap                  | Efficient min/max extraction           |
| **String prefix operations**      | Trie                  | Natural prefix sharing and search      |
| **Range queries on arrays**       | Segment Tree          | O(log n) range operations              |
| **Simple range sums**             | Fenwick Tree          | Elegant implementation                 |

#### **Advanced Problem-Solving Patterns**

**Pattern 1: Tree Dynamic Programming**

- **Concept**: Solve subproblems on subtrees and combine results
- **Examples**: Tree diameter, maximum path sum, subtree sizes
- **Template**:

```cpp
int treeDp(TreeNode* root) {
    if (!root) return baseCase;

    int leftResult = treeDp(root->left);
    int rightResult = treeDp(root->right);

    // Update global answer if needed
    globalAnswer = max(globalAnswer, combine(leftResult, rightResult, root->val));

    // Return what this subtree contributes to parent
    return contributeToParent(leftResult, rightResult, root->val);
}
```

**Pattern 2: Tree Serialization/Deserialization**

- **Concept**: Convert tree to/from linear representation
- **Applications**: Persistence, network transmission, debugging
- **Key insight**: Preorder with null markers preserves structure

**Pattern 3: Lowest Common Ancestor (LCA)**

- **Concept**: Find deepest common ancestor of two nodes
- **Variations**: Binary lifting, sparse table, offline LCA
- **Applications**: Distance queries, path analysis

**Pattern 4: Tree Isomorphism**

- **Concept**: Determine if two trees have same structure
- **Applications**: Pattern matching, duplicate detection
- **Techniques**: Canonical hashing, recursive comparison

#### **Interview Problem Patterns - Industry Focus**

**Google-Style Problems:**

- **Complex tree construction**: Build from multiple traversals
- **Optimization challenges**: Morris traversal, threading
- **Scale considerations**: Memory-efficient implementations

**Amazon-Style Problems:**

- **System design integration**: Trees in distributed systems
- **Real-world applications**: Filesystem simulation, decision trees
- **Performance analysis**: Big-O in practice, not just theory

**Microsoft-Style Problems:**

- **Algorithm correctness**: Edge case handling, invariant maintenance
- **Code quality**: Clean, maintainable tree implementations
- **Testing strategies**: Unit testing tree algorithms

**Facebook-Style Problems:**

- **Data structure design**: Custom tree variants for specific needs
- **Scalability**: Handling massive datasets with trees
- **User experience**: Real-time features using tree algorithms

#### **System Design with Trees - Real-World Architecture**

**1. Database Index Design:**

```
B+ Tree Structure for Database Index:
- Internal nodes: Only keys for navigation
- Leaf nodes: Key-value pairs with data
- Linked leaves: Range scan efficiency
- High fanout: Minimize disk I/O
```

**2. File System Implementation:**

```
Directory Tree Structure:
- Inodes: File metadata and tree structure
- Directory entries: Name to inode mapping
- Path resolution: Tree traversal for file lookup
- Hard links: Multiple paths to same inode
```

**3. Load Balancer Decision Trees:**

```
Request Routing Tree:
- Nodes: Routing decisions based on criteria
- Leaves: Backend server assignments
- Updates: Dynamic tree modification for scaling
- Optimization: Minimize decision path length
```

#### **Advanced Optimization Techniques**

**1. Cache-Oblivious Tree Algorithms:**

- **Problem**: Memory hierarchy affects performance
- **Solution**: Algorithms that work well across cache levels
- **Example**: Van Emde Boas tree layout

**2. Parallel Tree Algorithms:**

- **Challenge**: Trees are inherently sequential
- **Solutions**: Work-stealing, parallel prefix operations
- **Applications**: MapReduce tree processing

**3. External Memory Trees:**

- **Problem**: Trees too large for main memory
- **Solution**: B-trees optimized for disk I/O
- **Key insight**: Minimize expensive disk operations

**4. Probabilistic Trees:**

- **Examples**: Skip lists, treaps, randomized BSTs
- **Advantage**: Good expected performance without complex balancing
- **Trade-off**: Probabilistic vs deterministic guarantees

#### **Testing and Debugging Strategies**

**1. Tree Invariant Checking:**

```cpp
bool validateBST(TreeNode* root, long minVal, long maxVal) {
    if (!root) return true;
    if (root->val <= minVal || root->val >= maxVal) return false;
    return validateBST(root->left, minVal, root->val) &&
           validateBST(root->right, root->val, maxVal);
}
```

**2. Visualization Techniques:**

- **ASCII art printing**: Debug tree structure visually
- **Graphviz integration**: Generate professional tree diagrams
- **Step-by-step tracing**: Manual execution with print statements

**3. Stress Testing:**

- **Random tree generation**: Test with various tree shapes
- **Edge case enumeration**: Empty trees, single nodes, skewed trees
- **Performance profiling**: Identify bottlenecks in tree operations

**4. Property-Based Testing:**

- **Invariant maintenance**: Check BST property after each operation
- **Structural integrity**: Verify parent-child relationships
- **Functional correctness**: Compare with reference implementations

#### **Performance Engineering for Trees**

**1. Memory Layout Optimization:**

```cpp
// Cache-friendly node layout
struct OptimizedNode {
    int value;
    int height;           // Keep frequently accessed data together
    OptimizedNode* left;
    OptimizedNode* right;
    // Minimize padding and align to cache lines
};
```

**2. Branch Prediction Optimization:**

- **Minimize unpredictable branches**: Use arithmetic when possible
- **Profile-guided optimization**: Let compiler optimize for common paths
- **Branchless algorithms**: Use arithmetic instead of conditionals

**3. SIMD Utilization:**

- **Vectorized operations**: Process multiple tree elements simultaneously
- **Applications**: Parallel comparison in B-trees
- **Challenges**: Tree operations are often data-dependent

#### **Tree Algorithms in Modern Computing**

**1. Machine Learning Applications:**

- **Decision Trees**: Classification and regression
- **Random Forests**: Ensemble of decision trees
- **Gradient Boosting**: Sequential tree improvement
- **Tree-based feature selection**: Information gain calculations

**2. Blockchain and Cryptography:**

- **Merkle Trees**: Data integrity verification
- **Patricia Trees**: Ethereum state storage
- **Hash trees**: Efficient proof systems
- **Applications**: Cryptocurrency, distributed databases

**3. Compiler Design:**

- **Abstract Syntax Trees**: Program representation
- **Parse trees**: Syntax analysis
- **Expression trees**: Optimization and code generation
- **Call graphs**: Program analysis and optimization

**4. Computer Graphics:**

- **Octrees**: 3D space partitioning
- **BSP Trees**: Visibility determination
- **Scene graphs**: Hierarchical scene representation
- **Spatial indexing**: Collision detection and rendering

### 🎯 **Learning Objectives - Expert-Level Mastery**

By the end of Week 7, you will:

1. **Integration Skills**: Combine multiple tree concepts to solve complex problems
2. **Design Expertise**: Choose optimal tree structures for real-world systems
3. **Performance Engineering**: Optimize tree algorithms for production environments
4. **Problem Pattern Recognition**: Identify and apply tree-based solutions efficiently
5. **Code Quality**: Write maintainable, testable, and robust tree implementations
6. **System Thinking**: Understand how trees fit into larger software architectures
7. **Interview Readiness**: Confidently tackle any tree-related technical interview
8. **Research Foundation**: Ready to explore advanced topics like concurrent trees and distributed tree algorithms

**Final Assessment Criteria:**

- **Conceptual Mastery**: Explain any tree concept clearly to others
- **Implementation Fluency**: Code any tree algorithm without reference
- **Problem-Solving Speed**: Quickly identify optimal tree-based approaches
- **Optimization Mindset**: Consider performance implications of design choices
- **Real-World Application**: Connect tree algorithms to practical systems
- **Teaching Ability**: Help others understand tree concepts and debugging
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

- Week 1: Can implement basic tree operations and all traversal methods with deep understanding of recursive vs iterative approaches
- Week 2: Can solve complex tree problems using traversals and tree properties, recognizing problem patterns and applying appropriate algorithms
- Week 3: Proficient in BST operations and validation, understanding the mathematical properties that make BSTs efficient
- Week 4: Understand balancing concepts and rotations, can implement AVL trees and explain why self-balancing is crucial
- Week 5: Can implement heap from scratch, understand array representation, and apply heaps to priority queue problems
- Week 6: Comfortable with tries and basic segment trees, can choose appropriate advanced data structures for specific problems
- Week 7: Solve complex tree problems independently, integrate multiple concepts, and design tree-based systems

**Tips for Success:**

- **Visualization First**: Always draw trees before coding - understand the problem visually
- **Trace Algorithms**: Step through algorithms manually to build intuition before implementing
- **Master Both Approaches**: Practice both recursive and iterative solutions to understand trade-offs
- **Complexity Analysis**: Always analyze time and space complexity - understand why operations have certain costs
- **Peer Learning**: Discuss concepts with peers and teach others - teaching reveals gaps in understanding
- **Use Online Judges**: Practice on LeetCode, HackerRank, and similar platforms for automated testing and diverse problems
- **Build Projects**: Apply knowledge practically through file system simulators, expression evaluators, and autocomplete systems
- **Debug Systematically**: Learn to use debuggers effectively with tree structures and validate invariants
- **Connect to Real World**: Understand how trees are used in databases, operating systems, compilers, and web applications
- **Progressive Learning**: Each week builds on previous knowledge - ensure solid foundation before advancing
- **Code Quality**: Focus on writing clean, maintainable code with proper error handling and edge case management
- **Pattern Recognition**: Identify common problem patterns and standard solution templates
- **Performance Mindset**: Consider practical performance implications, not just theoretical complexity
- **Test Thoroughly**: Develop comprehensive test cases including edge cases and stress tests

**Advanced Study Path After Completion:**

- **Concurrent Trees**: Lock-free data structures and parallel algorithms
- **Distributed Trees**: Consistent hashing, distributed B-trees, and sharded tree structures
- **Persistent Trees**: Functional data structures and immutable trees
- **Specialized Trees**: Suffix trees, heavy-light decomposition, and link-cut trees
- **Research Areas**: Cache-oblivious algorithms, external memory data structures, and approximation algorithms

**Good luck mastering trees in C++! With this comprehensive foundation, you'll be prepared for advanced computer science topics and real-world software engineering challenges.**

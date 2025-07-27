# Week 7: Problem Solving & Comprehensive Projects - Implementation Guide

## 📋 **Table of Contents**

### **Top-Level Navigation**
1. [Week Overview](#-week-overview)
2. [Module 1: Mixed Tree Problems](#-module-1-mixed-tree-problems-day-1-2)
3. [Module 2: Advanced Problem Patterns](#-module-2-advanced-problem-patterns-day-3-4)
4. [Module 3: Comprehensive Projects](#-module-3-comprehensive-projects-day-5-7)
5. [Module 4: Assessment & Next Steps](#-module-4-assessment--next-steps)

### **Module-Level Navigation**

#### **Module 1: Mixed Tree Problems**
- [Binary Tree Integration Problems](#binary-tree-integration-problems)
- [Heap and Trie Problems](#heap-and-trie-problems)
- [Module 1 Summary](#module-1-summary)

#### **Module 2: Advanced Problem Patterns**
- [Tree Dynamic Programming](#tree-dynamic-programming)
- [LCA and Serialization](#lca-and-serialization)
- [Module 2 Summary](#module-2-summary)

#### **Module 3: Comprehensive Projects**
- [Project 1: File System Simulator](#project-1-file-system-simulator)
- [Project 2: Expression Evaluator](#project-2-expression-evaluator)
- [Project 3: Auto-complete System](#project-3-auto-complete-system)
- [Module 3 Summary](#module-3-summary)

#### **Module 4: Assessment & Next Steps**
- [Self-Assessment Checklist](#self-assessment-checklist)
- [Interview Readiness](#interview-readiness)
- [Advanced Topics](#advanced-topics-for-future-learning)
- [Final Conclusion](#conclusion)

---

## 🎯 **Week Overview**

Week 7 is the culmination of your 7-week trees journey. You'll integrate all previous knowledge to solve complex problems and build real-world projects that demonstrate mastery of tree concepts.

**Learning Outcomes:**
- Master complex tree problem-solving patterns
- Build production-ready tree-based systems
- Integrate multiple tree concepts seamlessly
- Achieve interview readiness for any tree-related question

---

## � **Module 1: Mixed Tree Problems (Day 1-2)**

### Binary Tree Integration Problems

This section covers fundamental tree problems that combine multiple concepts learned in previous weeks.

#### Problem 1: Binary Tree Maximum Path Sum (LeetCode 124)

```cpp
#include <iostream>
#include <algorithm>
#include <climits>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
private:
    int maxSum;

    int maxPathSumHelper(TreeNode* root) {
        if (!root) return 0;

        // Get max path sum from left and right subtrees
        // Use max(0, ...) to ignore negative paths
        int leftSum = max(0, maxPathSumHelper(root->left));
        int rightSum = max(0, maxPathSumHelper(root->right));

        // Update global maximum considering path through current node
        maxSum = max(maxSum, root->val + leftSum + rightSum);

        // Return max path sum ending at current node (for parent)
        return root->val + max(leftSum, rightSum);
    }

public:
    int maxPathSum(TreeNode* root) {
        maxSum = INT_MIN;
        maxPathSumHelper(root);
        return maxSum;
    }
};

// Test function
void testMaxPathSum() {
    /*
    Tree structure:
         1
        / \
       2   3
    Expected: 6 (2->1->3)
    */
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);

    Solution sol;
    cout << "Maximum Path Sum: " << sol.maxPathSum(root) << endl;
}
```

#### Problem 2: Validate Binary Search Tree (LeetCode 98)

```cpp
class BSTValidator {
public:
    bool isValidBST(TreeNode* root) {
        return validate(root, LONG_MIN, LONG_MAX);
    }

private:
    bool validate(TreeNode* root, long minVal, long maxVal) {
        if (!root) return true;

        // Check BST property
        if (root->val <= minVal || root->val >= maxVal) {
            return false;
        }

        // Recursively validate left and right subtrees with updated bounds
        return validate(root->left, minVal, root->val) &&
               validate(root->right, root->val, maxVal);
    }
};

// Alternative approach using inorder traversal
class BSTValidatorInorder {
private:
    TreeNode* prev = nullptr;

public:
    bool isValidBST(TreeNode* root) {
        return inorderCheck(root);
    }

private:
    bool inorderCheck(TreeNode* root) {
        if (!root) return true;

        // Check left subtree
        if (!inorderCheck(root->left)) return false;

        // Check current node
        if (prev && prev->val >= root->val) return false;
        prev = root;

        // Check right subtree
        return inorderCheck(root->right);
    }
};
```

#### Problem 3: Path Sum III (LeetCode 437)

```cpp
class PathSumSolution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        if (!root) return 0;

        // Paths starting from current node + paths in subtrees
        return pathSumFrom(root, targetSum) +
               pathSum(root->left, targetSum) +
               pathSum(root->right, targetSum);
    }

private:
    int pathSumFrom(TreeNode* root, long targetSum) {
        if (!root) return 0;

        int count = 0;
        if (root->val == targetSum) count = 1;

        count += pathSumFrom(root->left, targetSum - root->val);
        count += pathSumFrom(root->right, targetSum - root->val);

        return count;
    }
};

// Optimized version using prefix sum
#include <unordered_map>

class PathSumOptimized {
public:
    int pathSum(TreeNode* root, int targetSum) {
        unordered_map<long, int> prefixSum;
        prefixSum[0] = 1; // Base case
        return dfs(root, 0, targetSum, prefixSum);
    }

private:
    int dfs(TreeNode* root, long currentSum, int targetSum,
            unordered_map<long, int>& prefixSum) {
        if (!root) return 0;

        currentSum += root->val;
        int count = prefixSum[currentSum - targetSum];

        prefixSum[currentSum]++;
        count += dfs(root->left, currentSum, targetSum, prefixSum);
        count += dfs(root->right, currentSum, targetSum, prefixSum);
        prefixSum[currentSum]--; // Backtrack

        return count;
    }
};
```

### Heap and Trie Problems

This section demonstrates the practical application of heaps and tries in complex algorithmic scenarios.

#### Problem 4: Merge k Sorted Lists using Heap (LeetCode 23)

```cpp
#include <queue>
#include <vector>

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class MergeKSortedLists {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        // Custom comparator for min-heap
        auto compare = [](ListNode* a, ListNode* b) {
            return a->val > b->val;
        };

        priority_queue<ListNode*, vector<ListNode*>, decltype(compare)> pq(compare);

        // Add first node of each list to heap
        for (ListNode* list : lists) {
            if (list) pq.push(list);
        }

        ListNode dummy(0);
        ListNode* current = &dummy;

        while (!pq.empty()) {
            ListNode* smallest = pq.top();
            pq.pop();

            current->next = smallest;
            current = current->next;

            if (smallest->next) {
                pq.push(smallest->next);
            }
        }

        return dummy.next;
    }
};
```

#### Problem 5: Word Search II using Trie (LeetCode 212)

```cpp
class TrieNode {
public:
    TrieNode* children[26];
    string word;

    TrieNode() {
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
        word = "";
    }
};

class WordSearchII {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        TrieNode* root = buildTrie(words);
        vector<string> result;

        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                dfs(board, i, j, root, result);
            }
        }

        return result;
    }

private:
    TrieNode* buildTrie(vector<string>& words) {
        TrieNode* root = new TrieNode();

        for (string& word : words) {
            TrieNode* current = root;
            for (char c : word) {
                int index = c - 'a';
                if (!current->children[index]) {
                    current->children[index] = new TrieNode();
                }
                current = current->children[index];
            }
            current->word = word;
        }

        return root;
    }

    void dfs(vector<vector<char>>& board, int i, int j,
             TrieNode* node, vector<string>& result) {
        char c = board[i][j];
        if (c == '#' || !node->children[c - 'a']) return;

        node = node->children[c - 'a'];
        if (!node->word.empty()) {
            result.push_back(node->word);
            node->word = ""; // Avoid duplicates
        }

        board[i][j] = '#'; // Mark as visited

        // Explore all 4 directions
        if (i > 0) dfs(board, i - 1, j, node, result);
        if (j > 0) dfs(board, i, j - 1, node, result);
        if (i < board.size() - 1) dfs(board, i + 1, j, node, result);
        if (j < board[0].size() - 1) dfs(board, i, j + 1, node, result);

        board[i][j] = c; // Restore
    }
};
```

### Module 1 Summary

**Key Concepts Mastered:**
- Tree Dynamic Programming patterns
- BST validation techniques
- Path sum variations and optimizations
- Heap-based sorting algorithms
- Trie-based string search algorithms

**Skills Developed:**
- Integrating multiple tree concepts in one solution
- Optimizing tree algorithms using mathematical insights
- Applying different tree structures to appropriate problems

[↑ Back to Top](#-table-of-contents)

---

## 🧠 **Module 2: Advanced Problem Patterns (Day 3-4)**

### Tree Dynamic Programming

Advanced tree DP patterns that form the foundation of complex tree algorithms.

### Day 3: Tree Dynamic Programming

#### Problem 6: Binary Tree Diameter (LeetCode 543)

```cpp
class DiameterSolution {
private:
    int maxDiameter;

    int height(TreeNode* root) {
        if (!root) return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        // Update diameter: longest path through current node
        maxDiameter = max(maxDiameter, leftHeight + rightHeight);

        // Return height of current node
        return 1 + max(leftHeight, rightHeight);
    }

public:
    int diameterOfBinaryTree(TreeNode* root) {
        maxDiameter = 0;
        height(root);
        return maxDiameter;
    }
};
```

#### Problem 7: House Robber III (LeetCode 337)

```cpp
class HouseRobberIII {
public:
    int rob(TreeNode* root) {
        auto result = robHelper(root);
        return max(result.first, result.second);
    }

private:
    // Returns pair: (max money if rob current, max money if not rob current)
    pair<int, int> robHelper(TreeNode* root) {
        if (!root) return {0, 0};

        auto left = robHelper(root->left);
        auto right = robHelper(root->right);

        // If rob current node, can't rob children
        int robCurrent = root->val + left.second + right.second;

        // If not rob current, take max from children
        int notRobCurrent = max(left.first, left.second) +
                           max(right.first, right.second);

        return {robCurrent, notRobCurrent};
    }
};
```

### LCA and Serialization

Essential tree algorithms for interview preparation and real-world applications.

### Day 4: LCA and Serialization

#### Problem 8: Lowest Common Ancestor of Binary Tree (LeetCode 236)

```cpp
class LCASolution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;

        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (left && right) return root; // Found both in different subtrees
        return left ? left : right;     // Return the non-null one
    }
};

// LCA for BST (optimized)
class LCAForBST {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return nullptr;

        // Both nodes are in left subtree
        if (p->val < root->val && q->val < root->val) {
            return lowestCommonAncestor(root->left, p, q);
        }

        // Both nodes are in right subtree
        if (p->val > root->val && q->val > root->val) {
            return lowestCommonAncestor(root->right, p, q);
        }

        // One node is on left, one on right (or one is root)
        return root;
    }
};
```

#### Problem 9: Serialize and Deserialize Binary Tree (LeetCode 297)

```cpp
#include <sstream>

class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string result;
        serializeHelper(root, result);
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        istringstream iss(data);
        return deserializeHelper(iss);
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

    TreeNode* deserializeHelper(istringstream& iss) {
        string val;
        getline(iss, val, ',');

        if (val == "null") return nullptr;

        TreeNode* root = new TreeNode(stoi(val));
        root->left = deserializeHelper(iss);
        root->right = deserializeHelper(iss);

        return root;
    }
};
```

#### Problem 10: Morris Traversal (O(1) Space)

```cpp
class MorrisTraversal {
public:
    vector<int> inorderTraversal(TreeNode* root) {
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
                    // Make current the right child of predecessor
                    predecessor->right = current;
                    current = current->left;
                } else {
                    // Revert the changes
                    predecessor->right = nullptr;
                    result.push_back(current->val);
                    current = current->right;
                }
            }
        }

        return result;
    }
};
```

### Module 2 Summary

**Advanced Patterns Mastered:**
- Tree Dynamic Programming with global state management
- Lowest Common Ancestor algorithms for binary trees and BSTs
- Tree serialization and deserialization techniques
- Space-optimized tree traversal (Morris traversal)

**Problem-Solving Skills:**
- Identifying optimal substructure in trees
- Managing global variables in recursive solutions
- Converting between tree representations efficiently

[↑ Back to Top](#-table-of-contents)

---

## 🏗️ **Module 3: Comprehensive Projects (Day 5-7)**

### Project 1: File System Simulator

**Objective:** Build a complete file system using tree structures to demonstrate real-world tree applications.

**Technologies Used:**
- Directory tree structure
- Path navigation algorithms
- File and directory operations
- Error handling and validation

**Learning Outcomes:**
- Implement hierarchical data structures
- Handle complex path operations
- Build user-friendly command interfaces

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <sstream>

class FileSystemSimulator {
private:
    struct FileNode {
        string name;
        bool isDirectory;
        string content;
        unordered_map<string, FileNode*> children;
        FileNode* parent;

        FileNode(string n, bool isDir = false, FileNode* p = nullptr)
            : name(n), isDirectory(isDir), parent(p) {}
    };

    FileNode* root;
    FileNode* currentDir;

    vector<string> split(const string& path, char delimiter) {
        vector<string> result;
        stringstream ss(path);
        string item;

        while (getline(ss, item, delimiter)) {
            if (!item.empty()) {
                result.push_back(item);
            }
        }
        return result;
    }

    FileNode* navigateToPath(const string& path) {
        vector<string> pathComponents = split(path, '/');
        FileNode* current = (path[0] == '/') ? root : currentDir;

        for (const string& component : pathComponents) {
            if (component == "..") {
                if (current->parent) {
                    current = current->parent;
                }
            } else if (component != ".") {
                if (current->children.find(component) == current->children.end()) {
                    return nullptr; // Path doesn't exist
                }
                current = current->children[component];
            }
        }
        return current;
    }

    string getFullPath(FileNode* node) {
        if (!node || node == root) return "/";

        string path = getFullPath(node->parent);
        if (path != "/") path += "/";
        path += node->name;
        return path;
    }

public:
    FileSystemSimulator() {
        root = new FileNode("/", true);
        currentDir = root;
    }

    // List directory contents
    void ls(const string& path = "") {
        FileNode* target = path.empty() ? currentDir : navigateToPath(path);
        if (!target) {
            cout << "ls: " << path << ": No such file or directory" << endl;
            return;
        }

        if (!target->isDirectory) {
            cout << target->name << endl;
            return;
        }

        cout << "Contents of " << getFullPath(target) << ":" << endl;
        for (const auto& entry : target->children) {
            cout << (entry.second->isDirectory ? "d " : "f ")
                 << entry.first << endl;
        }
    }

    // Create directory
    void mkdir(const string& path) {
        vector<string> pathComponents = split(path, '/');
        FileNode* current = (path[0] == '/') ? root : currentDir;

        for (const string& component : pathComponents) {
            if (current->children.find(component) == current->children.end()) {
                current->children[component] = new FileNode(component, true, current);
            }
            current = current->children[component];

            if (!current->isDirectory) {
                cout << "mkdir: " << path << ": File exists" << endl;
                return;
            }
        }
        cout << "Directory created: " << path << endl;
    }

    // Create file
    void touch(const string& filename) {
        if (currentDir->children.find(filename) != currentDir->children.end()) {
            cout << "File already exists: " << filename << endl;
            return;
        }

        currentDir->children[filename] = new FileNode(filename, false, currentDir);
        cout << "File created: " << filename << endl;
    }

    // Remove file or directory
    void rm(const string& path) {
        FileNode* target = navigateToPath(path);
        if (!target || target == root) {
            cout << "rm: " << path << ": No such file or directory" << endl;
            return;
        }

        if (target->isDirectory && !target->children.empty()) {
            cout << "rm: " << path << ": Directory not empty" << endl;
            return;
        }

        // Remove from parent's children
        target->parent->children.erase(target->name);
        delete target;
        cout << "Removed: " << path << endl;
    }

    // Change directory
    void cd(const string& path) {
        FileNode* target = navigateToPath(path);
        if (!target) {
            cout << "cd: " << path << ": No such file or directory" << endl;
            return;
        }

        if (!target->isDirectory) {
            cout << "cd: " << path << ": Not a directory" << endl;
            return;
        }

        currentDir = target;
        cout << "Changed to: " << getFullPath(currentDir) << endl;
    }

    // Print working directory
    void pwd() {
        cout << getFullPath(currentDir) << endl;
    }

    // Write content to file
    void writeFile(const string& filename, const string& content) {
        FileNode* file = currentDir->children[filename];
        if (!file) {
            touch(filename);
            file = currentDir->children[filename];
        }

        if (file->isDirectory) {
            cout << "Cannot write to directory: " << filename << endl;
            return;
        }

        file->content = content;
        cout << "Content written to: " << filename << endl;
    }

    // Read file content
    void cat(const string& filename) {
        FileNode* file = currentDir->children[filename];
        if (!file) {
            cout << "cat: " << filename << ": No such file" << endl;
            return;
        }

        if (file->isDirectory) {
            cout << "cat: " << filename << ": Is a directory" << endl;
            return;
        }

        cout << "Content of " << filename << ":" << endl;
        cout << file->content << endl;
    }
};

// Test the file system
void testFileSystem() {
    FileSystemSimulator fs;

    cout << "=== File System Simulator Test ===" << endl;

    fs.pwd();
    fs.mkdir("documents");
    fs.mkdir("pictures");
    fs.ls();

    fs.cd("documents");
    fs.touch("readme.txt");
    fs.writeFile("readme.txt", "This is a readme file.");
    fs.cat("readme.txt");

    fs.mkdir("projects");
    fs.cd("projects");
    fs.touch("main.cpp");
    fs.writeFile("main.cpp", "#include <iostream>\nint main() { return 0; }");

    fs.cd("..");
    fs.ls();
    fs.cd("..");
    fs.pwd();
    fs.ls();
}
```

### Project 2: Expression Evaluator

**Objective:** Create a mathematical expression parser and evaluator using tree structures.

**Technologies Used:**
- Expression tree construction
- Infix to postfix conversion
- Tree-based evaluation
- Operator precedence handling

**Learning Outcomes:**
- Implement parser algorithms
- Build and traverse expression trees
- Handle mathematical operations systematically

```cpp
#include <iostream>
#include <string>
#include <stack>
#include <cctype>
#include <stdexcept>

class ExpressionEvaluator {
private:
    struct ExprNode {
        char op;
        double value;
        ExprNode* left;
        ExprNode* right;
        bool isOperator;

        ExprNode(double val) : value(val), op(0), left(nullptr),
                              right(nullptr), isOperator(false) {}
        ExprNode(char operation) : op(operation), value(0), left(nullptr),
                                  right(nullptr), isOperator(true) {}
    };

    int precedence(char op) {
        switch (op) {
            case '+':
            case '-': return 1;
            case '*':
            case '/': return 2;
            case '^': return 3;
            default: return 0;
        }
    }

    bool isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
    }

    string infixToPostfix(const string& infix) {
        string postfix = "";
        stack<char> operators;

        for (int i = 0; i < infix.length(); i++) {
            char c = infix[i];

            if (isspace(c)) continue;

            if (isdigit(c) || c == '.') {
                // Handle multi-digit numbers
                while (i < infix.length() && (isdigit(infix[i]) || infix[i] == '.')) {
                    postfix += infix[i++];
                }
                postfix += ' ';
                i--; // Adjust for the extra increment
            }
            else if (c == '(') {
                operators.push(c);
            }
            else if (c == ')') {
                while (!operators.empty() && operators.top() != '(') {
                    postfix += operators.top();
                    postfix += ' ';
                    operators.pop();
                }
                if (!operators.empty()) operators.pop(); // Remove '('
            }
            else if (isOperator(c)) {
                while (!operators.empty() && operators.top() != '(' &&
                       precedence(operators.top()) >= precedence(c)) {
                    postfix += operators.top();
                    postfix += ' ';
                    operators.pop();
                }
                operators.push(c);
            }
        }

        while (!operators.empty()) {
            postfix += operators.top();
            postfix += ' ';
            operators.pop();
        }

        return postfix;
    }

    ExprNode* buildExpressionTree(const string& postfix) {
        stack<ExprNode*> nodeStack;
        string token = "";

        for (int i = 0; i < postfix.length(); i++) {
            char c = postfix[i];

            if (c == ' ') {
                if (!token.empty()) {
                    if (isOperator(token[0]) && token.length() == 1) {
                        ExprNode* opNode = new ExprNode(token[0]);
                        if (nodeStack.size() >= 2) {
                            opNode->right = nodeStack.top(); nodeStack.pop();
                            opNode->left = nodeStack.top(); nodeStack.pop();
                        }
                        nodeStack.push(opNode);
                    } else {
                        ExprNode* numNode = new ExprNode(stod(token));
                        nodeStack.push(numNode);
                    }
                    token = "";
                }
            } else {
                token += c;
            }
        }

        return nodeStack.empty() ? nullptr : nodeStack.top();
    }

    double evaluateTree(ExprNode* root) {
        if (!root) return 0;

        if (!root->isOperator) {
            return root->value;
        }

        double leftVal = evaluateTree(root->left);
        double rightVal = evaluateTree(root->right);

        switch (root->op) {
            case '+': return leftVal + rightVal;
            case '-': return leftVal - rightVal;
            case '*': return leftVal * rightVal;
            case '/':
                if (rightVal == 0) throw runtime_error("Division by zero");
                return leftVal / rightVal;
            case '^': return pow(leftVal, rightVal);
            default: throw runtime_error("Unknown operator");
        }
    }

    void printTree(ExprNode* root, string prefix = "", bool isLast = true) {
        if (!root) return;

        cout << prefix << (isLast ? "└── " : "├── ");
        if (root->isOperator) {
            cout << root->op << endl;
        } else {
            cout << root->value << endl;
        }

        string newPrefix = prefix + (isLast ? "    " : "│   ");

        if (root->left || root->right) {
            if (root->left) {
                printTree(root->left, newPrefix, !root->right);
            }
            if (root->right) {
                printTree(root->right, newPrefix, true);
            }
        }
    }

public:
    double evaluate(const string& expression) {
        try {
            string postfix = infixToPostfix(expression);
            cout << "Postfix: " << postfix << endl;

            ExprNode* tree = buildExpressionTree(postfix);

            cout << "Expression Tree:" << endl;
            printTree(tree);

            double result = evaluateTree(tree);
            return result;
        } catch (const exception& e) {
            cout << "Error: " << e.what() << endl;
            return 0;
        }
    }
};

// Test the expression evaluator
void testExpressionEvaluator() {
    ExpressionEvaluator evaluator;

    cout << "=== Expression Evaluator Test ===" << endl;

    vector<string> expressions = {
        "3 + 4 * 2",
        "(3 + 4) * 2",
        "10 - 3 + 2",
        "2 ^ 3 + 1",
        "(5 + 3) * (4 - 2)",
        "15 / 3 + 2 * 4"
    };

    for (const string& expr : expressions) {
        cout << "\nExpression: " << expr << endl;
        double result = evaluator.evaluate(expr);
        cout << "Result: " << result << endl;
        cout << string(50, '-') << endl;
    }
}
```

### Project 3: Auto-complete System

**Objective:** Develop an intelligent auto-complete system using trie data structures.

**Technologies Used:**
- Trie-based string storage
- Prefix matching algorithms
- Frequency-based ranking
- Real-time suggestion generation

**Learning Outcomes:**
- Implement efficient string processing
- Build ranking and suggestion systems
- Optimize for real-time performance

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <unordered_map>

class AutoCompleteSystem {
private:
    struct TrieNode {
        unordered_map<char, TrieNode*> children;
        bool isEndOfWord;
        int frequency;
        string word;

        TrieNode() : isEndOfWord(false), frequency(0) {}
    };

    TrieNode* root;
    TrieNode* currentNode;
    string currentPrefix;

    void insertWord(const string& word, int freq = 1) {
        TrieNode* current = root;

        for (char c : word) {
            if (current->children.find(c) == current->children.end()) {
                current->children[c] = new TrieNode();
            }
            current = current->children[c];
        }

        current->isEndOfWord = true;
        current->frequency += freq;
        current->word = word;
    }

    void collectWords(TrieNode* node, vector<pair<string, int>>& results) {
        if (!node) return;

        if (node->isEndOfWord) {
            results.push_back({node->word, node->frequency});
        }

        for (auto& child : node->children) {
            collectWords(child.second, results);
        }
    }

public:
    AutoCompleteSystem(vector<string>& sentences, vector<int>& times) {
        root = new TrieNode();
        currentNode = root;
        currentPrefix = "";

        // Build initial trie
        for (int i = 0; i < sentences.size(); i++) {
            insertWord(sentences[i], times[i]);
        }
    }

    vector<string> input(char c) {
        if (c == '#') {
            // Save current sentence and reset
            if (!currentPrefix.empty()) {
                insertWord(currentPrefix);
            }
            currentNode = root;
            currentPrefix = "";
            return {};
        }

        currentPrefix += c;

        // Navigate in trie
        if (currentNode && currentNode->children.find(c) != currentNode->children.end()) {
            currentNode = currentNode->children[c];
        } else {
            currentNode = nullptr; // No valid path
        }

        vector<string> suggestions = getSuggestions();
        return suggestions;
    }

    vector<string> getSuggestions() {
        if (!currentNode) return {};

        vector<pair<string, int>> candidates;
        collectWords(currentNode, candidates);

        // Sort by frequency (descending) and lexicographically
        sort(candidates.begin(), candidates.end(),
             [](const pair<string, int>& a, const pair<string, int>& b) {
                 if (a.second != b.second) return a.second > b.second;
                 return a.first < b.first;
             });

        vector<string> result;
        for (int i = 0; i < min(3, (int)candidates.size()); i++) {
            result.push_back(candidates[i].first);
        }

        return result;
    }

    // Additional method for batch search
    vector<string> search(const string& prefix, int maxResults = 5) {
        TrieNode* current = root;

        // Navigate to prefix
        for (char c : prefix) {
            if (current->children.find(c) == current->children.end()) {
                return {}; // Prefix not found
            }
            current = current->children[c];
        }

        vector<pair<string, int>> candidates;
        collectWords(current, candidates);

        sort(candidates.begin(), candidates.end(),
             [](const pair<string, int>& a, const pair<string, int>& b) {
                 if (a.second != b.second) return a.second > b.second;
                 return a.first < b.first;
             });

        vector<string> result;
        for (int i = 0; i < min(maxResults, (int)candidates.size()); i++) {
            result.push_back(candidates[i].first);
        }

        return result;
    }

    void addSentence(const string& sentence, int frequency = 1) {
        insertWord(sentence, frequency);
    }

    void printTrie(TrieNode* node = nullptr, string prefix = "", int depth = 0) {
        if (!node) node = root;

        if (node->isEndOfWord) {
            cout << string(depth * 2, ' ') << prefix << " (freq: "
                 << node->frequency << ")" << endl;
        }

        for (auto& child : node->children) {
            printTrie(child.second, prefix + child.first, depth + 1);
        }
    }
};

// Test the autocomplete system
void testAutoCompleteSystem() {
    cout << "=== Auto-Complete System Test ===" << endl;

    vector<string> sentences = {
        "i love you", "island", "iroman", "i love leetcode"
    };
    vector<int> times = {5, 3, 2, 2};

    AutoCompleteSystem autoComplete(sentences, times);

    cout << "Initial Trie Structure:" << endl;
    autoComplete.printTrie();
    cout << endl;

    // Simulate typing "i "
    cout << "Typing 'i':" << endl;
    vector<string> suggestions = autoComplete.input('i');
    for (const string& suggestion : suggestions) {
        cout << "  " << suggestion << endl;
    }

    cout << "\nTyping ' ':" << endl;
    suggestions = autoComplete.input(' ');
    for (const string& suggestion : suggestions) {
        cout << "  " << suggestion << endl;
    }

    cout << "\nTyping 'l':" << endl;
    suggestions = autoComplete.input('l');
    for (const string& suggestion : suggestions) {
        cout << "  " << suggestion << endl;
    }

    // Complete the sentence
    autoComplete.input('o');
    autoComplete.input('v');
    autoComplete.input('e');
    autoComplete.input(' ');
    autoComplete.input('c');
    autoComplete.input('o');
    autoComplete.input('d');
    autoComplete.input('i');
    autoComplete.input('n');
    autoComplete.input('g');
    autoComplete.input('#'); // End sentence

    cout << "\nAfter adding 'i love coding':" << endl;

    // Test batch search
    cout << "\nBatch search for 'i':" << endl;
    vector<string> searchResults = autoComplete.search("i");
    for (const string& result : searchResults) {
        cout << "  " << result << endl;
    }
}
```

### Main Function to Run All Tests

```cpp
int main() {
    cout << "=== Week 7: Comprehensive Tree Projects ===" << endl;
    cout << string(60, '=') << endl;
    
    cout << "\n🔧 MODULE 1: Mixed Tree Problems" << endl;
    cout << string(40, '-') << endl;
    // Test individual problems
    cout << "\n1. Testing Maximum Path Sum:" << endl;
    testMaxPathSum();
    
    cout << "\n🧠 MODULE 2: Advanced Problem Patterns" << endl;
    cout << string(40, '-') << endl;
    cout << "Advanced patterns demonstrated in previous problems" << endl;
    
    cout << "\n🏗️ MODULE 3: Comprehensive Projects" << endl;
    cout << string(40, '-') << endl;
    
    // Test comprehensive projects
    cout << "\n1. File System Simulator:" << endl;
    testFileSystem();
    
    cout << "\n" << string(60, '-') << endl;
    
    cout << "\n2. Expression Evaluator:" << endl;
    testExpressionEvaluator();
    
    cout << "\n" << string(60, '-') << endl;
    
    cout << "\n3. Auto-Complete System:" << endl;
    testAutoCompleteSystem();
    
    cout << "\n📊 MODULE 4: Assessment Complete" << endl;
    cout << string(40, '-') << endl;
    cout << "Week 7 Implementation Complete!" << endl;
    cout << "You have successfully integrated all tree concepts!" << endl;
    
    cout << "\n" << string(60, '=') << endl;
    cout << "🎉 Congratulations on mastering trees! 🎉" << endl;
    cout << "Next: Advanced topics and real-world applications" << endl;
    
    return 0;
}
```

### Module 3 Summary

**Projects Completed:**
1. **File System Simulator** - Hierarchical data management
2. **Expression Evaluator** - Mathematical computation trees
3. **Auto-complete System** - Intelligent string processing

**Real-World Skills Acquired:**
- System design with tree structures
- User interface development
- Performance optimization techniques
- Error handling and edge case management

**Industry Applications:**
- Operating system development
- Compiler and interpreter design
- Search engine technology
- Database indexing systems

[↑ Back to Top](#-table-of-contents)

---

## 📊 **Module 4: Assessment & Next Steps**

### Self-Assessment Checklist

**Technical Mastery:**

- [ ] Can solve complex tree problems combining multiple concepts
- [ ] Implement advanced algorithms like Morris traversal
- [ ] Design and build complete systems using trees
- [ ] Optimize tree algorithms for performance
- [ ] Handle edge cases and error scenarios gracefully

**Problem-Solving Skills:**

- [ ] Quickly identify which tree structure fits a problem
- [ ] Apply appropriate algorithms and patterns
- [ ] Integrate multiple tree concepts in one solution
- [ ] Debug complex tree-based systems
- [ ] Analyze time and space complexity accurately

**Real-World Application:**

- [ ] Build practical systems like file simulators
- [ ] Implement expression evaluators and parsers
- [ ] Create efficient autocomplete systems
- [ ] Understand how trees fit in larger architectures
- [ ] Consider performance and scalability implications

### Interview Readiness

**LeetCode Problems to Master:**

1. Binary Tree Maximum Path Sum (124)
2. Serialize and Deserialize Binary Tree (297)
3. Word Search II (212)
4. Merge k Sorted Lists (23)
5. House Robber III (337)
6. Lowest Common Ancestor of Binary Tree (236)
7. Path Sum III (437)
8. Validate Binary Search Tree (98)
9. Binary Tree Diameter (543)
10. Implement Trie (208)

**System Design Topics:**

- Database indexing with B-trees
- File system design
- Auto-complete systems
- Expression parsing in compilers
- Memory management with trees

### Advanced Topics for Future Learning

**Next-Level Tree Concepts:**

1. **Concurrent Trees**: Lock-free data structures
   - Skip lists and concurrent B-trees
   - Memory ordering and synchronization
   - Performance in multi-threaded environments

2. **Distributed Trees**: Scalable tree structures
   - Consistent hashing trees
   - Distributed B-trees
   - CAP theorem implications

3. **Persistent Trees**: Functional data structures
   - Copy-on-write techniques
   - Version control systems
   - Immutable tree implementations

4. **Specialized Trees**: Domain-specific optimizations
   - Suffix trees for string algorithms
   - Heavy-light decomposition
   - Link-cut trees

5. **Cache-Oblivious Algorithms**: Memory hierarchy optimization
   - B-tree layout optimization
   - Cache-efficient traversals
   - External memory algorithms

**Learning Resources:**
- Advanced algorithms courses
- Research papers on tree structures
- Open-source database implementations
- Competitive programming platforms

[↑ Back to Top](#-table-of-contents)

### Conclusion

🎉 **Congratulations on Completing Week 7!** 🎉

You have successfully completed a comprehensive 7-week journey through tree data structures. Your achievements include:

#### **Technical Mastery Achieved:**
- ✅ **Deep theoretical understanding** of all major tree types
- ✅ **Practical implementation skills** for real-world projects  
- ✅ **Problem-solving patterns** for technical interviews
- ✅ **System design knowledge** for building scalable applications
- ✅ **Performance optimization** techniques for production code

#### **Skills Portfolio:**
1. **Data Structure Expertise**: Binary trees, BSTs, AVL trees, heaps, tries, segment trees
2. **Algorithm Proficiency**: Tree traversals, balancing, heapify operations, string processing
3. **Problem-Solving Ability**: Dynamic programming on trees, LCA, serialization
4. **System Design**: File systems, expression parsers, auto-complete systems
5. **Interview Readiness**: 20+ LeetCode problems solved with optimal solutions

#### **Career Impact:**
- **Software Engineering**: Ready for tree-related technical interviews
- **System Design**: Can architect tree-based solutions for large systems
- **Algorithm Development**: Capable of optimizing tree algorithms for performance
- **Research Foundation**: Prepared for advanced computer science topics

#### **What's Next?**
- Apply your knowledge to real projects
- Contribute to open-source tree implementations
- Explore advanced topics like concurrent and distributed trees
- Mentor others in tree data structure concepts

**You're now ready to tackle any tree-related challenge in your programming career!**

[↑ Back to Top](#-table-of-contents)

---

## 📚 **Quick Reference Guide**

### **Tree Type Selection Matrix**
| Problem Type | Recommended Tree | Time Complexity |
|-------------|------------------|-----------------|
| Hierarchical Data | Binary Tree | O(n) traversal |
| Sorted Access | BST/AVL | O(log n) operations |
| Priority Processing | Heap | O(log n) insert/extract |
| String Prefix | Trie | O(m) where m = string length |
| Range Queries | Segment Tree | O(log n) query/update |

### **Common Algorithm Patterns**
1. **Tree DP**: `solve(node) = combine(solve(left), solve(right), node.val)`
2. **Tree Traversal**: Preorder, Inorder, Postorder variations
3. **Tree Construction**: From traversals, from arrays, from strings
4. **Tree Modification**: Insertion, deletion, balancing operations

### **Implementation Checklist**
- [ ] Handle null/empty cases
- [ ] Implement proper error checking
- [ ] Consider iterative vs recursive approaches
- [ ] Optimize for space and time complexity
- [ ] Test with edge cases and large inputs

[↑ Back to Top](#-table-of-contents)

# Week 6: Advanced Tree Topics (Tries & Segment Trees) - Implementation Guide

## 📋 **Table of Contents**

### **Top-Level Navigation**

1. [Week Overview](#-week-overview)
2. [Module 1: Trie Data Structures](#-module-1-trie-data-structures)
3. [Module 2: String Processing with Tries](#-module-2-string-processing-with-tries)
4. [Module 3: Segment Trees](#-module-3-segment-trees)
5. [Module 4: Range Query Systems](#-module-4-range-query-systems)
6. [Module 5: Assessment & Applications](#-module-5-assessment--applications)

### **Module-Level Navigation**

#### **Module 1: Trie Data Structures**

- [Trie Fundamentals](#trie-fundamentals)
- [Trie Implementation](#trie-implementation)
- [Memory Optimization](#memory-optimization)
- [Module 1 Summary](#module-1-summary)

#### **Module 2: String Processing with Tries**

- [Prefix Operations](#prefix-operations)
- [String Search Algorithms](#string-search-algorithms)
- [Auto-completion Systems](#auto-completion-systems)
- [Module 2 Summary](#module-2-summary)

#### **Module 3: Segment Trees**

- [Segment Tree Concepts](#segment-tree-concepts)
- [Range Query Implementation](#range-query-implementation)
- [Update Operations](#update-operations)
- [Module 3 Summary](#module-3-summary)

#### **Module 4: Range Query Systems**

- [Advanced Range Operations](#advanced-range-operations)
- [Lazy Propagation](#lazy-propagation)
- [Performance Optimization](#performance-optimization)
- [Module 4 Summary](#module-4-summary)

#### **Module 5: Assessment & Applications**

- [Self-Assessment Checklist](#self-assessment-checklist)
- [Real-World Applications](#real-world-applications)
- [Performance Comparison](#performance-comparison)
- [Next Week Preview](#next-week-preview)

---

## 🎯 **Week Overview**

This week focuses on specialized tree structures for string processing and range queries. You'll explore how different tree designs solve specific problem domains optimally, going beyond general-purpose trees.

**Learning Outcomes:**

- Master trie data structures for efficient string processing
- Implement segment trees for range query systems
- Recognize when advanced trees solve problems optimally
- Analyze space-time trade-offs for specialized structures
- Code complex tree structures with proper optimization
- Apply tries and segment trees to real-world problems
- Understand performance implications of specialized designs

---

## 🔤 **Module 1: Trie Data Structures**

## Day 1-3: Trie (Prefix Tree) Implementation & String Operations

### 📚 Theory: The String Processing Specialist

**Teacher's Introduction:**
"After mastering trees for ordering and priority, we now explore trees designed for strings. Tries revolutionize string processing by exploiting the natural hierarchy in language - shared prefixes mean shared paths, making operations blazingly fast."

### 🔧 The Trie Structure

**Core Insight:**
Unlike BSTs which compare entire keys, tries decompose strings character by character, creating a tree where:

- **Each path represents a string prefix**
- **Shared prefixes share tree paths** (space efficiency)
- **Operations depend only on string length**, not dictionary size

### **Implementation 1: Basic Trie Structure & Core Operations**

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <queue>
using namespace std;

class TrieNode {
public:
    TrieNode* children[26];  // For lowercase letters a-z
    bool isEndOfWord;
    int wordCount;          // Number of words ending at this node
    int prefixCount;        // Number of words passing through this node

    TrieNode() {
        isEndOfWord = false;
        wordCount = 0;
        prefixCount = 0;
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }

    ~TrieNode() {
        for (int i = 0; i < 26; i++) {
            delete children[i];
        }
    }
};

class Trie {
private:
    TrieNode* root;
    int totalWords;

public:
    Trie() {
        root = new TrieNode();
        totalWords = 0;
    }

    ~Trie() {
        delete root;
    }

    // Insert a word into the trie
    void insert(const string& word) {
        cout << "\n=== Inserting word: \"" << word << "\" ===" << endl;

        TrieNode* current = root;

        for (int i = 0; i < word.length(); i++) {
            char c = word[i];
            int index = c - 'a';

            cout << "Processing character '" << c << "' at position " << i << endl;

            if (current->children[index] == nullptr) {
                current->children[index] = new TrieNode();
                cout << "  Created new node for '" << c << "'" << endl;
            } else {
                cout << "  Using existing node for '" << c << "'" << endl;
            }

            current = current->children[index];
            current->prefixCount++;

            cout << "  Prefix \"" << word.substr(0, i + 1)
                 << "\" now has count: " << current->prefixCount << endl;
        }

        if (!current->isEndOfWord) {
            current->isEndOfWord = true;
            totalWords++;
            cout << "Marked end of word. Total words in trie: " << totalWords << endl;
        } else {
            cout << "Word already exists in trie" << endl;
        }

        current->wordCount++;
    }

    // Search for a word in the trie
    bool search(const string& word) {
        cout << "\n=== Searching for word: \"" << word << "\" ===" << endl;

        TrieNode* current = root;

        for (int i = 0; i < word.length(); i++) {
            char c = word[i];
            int index = c - 'a';

            cout << "Looking for character '" << c << "' at position " << i << endl;

            if (current->children[index] == nullptr) {
                cout << "Character '" << c << "' not found. Word does not exist." << endl;
                return false;
            }

            current = current->children[index];
            cout << "Found '" << c << "', continuing..." << endl;
        }

        bool exists = current->isEndOfWord;
        cout << "Reached end of path. Word " << (exists ? "EXISTS" : "NOT FOUND")
             << " in trie." << endl;

        if (exists) {
            cout << "Word count: " << current->wordCount << endl;
        }

        return exists;
    }

    // Check if any word starts with the given prefix
    bool startsWith(const string& prefix) {
        cout << "\n=== Checking prefix: \"" << prefix << "\" ===" << endl;

        TrieNode* current = root;

        for (int i = 0; i < prefix.length(); i++) {
            char c = prefix[i];
            int index = c - 'a';

            if (current->children[index] == nullptr) {
                cout << "Prefix \"" << prefix << "\" not found in trie." << endl;
                return false;
            }

            current = current->children[index];
        }

        cout << "Prefix \"" << prefix << "\" found! "
             << current->prefixCount << " words have this prefix." << endl;
        return true;
    }

    // Get all words with given prefix
    vector<string> getWordsWithPrefix(const string& prefix) {
        vector<string> result;
        TrieNode* prefixNode = findPrefixNode(prefix);

        if (prefixNode != nullptr) {
            cout << "\n=== Finding all words with prefix: \"" << prefix << "\" ===" << endl;
            string currentWord = prefix;
            dfsCollectWords(prefixNode, currentWord, result);

            cout << "Found " << result.size() << " words with prefix \""
                 << prefix << "\":" << endl;
            for (const string& word : result) {
                cout << "  - " << word << endl;
            }
        }

        return result;
    }

    // Get statistics about the trie
    void getTrieStats() {
        cout << "\n=== TRIE STATISTICS ===" << endl;
        cout << "Total words: " << totalWords << endl;
        cout << "Total nodes: " << countNodes(root) << endl;
        cout << "Maximum depth: " << getMaxDepth(root, 0) << endl;
        cout << "Memory usage (estimated): " << getMemoryUsage() << " bytes" << endl;

        // Memory efficiency analysis
        int totalChars = getTotalCharacters();
        int nodesCreated = countNodes(root);
        double compressionRatio = (double)totalChars / nodesCreated;

        cout << "Total characters stored: " << totalChars << endl;
        cout << "Compression ratio: " << compressionRatio
             << " (higher = better sharing)" << endl;
    }

private:
    TrieNode* findPrefixNode(const string& prefix) {
        TrieNode* current = root;

        for (char c : prefix) {
            int index = c - 'a';
            if (current->children[index] == nullptr) {
                return nullptr;
            }
            current = current->children[index];
        }

        return current;
    }

    void dfsCollectWords(TrieNode* node, string& currentWord, vector<string>& result) {
        if (node->isEndOfWord) {
            result.push_back(currentWord);
        }

        for (int i = 0; i < 26; i++) {
            if (node->children[i] != nullptr) {
                char nextChar = 'a' + i;
                currentWord.push_back(nextChar);
                dfsCollectWords(node->children[i], currentWord, result);
                currentWord.pop_back();
            }
        }
    }

    int countNodes(TrieNode* node) {
        if (node == nullptr) return 0;

        int count = 1;
        for (int i = 0; i < 26; i++) {
            count += countNodes(node->children[i]);
        }
        return count;
    }

    int getMaxDepth(TrieNode* node, int depth) {
        if (node == nullptr) return depth - 1;

        int maxDepth = depth;
        for (int i = 0; i < 26; i++) {
            if (node->children[i] != nullptr) {
                maxDepth = max(maxDepth, getMaxDepth(node->children[i], depth + 1));
            }
        }
        return maxDepth;
    }

    int getMemoryUsage() {
        return countNodes(root) * sizeof(TrieNode);
    }

    int getTotalCharacters() {
        return getTotalCharsHelper(root, 0);
    }

    int getTotalCharsHelper(TrieNode* node, int depth) {
        if (node == nullptr) return 0;

        int chars = 0;
        if (node->isEndOfWord) {
            chars = depth * node->wordCount;
        }

        for (int i = 0; i < 26; i++) {
            chars += getTotalCharsHelper(node->children[i], depth + 1);
        }

        return chars;
    }
};

void demonstrateBasicTrieOperations() {
    cout << "\n=== BASIC TRIE OPERATIONS DEMONSTRATION ===" << endl;

    Trie trie;

    // Insert words demonstrating prefix sharing
    vector<string> words = {"cat", "cats", "car", "card", "care", "careful", "dog", "dogs"};

    cout << "Building trie with words: ";
    for (const string& word : words) {
        cout << word << " ";
    }
    cout << endl;

    for (const string& word : words) {
        trie.insert(word);
    }

    // Demonstrate search operations
    cout << "\n" << string(60, '=') << endl;
    cout << "SEARCH OPERATIONS" << endl;
    cout << string(60, '=') << endl;

    vector<string> searchWords = {"cat", "car", "care", "dog", "elephant", "ca"};
    for (const string& word : searchWords) {
        bool found = trie.search(word);
    }

    // Demonstrate prefix operations
    cout << "\n" << string(60, '=') << endl;
    cout << "PREFIX OPERATIONS" << endl;
    cout << string(60, '=') << endl;

    vector<string> prefixes = {"ca", "car", "dog", "xyz"};
    for (const string& prefix : prefixes) {
        bool hasPrefix = trie.startsWith(prefix);
        if (hasPrefix) {
            trie.getWordsWithPrefix(prefix);
        }
    }

    // Show statistics
    trie.getTrieStats();
}
```

### **Implementation 2: Advanced Trie Operations & Optimizations**

```cpp
class AdvancedTrie {
private:
    struct TrieNode {
        unordered_map<char, TrieNode*> children;  // Space-efficient for sparse alphabets
        bool isEndOfWord;
        int frequency;
        string word;  // Store complete word at end nodes for easy retrieval

        TrieNode() : isEndOfWord(false), frequency(0) {}

        ~TrieNode() {
            for (auto& pair : children) {
                delete pair.second;
            }
        }
    };

    TrieNode* root;

public:
    AdvancedTrie() {
        root = new TrieNode();
    }

    ~AdvancedTrie() {
        delete root;
    }

    // Insert with frequency tracking
    void insert(const string& word, int frequency = 1) {
        TrieNode* current = root;

        for (char c : word) {
            if (current->children.find(c) == current->children.end()) {
                current->children[c] = new TrieNode();
            }
            current = current->children[c];
        }

        current->isEndOfWord = true;
        current->frequency += frequency;
        current->word = word;
    }

    // Delete operation with node cleanup
    bool deleteWord(const string& word) {
        return deleteHelper(root, word, 0);
    }

    // Autocomplete with frequency ranking
    vector<pair<string, int>> autocomplete(const string& prefix, int maxResults = 5) {
        cout << "\n=== AUTOCOMPLETE for prefix: \"" << prefix << "\" ===" << endl;

        TrieNode* prefixNode = findPrefixNode(prefix);
        if (prefixNode == nullptr) {
            cout << "No words found with prefix \"" << prefix << "\"" << endl;
            return {};
        }

        vector<pair<string, int>> suggestions;
        collectSuggestions(prefixNode, suggestions);

        // Sort by frequency (descending)
        sort(suggestions.begin(), suggestions.end(),
             [](const pair<string, int>& a, const pair<string, int>& b) {
                 return a.second > b.second;
             });

        // Limit results
        if (suggestions.size() > maxResults) {
            suggestions.resize(maxResults);
        }

        cout << "Top " << suggestions.size() << " suggestions:" << endl;
        for (int i = 0; i < suggestions.size(); i++) {
            cout << (i + 1) << ". " << suggestions[i].first
                 << " (frequency: " << suggestions[i].second << ")" << endl;
        }

        return suggestions;
    }

    // Longest common prefix of all words
    string longestCommonPrefix() {
        cout << "\n=== FINDING LONGEST COMMON PREFIX ===" << endl;

        string lcp = "";
        TrieNode* current = root;

        while (current->children.size() == 1 && !current->isEndOfWord) {
            auto it = current->children.begin();
            lcp += it->first;
            current = it->second;

            cout << "Extended LCP to: \"" << lcp << "\"" << endl;
        }

        cout << "Longest common prefix: \"" << lcp << "\"" << endl;
        return lcp;
    }

    // Count words with given prefix
    int countWordsWithPrefix(const string& prefix) {
        TrieNode* prefixNode = findPrefixNode(prefix);
        if (prefixNode == nullptr) return 0;

        return countWordsInSubtree(prefixNode);
    }

    // Word break problem: check if string can be segmented
    bool wordBreak(const string& s) {
        cout << "\n=== WORD BREAK for string: \"" << s << "\" ===" << endl;

        int n = s.length();
        vector<bool> dp(n + 1, false);
        dp[0] = true;  // Empty string can always be segmented

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && search(s.substr(j, i - j))) {
                    dp[i] = true;
                    cout << "Found word: \"" << s.substr(j, i - j)
                         << "\" ending at position " << i << endl;
                    break;
                }
            }
        }

        bool canBreak = dp[n];
        cout << "String \"" << s << "\" "
             << (canBreak ? "CAN" : "CANNOT") << " be segmented" << endl;
        return canBreak;
    }

private:
    bool deleteHelper(TrieNode* node, const string& word, int index) {
        if (index == word.length()) {
            if (!node->isEndOfWord) return false;

            node->isEndOfWord = false;
            node->frequency = 0;

            // Return true if node has no children (can be deleted)
            return node->children.empty();
        }

        char c = word[index];
        auto it = node->children.find(c);
        if (it == node->children.end()) return false;

        bool shouldDeleteChild = deleteHelper(it->second, word, index + 1);

        if (shouldDeleteChild) {
            delete it->second;
            node->children.erase(it);

            // Return true if current node can be deleted
            return !node->isEndOfWord && node->children.empty();
        }

        return false;
    }

    TrieNode* findPrefixNode(const string& prefix) {
        TrieNode* current = root;

        for (char c : prefix) {
            auto it = current->children.find(c);
            if (it == current->children.end()) {
                return nullptr;
            }
            current = it->second;
        }

        return current;
    }

    void collectSuggestions(TrieNode* node, vector<pair<string, int>>& suggestions) {
        if (node->isEndOfWord) {
            suggestions.push_back({node->word, node->frequency});
        }

        for (auto& pair : node->children) {
            collectSuggestions(pair.second, suggestions);
        }
    }

    int countWordsInSubtree(TrieNode* node) {
        int count = node->isEndOfWord ? 1 : 0;

        for (auto& pair : node->children) {
            count += countWordsInSubtree(pair.second);
        }

        return count;
    }

    bool search(const string& word) {
        TrieNode* current = root;

        for (char c : word) {
            auto it = current->children.find(c);
            if (it == current->children.end()) {
                return false;
            }
            current = it->second;
        }

        return current->isEndOfWord;
    }
};

void demonstrateAdvancedTrieOperations() {
    cout << "\n=== ADVANCED TRIE OPERATIONS DEMONSTRATION ===" << endl;

    AdvancedTrie trie;

    // Build dictionary with frequencies
    vector<pair<string, int>> dictionary = {
        {"cat", 100}, {"cats", 50}, {"car", 200}, {"card", 80},
        {"care", 90}, {"careful", 30}, {"carry", 60}, {"dog", 150},
        {"dogs", 40}, {"door", 70}, {"down", 120}
    };

    cout << "Building trie with frequency data:" << endl;
    for (const auto& entry : dictionary) {
        cout << "Inserting: " << entry.first << " (freq: " << entry.second << ")" << endl;
        trie.insert(entry.first, entry.second);
    }

    // Demonstrate autocomplete
    trie.autocomplete("ca", 3);
    trie.autocomplete("do", 3);

    // Find longest common prefix
    trie.longestCommonPrefix();

    // Count words with prefix
    cout << "\nWords with prefix 'ca': " << trie.countWordsWithPrefix("ca") << endl;
    cout << "Words with prefix 'dog': " << trie.countWordsWithPrefix("dog") << endl;

    // Word break demonstration
    trie.wordBreak("catdog");
    trie.wordBreak("carefuldog");
    trie.wordBreak("invalidword");

    cout << "\nTrie Applications Summary:" << endl;
    cout << "✅ Autocomplete systems (search engines, IDEs)" << endl;
    cout << "✅ Spell checkers and correction systems" << endl;
    cout << "✅ IP routing and network prefix matching" << endl;
    cout << "✅ Dictionary and word game implementations" << endl;
    cout << "✅ Text processing and natural language tasks" << endl;
}
```

**Teaching Points:**

1. **Prefix Sharing**: Common prefixes create shared paths
2. **Character-by-Character**: Operations depend on string length, not dictionary size
3. **Space-Time Trade-offs**: Array vs HashMap children implementation
4. **Memory Management**: Proper cleanup in destructor

---

## Day 4-5: Segment Trees for Range Queries

### 📚 Theory: The Range Query Specialist

**Teacher's Introduction:**
"When arrays need efficient range operations, segment trees provide the perfect solution. By building a tree on top of an array, we transform O(n) range queries into O(log n) operations while maintaining O(log n) updates."

### 🔧 Segment Tree Structure

**Core Concept:**

- **Each node represents a range** [L, R] of array indices
- **Leaf nodes** represent single elements
- **Internal nodes** combine information from child ranges
- **Complete binary tree** structure for efficient array representation

### **Implementation 1: Basic Segment Tree for Range Sum Queries**

```cpp
class SegmentTree {
private:
    vector<long long> tree;
    vector<int> arr;
    int n;

public:
    SegmentTree(vector<int>& inputArr) {
        arr = inputArr;
        n = arr.size();
        tree.resize(4 * n);  // Safe upper bound for tree size
        build(1, 0, n - 1);
    }

    // Build the segment tree
    void build(int node, int start, int end) {
        cout << "Building node " << node << " for range [" << start << ", " << end << "]" << endl;

        if (start == end) {
            // Leaf node
            tree[node] = arr[start];
            cout << "  Leaf node " << node << " = " << tree[node] << endl;
        } else {
            int mid = (start + end) / 2;

            // Build left and right subtrees
            build(2 * node, start, mid);
            build(2 * node + 1, mid + 1, end);

            // Combine results from children
            tree[node] = tree[2 * node] + tree[2 * node + 1];
            cout << "  Internal node " << node << " = " << tree[2 * node]
                 << " + " << tree[2 * node + 1] << " = " << tree[node] << endl;
        }
    }

    // Query range sum from L to R
    long long query(int L, int R) {
        cout << "\n=== Range Sum Query [" << L << ", " << R << "] ===" << endl;
        long long result = queryHelper(1, 0, n - 1, L, R);
        cout << "Result: " << result << endl;
        return result;
    }

    // Update value at index idx to newVal
    void update(int idx, int newVal) {
        cout << "\n=== Update index " << idx << " to " << newVal << " ===" << endl;
        cout << "Old value: " << arr[idx] << endl;
        arr[idx] = newVal;
        updateHelper(1, 0, n - 1, idx, newVal);
        cout << "Update complete." << endl;
    }

    // Display the segment tree structure
    void displayTree() {
        cout << "\n=== SEGMENT TREE STRUCTURE ===" << endl;
        cout << "Array: ";
        for (int i = 0; i < n; i++) {
            cout << arr[i] << " ";
        }
        cout << endl;

        cout << "\nTree array representation:" << endl;
        for (int i = 1; i < tree.size(); i++) {
            if (tree[i] != 0) {  // Only show non-zero nodes
                cout << "tree[" << i << "] = " << tree[i] << endl;
            }
        }

        cout << "\nTree structure visualization:" << endl;
        displayTreeHelper(1, 0, n - 1, 0);
    }

private:
    long long queryHelper(int node, int start, int end, int L, int R) {
        cout << "Visiting node " << node << " [" << start << ", " << end << "]" << endl;

        // Complete overlap: current range is completely within query range
        if (start >= L && end <= R) {
            cout << "  Complete overlap: returning " << tree[node] << endl;
            return tree[node];
        }

        // No overlap: current range is completely outside query range
        if (end < L || start > R) {
            cout << "  No overlap: returning 0" << endl;
            return 0;
        }

        // Partial overlap: split and query both children
        cout << "  Partial overlap: splitting range" << endl;
        int mid = (start + end) / 2;
        long long leftSum = queryHelper(2 * node, start, mid, L, R);
        long long rightSum = queryHelper(2 * node + 1, mid + 1, end, L, R);

        long long result = leftSum + rightSum;
        cout << "  Combining: " << leftSum << " + " << rightSum << " = " << result << endl;
        return result;
    }

    void updateHelper(int node, int start, int end, int idx, int newVal) {
        cout << "Updating node " << node << " [" << start << ", " << end << "]" << endl;

        if (start == end) {
            // Leaf node
            tree[node] = newVal;
            cout << "  Updated leaf node " << node << " to " << newVal << endl;
        } else {
            int mid = (start + end) / 2;

            if (idx <= mid) {
                // Update left child
                updateHelper(2 * node, start, mid, idx, newVal);
            } else {
                // Update right child
                updateHelper(2 * node + 1, mid + 1, end, idx, newVal);
            }

            // Update current node based on children
            long long oldValue = tree[node];
            tree[node] = tree[2 * node] + tree[2 * node + 1];
            cout << "  Updated internal node " << node << " from " << oldValue
                 << " to " << tree[node] << endl;
        }
    }

    void displayTreeHelper(int node, int start, int end, int depth) {
        if (node >= tree.size() || tree[node] == 0) return;

        string indent(depth * 2, ' ');
        cout << indent << "Node " << node << " [" << start << ", " << end
             << "] = " << tree[node] << endl;

        if (start != end) {
            int mid = (start + end) / 2;
            displayTreeHelper(2 * node, start, mid, depth + 1);
            displayTreeHelper(2 * node + 1, mid + 1, end, depth + 1);
        }
    }
};

void demonstrateSegmentTreeBasics() {
    cout << "\n=== SEGMENT TREE BASICS DEMONSTRATION ===" << endl;

    vector<int> arr = {1, 3, 5, 7, 9, 11};
    cout << "Building segment tree for array: ";
    for (int val : arr) {
        cout << val << " ";
    }
    cout << endl;

    SegmentTree segTree(arr);
    segTree.displayTree();

    // Demonstrate range queries
    cout << "\n" << string(60, '=') << endl;
    cout << "RANGE QUERY DEMONSTRATIONS" << endl;
    cout << string(60, '=') << endl;

    vector<pair<int, int>> queries = {{1, 3}, {0, 2}, {2, 5}, {4, 5}};
    for (auto& query : queries) {
        segTree.query(query.first, query.second);
    }

    // Demonstrate updates
    cout << "\n" << string(60, '=') << endl;
    cout << "UPDATE DEMONSTRATIONS" << endl;
    cout << string(60, '=') << endl;

    segTree.update(2, 10);  // Change arr[2] from 5 to 10

    cout << "\nAfter update, re-querying range [1, 3]:" << endl;
    segTree.query(1, 3);

    segTree.displayTree();
}
```

### **Implementation 2: Advanced Segment Trees with Different Operations**

```cpp
// Generic segment tree supporting different operations
template<typename T>
class GenericSegmentTree {
private:
    vector<T> tree;
    vector<T> arr;
    int n;
    function<T(T, T)> combine;  // Function to combine two values
    T identity;                 // Identity element for the operation

public:
    GenericSegmentTree(vector<T>& inputArr, function<T(T, T)> combineFunc, T identityVal)
        : arr(inputArr), n(inputArr.size()), combine(combineFunc), identity(identityVal) {
        tree.resize(4 * n);
        build(1, 0, n - 1);
    }

    void build(int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(2 * node, start, mid);
            build(2 * node + 1, mid + 1, end);
            tree[node] = combine(tree[2 * node], tree[2 * node + 1]);
        }
    }

    T query(int L, int R) {
        return queryHelper(1, 0, n - 1, L, R);
    }

    void update(int idx, T newVal) {
        arr[idx] = newVal;
        updateHelper(1, 0, n - 1, idx, newVal);
    }

private:
    T queryHelper(int node, int start, int end, int L, int R) {
        if (start >= L && end <= R) {
            return tree[node];
        }

        if (end < L || start > R) {
            return identity;
        }

        int mid = (start + end) / 2;
        T leftResult = queryHelper(2 * node, start, mid, L, R);
        T rightResult = queryHelper(2 * node + 1, mid + 1, end, L, R);
        return combine(leftResult, rightResult);
    }

    void updateHelper(int node, int start, int end, int idx, T newVal) {
        if (start == end) {
            tree[node] = newVal;
        } else {
            int mid = (start + end) / 2;
            if (idx <= mid) {
                updateHelper(2 * node, start, mid, idx, newVal);
            } else {
                updateHelper(2 * node + 1, mid + 1, end, idx, newVal);
            }
            tree[node] = combine(tree[2 * node], tree[2 * node + 1]);
        }
    }
};

// Range Minimum Query Segment Tree
class RMQSegmentTree {
private:
    GenericSegmentTree<int> segTree;

public:
    RMQSegmentTree(vector<int>& arr)
        : segTree(arr, [](int a, int b) { return min(a, b); }, INT_MAX) {}

    int rangeMinQuery(int L, int R) {
        cout << "\nRange Minimum Query [" << L << ", " << R << "]: ";
        int result = segTree.query(L, R);
        cout << result << endl;
        return result;
    }

    void update(int idx, int newVal) {
        cout << "Updating index " << idx << " to " << newVal << endl;
        segTree.update(idx, newVal);
    }
};

// Range Maximum Query Segment Tree
class RMAXSegmentTree {
private:
    GenericSegmentTree<int> segTree;

public:
    RMAXSegmentTree(vector<int>& arr)
        : segTree(arr, [](int a, int b) { return max(a, b); }, INT_MIN) {}

    int rangeMaxQuery(int L, int R) {
        cout << "\nRange Maximum Query [" << L << ", " << R << "]: ";
        int result = segTree.query(L, R);
        cout << result << endl;
        return result;
    }

    void update(int idx, int newVal) {
        segTree.update(idx, newVal);
    }
};

void demonstrateAdvancedSegmentTrees() {
    cout << "\n=== ADVANCED SEGMENT TREE OPERATIONS ===" << endl;

    vector<int> arr = {2, 5, 1, 8, 3, 9, 4, 6, 7};
    cout << "Array: ";
    for (int val : arr) {
        cout << val << " ";
    }
    cout << endl;

    // Range Minimum Query
    cout << "\n--- RANGE MINIMUM QUERIES ---" << endl;
    RMQSegmentTree rmqTree(arr);
    rmqTree.rangeMinQuery(1, 4);
    rmqTree.rangeMinQuery(0, 8);
    rmqTree.rangeMinQuery(3, 6);

    // Range Maximum Query
    cout << "\n--- RANGE MAXIMUM QUERIES ---" << endl;
    RMAXSegmentTree rmaxTree(arr);
    rmaxTree.rangeMaxQuery(1, 4);
    rmaxTree.rangeMaxQuery(0, 8);
    rmaxTree.rangeMaxQuery(3, 6);

    // Demonstrate updates
    cout << "\n--- UPDATES AND RE-QUERIES ---" << endl;
    rmqTree.update(2, 10);  // Change arr[2] from 1 to 10
    rmqTree.rangeMinQuery(1, 4);  // Should show different result

    cout << "\nSegment Tree vs Naive Approach Comparison:" << endl;
    cout << "| Operation     | Naive Array | Segment Tree |" << endl;
    cout << "|---------------|-------------|--------------|" << endl;
    cout << "| Range Query   | O(n)        | O(log n)     |" << endl;
    cout << "| Point Update  | O(1)        | O(log n)     |" << endl;
    cout << "| Space         | O(n)        | O(n)         |" << endl;
    cout << "| Build Time    | O(1)        | O(n)         |" << endl;
}
```

**Teaching Points:**

1. **Tree on Array**: Segment tree adds logarithmic structure to arrays
2. **Range Decomposition**: Complex ranges split into simpler ones
3. **Generic Operations**: Any associative operation works
4. **Space-Time Trade-off**: O(n) extra space for O(log n) operations

---

## Day 6-7: Applications & Performance Analysis

### 📚 Theory: When to Use Advanced Trees

**Teacher's Introduction:**
"Understanding when to apply tries vs segment trees vs other structures is crucial for system design. Each specialized tree excels in specific domains but may be overkill for simple problems."

### **Implementation 1: Real-World Application Examples**

```cpp
// Application 1: Search Engine Autocomplete System
class SearchEngineAutocomplete {
private:
    AdvancedTrie trie;
    unordered_map<string, int> queryFrequency;

public:
    void addSearchQuery(const string& query) {
        queryFrequency[query]++;
        trie.insert(query, queryFrequency[query]);
    }

    vector<string> getSuggestions(const string& prefix, int maxSuggestions = 10) {
        auto suggestions = trie.autocomplete(prefix, maxSuggestions);

        vector<string> result;
        for (const auto& suggestion : suggestions) {
            result.push_back(suggestion.first);
        }

        return result;
    }

    void simulateSearchEngine() {
        cout << "\n=== SEARCH ENGINE AUTOCOMPLETE SIMULATION ===" << endl;

        // Simulate popular search queries
        vector<pair<string, int>> queries = {
            {"python programming", 1000},
            {"python tutorial", 800},
            {"python data structures", 600},
            {"java programming", 900},
            {"java tutorial", 700},
            {"javascript", 1200},
            {"javascript tutorial", 500},
            {"data structures", 400},
            {"algorithms", 350}
        };

        cout << "Adding search queries to system:" << endl;
        for (const auto& query : queries) {
            for (int i = 0; i < query.second / 100; i++) {  // Simulate frequency
                addSearchQuery(query.first);
            }
            cout << "Added: " << query.first << " (frequency: " << query.second << ")" << endl;
        }

        // Test autocomplete
        vector<string> testPrefixes = {"py", "java", "javascript", "data"};
        for (const string& prefix : testPrefixes) {
            cout << "\nAutocomplete for '" << prefix << "':" << endl;
            auto suggestions = getSuggestions(prefix, 3);
            for (int i = 0; i < suggestions.size(); i++) {
                cout << "  " << (i + 1) << ". " << suggestions[i] << endl;
            }
        }
    }
};

// Application 2: Range Query Database System
class RangeQueryDatabase {
private:
    struct Column {
        string name;
        GenericSegmentTree<long long>* segTree;
        vector<long long> data;

        Column(const string& n, const vector<long long>& d) : name(n), data(d) {
            segTree = new GenericSegmentTree<long long>(
                const_cast<vector<long long>&>(data),
                [](long long a, long long b) { return a + b; },
                0LL
            );
        }

        ~Column() {
            delete segTree;
        }
    };

    vector<unique_ptr<Column>> columns;

public:
    void addColumn(const string& name, const vector<long long>& data) {
        columns.push_back(make_unique<Column>(name, data));
        cout << "Added column '" << name << "' with " << data.size() << " rows" << endl;
    }

    long long rangeSum(const string& columnName, int startRow, int endRow) {
        for (const auto& col : columns) {
            if (col->name == columnName) {
                return col->segTree->query(startRow, endRow);
            }
        }
        throw runtime_error("Column not found: " + columnName);
    }

    void updateValue(const string& columnName, int row, long long newValue) {
        for (const auto& col : columns) {
            if (col->name == columnName) {
                col->segTree->update(row, newValue);
                return;
            }
        }
        throw runtime_error("Column not found: " + columnName);
    }

    void simulateDatabase() {
        cout << "\n=== RANGE QUERY DATABASE SIMULATION ===" << endl;

        // Create sample data
        vector<long long> sales = {1000, 1500, 800, 2200, 1800, 1200, 2500, 1900, 1100, 2000};
        vector<long long> costs = {600, 900, 500, 1300, 1100, 700, 1500, 1200, 700, 1200};

        addColumn("sales", sales);
        addColumn("costs", costs);

        cout << "\nSample queries:" << endl;

        // Range sum queries
        cout << "Sales sum for rows 0-4: " << rangeSum("sales", 0, 4) << endl;
        cout << "Costs sum for rows 5-9: " << rangeSum("costs", 5, 9) << endl;
        cout << "Total sales (all rows): " << rangeSum("sales", 0, 9) << endl;

        // Update and re-query
        cout << "\nUpdating sales[3] from 2200 to 3000..." << endl;
        updateValue("sales", 3, 3000);
        cout << "Sales sum for rows 0-4 after update: " << rangeSum("sales", 0, 4) << endl;

        cout << "\nDatabase advantages with segment trees:" << endl;
        cout << "✅ Fast range aggregations: O(log n)" << endl;
        cout << "✅ Efficient updates: O(log n)" << endl;
        cout << "✅ Multiple columns supported" << endl;
        cout << "✅ Scalable to large datasets" << endl;
    }
};

void demonstrateRealWorldApplications() {
    cout << "\n=== REAL-WORLD APPLICATIONS DEMONSTRATION ===" << endl;

    // Search engine simulation
    SearchEngineAutocomplete searchEngine;
    searchEngine.simulateSearchEngine();

    // Database simulation
    RangeQueryDatabase database;
    database.simulateDatabase();
}
```

### **Implementation 2: Performance Comparison & Decision Framework**

```cpp
class PerformanceAnalyzer {
public:
    // Compare trie vs hash table for string operations
    static void compareStringDataStructures() {
        cout << "\n=== STRING DATA STRUCTURE COMPARISON ===" << endl;

        cout << "\n| Operation          | Hash Table  | Trie        | Binary Search Tree |" << endl;
        cout << "|-------------------|-------------|-------------|-------------------|" << endl;
        cout << "| Insert            | O(1) avg    | O(m)        | O(m log n)        |" << endl;
        cout << "| Search            | O(1) avg    | O(m)        | O(m log n)        |" << endl;
        cout << "| Delete            | O(1) avg    | O(m)        | O(m log n)        |" << endl;
        cout << "| Prefix Search     | O(k)        | O(p + k)    | O(m log n)        |" << endl;
        cout << "| Autocomplete      | O(k)        | O(p + k)    | O(n)              |" << endl;
        cout << "| Memory Usage      | O(total)    | O(shared)   | O(total)          |" << endl;
        cout << "| Cache Performance | Good        | Excellent   | Poor              |" << endl;

        cout << "\nWhere:" << endl;
        cout << "  m = average string length" << endl;
        cout << "  n = number of strings" << endl;
        cout << "  p = prefix length" << endl;
        cout << "  k = number of results" << endl;

        cout << "\n🎯 Choose Trie when:" << endl;
        cout << "✅ Frequent prefix operations" << endl;
        cout << "✅ Autocomplete functionality needed" << endl;
        cout << "✅ Memory efficiency important (shared prefixes)" << endl;
        cout << "✅ Lexicographic ordering required" << endl;

        cout << "\n🎯 Choose Hash Table when:" << endl;
        cout << "✅ Only exact matches needed" << endl;
        cout << "✅ Maximum performance for simple operations" << endl;
        cout << "✅ No prefix operations required" << endl;
    }

    // Compare segment tree vs other range query solutions
    static void compareRangeQueryStructures() {
        cout << "\n=== RANGE QUERY DATA STRUCTURE COMPARISON ===" << endl;

        cout << "\n| Operation      | Naive Array | Prefix Sum | Sqrt Decomp | Segment Tree | Fenwick Tree |" << endl;
        cout << "|----------------|-------------|------------|-------------|--------------|--------------|" << endl;
        cout << "| Build          | O(1)        | O(n)       | O(n)        | O(n)         | O(n)         |" << endl;
        cout << "| Range Query    | O(n)        | O(1)       | O(√n)       | O(log n)     | O(log n)     |" << endl;
        cout << "| Point Update   | O(1)        | O(n)       | O(1)        | O(log n)     | O(log n)     |" << endl;
        cout << "| Range Update   | O(n)        | O(n)       | O(√n)       | O(log n)*    | O(log n)*    |" << endl;
        cout << "| Space          | O(n)        | O(n)       | O(n)        | O(n)         | O(n)         |" << endl;
        cout << "| Implementation | Trivial     | Easy       | Medium      | Hard         | Medium       |" << endl;

        cout << "\n* With lazy propagation" << endl;

        cout << "\n🎯 Choose Segment Tree when:" << endl;
        cout << "✅ Both queries and updates are frequent" << endl;
        cout << "✅ Need general associative operations (min, max, gcd)" << endl;
        cout << "✅ Range updates required" << endl;
        cout << "✅ 2D or higher dimensional queries needed" << endl;

        cout << "\n🎯 Choose Fenwick Tree when:" << endl;
        cout << "✅ Only sum/XOR queries needed" << endl;
        cout << "✅ Simpler implementation preferred" << endl;
        cout << "✅ Lower memory usage important" << endl;

        cout << "\n🎯 Choose Sqrt Decomposition when:" << endl;
        cout << "✅ Learning/prototyping phase" << endl;
        cout << "✅ Complex queries that don't fit segment tree model" << endl;
        cout << "✅ Implementation time is limited" << endl;
    }

    // Decision framework for choosing data structures
    static void provideDecisionFramework() {
        cout << "\n=== DATA STRUCTURE DECISION FRAMEWORK ===" << endl;

        cout << "\n📊 Problem Type Analysis:" << endl;
        cout << "\n1. STRING PROCESSING PROBLEMS:" << endl;
        cout << "   🔍 Exact string matching only → Hash Table" << endl;
        cout << "   🔍 Prefix operations needed → Trie" << endl;
        cout << "   🔍 Autocomplete required → Trie" << endl;
        cout << "   🔍 Spell checking → Trie with edit distance" << endl;
        cout << "   🔍 IP routing → Compressed Trie" << endl;

        cout << "\n2. RANGE QUERY PROBLEMS:" << endl;
        cout << "   📈 Read-only range sums → Prefix Sum Array" << endl;
        cout << "   📈 Frequent updates + range sums → Fenwick Tree" << endl;
        cout << "   📈 Range min/max queries → Segment Tree" << endl;
        cout << "   📈 Complex range operations → Segment Tree" << endl;
        cout << "   📈 2D range queries → 2D Segment Tree" << endl;

        cout << "\n3. GENERAL CONSIDERATIONS:" << endl;
        cout << "   ⚡ Implementation time limited → Choose simpler structure" << endl;
        cout << "   ⚡ Memory constrained → Choose space-efficient option" << endl;
        cout << "   ⚡ Performance critical → Profile different options" << endl;
        cout << "   ⚡ Maintainability important → Choose well-known patterns" << endl;

        cout << "\n💡 OPTIMIZATION TIPS:" << endl;
        cout << "• Start with simpler solution, optimize if needed" << endl;
        cout << "• Consider hybrid approaches for complex problems" << endl;
        cout << "• Cache-friendly access patterns matter in practice" << endl;
        cout << "• Don't over-engineer for small datasets" << endl;
    }
};

void demonstratePerformanceAnalysis() {
    PerformanceAnalyzer::compareStringDataStructures();
    PerformanceAnalyzer::compareRangeQueryStructures();
    PerformanceAnalyzer::provideDecisionFramework();
}
```

---

## Main Function & Testing Framework

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>
#include <functional>
#include <memory>
#include <climits>
using namespace std;

// Forward declarations
void demonstrateBasicTrieOperations();
void demonstrateAdvancedTrieOperations();
void demonstrateSegmentTreeBasics();
void demonstrateAdvancedSegmentTrees();
void demonstrateRealWorldApplications();
void demonstratePerformanceAnalysis();

int main() {
    cout << "🌳 Week 6: Advanced Tree Topics (Tries & Segment Trees)" << endl;
    cout << "======================================================" << endl;

    // Day 1-3: Trie Implementation & String Operations
    cout << "\n📅 DAY 1-3: TRIE IMPLEMENTATION & STRING OPERATIONS" << endl;
    demonstrateBasicTrieOperations();
    demonstrateAdvancedTrieOperations();

    // Day 4-5: Segment Trees for Range Queries
    cout << "\n📅 DAY 4-5: SEGMENT TREES FOR RANGE QUERIES" << endl;
    demonstrateSegmentTreeBasics();
    demonstrateAdvancedSegmentTrees();

    // Day 6-7: Applications & Performance Analysis
    cout << "\n📅 DAY 6-7: APPLICATIONS & PERFORMANCE ANALYSIS" << endl;
    demonstrateRealWorldApplications();
    demonstratePerformanceAnalysis();

    cout << "\n🎉 Week 6 Advanced Tree Topics Mastery Complete!" << endl;
    cout << "Next: Week 7 - Tree Algorithm Applications & Advanced Techniques" << endl;

    return 0;
}
```

---

## 📚 Week 6 Summary & Key Takeaways

### **Advanced Tree Concepts Mastered:**

1. **Trie (Prefix Tree) Mastery**

   - Character-by-character string decomposition
   - Prefix sharing for memory efficiency
   - O(m) operations independent of dictionary size
   - Applications: autocomplete, spell checking, IP routing

2. **Segment Tree Expertise**

   - Range query optimization from O(n) to O(log n)
   - Tree-on-array structure for efficient range operations
   - Generic design supporting any associative operation
   - Applications: database systems, computational geometry

3. **Specialized vs General-Purpose Trade-offs**
   - When to use specialized trees vs hash tables/arrays
   - Space-time complexity analysis for different scenarios
   - Implementation complexity vs performance benefits

### **Critical Algorithm Patterns:**

- **Prefix Decomposition**: Breaking strings into character paths
- **Range Decomposition**: Splitting complex ranges into tree traversals
- **Tree-on-Data**: Adding tree structure to existing data for efficiency
- **Specialization Benefits**: Purpose-built structures vs general solutions

### **Real-World Applications:**

- **Search Engines**: Autocomplete and suggestion systems
- **Databases**: Fast range aggregations and analytics
- **Network Systems**: IP routing and prefix matching
- **Text Processing**: Dictionary operations and spell checking

### **Student Assessment Rubric**

**Advanced Tree Fundamentals (Must Have):**

- [ ] Implements complete trie with insert/search/delete operations
- [ ] Builds segment tree with range queries and point updates
- [ ] Understands space-time trade-offs for specialized structures
- [ ] Can choose appropriate tree type for given problem domain

**Expert Implementation Skills (Should Have):**

- [ ] Implements advanced trie operations (autocomplete, word break)
- [ ] Masters generic segment tree supporting different operations
- [ ] Optimizes memory usage with efficient node representations
- [ ] Integrates trees into larger system architectures

**Professional Knowledge (Nice to Have):**

- [ ] Understands when specialization is worth implementation complexity
- [ ] Can design hybrid solutions combining multiple tree types
- [ ] Optimizes for real-world constraints (cache, memory, latency)
- [ ] Connects tree concepts to industry applications

### 🎯 Common Mistakes to Avoid

1. **Over-Engineering**: Using complex trees for simple problems
2. **Memory Leaks**: Improper cleanup in tree destructors
3. **Index Confusion**: Segment tree array indexing errors
4. **Alphabet Assumptions**: Hardcoding alphabet size in trie implementations

### 🗣️ Advanced Discussion Questions

1. "When is the implementation complexity of segment trees justified over simpler solutions?"
2. "How do tries enable fuzzy string matching and spell correction?"
3. "What are the practical limits of segment tree depth and performance?"
4. "How do modern databases use tree-like structures for query optimization?"

---

## Next Week Preview: Tree Algorithm Applications

Week 7 will integrate all tree knowledge into advanced applications:

- **Tree-Based Algorithms**: Complex algorithms leveraging multiple tree types
- **System Design**: Using trees in distributed systems and databases
- **Advanced Optimization**: Memory management, cache optimization, persistence
- **Integration Patterns**: Combining trees with other data structures

**🌟 Congratulations on mastering Advanced Tree Topics! You now have the tools to solve complex string processing and range query problems efficiently.**

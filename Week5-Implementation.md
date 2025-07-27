# Week 5: Heaps & Priority Queues - Teacher's Guide

This document provides comprehensive teaching materials for Week 5, focusing on heap data structures and their primary application in priority queues. This week transitions from tree-based searching structures to tree-based ordering structures, introducing a fundamentally different approach to organizing data.

---

## 🎯 Learning Objectives for Week 5

By the end of this week, students should be able to:

1. **Conceptual Understanding**: Grasp heap properties and complete binary tree structure
2. **Implementation Skills**: Build heaps from scratch using arrays with mathematical index relationships
3. **Algorithm Analysis**: Understand why heapify operations are O(log n) and build-heap is O(n)
4. **Optimization**: Implement linear-time heap construction using bottom-up approach
5. **Application Recognition**: Identify when heaps solve problems optimally (priority queues, sorting, selection)
6. **STL Proficiency**: Use priority_queue effectively with custom comparators
7. **Problem Solving**: Apply heaps to various algorithmic challenges like top-K problems and graph algorithms

---

## Day 1-2: Heap Fundamentals & Array Implementation

### 📚 Theory: From Trees to Heaps

**Teacher's Introduction:**
"After mastering search trees, we now explore ordering trees. Heaps don't maintain sorted order like BSTs, but they guarantee that the most important element (max or min) is always at the top. This makes them perfect for priority-based systems."

### 🔧 The Heap Property

**Heap Definition:**
A heap is a **complete binary tree** that satisfies the **heap property**:
- **Max-Heap**: Parent ≥ All children (root = maximum)
- **Min-Heap**: Parent ≤ All children (root = minimum)

**Key Insight**: Unlike BSTs, heaps only care about parent-child relationships, not global ordering!

### **Implementation 1: Basic Heap Structure & Array Mathematics**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

class MaxHeap {
private:
    vector<int> heap;
    
    // Mathematical relationships for array-based tree
    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }
    
    // Check if index is valid
    bool hasParent(int i) { return i > 0; }
    bool hasLeftChild(int i) { return leftChild(i) < heap.size(); }
    bool hasRightChild(int i) { return rightChild(i) < heap.size(); }
    
public:
    MaxHeap() {}
    
    // Constructor from array
    MaxHeap(vector<int>& arr) {
        heap = arr;
        buildHeap();
    }
    
    // Basic operations
    bool isEmpty() { return heap.empty(); }
    int size() { return heap.size(); }
    
    // Peek at maximum (root)
    int peek() {
        if (isEmpty()) throw runtime_error("Heap is empty");
        return heap[0];
    }
    
    // Display heap as array
    void displayArray() {
        cout << "Heap array: [";
        for (int i = 0; i < heap.size(); i++) {
            cout << heap[i];
            if (i < heap.size() - 1) cout << ", ";
        }
        cout << "]" << endl;
    }
    
    // Display mathematical relationships
    void explainArrayMath() {
        cout << "\n=== Array Mathematics Explanation ===" << endl;
        for (int i = 0; i < min(7, (int)heap.size()); i++) {
            cout << "Index " << i << " (value " << heap[i] << "):" << endl;
            
            if (hasParent(i)) {
                int p = parent(i);
                cout << "  Parent: index " << p << " (value " << heap[p] << ")" << endl;
            } else {
                cout << "  Parent: None (this is root)" << endl;
            }
            
            if (hasLeftChild(i)) {
                int l = leftChild(i);
                cout << "  Left child: index " << l << " (value " << heap[l] << ")" << endl;
            }
            
            if (hasRightChild(i)) {
                int r = rightChild(i);
                cout << "  Right child: index " << r << " (value " << heap[r] << ")" << endl;
            }
            cout << endl;
        }
    }
    
    // Check if heap property is satisfied
    bool isValidHeap() {
        for (int i = 0; i < heap.size(); i++) {
            if (hasLeftChild(i) && heap[i] < heap[leftChild(i)]) {
                cout << "Heap property violated at index " << i 
                     << ": parent " << heap[i] 
                     << " < left child " << heap[leftChild(i)] << endl;
                return false;
            }
            if (hasRightChild(i) && heap[i] < heap[rightChild(i)]) {
                cout << "Heap property violated at index " << i 
                     << ": parent " << heap[i] 
                     << " < right child " << heap[rightChild(i)] << endl;
                return false;
            }
        }
        return true;
    }
    
    // Calculate heap statistics
    void analyzeHeap() {
        if (isEmpty()) {
            cout << "Empty heap" << endl;
            return;
        }
        
        int height = 0;
        int nodes = heap.size();
        int temp = nodes;
        while (temp > 1) {
            temp /= 2;
            height++;
        }
        
        cout << "\n=== Heap Analysis ===" << endl;
        cout << "Number of nodes: " << nodes << endl;
        cout << "Height: " << height << endl;
        cout << "Theoretical minimum height: " << (int)log2(nodes) << endl;
        cout << "Is complete binary tree: " << isCompleteTree() << endl;
        cout << "Satisfies heap property: " << isValidHeap() << endl;
        cout << "Maximum element: " << peek() << endl;
    }
    
private:
    bool isCompleteTree() {
        // In array representation, heap is complete if no gaps
        return true;  // Array representation guarantees completeness
    }
    
    void buildHeap() {
        // Will implement in next section
        for (int i = heap.size() / 2 - 1; i >= 0; i--) {
            heapifyDown(i);
        }
    }
    
    void heapifyDown(int index); // Forward declaration
};

void demonstrateHeapBasics() {
    cout << "\n=== HEAP FUNDAMENTALS DEMONSTRATION ===" << endl;
    
    // Create sample data
    vector<int> data = {100, 19, 36, 17, 3, 25, 1, 2, 7};
    
    cout << "Original array: ";
    for (int val : data) cout << val << " ";
    cout << endl;
    
    // Create heap (will be built properly)
    MaxHeap heap(data);
    
    cout << "\nAfter heap construction:" << endl;
    heap.displayArray();
    heap.analyzeHeap();
    
    // Demonstrate array mathematics
    heap.explainArrayMath();
    
    cout << "\nKey Concepts:" << endl;
    cout << "1. Complete Binary Tree: All levels filled except possibly last" << endl;
    cout << "2. Array Representation: No pointers needed, use math formulas" << endl;
    cout << "3. Heap Property: Parent-child ordering constraint" << endl;
    cout << "4. Height: Always O(log n) due to complete tree structure" << endl;
}
```

### **Implementation 2: Heap Visualization & Structure Understanding**

```cpp
class HeapVisualizer {
public:
    // Print heap as tree structure
    static void printHeapTree(const vector<int>& heap) {
        if (heap.empty()) {
            cout << "Empty heap" << endl;
            return;
        }
        
        int height = (int)log2(heap.size()) + 1;
        int index = 0;
        
        cout << "\nHeap Tree Structure:" << endl;
        printLevel(heap, 0, height, 0);
    }
    
    // Print level by level with proper spacing
    static void printLevel(const vector<int>& heap, int index, int height, int level) {
        if (index >= heap.size()) return;
        
        int nodesInLevel = 1 << level;  // 2^level
        int spacing = (1 << (height - level)) - 1;
        
        // Print current level
        for (int i = 0; i < nodesInLevel && index < heap.size(); i++) {
            // Print spacing
            for (int j = 0; j < spacing; j++) cout << "  ";
            
            cout << heap[index];
            index++;
            
            // Print spacing between nodes
            for (int j = 0; j < spacing; j++) cout << "  ";
            cout << " ";
        }
        cout << endl;
        
        // Print connections if not last level
        if (level < height - 1 && index < heap.size()) {
            for (int i = 0; i < spacing/2; i++) cout << " ";
            for (int i = 0; i < nodesInLevel && (index - nodesInLevel + i) < heap.size(); i++) {
                cout << "/ \\ ";
                for (int j = 0; j < spacing; j++) cout << "  ";
            }
            cout << endl;
        }
    }
    
    // Compare heap vs regular binary tree
    static void compareStructures() {
        cout << "\n=== HEAP vs BINARY TREE COMPARISON ===" << endl;
        
        cout << "\nRegular Binary Tree (BST-like):" << endl;
        cout << "      50" << endl;
        cout << "     /  \\" << endl;
        cout << "   30    70" << endl;
        cout << "  /  \\  /  \\" << endl;
        cout << " 20  40 60  80" << endl;
        cout << "Properties: Left < Root < Right (global ordering)" << endl;
        
        cout << "\nMax-Heap:" << endl;
        cout << "      100" << endl;
        cout << "     /   \\" << endl;
        cout << "   80     90" << endl;
        cout << "  /  \\   /  \\" << endl;
        cout << " 70  60 50  40" << endl;
        cout << "Properties: Parent ≥ Children (local ordering only)" << endl;
        
        cout << "\nKey Differences:" << endl;
        cout << "• BST: Global ordering enables search" << endl;
        cout << "• Heap: Local ordering enables priority access" << endl;
        cout << "• BST: Can be unbalanced" << endl;
        cout << "• Heap: Always complete (balanced by definition)" << endl;
        cout << "• BST: Complex rebalancing" << endl;
        cout << "• Heap: Simple heapify operations" << endl;
    }
    
    // Demonstrate why array representation works
    static void explainArrayAdvantages() {
        cout << "\n=== WHY ARRAY REPRESENTATION? ===" << endl;
        
        cout << "\nPointer-based tree:" << endl;
        cout << "struct TreeNode {" << endl;
        cout << "    int data;           // 4 bytes" << endl;
        cout << "    TreeNode* left;     // 8 bytes" << endl;
        cout << "    TreeNode* right;    // 8 bytes" << endl;
        cout << "};                      // Total: 20 bytes per node" << endl;
        
        cout << "\nArray-based heap:" << endl;
        cout << "vector<int> heap;       // 4 bytes per element" << endl;
        cout << "// Navigation via math: parent(i) = (i-1)/2" << endl;
        
        cout << "\nAdvantages of Array Representation:" << endl;
        cout << "1. Memory: 75% less memory usage (no pointers)" << endl;
        cout << "2. Cache: Better locality of reference" << endl;
        cout << "3. Navigation: O(1) parent/child access via math" << endl;
        cout << "4. Simplicity: No pointer management" << endl;
        cout << "5. Complete tree: Guaranteed by array structure" << endl;
    }
};

void demonstrateHeapVisualization() {
    cout << "\n=== HEAP VISUALIZATION & STRUCTURE ===" << endl;
    
    // Create sample heaps
    vector<int> maxHeapData = {100, 80, 90, 70, 60, 50, 40, 30, 20};
    vector<int> minHeapData = {1, 3, 2, 7, 8, 5, 6, 10, 9};
    
    cout << "Max-Heap Example:" << endl;
    HeapVisualizer::printHeapTree(maxHeapData);
    
    cout << "\nMin-Heap Example:" << endl;
    HeapVisualizer::printHeapTree(minHeapData);
    
    // Compare structures
    HeapVisualizer::compareStructures();
    
    // Explain array advantages
    HeapVisualizer::explainArrayAdvantages();
    
    cout << "\nMemory Layout Visualization:" << endl;
    cout << "Array:  [100, 80, 90, 70, 60, 50, 40]" << endl;
    cout << "Index:   0   1   2   3   4   5   6" << endl;
    cout << "         |   |   |   |   |   |   |" << endl;
    cout << "Tree:   100-+-+-+-+-+-+-+" << endl;
    cout << "           / \\           " << endl;
    cout << "         80   90         " << endl;
    cout << "        / \\ / \\         " << endl;
    cout << "       70 60 50 40       " << endl;
}
```

**Teaching Points:**
1. **Complete Tree**: Always balanced by definition
2. **Array Math**: Parent/child relationships via formulas
3. **Memory Efficiency**: No pointers needed
4. **Heap vs BST**: Different ordering guarantees

---

## Day 3-4: Heapify Operations & Dynamic Heap Maintenance

### 📚 Theory: The Heapify Operations

**Teacher's Introduction:**
"Heapify operations are the heart of heap maintenance. These elegant algorithms restore heap property when it's violated, enabling efficient insertion and extraction operations."

### **Implementation 1: Heapify-Up (Bubble-Up) Operation**

```cpp
class HeapOperations {
private:
    vector<int> heap;
    
    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }
    bool hasParent(int i) { return i > 0; }
    bool hasLeftChild(int i) { return leftChild(i) < heap.size(); }
    bool hasRightChild(int i) { return rightChild(i) < heap.size(); }
    
public:
    // Insert operation with heapify-up
    void insert(int key) {
        cout << "\n=== Inserting " << key << " ===" << endl;
        
        // Step 1: Add element at the end
        heap.push_back(key);
        int index = heap.size() - 1;
        
        cout << "Step 1: Added " << key << " at index " << index << endl;
        displayArray();
        
        // Step 2: Heapify-up to restore heap property
        cout << "Step 2: Heapify-up to maintain max-heap property" << endl;
        heapifyUp(index);
        
        cout << "Final state after insertion:" << endl;
        displayArray();
    }
    
    // Heapify-up (bubble-up) operation
    void heapifyUp(int index) {
        cout << "\nHeapify-up starting from index " << index 
             << " (value " << heap[index] << ")" << endl;
        
        while (hasParent(index)) {
            int parentIndex = parent(index);
            
            cout << "Comparing " << heap[index] << " with parent " 
                 << heap[parentIndex] << " at index " << parentIndex << endl;
            
            // Max-heap: if parent >= child, we're done
            if (heap[parentIndex] >= heap[index]) {
                cout << "Heap property satisfied. Stopping." << endl;
                break;
            }
            
            // Swap with parent
            cout << "Swapping " << heap[index] << " with " << heap[parentIndex] << endl;
            swap(heap[index], heap[parentIndex]);
            index = parentIndex;
            
            displayArray();
        }
        
        if (index == 0) {
            cout << "Reached root. Heapify-up complete." << endl;
        }
    }
    
    // Extract maximum (root) with heapify-down
    int extractMax() {
        if (heap.empty()) {
            throw runtime_error("Heap is empty");
        }
        
        cout << "\n=== Extracting Maximum ===" << endl;
        
        int maxValue = heap[0];
        cout << "Maximum value to extract: " << maxValue << endl;
        
        // Step 1: Replace root with last element
        heap[0] = heap.back();
        heap.pop_back();
        
        cout << "Step 1: Replaced root with last element" << endl;
        if (!heap.empty()) {
            displayArray();
            
            // Step 2: Heapify-down to restore heap property
            cout << "Step 2: Heapify-down from root" << endl;
            heapifyDown(0);
        }
        
        cout << "Final state after extraction:" << endl;
        displayArray();
        
        return maxValue;
    }
    
    // Heapify-down (bubble-down) operation
    void heapifyDown(int index) {
        cout << "\nHeapify-down starting from index " << index 
             << " (value " << heap[index] << ")" << endl;
        
        while (hasLeftChild(index)) {
            // Find largest among node and its children
            int largestIndex = index;
            int leftIndex = leftChild(index);
            int rightIndex = rightChild(index);
            
            cout << "Checking children of index " << index << ":" << endl;
            
            if (heap[leftIndex] > heap[largestIndex]) {
                largestIndex = leftIndex;
                cout << "  Left child " << heap[leftIndex] 
                     << " is larger than parent" << endl;
            }
            
            if (hasRightChild(index) && heap[rightIndex] > heap[largestIndex]) {
                largestIndex = rightIndex;
                cout << "  Right child " << heap[rightIndex] 
                     << " is largest" << endl;
            }
            
            // If heap property is satisfied, stop
            if (largestIndex == index) {
                cout << "Heap property satisfied. Stopping." << endl;
                break;
            }
            
            // Swap with largest child
            cout << "Swapping " << heap[index] << " with " 
                 << heap[largestIndex] << " at index " << largestIndex << endl;
            swap(heap[index], heap[largestIndex]);
            index = largestIndex;
            
            displayArray();
        }
        
        cout << "Heapify-down complete." << endl;
    }
    
    // Utility functions
    void displayArray() {
        cout << "Heap: [";
        for (int i = 0; i < heap.size(); i++) {
            cout << heap[i];
            if (i < heap.size() - 1) cout << ", ";
        }
        cout << "]" << endl;
    }
    
    bool isEmpty() { return heap.empty(); }
    int peek() { return heap.empty() ? -1 : heap[0]; }
    int size() { return heap.size(); }
};

void demonstrateHeapifyOperations() {
    cout << "\n=== HEAPIFY OPERATIONS DEMONSTRATION ===" << endl;
    
    HeapOperations heap;
    
    // Demonstrate insertions with heapify-up
    vector<int> insertSequence = {50, 30, 70, 20, 60, 80, 40};
    
    cout << "Inserting sequence: ";
    for (int val : insertSequence) {
        cout << val << " ";
    }
    cout << endl;
    
    for (int val : insertSequence) {
        heap.insert(val);
    }
    
    cout << "\n" << string(50, '=') << endl;
    
    // Demonstrate extractions with heapify-down
    cout << "Extracting all elements (should come out in sorted order):" << endl;
    
    while (!heap.isEmpty()) {
        int max = heap.extractMax();
        cout << "Extracted: " << max << endl;
    }
    
    cout << "\nKey Observations:" << endl;
    cout << "• Heapify-up: Used for insertion, goes from child to parent" << endl;
    cout << "• Heapify-down: Used for extraction, goes from parent to children" << endl;
    cout << "• Both operations: O(log n) time complexity" << endl;
    cout << "• Extracted elements come out in sorted order (heap sort!)" << endl;
}
```

### **Implementation 2: Complexity Analysis & Performance Metrics**

```cpp
class HeapComplexityAnalyzer {
public:
    // Analyze heapify operation complexity
    static void analyzeHeapifyComplexity() {
        cout << "\n=== HEAPIFY COMPLEXITY ANALYSIS ===" << endl;
        
        cout << "\nHeapify-Up Analysis:" << endl;
        cout << "• Worst case: Element bubbles from leaf to root" << endl;
        cout << "• Distance traveled: Height of tree = log₂(n)" << endl;
        cout << "• Operations per level: 1 comparison + 1 potential swap" << endl;
        cout << "• Time complexity: O(log n)" << endl;
        cout << "• Space complexity: O(1) iterative, O(log n) recursive" << endl;
        
        cout << "\nHeapify-Down Analysis:" << endl;
        cout << "• Worst case: Element sinks from root to leaf" << endl;
        cout << "• Distance traveled: Height of tree = log₂(n)" << endl;
        cout << "• Operations per level: 2 comparisons + 1 potential swap" << endl;
        cout << "• Time complexity: O(log n)" << endl;
        cout << "• Space complexity: O(1) iterative, O(log n) recursive" << endl;
        
        demonstrateComplexityWithExample();
    }
    
    static void demonstrateComplexityWithExample() {
        cout << "\nComplexity Demonstration with n=15 nodes:" << endl;
        cout << "Tree height: ⌊log₂(15)⌋ = 3" << endl;
        cout << "Maximum swaps in heapify-up: 3" << endl;
        cout << "Maximum swaps in heapify-down: 3" << endl;
        
        cout << "\nScaling analysis:" << endl;
        vector<int> sizes = {15, 31, 63, 127, 255, 511, 1023};
        
        cout << "| Nodes | Height | Max Swaps |" << endl;
        cout << "|-------|--------|-----------|" << endl;
        
        for (int n : sizes) {
            int height = (int)log2(n);
            printf("| %5d | %6d | %9d |\n", n, height, height);
        }
        
        cout << "\nObservation: As n doubles, height increases by only 1!" << endl;
        cout << "This logarithmic growth is why heaps are efficient." << endl;
    }
    
    // Compare heap operations with other data structures
    static void compareDataStructures() {
        cout << "\n=== HEAP vs OTHER DATA STRUCTURES ===" << endl;
        
        cout << "\nOperation Comparison:" << endl;
        cout << "| Operation    | Unsorted Array | Sorted Array | Binary Heap | BST (avg) |" << endl;
        cout << "|--------------|----------------|--------------|-------------|-----------|" << endl;
        cout << "| Find Max/Min | O(n)           | O(1)         | O(1)        | O(log n)  |" << endl;
        cout << "| Extract Max  | O(n)           | O(n)         | O(log n)    | O(log n)  |" << endl;
        cout << "| Insert       | O(1)           | O(n)         | O(log n)    | O(log n)  |" << endl;
        cout << "| Build from n | O(n)           | O(n log n)   | O(n)        | O(n log n)|" << endl;
        
        cout << "\nHeap Advantages:" << endl;
        cout << "✅ O(1) access to priority element" << endl;
        cout << "✅ O(log n) insertion and extraction" << endl;
        cout << "✅ O(n) heap construction from array" << endl;
        cout << "✅ Space efficient (array-based)" << endl;
        cout << "✅ Cache-friendly memory access" << endl;
        
        cout << "\nHeap Limitations:" << endl;
        cout << "❌ No efficient search for arbitrary elements" << endl;
        cout << "❌ No range queries" << endl;
        cout << "❌ Only priority element easily accessible" << endl;
    }
};

void demonstrateComplexityAnalysis() {
    HeapComplexityAnalyzer::analyzeHeapifyComplexity();
    HeapComplexityAnalyzer::compareDataStructures();
}
```

**Teaching Points:**
1. **Heapify Direction**: Up for insertion, down for extraction
2. **Logarithmic Complexity**: Due to complete tree height
3. **Comparison Strategy**: Parent vs children determines movement
4. **Performance**: Efficient priority queue operations

---

## Day 5-6: Build Heap Algorithm & Heap Sort

### 📚 Theory: The Linear-Time Build Heap

**Teacher's Introduction:**
"One of the most beautiful algorithms in computer science: building a heap from an unsorted array in O(n) time. This seems impossible since n insertions should take O(n log n), but mathematics reveals a surprising optimization."

### **Implementation 1: Build Heap - Bottom-Up vs Top-Down**

```cpp
class HeapBuilder {
private:
    vector<int> heap;
    int comparisons;
    int swaps;
    
    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }
    bool hasLeftChild(int i) { return leftChild(i) < heap.size(); }
    bool hasRightChild(int i) { return rightChild(i) < heap.size(); }
    
public:
    // Top-down approach: Insert elements one by one
    void buildHeapTopDown(vector<int> arr) {
        cout << "\n=== TOP-DOWN HEAP CONSTRUCTION ===" << endl;
        cout << "Building heap by inserting elements one by one..." << endl;
        
        heap.clear();
        comparisons = 0;
        swaps = 0;
        
        for (int i = 0; i < arr.size(); i++) {
            cout << "\nInserting element " << (i+1) << ": " << arr[i] << endl;
            insertWithCount(arr[i]);
            displayArray();
        }
        
        cout << "\nTop-down statistics:" << endl;
        cout << "Total comparisons: " << comparisons << endl;
        cout << "Total swaps: " << swaps << endl;
        cout << "Time complexity: O(n log n)" << endl;
    }
    
    // Bottom-up approach: Start from last non-leaf and heapify down
    void buildHeapBottomUp(vector<int> arr) {
        cout << "\n=== BOTTOM-UP HEAP CONSTRUCTION ===" << endl;
        cout << "Building heap by heapifying from bottom to top..." << endl;
        
        heap = arr;
        comparisons = 0;
        swaps = 0;
        
        cout << "Original array: ";
        displayArray();
        
        // Start from last non-leaf node
        int lastNonLeaf = heap.size() / 2 - 1;
        cout << "\nLast non-leaf node at index: " << lastNonLeaf << endl;
        
        for (int i = lastNonLeaf; i >= 0; i--) {
            cout << "\nHeapifying subtree rooted at index " << i 
                 << " (value " << heap[i] << ")" << endl;
            heapifyDownWithCount(i);
            displayArray();
        }
        
        cout << "\nBottom-up statistics:" << endl;
        cout << "Total comparisons: " << comparisons << endl;
        cout << "Total swaps: " << swaps << endl;
        cout << "Time complexity: O(n)" << endl;
    }
    
    // Mathematical proof of O(n) complexity for bottom-up
    void explainLinearComplexity() {
        cout << "\n=== WHY BOTTOM-UP IS O(n)? ===" << endl;
        
        int n = heap.size();
        int height = (int)log2(n);
        
        cout << "Mathematical analysis for n = " << n << " nodes:" << endl;
        cout << "Tree height: " << height << endl;
        
        cout << "\nNodes by height level:" << endl;
        cout << "| Height | Max Nodes | Work per Node | Total Work |" << endl;
        cout << "|--------|-----------|---------------|------------|" << endl;
        
        int totalWork = 0;
        for (int h = 0; h <= height; h++) {
            int maxNodes = n / (1 << (h + 1));  // n / 2^(h+1)
            int workPerNode = h;
            int totalAtLevel = maxNodes * workPerNode;
            totalWork += totalAtLevel;
            
            printf("| %6d | %9d | %13d | %10d |\n", 
                   h, maxNodes, workPerNode, totalAtLevel);
        }
        
        cout << "\nTotal work: " << totalWork << " operations" << endl;
        cout << "This forms a geometric series that sums to O(n)" << endl;
        
        cout << "\nKey insight: Most nodes are leaves (height 0)" << endl;
        cout << "Leaves require no work, so we save significantly!" << endl;
    }
    
private:
    void insertWithCount(int key) {
        heap.push_back(key);
        heapifyUpWithCount(heap.size() - 1);
    }
    
    void heapifyUpWithCount(int index) {
        while (index > 0) {
            int parentIndex = parent(index);
            comparisons++;
            
            if (heap[parentIndex] >= heap[index]) break;
            
            swap(heap[index], heap[parentIndex]);
            swaps++;
            index = parentIndex;
        }
    }
    
    void heapifyDownWithCount(int index) {
        while (hasLeftChild(index)) {
            int largestIndex = index;
            int leftIndex = leftChild(index);
            
            comparisons++;
            if (heap[leftIndex] > heap[largestIndex]) {
                largestIndex = leftIndex;
            }
            
            if (hasRightChild(index)) {
                int rightIndex = rightChild(index);
                comparisons++;
                if (heap[rightIndex] > heap[largestIndex]) {
                    largestIndex = rightIndex;
                }
            }
            
            if (largestIndex == index) break;
            
            swap(heap[index], heap[largestIndex]);
            swaps++;
            index = largestIndex;
        }
    }
    
    void displayArray() {
        cout << "Heap: [";
        for (int i = 0; i < heap.size(); i++) {
            cout << heap[i];
            if (i < heap.size() - 1) cout << ", ";
        }
        cout << "]" << endl;
    }
};

void demonstrateBuildHeap() {
    cout << "\n=== BUILD HEAP ALGORITHMS COMPARISON ===" << endl;
    
    vector<int> testData = {4, 10, 3, 5, 1, 15, 6, 17, 8};
    
    cout << "Input array: ";
    for (int val : testData) cout << val << " ";
    cout << endl;
    
    // Compare both approaches
    HeapBuilder builder;
    
    builder.buildHeapTopDown(testData);
    builder.buildHeapBottomUp(testData);
    builder.explainLinearComplexity();
}
```

### **Implementation 2: Heap Sort Implementation**

```cpp
class HeapSort {
private:
    vector<int> arr;
    int comparisons = 0;
    int swaps = 0;
    
    void heapify(int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        comparisons++;
        if (left < n && arr[left] > arr[largest]) {
            largest = left;
        }
        
        comparisons++;
        if (right < n && arr[right] > arr[largest]) {
            largest = right;
        }
        
        if (largest != i) {
            swap(arr[i], arr[largest]);
            swaps++;
            heapify(n, largest);
        }
    }
    
public:
    vector<int> heapSort(vector<int> input) {
        arr = input;
        int n = arr.size();
        comparisons = 0;
        swaps = 0;
        
        cout << "\n=== HEAP SORT ALGORITHM ===" << endl;
        cout << "Input array: ";
        printArray(n);
        
        // Phase 1: Build max heap (O(n))
        cout << "\nPhase 1: Building max heap..." << endl;
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(n, i);
        }
        cout << "Max heap built: ";
        printArray(n);
        
        // Phase 2: Extract elements one by one (O(n log n))
        cout << "\nPhase 2: Extracting elements in sorted order..." << endl;
        for (int i = n - 1; i > 0; i--) {
            // Move current root to end
            swap(arr[0], arr[i]);
            swaps++;
            
            cout << "Moved " << arr[i] << " to position " << i << ": ";
            printArrayWithSeparator(i);
            
            // Heapify reduced heap
            heapify(i, 0);
            cout << "After heapify: ";
            printArrayWithSeparator(i);
        }
        
        cout << "\nFinal sorted array: ";
        printArray(n);
        
        cout << "\nHeap Sort Statistics:" << endl;
        cout << "Total comparisons: " << comparisons << endl;
        cout << "Total swaps: " << swaps << endl;
        cout << "Time complexity: O(n log n)" << endl;
        cout << "Space complexity: O(1) - in-place sorting" << endl;
        
        return arr;
    }
    
    // Compare with other sorting algorithms
    void compareWithOtherSorts() {
        cout << "\n=== SORTING ALGORITHMS COMPARISON ===" << endl;
        
        cout << "| Algorithm    | Best Case | Average Case | Worst Case | Space | Stable |" << endl;
        cout << "|--------------|-----------|--------------|------------|-------|--------|" << endl;
        cout << "| Heap Sort    | O(n log n)| O(n log n)   | O(n log n) | O(1)  | No     |" << endl;
        cout << "| Quick Sort   | O(n log n)| O(n log n)   | O(n²)      | O(log)| No     |" << endl;
        cout << "| Merge Sort   | O(n log n)| O(n log n)   | O(n log n) | O(n)  | Yes    |" << endl;
        cout << "| Bubble Sort  | O(n)      | O(n²)        | O(n²)      | O(1)  | Yes    |" << endl;
        
        cout << "\nHeap Sort Advantages:" << endl;
        cout << "✅ Guaranteed O(n log n) performance" << endl;
        cout << "✅ In-place sorting (O(1) space)" << endl;
        cout << "✅ Not affected by input order" << endl;
        
        cout << "\nHeap Sort Disadvantages:" << endl;
        cout << "❌ Not stable (relative order not preserved)" << endl;
        cout << "❌ Poor cache performance compared to quicksort" << endl;
        cout << "❌ Higher constant factors than quicksort" << endl;
    }
    
private:
    void printArray(int n) {
        cout << "[";
        for (int i = 0; i < n; i++) {
            cout << arr[i];
            if (i < n - 1) cout << ", ";
        }
        cout << "]" << endl;
    }
    
    void printArrayWithSeparator(int sortedPortion) {
        cout << "[";
        for (int i = 0; i < arr.size(); i++) {
            if (i == sortedPortion) cout << "| ";
            cout << arr[i];
            if (i < arr.size() - 1) cout << ", ";
        }
        cout << "] (sorted | unsorted)" << endl;
    }
};

void demonstrateHeapSort() {
    cout << "\n=== HEAP SORT DEMONSTRATION ===" << endl;
    
    vector<int> testData = {64, 34, 25, 12, 22, 11, 90, 5};
    
    HeapSort sorter;
    vector<int> sorted = sorter.heapSort(testData);
    sorter.compareWithOtherSorts();
    
    cout << "\nHeap Sort Applications:" << endl;
    cout << "• Systems requiring predictable performance" << endl;
    cout << "• Memory-constrained environments" << endl;
    cout << "• Real-time systems (guaranteed O(n log n))" << endl;
    cout << "• When stability is not required" << endl;
}
```

**Teaching Points:**
1. **Build Heap Optimization**: Bottom-up is linear time
2. **Mathematical Beauty**: Geometric series analysis
3. **Heap Sort**: In-place O(n log n) sorting
4. **Performance Guarantees**: No worst-case degradation

---

## Day 7: Priority Queues & Real-World Applications

### 📚 Theory: Priority Queues in Practice

**Teacher's Introduction:**
"Heaps shine brightest as priority queues - abstract data types where elements are served based on priority rather than arrival order. From operating systems to graph algorithms, priority queues are everywhere."

### **Implementation 1: Custom Priority Queue with STL Integration**

```cpp
#include <queue>
#include <functional>

// Custom priority queue implementation
template<typename T, typename Compare = less<T>>
class CustomPriorityQueue {
private:
    vector<T> heap;
    Compare comp;
    
    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }
    bool hasParent(int i) { return i > 0; }
    bool hasLeftChild(int i) { return leftChild(i) < heap.size(); }
    bool hasRightChild(int i) { return rightChild(i) < heap.size(); }
    
public:
    CustomPriorityQueue(Compare c = Compare()) : comp(c) {}
    
    void push(const T& item) {
        heap.push_back(item);
        heapifyUp(heap.size() - 1);
    }
    
    T top() const {
        if (empty()) throw runtime_error("Priority queue is empty");
        return heap[0];
    }
    
    void pop() {
        if (empty()) throw runtime_error("Priority queue is empty");
        
        heap[0] = heap.back();
        heap.pop_back();
        
        if (!empty()) {
            heapifyDown(0);
        }
    }
    
    bool empty() const { return heap.empty(); }
    size_t size() const { return heap.size(); }
    
private:
    void heapifyUp(int index) {
        while (hasParent(index)) {
            int parentIndex = parent(index);
            if (comp(heap[index], heap[parentIndex])) break;
            
            swap(heap[index], heap[parentIndex]);
            index = parentIndex;
        }
    }
    
    void heapifyDown(int index) {
        while (hasLeftChild(index)) {
            int priorityIndex = index;
            int leftIndex = leftChild(index);
            
            if (!comp(heap[leftIndex], heap[priorityIndex])) {
                priorityIndex = leftIndex;
            }
            
            if (hasRightChild(index)) {
                int rightIndex = rightChild(index);
                if (!comp(heap[rightIndex], heap[priorityIndex])) {
                    priorityIndex = rightIndex;
                }
            }
            
            if (priorityIndex == index) break;
            
            swap(heap[index], heap[priorityIndex]);
            index = priorityIndex;
        }
    }
};

// Application 1: Task Scheduler
struct Task {
    string name;
    int priority;
    int arrivalTime;
    
    Task(string n, int p, int t) : name(n), priority(p), arrivalTime(t) {}
};

struct TaskComparator {
    bool operator()(const Task& a, const Task& b) const {
        if (a.priority != b.priority) {
            return a.priority < b.priority;  // Higher priority first
        }
        return a.arrivalTime > b.arrivalTime;  // Earlier arrival first if same priority
    }
};

class TaskScheduler {
private:
    priority_queue<Task, vector<Task>, TaskComparator> taskQueue;
    int currentTime = 0;
    
public:
    void addTask(const string& name, int priority, int arrivalTime) {
        taskQueue.push(Task(name, priority, arrivalTime));
        cout << "Added task: " << name << " (priority: " << priority 
             << ", arrival: " << arrivalTime << ")" << endl;
    }
    
    void processAllTasks() {
        cout << "\n=== TASK PROCESSING ORDER ===" << endl;
        
        while (!taskQueue.empty()) {
            Task nextTask = taskQueue.top();
            taskQueue.pop();
            
            currentTime = max(currentTime, nextTask.arrivalTime) + 1;
            
            cout << "Time " << currentTime << ": Processing " << nextTask.name 
                 << " (priority: " << nextTask.priority << ")" << endl;
        }
    }
    
    void demonstrateScheduling() {
        cout << "\n=== TASK SCHEDULER DEMONSTRATION ===" << endl;
        
        // Add tasks with different priorities
        addTask("System Backup", 1, 0);      // Low priority
        addTask("User Interface", 5, 1);     // Medium priority
        addTask("Emergency Alert", 10, 2);   // High priority
        addTask("Data Processing", 3, 0);    // Low-medium priority
        addTask("Critical Update", 10, 3);   // High priority
        
        processAllTasks();
        
        cout << "\nScheduling Rules:" << endl;
        cout << "1. Higher priority tasks processed first" << endl;
        cout << "2. Same priority: FIFO (first arrival first)" << endl;
        cout << "3. Tasks can't start before arrival time" << endl;
    }
};

void demonstrateCustomPriorityQueue() {
    cout << "\n=== CUSTOM PRIORITY QUEUE DEMONSTRATION ===" << endl;
    
    // Test with integers (max-heap)
    CustomPriorityQueue<int> maxPQ;
    vector<int> values = {3, 1, 4, 1, 5, 9, 2, 6};
    
    cout << "Inserting values: ";
    for (int val : values) {
        cout << val << " ";
        maxPQ.push(val);
    }
    cout << endl;
    
    cout << "Extracting in priority order: ";
    while (!maxPQ.empty()) {
        cout << maxPQ.top() << " ";
        maxPQ.pop();
    }
    cout << endl;
    
    // Test with custom comparator (min-heap)
    CustomPriorityQueue<int, greater<int>> minPQ;
    for (int val : values) {
        minPQ.push(val);
    }
    
    cout << "Min-heap extraction: ";
    while (!minPQ.empty()) {
        cout << minPQ.top() << " ";
        minPQ.pop();
    }
    cout << endl;
    
    // Task scheduler demonstration
    TaskScheduler scheduler;
    scheduler.demonstrateScheduling();
}
```

### **Implementation 2: Advanced Priority Queue Applications**

```cpp
// Application 2: Dijkstra's Algorithm (Shortest Path)
struct Edge {
    int to;
    int weight;
    Edge(int t, int w) : to(t), weight(w) {}
};

struct DijkstraNode {
    int vertex;
    int distance;
    DijkstraNode(int v, int d) : vertex(v), distance(d) {}
    
    bool operator>(const DijkstraNode& other) const {
        return distance > other.distance;  // Min-heap based on distance
    }
};

class DijkstraAlgorithm {
private:
    vector<vector<Edge>> graph;
    int vertices;
    
public:
    DijkstraAlgorithm(int v) : vertices(v), graph(v) {}
    
    void addEdge(int from, int to, int weight) {
        graph[from].push_back(Edge(to, weight));
    }
    
    vector<int> shortestPaths(int source) {
        cout << "\n=== DIJKSTRA'S ALGORITHM DEMONSTRATION ===" << endl;
        
        vector<int> dist(vertices, INT_MAX);
        priority_queue<DijkstraNode, vector<DijkstraNode>, greater<DijkstraNode>> pq;
        
        dist[source] = 0;
        pq.push(DijkstraNode(source, 0));
        
        cout << "Starting from vertex " << source << endl;
        
        while (!pq.empty()) {
            DijkstraNode current = pq.top();
            pq.pop();
            
            int u = current.vertex;
            int d = current.distance;
            
            if (d > dist[u]) continue;  // Already processed with better distance
            
            cout << "Processing vertex " << u << " (distance: " << d << ")" << endl;
            
            for (const Edge& edge : graph[u]) {
                int v = edge.to;
                int weight = edge.weight;
                
                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.push(DijkstraNode(v, dist[v]));
                    cout << "  Updated distance to vertex " << v 
                         << ": " << dist[v] << endl;
                }
            }
        }
        
        return dist;
    }
    
    void demonstrateDijkstra() {
        // Create sample graph
        addEdge(0, 1, 4);
        addEdge(0, 2, 2);
        addEdge(1, 2, 1);
        addEdge(1, 3, 5);
        addEdge(2, 3, 8);
        addEdge(2, 4, 10);
        addEdge(3, 4, 2);
        
        cout << "Graph edges:" << endl;
        for (int i = 0; i < vertices; i++) {
            for (const Edge& e : graph[i]) {
                cout << i << " -> " << e.to << " (weight: " << e.weight << ")" << endl;
            }
        }
        
        vector<int> distances = shortestPaths(0);
        
        cout << "\nShortest distances from vertex 0:" << endl;
        for (int i = 0; i < vertices; i++) {
            cout << "To vertex " << i << ": " << distances[i] << endl;
        }
    }
};

// Application 3: Merge K Sorted Arrays
class MergeKSorted {
private:
    struct ArrayElement {
        int value;
        int arrayIndex;
        int elementIndex;
        
        ArrayElement(int v, int ai, int ei) 
            : value(v), arrayIndex(ai), elementIndex(ei) {}
        
        bool operator>(const ArrayElement& other) const {
            return value > other.value;  // Min-heap
        }
    };
    
public:
    vector<int> mergeKSortedArrays(vector<vector<int>>& arrays) {
        cout << "\n=== MERGE K SORTED ARRAYS ===" << endl;
        
        priority_queue<ArrayElement, vector<ArrayElement>, greater<ArrayElement>> pq;
        vector<int> result;
        
        // Initialize heap with first element from each array
        for (int i = 0; i < arrays.size(); i++) {
            if (!arrays[i].empty()) {
                pq.push(ArrayElement(arrays[i][0], i, 0));
            }
        }
        
        cout << "Initial heap contains first elements from each array" << endl;
        
        while (!pq.empty()) {
            ArrayElement current = pq.top();
            pq.pop();
            
            result.push_back(current.value);
            cout << "Added " << current.value << " from array " 
                 << current.arrayIndex << endl;
            
            // Add next element from the same array
            int nextIndex = current.elementIndex + 1;
            if (nextIndex < arrays[current.arrayIndex].size()) {
                pq.push(ArrayElement(
                    arrays[current.arrayIndex][nextIndex],
                    current.arrayIndex,
                    nextIndex
                ));
            }
        }
        
        return result;
    }
    
    void demonstrateMerging() {
        vector<vector<int>> arrays = {
            {1, 4, 7},
            {2, 5, 8},
            {3, 6, 9, 10}
        };
        
        cout << "Input arrays:" << endl;
        for (int i = 0; i < arrays.size(); i++) {
            cout << "Array " << i << ": ";
            for (int val : arrays[i]) {
                cout << val << " ";
            }
            cout << endl;
        }
        
        vector<int> merged = mergeKSortedArrays(arrays);
        
        cout << "\nMerged result: ";
        for (int val : merged) {
            cout << val << " ";
        }
        cout << endl;
        
        cout << "\nTime complexity: O(n log k) where n = total elements, k = number of arrays" << endl;
        cout << "Space complexity: O(k) for the priority queue" << endl;
    }
};

void demonstrateAdvancedApplications() {
    cout << "\n=== ADVANCED PRIORITY QUEUE APPLICATIONS ===" << endl;
    
    // Dijkstra's Algorithm
    DijkstraAlgorithm dijkstra(5);
    dijkstra.demonstrateDijkstra();
    
    // Merge K Sorted Arrays
    MergeKSorted merger;
    merger.demonstrateMerging();
    
    cout << "\n=== REAL-WORLD PRIORITY QUEUE USAGE ===" << endl;
    cout << "🖥️  Operating Systems:" << endl;
    cout << "   • Process scheduling (higher priority processes first)" << endl;
    cout << "   • Event handling in GUI systems" << endl;
    
    cout << "\n🌐 Network Systems:" << endl;
    cout << "   • Network packet routing (QoS priorities)" << endl;
    cout << "   • Load balancing algorithms" << endl;
    
    cout << "\n🎮 Game Development:" << endl;
    cout << "   • AI pathfinding (A* algorithm)" << endl;
    cout << "   • Event scheduling in game engines" << endl;
    
    cout << "\n📊 Data Processing:" << endl;
    cout << "   • Stream processing (handle high-priority data first)" << endl;
    cout << "   • Database query optimization" << endl;
    
    cout << "\n🚗 Real-time Systems:" << endl;
    cout << "   • Traffic light control systems" << endl;
    cout << "   • Medical device monitoring" << endl;
}
```

---

## Main Function & Testing Framework

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <climits>
#include <functional>
using namespace std;

// Forward declarations
void demonstrateHeapBasics();
void demonstrateHeapVisualization();
void demonstrateHeapifyOperations();
void demonstrateComplexityAnalysis();
void demonstrateBuildHeap();
void demonstrateHeapSort();
void demonstrateCustomPriorityQueue();
void demonstrateAdvancedApplications();

int main() {
    cout << "🌳 Week 5: Heaps & Priority Queues" << endl;
    cout << "===================================" << endl;
    
    // Day 1-2: Heap Fundamentals & Array Implementation
    cout << "\n📅 DAY 1-2: HEAP FUNDAMENTALS & ARRAY IMPLEMENTATION" << endl;
    demonstrateHeapBasics();
    demonstrateHeapVisualization();
    
    // Day 3-4: Heapify Operations & Dynamic Maintenance
    cout << "\n📅 DAY 3-4: HEAPIFY OPERATIONS & DYNAMIC HEAP MAINTENANCE" << endl;
    demonstrateHeapifyOperations();
    demonstrateComplexityAnalysis();
    
    // Day 5-6: Build Heap Algorithm & Heap Sort
    cout << "\n📅 DAY 5-6: BUILD HEAP ALGORITHM & HEAP SORT" << endl;
    demonstrateBuildHeap();
    demonstrateHeapSort();
    
    // Day 7: Priority Queues & Applications
    cout << "\n📅 DAY 7: PRIORITY QUEUES & REAL-WORLD APPLICATIONS" << endl;
    demonstrateCustomPriorityQueue();
    demonstrateAdvancedApplications();
    
    cout << "\n🎉 Week 5 Heaps & Priority Queues Mastery Complete!" << endl;
    cout << "Next: Week 6 - Advanced Tree Topics (Tries & Segment Trees)" << endl;
    
    return 0;
}
```

---

## 📚 Week 5 Summary & Key Takeaways

### **Heap Concepts Mastered:**

1. **Heap Properties & Structure**
   - Complete binary tree with ordering constraint
   - Max-heap: parent ≥ children, Min-heap: parent ≤ children
   - Array representation with mathematical index relationships
   - Height guarantee: O(log n) for all operations

2. **Core Heap Operations**
   - Insert with heapify-up: O(log n)
   - Extract-max/min with heapify-down: O(log n)
   - Peek: O(1) access to priority element
   - Build heap: O(n) using bottom-up approach

3. **Advanced Algorithms**
   - Linear-time heap construction (mathematical proof)
   - Heap sort: O(n log n) in-place sorting
   - Priority queue implementation with custom comparators
   - Real-world applications in algorithms and systems

### **Critical Algorithm Patterns:**

- **Heapify Operations**: Bubble-up for insertion, bubble-down for extraction
- **Complete Tree Property**: Enables efficient array representation
- **Priority-Based Processing**: Most important element always accessible
- **Geometric Series Optimization**: Bottom-up build heap achieves linear time

### **Real-World Applications:**

- **Operating Systems**: Process scheduling, event handling
- **Graph Algorithms**: Dijkstra's shortest path, Prim's MST
- **Data Processing**: Merge K sorted streams, top-K problems
- **Game Development**: Pathfinding algorithms, event systems

### **Student Assessment Rubric**

**Heap Fundamentals (Must Have):**
- [ ] Understands heap property and complete tree structure
- [ ] Can implement heapify-up and heapify-down operations
- [ ] Grasps array representation with mathematical relationships
- [ ] Performs complexity analysis for heap operations

**Advanced Heap Skills (Should Have):**
- [ ] Implements build heap algorithm with O(n) complexity
- [ ] Masters heap sort with in-place sorting
- [ ] Uses STL priority_queue with custom comparators
- [ ] Applies heaps to solve algorithmic problems

**Expert Knowledge (Nice to Have):**
- [ ] Understands mathematical proof of linear build-heap
- [ ] Connects heaps to real-world system applications
- [ ] Optimizes heap operations for specific use cases
- [ ] Designs priority queue solutions for complex problems

### 🎯 Common Mistakes to Avoid

1. **Index Confusion**: Mixing 0-based and 1-based array indexing
2. **Heap Property**: Confusing heap ordering with BST ordering
3. **Build Heap**: Using top-down instead of bottom-up approach
4. **Comparator Logic**: Incorrect custom comparator implementation

### 🗣️ Advanced Discussion Questions

1. "Why can we build a heap in O(n) time but sorting requires O(n log n)?"
2. "How do priority queues enable efficient graph algorithms?"
3. "When would you choose heap sort over quicksort or mergesort?"
4. "How do operating systems use priority queues for process scheduling?"

---

## Next Week Preview: Advanced Tree Topics

Week 6 will explore specialized tree structures:

- **Tries (Prefix Trees)**: String processing and dictionary operations
- **Segment Trees**: Range query optimization and lazy propagation
- **Advanced Applications**: Text processing, computational geometry
- **Performance Optimization**: Space-time trade-offs in tree design

**🌟 Congratulations on mastering Heaps & Priority Queues! You now understand efficient priority-based data processing.**

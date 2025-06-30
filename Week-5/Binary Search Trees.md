# Binary Search Tree (BST)

## What is a Binary Search Tree?

A **Binary Search Tree (BST)** is a hierarchical data structure that extends the binary tree by maintaining order among its elements. Each node in a BST has the following properties:

- **Left Subtree Property**: The left child and its descendants contain values **less than** the current node.
- **Right Subtree Property**: The right child and its descendants contain values **greater than** the current node.
- **Recursive Structure**: Both the left and right subtrees themselves must be valid BSTs.

This structure ensures that common operations like search, insert, and delete can be performed efficiently, usually in O(log n) time for balanced trees.

---

## Node Structure

Each node contains a data field and two pointers referring to its left and right children.

```cpp
struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
    }
};
```

---

## Insertion

To insert a value into the BST, we compare the value with the current node:

- If the tree is empty, the new node becomes the root.
- If the value is less than the current node, we insert it into the left subtree.
- If the value is greater, we insert it into the right subtree.
- We recursively find the correct location to maintain the BST property.

```cpp
Node* insert(Node* root, int key) {
    if (root == nullptr)
        return new Node(key);

    if (key < root->data)
        root->left = insert(root->left, key);
    else if (key > root->data)
        root->right = insert(root->right, key);

    return root;
}
```
Time Complexity: 

The worst-case time complexity of insert operations is O(h) where h is the height of the Binary Search Tree. 
In the worst case, we may have to travel from the root to the deepest leaf node. The height of a skewed tree may become n and the time complexity of insertion operation may become O(n). 

---

## Search

To search for a value in a BST:

- Start at the root and compare the value.
- If the value equals the root's data, we return true.
- If the value is less, search the left subtree.
- If the value is more, search the right subtree.
- If we reach a null node, the value is not in the tree.

```cpp
bool search(Node* root, int key) {
    if (root == nullptr)
        return false;
    if (root->data == key)
        return true;
    if (key < root->data)
        return search(root->left, key);
    else
        return search(root->right, key);
}
```
Time complexity: O(h), where h is the height of the BST.
Auxiliary Space: O(h) This is because of the space needed to store the recursion stack.

---

## Deletion

Deleting a node from a BST involves three cases:

1. **Node is a Leaf**: Simply remove the node.
2. **Node has One Child**: Remove the node and link its parent directly to its child.
3. **Node has Two Children**: Find the inorder successor (smallest node in the right subtree), replace the node’s value with it, and then delete the successor node.

This ensures the BST properties remain intact after deletion.

```cpp
Node* minValueNode(Node* node) {
    Node* current = node;
    while (current && current->left != nullptr)
        current = current->left;
    return current;
}

Node* deleteNode(Node* root, int key) {
    if (root == nullptr) return root;

    if (key < root->data)
        root->left = deleteNode(root->left, key);
    else if (key > root->data)
        root->right = deleteNode(root->right, key);
    else {
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        } else if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

        Node* temp = minValueNode(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}
```
Time Complexity: O(h), where h is the height of the BST. 
Auxiliary Space: O(h).

---

## Traversals

BSTs can be traversed in the same ways as Binary Trees:

- **Inorder Traversal**: Visits nodes in sorted order (left → root → right). Very useful for extracting sorted data from a BST.
- **Preorder Traversal**: Visits nodes in the order (root → left → right), helpful for creating a copy of the tree.
- **Postorder Traversal**: Visits nodes in the order (left → right → root), useful for deleting or freeing the tree.
- **Level Order Traversal**: Visits nodes level by level using a queue.

---

## Example

Here is an example where we insert nodes, print the tree, and delete a node.

```cpp
int main() {
    Node* root = nullptr;
    root = insert(root, 50);
    insert(root, 30);
    insert(root, 70);
    insert(root, 20);
    insert(root, 40);
    insert(root, 60);
    insert(root, 80);

    cout << "Inorder before deletion: ";
    inorder(root);

    root = deleteNode(root, 70);

    cout << "\nInorder after deletion: ";
    inorder(root);

    return 0;
}
```

---

## Output

```
Inorder before deletion: 20 30 40 50 60 70 80
Inorder after deletion: 20 30 40 50 60 80
```

Here is an other example where we find the minimum of a BST with the help of inorder traversal:

```cpp
// C++ code to find minimum value in BST
// using inorder traversal
#include <bits/stdc++.h>
using namespace std;

struct Node {
    int data;
    Node *left, *right;

    Node(int val) {
        data = val;
        left = right = nullptr;
    }
};

// Recursive function to solve and store elements 
// in a vector
void inorder(Node* root, vector<int>& sortedInorder) {
  
    // Base Case
    if (root == nullptr) return;

    // Traverse left subtree
    inorder(root->left, sortedInorder);

    // Store the current node's data
    sortedInorder.push_back(root->data);

    // Traverse right subtree
    inorder(root->right, sortedInorder);
}

// Function to find the minimum value in BST
int minValue(Node* root) {
    if (root == nullptr) {
        return -1;
    }
    
    vector<int> sortedInorder;
    
    // Call the recursive inorder function
    inorder(root, sortedInorder);
    
    // Return the first element, which is the minimum
    return sortedInorder[0];
}

int main() {

    // Representation of input binary search tree
    //        5
    //       / \
    //      4   6
    //     /     \
    //    3       7
    //   / 
    //  1
    Node* root = new Node(5);
    root->left = new Node(4);
    root->right = new Node(6);
    root->left->left = new Node(3);
    root->right->right = new Node(7);
    root->left->left->left = new Node(1);

    cout << minValue(root) << "\n";

    return 0;
}
```
## Output

```
1
```

Time Complexity: O(n), since we traversed through all the elements in a BST.
Auxiliary Space: O(n), we are storing all the n nodes in an array.


# Problems

## Easy
- [Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/description/)  | [Solution](https://www.geeksforgeeks.org/dsa/sum-of-nodes-in-a-binary-search-tree-with-values-from-a-given-range/?utm_source=chatgpt.com)
- [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/) | [Solution](https://www.geeksforgeeks.org/dsa/sorted-array-to-balanced-bst/)  
- [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)  | [Solution](https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/)
- [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/description/)  | [Solution](https://www.geeksforgeeks.org/dsa/shortest-distance-between-two-nodes-in-bst/)
- [Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://algo.monster/liteproblems/653)
- [Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://www.geeksforgeeks.org/dsa/find-mode-in-binary-search-tree/)
- [Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://www.geeksforgeeks.org/pair-with-minimum-absolute-difference-bst/)

## Medium
- [Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)  | [Solution](https://www.geeksforgeeks.org/dsa/transform-bst-sum-tree/)
- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/) | [Solution](https://www.geeksforgeeks.org/dsa/a-program-to-check-if-a-binary-tree-is-bst-or-not/)
- [Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://www.geeksforgeeks.org/dsa/remove-bst-keys-outside-the-given-range/)
- [Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/description/?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://www.geeksforgeeks.org/dsa/sorted-linked-list-to-balanced-bst/)
- [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://algo.monster/liteproblems/173)


## Hard
- [Maximum Sum BST in Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/description/)  | [Solution](https://www.geeksforgeeks.org/maximum-sub-tree-sum-in-a-binary-tree-such-that-the-sub-tree-is-also-a-bst/?utm_source=chatgpt.com)
- [Number of Ways to Reorder Array to Get Same BST](https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst?envType=problem-list-v2&envId=binary-tree)  | [Solution](https://algo.monster/liteproblems/1569)
- [Merge BSTs to create a single BST](https://leetcode.com/problems/merge-bsts-to-create-single-bst?envType=problem-list-v2&envId=binary-tree) | [Solution](https://algo.monster/liteproblems/1932?utm_source=chatgpt.com)




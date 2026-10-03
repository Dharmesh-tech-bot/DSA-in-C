#  Tree Data Structure in C

> **Overview**: A tree is a hierarchical, non-linear data structure used to represent and organize data in a way that is easy to navigate and search. It consists of a central root node, structural nodes, and sub-nodes connected via edges.

Unlike linear data structures (like arrays or linked lists) where data is stored sequentially, a tree organizes data across multiple levels. This hierarchical model allows for highly efficient storage, traversal, and retrieval operations.

### Recursive Definition
> A tree consists of a root, and zero or more subtrees T_1, T_2, ..., T_k such that there is an edge from the root of the tree to the root of each subtree.

---

##  Syntax in C

To represent a standard Binary Tree node in C, we use a struct that holds the data and pointers to its children:

    struct Node {
      int data;
      struct Node *left_child;
      struct Node *right_child;
    };


##  Basic Terminologies

Understanding these terms is crucial for tree-based interview questions:


Term                    | Definition
------------------------|------------------------------------------------------------
Root Node               | The topmost node of a tree that does not have a parent. A non-empty tree has exactly one root.
Parent Node             | The predecessor of a node.
Child Node              | The immediate successor of a node.
Leaf / External Node    | A node that does not have any child nodes.
Internal Node           | Any node with at least one child.
Sibling                 | Nodes that share the same parent.
Ancestor                | Any predecessor node on the path from the root to a specific node.
Descendant              | Any successor node on the path from a specific node to a leaf node.
Neighbour               | The parent or child nodes of a given node.
Subtree                 | Any node of the tree along with all of its descendants.
Level                   | The count of edges on the path from the root node to that node (Root is at Level 0).

##  Properties of a Tree

- Edges: If a tree has N nodes, it will have exactly N-1 edges. There is only one valid path between any two nodes.
- Depth of a Node: The number of edges in the path from the root to that specific node.
- Height of a Node: The length of the longest path from the node to a leaf node.
- Height of the Tree: The length of the longest path from the root of the tree to a leaf node.
- Degree of a Node: The total count of subtrees attached to that node (Leaf nodes have a degree of 0). The degree of the tree is the maximum degree of any node within it.
- Structure: It has no loops, no circuits, and no self-loops.

##  Basic Operations

- Create: Initialize a new tree.
- Insert: Add new data into the tree at the appropriate position.
- Search: Traverse the tree to check if specific data is present.
- Traversals:
    - Preorder:   Visit Root -> Traverse Left -> Traverse Right.
    - Inorder:    Traverse Left -> Visit Root -> Traverse Right.
    - Postorder:  Traverse Left -> Traverse Right -> Visit Root.

Note: Trees can be traversed using Depth-First Search (DFS) or Breadth-First Search (BFS).

##  Types of Tree Data Structures

- General Tree: No restriction on the number of child nodes. A parent can have N children.
- Binary Tree: A node can have a maximum of two child nodes (left and right).
- Balanced Tree: The height of the left and right sub-trees are strictly equal or differ by at most 1.
- Binary Search Tree (BST): A specialized binary tree used for searching/sorting. The left child's
  value is less than the parent's, and the right child's value is greater.
    - Sub-types: Full, Complete, Skewed, Strictly, and Extended Binary Trees.
    - Self-balancing variants: AVL Tree, Red-Black Tree.

##  Applications

Trees are widely used across various domains in computer science:

- Spanning Trees:       Used in network routers to direct packets via the shortest path.
- Binary Search Trees:  Maintaining dynamically sorted streams of data.
- Hierarchical Data:    Storing file systems or organizational structures.
- Syntax Trees:         Representing source code structures in compilers.
- Tries:                Enabling fast dynamic spell-checking and prefix searching.
- Heaps:                Array-represented trees used to implement Priority Queues.
- Databases:            Organizing records for efficient O(log N) searching and sorting (e.g., B-Trees).
- AI/ML:                Decision trees for classification and predictive modeling.
- Networking:           Routing algorithms (like IP routing) to find the best data paths.

##  Advantages vs Disadvantages

 Advantages
--------------
- Offers highly efficient insertion, deletion, and search operations compared to linear structures.
- Naturally represents hierarchical and recursive relationships.
- Highly flexible and dynamic; auto-organizes as data is modified.
- Memory-efficient structurally when compared to certain implementations of standard lists.
- Forms the backbone of advanced algorithms (Huffman coding, decision algorithms).

 Disadvantages
-----------------
- Requires additional memory overhead to store pointer references (left/right children).
- Inefficient for data that is strictly linear or non-hierarchical.
- Unbalanced trees (e.g., skewed trees) degrade performance to O(N), destroying the efficiency
  of tree algorithms.
- Deep trees with many levels can become computationally expensive to traverse.
- Hash tables offer better scalability and O(1) lookups for simple key-value pairings.

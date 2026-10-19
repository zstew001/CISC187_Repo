# Binary Search Trees

## Part 1: Is It a Binary Search Tree?

### Tree A
```     50
       /  \
     30    70
    / \    / \
   20 40  60 80
```
### Yes, this is a valid BST. Every value in the left subtree is < ```50``` and every value in the right subtree is > ```50```. The same case is true for nodes ```30``` and ```70```.

### Tree B
```
        50
       /  \
     30    70
       \
        60
```
### No, this is not a valid BST. Although ```60``` is > than its parent ```30```, it is still in the left subtree of ```50```. Since every value in that subtree must be < ```50```, the BST ordering property is doesn't hold true.

### Tree C
```
        40
       /  \
     20    70
    / \    /
   10 30  50
```
### Yes, this is a valid BST. All values in the left subtree are < ```40``` and all values in the right subtree are > ```40```. The ordering property is also consistent in the remaining nodes.

## Analysis:

### Checking only a node and its child nodes isn't sufficient because the BST invariant applies to the entire subtree. Tree B demonstrates this because ```60``` satisfies the ordering requirement relative to ```30```, but breaks the requirement in ```50```.

## Part 2: Construct a BST

### Insertion order: ```45, 25, 70, 15, 35, 60, 90, 30, 40, 80```
```
             45
           /    \
         25      70
        /  \    /  \
       15  35  60   90
          /  \      /
         30  40    80
```
### Root: ```45```
### Leaf nodes: ```15, 30, 40, 60, 80```
### Parent of ```30```: ```35```
### Children of ```25```: ```15``` and ```35```
### Left subtree of ```45```: ```25, 15, 35, 30, 40```
### Right subtree of ```45```: ```70, 60, 90, 80```
### Depth of ```40```: ```3``` (45 -> 25 -> 35 -> 40)
### Depth of ```80```: ```3``` (45 -> 70 -> 90 -> 80)
### Height of the tree: ```3``` edges


## Part 3: Trace BST Search

### Search for ```40```

| Current Node | Target | Comparison | Direction |
| --- | --- | --- | --- |
| 45 | 40 | 40 < 45 | Left |
| 25 | 40 | 40 > 25 | Right |
| 35 | 40 | 40 > 35 | Right |
| 40 | 40 | 40 = 40 | Found |

### Search path: ```45 -> 25 -> 35 -> 40```

### Search for ```75```

| Current Node | Target | Comparison | Direction |
| --- | --- | --- | --- |
| 45 | 75 | 75 > 45 | Right |
| 70 | 75 | 75 > 70 | Right |
| 90 | 75 | 75 < 90 | Left |
| 80 | 75 | 75 < 80 | Left |

### Search path: ```45 -> 70 -> 90 -> 80 -> nullptr```

### The search determines that ```75``` doesn't exist when it attempts to move left from ```80``` and reaches ```nullptr```.

## Analysis:

### BST search doesn't need to examine both subtrees because all comparisons determine which side the target can be found in. If the target is < than current node, the right subtree can be ignored because every value there should be > than current node. This case holds true for the opposite direction.

## Part 4: Implement a BST Class
### The full program is included below. I used the ```Node``` structure and ```BinarySearchTree``` class provided, then added required functions for insertion, search, traversal, min/max, deletion, and memory management. ```main()``` includes tests from Part 15. 

```C++
#include <iostream>
#include <stdexcept>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

class BinarySearchTree {
private:
    Node* root;

    Node* insert(Node* node, int value);
    bool search(Node* node, int value) const;

    void inorder(Node* node) const;
    void preorder(Node* node) const;
    void postorder(Node* node) const;

    Node* findMin(Node* node) const;
    Node* findMax(Node* node) const;

    Node* remove(Node* node, int value);
    void deleteTree(Node* node);

public:
    BinarySearchTree();
    ~BinarySearchTree();

    bool empty() const;

    void insert(int value);
    bool search(int value) const;
    bool remove(int value);

    void inorder() const;
    void preorder() const;
    void postorder() const;

    int minimum() const;
    int maximum() const;
};

BinarySearchTree::BinarySearchTree() {
    root = nullptr;
}

BinarySearchTree::~BinarySearchTree() {
    deleteTree(root);
}

bool BinarySearchTree::empty() const {
    return root == nullptr;
}

void BinarySearchTree::insert(int value) {
    root = insert(root, value);
}

Node* BinarySearchTree::insert(Node* node, int value) {
    if (node == nullptr) {
        return new Node(value);
    }

    if (value < node->data) {
        node->left = insert(node->left, value);
    }
    else if (value > node->data) {
        node->right = insert(node->right, value);
    }

    return node;
}

bool BinarySearchTree::search(int value) const {
    return search(root, value);
}

bool BinarySearchTree::search(Node* node, int value) const {
    if (node == nullptr) {
        return false;
    }

    if (value == node->data) {
        return true;
    }
    if (value < node->data) {
        return search(node->left, value);
    }

    return search(node->right, value);
}

void BinarySearchTree::inorder() const {
    inorder(root);
    cout << endl;
}

void BinarySearchTree::inorder(Node* node) const {
    if (node == nullptr) {
        return;
    }

    inorder(node->left);
    cout << node->data << " ";
    inorder(node->right);
}

void BinarySearchTree::preorder() const {
    preorder(root);
    cout << endl;
}

void BinarySearchTree::preorder(Node* node) const {
    if (node == nullptr) {
        return;
    }

    cout << node->data << " ";
    preorder(node->left);
    preorder(node->right);
}

void BinarySearchTree::postorder() const {
    postorder(root);
    cout << endl;
}

void BinarySearchTree::postorder(Node* node) const {
    if (node == nullptr) {
        return;
    }

    postorder(node->left);
    postorder(node->right);
    cout << node->data << " ";
}

Node* BinarySearchTree::findMin(Node* node) const {
    while (node != nullptr && node->left != nullptr) {
        node = node->left;
    }

    return node;
}

Node* BinarySearchTree::findMax(Node* node) const {
    while (node != nullptr && node->right != nullptr) {
        node = node->right;
    }

    return node;
}

int BinarySearchTree::minimum() const {
    if (empty()) {
        throw underflow_error("Cannot find minimum of an empty tree");
    }

    return findMin(root)->data;
}

int BinarySearchTree::maximum() const {
    if (empty()) {
        throw underflow_error("Cannot find maximum of an empty tree");
    }

    return findMax(root)->data;
}

bool BinarySearchTree::remove(int value) {
    if (!search(value)) {
        return false;
    }

    root = remove(root, value);
    return true;
}

Node* BinarySearchTree::remove(Node* node, int value) {
    if (node == nullptr) {
        return nullptr;
    }

    if (value < node->data) {
        node->left = remove(node->left, value);
    }
    else if (value > node->data) {
        node->right = remove(node->right, value);
    }
    else{
        if (node->left == nullptr) {
            Node* rightChild = node->right;
            delete node;
            return rightChild;
        }

        if (node->right == nullptr) {
            Node* leftChild = node->left;
            delete node;
            return leftChild;
        }

        Node* successorNode = findMin(node->right);
        node->data = successorNode->data;
        node->right = remove(node->right, successorNode->data);
    }

    return node;
}

void BinarySearchTree::deleteTree(Node* node) {
    if (node == nullptr) {
        return;
    }

    deleteTree(node->left);
    deleteTree(node->right);
    delete node;
}

bool allValuesFound(const BinarySearchTree& myTree, const int expectedValues[], int valueCount) {
    for (int currentIndex = 0; currentIndex < valueCount; currentIndex++) {
        if (!myTree.search(expectedValues[currentIndex])) {
            return false;
        }
    }

    return true;
}

int main() {
    cout << boolalpha;
    BinarySearchTree myTree;

    cout << "Initially empty: " << myTree.empty() << endl;

    int myValues[] = {50, 30, 70, 20, 40, 60, 80};
    const int valueCount = 7;

    for (int currentIndex = 0; currentIndex < valueCount; currentIndex++) {
        myTree.insert(myValues[currentIndex]);
    }

    cout << "Empty after insertion: " << myTree.empty() << endl;
    cout << "Inorder: ";
    myTree.inorder();

    myTree.insert(30);
    cout << "Inorder after duplicate insertion (30): ";
    myTree.inorder();

    cout << "Search for root (50): " << myTree.search(50) << endl;
    cout << "Search for leaf (20): " << myTree.search(20) << endl;
    cout << "Search for internal node (70): " << myTree.search(70) << endl;
    cout << "Search for missing value (75): " << myTree.search(75) << endl;

    cout << "Preorder: ";
    myTree.preorder();
    cout << "Postorder: ";
    myTree.postorder();
    cout << "Minimum: " << myTree.minimum() << endl;
    cout << "Maximum: " << myTree.maximum() << endl;

    cout << "\nRemove leaf (20): " << myTree.remove(20) << endl;
    cout << "Inorder: ";
    myTree.inorder();
    int afterLeaf[] = {30, 40, 50, 60, 70, 80};
    cout << "20 no longer found: " << !myTree.search(20) << endl;
    cout << "Other values found: " << allValuesFound(myTree, afterLeaf, 6) << endl;

    cout << "\nRemove one-child node (30): " << myTree.remove(30) << endl;
    cout << "Inorder: ";
    myTree.inorder();
    int afterOneChild[] = {40, 50, 60, 70, 80};
    cout << "30 no longer found: " << !myTree.search(30) << endl;
    cout << "Other values found: " << allValuesFound(myTree, afterOneChild, 5) << endl;

    cout << "\nRemove two-child node (50): " << myTree.remove(50) << endl;
    cout << "Inorder: ";
    myTree.inorder();
    int afterTwoChildren[] = {40, 60, 70, 80};
    cout << "50 no longer found: " << !myTree.search(50) << endl;
    cout << "Other values found: " << allValuesFound(myTree, afterTwoChildren, 4) << endl;
    cout << "Preorder after deletions: ";
    myTree.preorder();
    cout << "Remove missing value (75): " << myTree.remove(75) << endl;

    return 0;
}
```

## Part 5 Analysis:
### 1. Insertion only follows one path because each comparison determines whether the value should go left or right. There's no need to check both subtrees since only one of them contains the correct position.
### 2. The recursive helper returns a Node*, so the previous call can update the child pointer. This also allows the root pointer to be updated when inserting the first value.
### 3. When insertion reaches nullptr, a new node is created in that position. The function then returns the pointer so it can be connected to the parent node.
### 4. If the value already exists, it isn't inserted again. Neither the < nor > comparison will be true, so the function just returns the existing node.

## Part 6 Analysis: 






## **A 手撕BST**

分数 50

全屏浏览切换布局

作者 朱允刚

单位 吉林大学

对一棵初始为空的二叉查找树（Binary Search Tree, BST）进行若干插入或删除操作，请输出最后的二叉查找树。

![bst.png](https://images.ptausercontent.com/bea066e0-8357-443b-a0c5-99659c1bf50e.png)

 输入格式:

输入第一行为一个整数 *T*，表示操作数目。随后 *T* 行，每行为Insert K（表示插入关键词为K的结点，若树中已有关键词为K的结点，则不插入）或Remove K（表示删除关键词为K的结点，若树中无关键词为K的结点，则不删除），其中K为整数。 *T* 不超过2×105，树高不超过104。

 输出格式:

输出经上述操作后得到的二叉查找树的中根序列和先根序列，序列中每个整数后一个空格，两个序列之间用空行间隔。

输入样例:

```in
16
Insert 17
Insert 31
Insert 13
Insert 11
Insert 20
Insert 35
Insert 25
Insert 8
Insert 4
Insert 11
Insert 24
Insert 40
Insert 27
Insert 9
Remove 17
Remove 13
```

 输出样例:

```out
4 8 9 11 20 24 25 27 31 35 40 

20 11 8 4 9 31 25 24 27 35 40 
```

### 代码

```c++
// 函数实现都是抄我在 Leetcode 上23年底写的
#include <bits/stdc++.h>
using namespace std;

struct Node{
    Node * left = nullptr;
    Node * right = nullptr;
    int val = 0;
    Node() {};
    Node(int x) {val = x;};
    Node(int x, Node * left, Node * right) : val(x), left(left), right(right) {};
};

class BSTsystem {
public:
    Node* insertIntoBST(Node* root, int val) {
        traverse(root, val);
        return root;
    }
    //注意传参是引用形式才能影响指针拷贝
    void traverse(Node* &root, int val) {
        //当本身就是空树时，申请空间
        if(root == nullptr) {
            root = new Node(val);
            return;
        }
        //找到只有一个子树的节点，进行插入
        if(root->left == nullptr && root->val > val) {
            root->left = new Node(val);
            return;
        }
        if(root->right == nullptr && root->val < val) {
            root->right = new Node(val);
            return;
        }
        //经典查找位置
        if(root->val > val)
            traverse(root->left, val);

        if(root->val < val)
            traverse(root->right, val);
        return;
    }

    Node* funDelete(Node *root, int key) {
        //用root的值比较key值
        if(root == nullptr)
            return nullptr;
        if(root->val > key)
            root->left = funDelete(root->left, key);
        if(root->val < key)
            root->right = funDelete(root->right, key);
        if(root->val == key) {
            if(root->left == nullptr && root->right == nullptr)
                return nullptr;
            else if(root->right == nullptr)
                return root->left;
            else if(root->left == nullptr)
                return root->right;
            else {//左右子树都存在，找右子树中最小的元素
                //设置指针temp去找最小值
                Node * temp = root->right;
                while(temp->left != nullptr)
                    temp = temp->left;

                root->right = funDelete(root->right, temp->val);
                temp->right = root->right;
                temp->left = root->left;
                return temp;
            }
        }
        return root;
    }

    Node* searchBST(Node* root, int val) {
        if(root == nullptr)
            return nullptr;
        if(root->val == val)
            return root;
        if(root->val > val)
            return searchBST(root->left, val);
        if(root->val < val)
            return searchBST(root->right, val);
        return root;
    }

    void posTraverse(Node* root) {
        if(root == nullptr)
            return;
        cout << root->val << " ";
        posTraverse(root->left);
        posTraverse(root->right);
    }

    void inorderTraverse(Node* root) {
        if(root == nullptr)
            return;
        inorderTraverse(root->left);
        cout << root->val << " ";
        inorderTraverse(root->right);
    }
};




int main () {
    int N;
    cin >> N;
    BSTsystem BST;

    string s; int val;
    cin >> s >> val;
    Node * root = new Node(val);

    for (int i = 1; i < N; i++) {
        string label;
        int nodeVal;
        cin >> s >> nodeVal;
        if (s == "Insert") {
            root = BST.insertIntoBST(root, nodeVal);
        }
        else if (s == "Remove") {
            root = BST.funDelete(root, nodeVal);
        }
    }
    BST.inorderTraverse(root);
    cout << endl;
    BST.posTraverse(root);
    return 0;
}
```

## **B 手撕AVL树（初级版）**

分数 50

全屏浏览切换布局

作者 朱允刚

单位 吉林大学

对一棵初始为空的高度平衡树（AVL树）进行若干插入或删除操作，请输出最后得到的AVL树。

**备注：
（1）当有多种旋转方案时，优先选择旋转次数少的方案。
（2）70%的测试点只包含插入操作，如果你只实现插入操作，也能获得70%的分数。**

 输入格式:

输入第一行为一个整数 *T*，表示操作数目。随后 *T* 行，每行为Insert K（表示插入关键词为K的结点，若树中已有关键词为K的结点，则不插入）或Remove K（表示删除关键词为K的结点，若树中无关键词为K的结点，则不删除），其中K为整数。 *T* 不超过2×105。

 输出格式:

输出经上述操作后得到的高度平衡树的中根序列和先根序列，序列中每个整数后一个空格，两个序列之间用空行间隔。

 输入样例:

```in
16
Insert 17
Insert 31
Insert 13
Insert 11
Insert 20
Insert 35
Insert 25
Insert 8
Insert 4
Insert 11
Insert 24
Insert 40
Insert 27
Insert 9
Remove 17
Remove 13
```

 输出样例:

```out
4 8 9 11 20 24 25 27 31 35 40 

20 8 4 11 9 31 25 24 27 35 40 
```

### 代码

```c++
#include <bits/stdc++.h>
using namespace std;

struct Node {
    Node* left = nullptr;
    Node* right = nullptr;
    int height = 1;
    int val = 0;
    Node() {}
    Node(int x) : val(x) {}
    Node(int x, int height) : val(x), height(height) {}
    Node(int x, int height, Node* left, Node* right) : val(x), height(height), left(left), right(right) {}
};

class Revolve {
private:
    // 用 getHeight 防止空指针访问 height
    int getHeight(Node* node) {
        if (node == nullptr)
            return 0;
        return node->height;
    }
public:
    Node* leftRevolve(Node* root) {
        Node* rootRight = root->right;
        root->right = rootRight->left;
        rootRight->left = root;
        // 更新高度，先子后父
        root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
        rootRight->height = max(getHeight(rootRight->left), getHeight(rootRight->right)) + 1;
        return rootRight;
    }

    Node* rightRevolve(Node* root) {
        Node* rootLeft = root->left;
        root->left = rootLeft->right;
        rootLeft->right = root;
        // 更新高度，先子后父
        root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
        rootLeft->height = max(getHeight(rootLeft->left), getHeight(rootLeft->right)) + 1;
        return rootLeft;
    }

};

class AVLsystem {
private:
    class Revolve revolve;
    // 用 getHeight 防止空指针访问 height
    int getHeight(Node* node) {
        if (node == nullptr)
            return 0;
        return node->height;
    }
public:
    Node* insert(Node* root, int val) {
        if (root == nullptr) {
            // 遍历到底的插入高度为 1
            return new Node(val, 1);
        } else if (val < root->val) {
            // 要向左插入
            root->left = insert(root->left, val);
            root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
        } else if (val > root->val) {
            // 要向右插入
            root->right = insert(root->right, val);
            root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
        }
        root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
        if (getHeight(root->left) - getHeight(root->right) >= 2) {
            if (val < root->left->val) // LL 右旋
                root = revolve.rightRevolve(root);
            else { // LR 先左后右
                root->left = revolve.leftRevolve(root->left);
                root = revolve.rightRevolve(root);
            }
        }
        if (getHeight(root->right) - getHeight(root->left) >= 2) {
            if (val > root->right->val) // RR 左旋
                root = revolve.leftRevolve(root);
            else { // RL 先右后左
                root->right = revolve.rightRevolve(root->right);
                root = revolve.leftRevolve(root);
            }
        }
        return root;
    }

    Node* deleteNode(Node* root, int val) {
        if (root == nullptr)
            return root;

        if (val < root->val) {
            root->left = deleteNode(root->left, val);
        } else if (val > root->val) {
            root->right = deleteNode(root->right, val);
        } else if (val == root->val) {
            // 左右只有一边有，对该侧进行删除
            if (root->left == nullptr || root->right == nullptr) {
                Node* temp = (root->left != nullptr) ? root->left : root->right;
                if (temp == nullptr) {
                    root = nullptr;
                } else {
                    root = temp;
                }
            } else {
                // 找右子树的最左节点
                Node* temp = root->right;
                while (temp->left != nullptr)
                    temp = temp->left;
                root->right = deleteNode(root->right, temp->val);
                temp->height = root->height;
                temp->right = root->right;
                temp->left = root->left;
                root = temp;
            }
        }

        if (root == nullptr)
            return root;

        // 调整高度 和插入相似
        root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
        if (getHeight(root->left) - getHeight(root->right) >= 2) {
            if (getHeight(root->left->right) > getHeight(root->left->left))
                root->left = revolve.leftRevolve(root->left);
            root = revolve.rightRevolve(root);
        } else if (getHeight(root->right) - getHeight(root->left) >= 2) {
            if (getHeight(root->right->left) > getHeight(root->right->right))
                root->right = revolve.rightRevolve(root->right);
            root = revolve.leftRevolve(root);
        }
        return root;
    }
    void inorderTraverse(Node* root) {
        if (root != nullptr) {
            inorderTraverse(root->left);
            cout << root->val << " ";
            inorderTraverse(root->right);
        }
    }

    void posTraverse(Node* root) {
        if (root != nullptr) {
            cout << root->val << " ";
            posTraverse(root->left);
            posTraverse(root->right);
        }
    }

};



int main () {
    int N;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin >> N;
    AVLsystem AVL;

    string s; int val;
    cin >> s >> val;
    Node * root = new Node(val);

    for (int i = 1; i < N; i++) {
        string label;
        int nodeVal;
        cin >> s >> nodeVal;
        if (s == "Insert") {
            root = AVL.insert(root, nodeVal);
        }
        else if (s == "Remove") {
            root = AVL.deleteNode(root, nodeVal);
        }
    }
    AVL.inorderTraverse(root);
    cout << endl << endl;
    AVL.posTraverse(root);
    return 0;
}
```

## **红黑树**

### 题解

```c++
#include <bits/stdc++.h>
using namespace std;
template <typename K, typename V>
class RBTree {
private:
    static const bool RED = false;
    static const bool BLACK = true;

    struct RBNode {
        K key;
        V value;
        bool color;
        RBNode* left;
        RBNode* right;
        RBNode* parent;

        RBNode(RBNode* parent, const K& key, const V& value)
                : key(key), value(value), color(RED), left(nullptr), right(nullptr), parent(parent) {}
    };

    RBNode* root;

    void inorder(RBNode* root) {
        if (root != nullptr) {
            inorder(root->left);
            std::cout << root->key << " " << (root->color ? "B" : "R") << " ";
            inorder(root->right);
        }
    }

    void preorder(RBNode* root) {
        if (root != nullptr) {
            std::cout << root->key << " " << (root->color ? "B" : "R") << " ";
            preorder(root->left);
            preorder(root->right);
        }
    }

public:
    RBTree() : root(nullptr) {}

    void preorderTraverse() {
        preorder(root);
        std::cout << std::endl;
    }

    void inorderTraverse() {
        inorder(root);
        std::cout << std::endl;
    }

    bool colorOf(RBNode* node) {
        return node != nullptr ? node->color : BLACK;
    }

    void insert(const K& key, const V& value) {
        RBNode* t = root;
        if (t == nullptr) {
            root = new RBNode(nullptr, key, value);
            setColor(root, BLACK);
            return;
        }
        int cmp = 0;
        RBNode* parent = nullptr;
        while (t != nullptr) {
            parent = t;
            cmp = key - t->key;
            if (cmp < 0) {
                t = t->left;
            } else if (cmp > 0) {
                t = t->right;
            } else {
                t->value = value;
                return;
            }
        }
        RBNode * e = new RBNode(parent, key, value);
        if (cmp < 0) {
            parent->left = e;
        } else {
            parent->right = e;
        }

        fixAfterInsert(e);
    }

    void remove(const K& key) {
        RBNode* node = justFind(root, key);
        if (node == nullptr) return;
        deleteNode(node);
        return;
    }

private:
    RBNode* predecessor(RBNode* node) {
        if (node == nullptr) {
            return nullptr;
        } else if (node->left != nullptr) {
            RBNode* p = node->left;
            while (p->right != nullptr) {
                p = p->right;
            }
            return p;
        } else {
            RBNode* p = node->parent;
            RBNode* ch = node;
            while (p != nullptr && ch == p->left) {
                ch = p;
                p = p->parent;
            }
            return p;
        }
    }

    RBNode* successor(RBNode* node) {
        if (node == nullptr) {
            return nullptr;
        } else if (node->right != nullptr) {
            RBNode* p = node->right;
            while (p->left != nullptr) {
                p = p->left;
            }
            return p;
        } else {
            RBNode* p = node->parent;
            RBNode* ch = node;
            while (p != nullptr && ch == p->right) {
                ch = p;
                p = p->parent;
            }
            return p;
        }
    }

    RBNode* justFind(RBNode* root , int val) {
        if (root == NULL)
            return NULL;
        if (root->value > val) {
            return justFind(root->left, val);
        }
        if (root->value < val) {
            return justFind(root->right, val);
        }
        if (root->value == val)
            return root;
    }

    void setColor(RBNode* node, bool color) {
        if (node != nullptr) {
            node->color = color;
        }
    }

    void leftRotate(RBNode* g) {
        if (g != nullptr) {
            RBNode* p = g->right;
            g->right = p->left;
            if (p->left != nullptr) {
                p->left->parent = g;
            }
            p->parent= g->parent;
            if (g->parent == nullptr) {
                root = p;
            } else if (g == g->parent->left) {
                g->parent->left = p;
            } else {
                g->parent->right = p;
            }
            p->left = g;
            g->parent = p;
        }
    }

    void rightRotate(RBNode* g) {
        if (g != nullptr) {
            RBNode* p = g->left;
            g->left = p->right;
            if (p->right != nullptr) {
                p->right->parent = g;
            }
            p->parent = g->parent;
            if (g->parent == nullptr) {
                root = p;
            } else if (g == g->parent->left) {
                g->parent->left = p;
            } else {
                g->parent->right = p;
            }
            p->right = g;
            g->parent = p;
        }
    }

    void fixAfterInsert(RBNode* x) {
        x->color = RED;
        while (x != nullptr && x != root && x->parent->color == RED) {
            if (parentOf(x) == leftOf(parentOf(parentOf(x)))) {
                RBNode* y = rightOf(parentOf(parentOf(x)));
                if (colorOf(y) == RED) {
                    setColor(parentOf(x), BLACK);
                    setColor(y, BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    x = parentOf(parentOf(x));
                } else {
                    if (x == rightOf(parentOf(x))) {
                        x = parentOf(x);
                        leftRotate(x);
                    }
                    setColor(parentOf(x), BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    rightRotate(parentOf(parentOf(x)));
                }
            } else {
                RBNode* y = leftOf(parentOf(parentOf(x)));
                if (colorOf(y) == RED) {
                    setColor(parentOf(x), BLACK);
                    setColor(y, BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    x = parentOf(parentOf(x));
                } else {
                    if (x == leftOf(parentOf(x))) {
                        x = parentOf(x);
                        rightRotate(x);
                    }
                    setColor(parentOf(x), BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    leftRotate(parentOf(parentOf(x)));
                }
            }
        }
        root->color = BLACK;
    }

    void deleteNode(RBNode* node) {
        RBNode* e = node;
        RBNode* p = nullptr;
        if (e->left != nullptr && e->right != nullptr) {
            RBNode* s = successor(e);
            e->key = s->key;
            e->value = s->value;
            e = s;
        }
        RBNode* replacement = (e->left != nullptr ? e->left : e->right);
        if (replacement != nullptr) {
            replacement->parent = e->parent;
            if (e->parent == nullptr) {
                root = replacement;
            } else if (e == e->parent->left) {
                e->parent->left = replacement;
            } else {
                e->parent->right = replacement;
            }
            e->left = e->right = e->parent = nullptr;
            if (e->color == BLACK) {
                fixAfterDelete(replacement);
            }
        } else if (e->parent == nullptr) {
            root = nullptr;
        } else {
            if (e->color == BLACK) {
                fixAfterDelete(e);
            }
            if (e->parent != nullptr) {
                if (e == e->parent->left) {
                    e->parent->left = nullptr;
                } else if (e == e->parent->right) {
                    e->parent->right = nullptr;
                }
                e->parent = nullptr;
            }
        }
    }

    void fixAfterDelete(RBNode* x) {
        while (x != root && colorOf(x) == BLACK) {
            if (x == leftOf(parentOf(x))) {
                RBNode* sib = rightOf(parentOf(x));
                if (colorOf(sib) == RED) {
                    setColor(sib, BLACK);
                    setColor(parentOf(x), RED);
                    leftRotate(parentOf(x));
                    sib = rightOf(parentOf(x));
                }
                if (colorOf(leftOf(sib)) == BLACK && colorOf(rightOf(sib)) == BLACK) {
                    setColor(sib, RED);
                    x = parentOf(x);
                } else {
                    if (colorOf(rightOf(sib)) == BLACK) {
                        setColor(leftOf(sib), BLACK);
                        setColor(sib, RED);
                        rightRotate(sib);
                        sib = rightOf(parentOf(x));
                    }
                    setColor(sib, colorOf(parentOf(x)));
                    setColor(parentOf(x), BLACK);
                    setColor(rightOf(sib), BLACK);
                    leftRotate(parentOf(x));
                    x = root;
                }
            } else {
                RBNode* sib = leftOf(parentOf(x));
                if (colorOf(sib) == RED) {
                    setColor(sib, BLACK);
                    setColor(parentOf(x), RED);
                    rightRotate(parentOf(x));
                    sib = leftOf(parentOf(x));
                }
                if (colorOf(rightOf(sib)) == BLACK && colorOf(leftOf(sib)) == BLACK) {
                    setColor(sib, RED);
                    x = parentOf(x);
                } else {
                    if (colorOf(leftOf(sib)) == BLACK) {
                        setColor(rightOf(sib), BLACK);
                        setColor(sib, RED);
                        leftRotate(sib);
                        sib = leftOf(parentOf(x));
                    }
                    setColor(sib, colorOf(parentOf(x)));
                    setColor(parentOf(x), BLACK);
                    setColor(leftOf(sib), BLACK);
                    rightRotate(parentOf(x));
                    x = root;
                }
            }
        }
        setColor(x, BLACK);
    }
    RBNode* parentOf(RBNode* node) {
        return node != nullptr ? node->parent : nullptr;
    }

    RBNode* leftOf(RBNode* node) {
        return node != nullptr ? node->left : nullptr;
    }

    RBNode* rightOf(RBNode* node) {
        return node != nullptr ? node->right : nullptr;
    }
};

int main() {
    RBTree<int, int> tree;
    int n;
    cin >> n;
    while (n-- > 0) {
        string operation, value;
        cin >> operation >> value;
        if (operation == "Insert") {
            tree.insert(stoi(value), 1);
        } else {
            tree.remove(stoi(value));
        }
    }
    tree.inorderTraverse();
    cout << endl;
    tree.preorderTraverse();

    return 0;
}
```

### 错误删除：

```c++
#include <bits/stdc++.h>
using namespace std;
template <typename K, typename V>
class RBTree {
private:
    static const bool RED = false;
    static const bool BLACK = true;

    struct RBNode {
        K key;
        V value;
        bool color;
        RBNode* left;
        RBNode* right;
        RBNode* parent;

        RBNode(RBNode* parent, const K& key, const V& value)
                : key(key), value(value), color(RED), left(nullptr), right(nullptr), parent(parent) {}
    };

    RBNode* root;

    void inorder(RBNode* root) {
        if (root != nullptr) {
            inorder(root->left);
            std::cout << root->key << " " << (root->color ? "B" : "R") << " ";
            inorder(root->right);
        }
    }

    void preorder(RBNode* root) {
        if (root != nullptr) {
            std::cout << root->key << " " << (root->color ? "B" : "R") << " ";
            preorder(root->left);
            preorder(root->right);
        }
    }

public:
    RBTree() : root(nullptr) {}

    void preorderTraverse() {
        preorder(root);
        std::cout << std::endl;
    }

    void inorderTraverse() {
        inorder(root);
        std::cout << std::endl;
    }

    bool colorOf(RBNode* node) {
        return node != nullptr ? node->color : BLACK;
    }

    void insert(const K& key, const V& value) {
        RBNode* t = root;
        if (t == nullptr) {
            root = new RBNode(nullptr, key, value);
            setColor(root, BLACK);
            return;
        }
        int cmp = 0;
        RBNode* parent = nullptr;
        while (t != nullptr) {
            parent = t;
            cmp = key - t->key;
            if (cmp < 0) {
                t = t->left;
            } else if (cmp > 0) {
                t = t->right;
            } else {
                t->value = value;
                return;
            }
        }
        RBNode * e = new RBNode(parent, key, value);
        if (cmp < 0) {
            parent->left = e;
        } else {
            parent->right = e;
        }

        fixAfterInsert(e);
    }

    void remove(const K& key) {
        RBNode* node = justFind(root, key);
        if (node == nullptr) return;
        deleteNode(node);
        return;
    }

private:
    RBNode* predecessor(RBNode* node) {
        if (node == nullptr) {
            return nullptr;
        } else if (node->left != nullptr) {
            RBNode* p = node->left;
            while (p->right != nullptr) {
                p = p->right;
            }
            return p;
        } else {
            RBNode* p = node->parent;
            RBNode* ch = node;
            while (p != nullptr && ch == p->left) {
                ch = p;
                p = p->parent;
            }
            return p;
        }
    }

    RBNode* successor(RBNode* node) {
        if (node == nullptr) {
            return nullptr;
        } else if (node->right != nullptr) {
            RBNode* p = node->right;
            while (p->left != nullptr) {
                p = p->left;
            }
            return p;
        } else {
            RBNode* p = node->parent;
            RBNode* ch = node;
            while (p != nullptr && ch == p->right) {
                ch = p;
                p = p->parent;
            }
            return p;
        }
    }

    RBNode* justFind(RBNode* root , int val) {
        if (root == NULL)
            return NULL;
        if (root->value > val) {
            return justFind(root->left, val);
        }
        if (root->value < val) {
            return justFind(root->right, val);
        }
        if (root->value == val)
            return root;
    }

    void setColor(RBNode* node, bool color) {
        if (node != nullptr) {
            node->color = color;
        }
    }

    void leftRotate(RBNode* g) {
        if (g != nullptr) {
            RBNode* p = g->right;
            g->right = p->left;
            if (p->left != nullptr) {
                p->left->parent = g;
            }
            p->parent= g->parent;
            if (g->parent == nullptr) {
                root = p;
            } else if (g == g->parent->left) {
                g->parent->left = p;
            } else {
                g->parent->right = p;
            }
            p->left = g;
            g->parent = p;
        }
    }

    void rightRotate(RBNode* g) {
        if (g != nullptr) {
            RBNode* p = g->left;
            g->left = p->right;
            if (p->right != nullptr) {
                p->right->parent = g;
            }
            p->parent = g->parent;
            if (g->parent == nullptr) {
                root = p;
            } else if (g == g->parent->left) {
                g->parent->left = p;
            } else {
                g->parent->right = p;
            }
            p->right = g;
            g->parent = p;
        }
    }

    void fixAfterInsert(RBNode* x) {
        x->color = RED;
        while (x != nullptr && x != root && x->parent->color == RED) {
            if (parentOf(x) == leftOf(parentOf(parentOf(x)))) {
                RBNode* y = rightOf(parentOf(parentOf(x)));
                if (colorOf(y) == RED) {
                    setColor(parentOf(x), BLACK);
                    setColor(y, BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    x = parentOf(parentOf(x));
                } else {
                    if (x == rightOf(parentOf(x))) {
                        x = parentOf(x);
                        leftRotate(x);
                    }
                    setColor(parentOf(x), BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    rightRotate(parentOf(parentOf(x)));
                }
            } else {
                RBNode* y = leftOf(parentOf(parentOf(x)));
                if (colorOf(y) == RED) {
                    setColor(parentOf(x), BLACK);
                    setColor(y, BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    x = parentOf(parentOf(x));
                } else {
                    if (x == leftOf(parentOf(x))) {
                        x = parentOf(x);
                        rightRotate(x);
                    }
                    setColor(parentOf(x), BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    leftRotate(parentOf(parentOf(x)));
                }
            }
        }
        root->color = BLACK;
    }

    void deleteNode(RBNode* node) {
        RBNode* e = node;
        RBNode* p = nullptr;
        if (e->left != nullptr && e->right != nullptr) {
            RBNode* s = successor(e);
            e->key = s->key;
            e->value = s->value;
            e = s;
        }
        RBNode* replacement = (e->left != nullptr ? e->left : e->right);
        if (replacement != nullptr) {
            replacement->parent = e->parent;
            if (e->parent == nullptr) {
                root = replacement;
            } else if (e == e->parent->left) {
                e->parent->left = replacement;
            } else {
                e->parent->right = replacement;
            }
            e->left = e->right = e->parent = nullptr;
            if (e->color == BLACK) {
                fixAfterDelete(replacement);
            }
        } else if (e->parent == nullptr) {
            root = nullptr;
        } else {
            if (e->color == BLACK) {
                fixAfterDelete(e);
            }
            if (e->parent != nullptr) {
                if (e == e->parent->left) {
                    e->parent->left = nullptr;
                } else if (e == e->parent->right) {
                    e->parent->right = nullptr;
                }
                e->parent = nullptr;
            }
        }
    }

    void fixAfterDelete(RBNode* x) {
        while (x != root && colorOf(x) == BLACK) {
            if (x == leftOf(parentOf(x))) {
                RBNode* sib = rightOf(parentOf(x));
                if (colorOf(sib) == RED) {
                    setColor(sib, BLACK);
                    setColor(parentOf(x), RED);
                    leftRotate(parentOf(x));
                    sib = rightOf(parentOf(x));
                }
                if (colorOf(leftOf(sib)) == BLACK && colorOf(rightOf(sib)) == BLACK) {
                    setColor(sib, RED);
                    x = parentOf(x);
                } else {
                    if (colorOf(rightOf(sib)) == BLACK) {
                        setColor(leftOf(sib), BLACK);
                        setColor(sib, RED);
                        rightRotate(sib);
                        sib = rightOf(parentOf(x));
                    }
                    setColor(sib, colorOf(parentOf(x)));
                    setColor(parentOf(x), BLACK);
                    setColor(rightOf(sib), BLACK);
                    leftRotate(parentOf(x));
                    x = root;
                }
            } else {
                RBNode* sib = leftOf(parentOf(x));
                if (colorOf(sib) == RED) {
                    setColor(sib, BLACK);
                    setColor(parentOf(x), RED);
                    rightRotate(parentOf(x));
                    sib = leftOf(parentOf(x));
                }
                if (colorOf(rightOf(sib)) == BLACK && colorOf(leftOf(sib)) == BLACK) {
                    setColor(sib, RED);
                    x = parentOf(x);
                } else {
                    if (colorOf(leftOf(sib)) == BLACK) {
                        setColor(rightOf(sib), BLACK);
                        setColor(sib, RED);
                        leftRotate(sib);
                        sib = leftOf(parentOf(x));
                    }
                    setColor(sib, colorOf(parentOf(x)));
                    setColor(parentOf(x), BLACK);
                    setColor(leftOf(sib), BLACK);
                    rightRotate(parentOf(x));
                    x = root;
                }
            }
        }
        setColor(x, BLACK);
    }
    RBNode* parentOf(RBNode* node) {
        return node != nullptr ? node->parent : nullptr;
    }

    RBNode* leftOf(RBNode* node) {
        return node != nullptr ? node->left : nullptr;
    }

    RBNode* rightOf(RBNode* node) {
        return node != nullptr ? node->right : nullptr;
    }
};

int main() {
    RBTree<int, int> tree;
    int n;
    cin >> n;
    while (n-- > 0) {
        string operation, value;
        cin >> operation >> value;
        if (operation == "Insert") {
            tree.insert(stoi(value), 1);
        } else {
            tree.remove(stoi(value));
        }
    }
    tree.inorderTraverse();
    cout << endl;
    tree.preorderTraverse();

    return 0;
}
```


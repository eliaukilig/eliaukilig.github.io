# My First Post


# test1
## test2
### test3
#### test4

# Math 
$$sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R$$

# code 

```cpp
#include &lt;iostream&gt;

enum Color { RED, BLACK };

template &lt;typename T&gt;
class RedBlackTree {
private:
    struct Node {
        T data;
        Color color;
        Node* left;
        Node* right;
        Node* parent;
        
        Node(T data) : data(data), color(RED), left(nullptr), right(nullptr), parent(nullptr) {}
    };
    
    Node* root;
    Node* NIL; // 哨兵节点
    
    // 左旋
    void leftRotate(Node* x) {
        Node* y = x-&gt;right;
        x-&gt;right = y-&gt;left;
        
        if (y-&gt;left != NIL)
            y-&gt;left-&gt;parent = x;
            
        y-&gt;parent = x-&gt;parent;
        
        if (x-&gt;parent == nullptr)
            root = y;
        else if (x == x-&gt;parent-&gt;left)
            x-&gt;parent-&gt;left = y;
        else
            x-&gt;parent-&gt;right = y;
            
        y-&gt;left = x;
        x-&gt;parent = y;
    }
    
    // 右旋
    void rightRotate(Node* y) {
        Node* x = y-&gt;left;
        y-&gt;left = x-&gt;right;
        
        if (x-&gt;right != NIL)
            x-&gt;right-&gt;parent = y;
            
        x-&gt;parent = y-&gt;parent;
        
        if (y-&gt;parent == nullptr)
            root = x;
        else if (y == y-&gt;parent-&gt;left)
            y-&gt;parent-&gt;left = x;
        else
            y-&gt;parent-&gt;right = x;
            
        x-&gt;right = y;
        y-&gt;parent = x;
    }
    
    // 插入修复
    void insertFixup(Node* z) {
        while (z-&gt;parent &amp;&amp; z-&gt;parent-&gt;color == RED) {
            if (z-&gt;parent == z-&gt;parent-&gt;parent-&gt;left) {
                Node* y = z-&gt;parent-&gt;parent-&gt;right;
                if (y-&gt;color == RED) {
                    // Case 1: uncle is red
                    z-&gt;parent-&gt;color = BLACK;
                    y-&gt;color = BLACK;
                    z-&gt;parent-&gt;parent-&gt;color = RED;
                    z = z-&gt;parent-&gt;parent;
                } else {
                    if (z == z-&gt;parent-&gt;right) {
                        // Case 2: uncle is black and z is a right child
                        z = z-&gt;parent;
                        leftRotate(z);
                    }
                    // Case 3: uncle is black and z is a left child
                    z-&gt;parent-&gt;color = BLACK;
                    z-&gt;parent-&gt;parent-&gt;color = RED;
                    rightRotate(z-&gt;parent-&gt;parent);
                }
            } else {
                Node* y = z-&gt;parent-&gt;parent-&gt;left;
                if (y-&gt;color == RED) {
                    // Case 1: uncle is red
                    z-&gt;parent-&gt;color = BLACK;
                    y-&gt;color = BLACK;
                    z-&gt;parent-&gt;parent-&gt;color = RED;
                    z = z-&gt;parent-&gt;parent;
                } else {
                    if (z == z-&gt;parent-&gt;left) {
                        // Case 2: uncle is black and z is a left child
                        z = z-&gt;parent;
                        rightRotate(z);
                    }
                    // Case 3: uncle is black and z is a right child
                    z-&gt;parent-&gt;color = BLACK;
                    z-&gt;parent-&gt;parent-&gt;color = RED;
                    leftRotate(z-&gt;parent-&gt;parent);
                }
            }
        }
        root-&gt;color = BLACK;
    }
    
    // 删除修复
    void deleteFixup(Node* x) {
        while (x != root &amp;&amp; x-&gt;color == BLACK) {
            if (x == x-&gt;parent-&gt;left) {
                Node* w = x-&gt;parent-&gt;right;
                if (w-&gt;color == RED) {
                    // Case 1: x的兄弟w是红色的
                    w-&gt;color = BLACK;
                    x-&gt;parent-&gt;color = RED;
                    leftRotate(x-&gt;parent);
                    w = x-&gt;parent-&gt;right;
                }
                if (w-&gt;left-&gt;color == BLACK &amp;&amp; w-&gt;right-&gt;color == BLACK) {
                    // Case 2: x的兄弟w是黑色的，w的两个子节点都是黑色的
                    w-&gt;color = RED;
                    x = x-&gt;parent;
                } else {
                    if (w-&gt;right-&gt;color == BLACK) {
                        // Case 3: x的兄弟w是黑色的，w的左孩子是红色的，右孩子是黑色的
                        w-&gt;left-&gt;color = BLACK;
                        w-&gt;color = RED;
                        rightRotate(w);
                        w = x-&gt;parent-&gt;right;
                    }
                    // Case 4: x的兄弟w是黑色的，w的右孩子是红色的
                    w-&gt;color = x-&gt;parent-&gt;color;
                    x-&gt;parent-&gt;color = BLACK;
                    w-&gt;right-&gt;color = BLACK;
                    leftRotate(x-&gt;parent);
                    x = root;
                }
            } else {
                Node* w = x-&gt;parent-&gt;left;
                if (w-&gt;color == RED) {
                    w-&gt;color = BLACK;
                    x-&gt;parent-&gt;color = RED;
                    rightRotate(x-&gt;parent);
                    w = x-&gt;parent-&gt;left;
                }
                if (w-&gt;right-&gt;color == BLACK &amp;&amp; w-&gt;left-&gt;color == BLACK) {
                    w-&gt;color = RED;
                    x = x-&gt;parent;
                } else {
                    if (w-&gt;left-&gt;color == BLACK) {
                        w-&gt;right-&gt;color = BLACK;
                        w-&gt;color = RED;
                        leftRotate(w);
                        w = x-&gt;parent-&gt;left;
                    }
                    w-&gt;color = x-&gt;parent-&gt;color;
                    x-&gt;parent-&gt;color = BLACK;
                    w-&gt;left-&gt;color = BLACK;
                    rightRotate(x-&gt;parent);
                    x = root;
                }
            }
        }
        x-&gt;color = BLACK;
    }
    
    // 找到最小值节点
    Node* minimum(Node* node) {
        while (node-&gt;left != NIL)
            node = node-&gt;left;
        return node;
    }
    
    // 删除节点
    void transplant(Node* u, Node* v) {
        if (u-&gt;parent == nullptr)
            root = v;
        else if (u == u-&gt;parent-&gt;left)
            u-&gt;parent-&gt;left = v;
        else
            u-&gt;parent-&gt;right = v;
        v-&gt;parent = u-&gt;parent;
    }
    
    // 删除对应节点
    void deleteNodeHelper(Node* node, T key) {
        Node* z = NIL;
        Node* x, *y;
        
        // 查找要删除的节点
        while (node != NIL) {
            if (node-&gt;data == key)
                z = node;
                
            if (node-&gt;data &lt;= key)
                node = node-&gt;right;
            else
                node = node-&gt;left;
        }
        
        if (z == NIL)
            return;
            
        y = z;
        Color y_original_color = y-&gt;color;
        
        if (z-&gt;left == NIL) {
            x = z-&gt;right;
            transplant(z, z-&gt;right);
        } else if (z-&gt;right == NIL) {
            x = z-&gt;left;
            transplant(z, z-&gt;left);
        } else {
            y = minimum(z-&gt;right);
            y_original_color = y-&gt;color;
            x = y-&gt;right;
            
            if (y-&gt;parent == z)
                x-&gt;parent = y;
            else {
                transplant(y, y-&gt;right);
                y-&gt;right = z-&gt;right;
                y-&gt;right-&gt;parent = y;
            }
            
            transplant(z, y);
            y-&gt;left = z-&gt;left;
            y-&gt;left-&gt;parent = y;
            y-&gt;color = z-&gt;color;
        }
        
        delete z;
        
        if (y_original_color == BLACK)
            deleteFixup(x);
    }
    
    // 中序遍历
    void inorderHelper(Node* node) {
        if (node != NIL) {
            inorderHelper(node-&gt;left);
            std::cout &lt;&lt; node-&gt;data &lt;&lt; &#34; &#34; &lt;&lt; (node-&gt;color == RED ? &#34;RED&#34; : &#34;BLACK&#34;) &lt;&lt; std::endl;
            inorderHelper(node-&gt;right);
        }
    }
    
    // 销毁树
    void destroyTree(Node* node) {
        if (node != NIL) {
            destroyTree(node-&gt;left);
            destroyTree(node-&gt;right);
            delete node;
        }
    }
    
public:
    RedBlackTree() {
        NIL = new Node(T());
        NIL-&gt;color = BLACK;
        NIL-&gt;left = nullptr;
        NIL-&gt;right = nullptr;
        root = NIL;
    }
    
    ~RedBlackTree() {
        destroyTree(root);
        delete NIL;
    }
    
    // 插入
    void insert(T key) {
        Node* z = new Node(key);
        z-&gt;left = NIL;
        z-&gt;right = NIL;
        
        Node* y = nullptr;
        Node* x = root;
        
        while (x != NIL) {
            y = x;
            if (z-&gt;data &lt; x-&gt;data)
                x = x-&gt;left;
            else
                x = x-&gt;right;
        }
        
        z-&gt;parent = y;
        
        if (y == nullptr)
            root = z; // 树为空
        else if (z-&gt;data &lt; y-&gt;data)
            y-&gt;left = z;
```

$sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R$

| 左对齐 | 居中对齐 | 右对齐 |
|:-------|:--------:|-------:|
| 文本1  |   文本2  |   文本3|
| 文本4  |   文本5  |   文本6|


---

> Author:   
> URL: https://example.org/posts/my-first-post/  


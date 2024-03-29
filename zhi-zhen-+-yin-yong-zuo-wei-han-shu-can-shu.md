# 指针 + 引用作为函数参数

先上代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node {
    int v;
    Node* next;
};

void del(Node*& node) {
    node = NULL;
}

void show(Node* node) {
    while (node) {
        cout << node << " ";
        node = node->next;
    }
    cout << endl;
}

int main() {
    Node* A = new Node();
    Node* B = new Node();
    Node* C = new Node();
    A->v = 1;
    A->next = B;
    B->v = 2;
    B->next = C;
    C->v = 3;
    C->next = NULL;
    show(A);    // 011DE148 011DE570 011DE110

    del(B);        // A = NULL, but A->next = 011DE570
    show(A);    // 011DE148 011DE570 011DE110

    del(A->next);    // A->next = NULL
    show(A);    // 011DE148
    show(C);    // 011DE110

    return 0;
}
```

* `del(B)`: 让指针 `B` 指向 `NULL`，数字 `2` 所在的结点占据的内存空间并未被释放。
* `del(A->next)`: 让 `A->next` 指向 `NULL`，数字 `2` 所在的结点占据的内存空间并未被释放。
* 以上两种方法都只是重定向指针指向 `NULL`，并未释放结点占据的内存空间，因此会造成内存泄漏。正确的删除结点函数的写法见[237. Delete Node in a Linked List](https://github.com/JiaHeng-DLUT/LeetCode/blob/master/链表/237.delete-node-in-a-linked-list.md)。

![Delete Node in a Linked List](.gitbook/assets/delete-node-in-a-linked-list.svg)


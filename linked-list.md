# Linked List

## Comparison between array and linked list

| 线性表（linear list） | 顺序表（sequence list） | 链表（linked list） |
| :---: | :---: | :---: |
|  | 按正常方式定义一个数组时，计算机会从内存中取出一块**连续的地址**来存放给**定长度**的数组； | 由**若干个结点**组成（每个结点代表一个元素），且结点在内存中的存储位置通常是**不连续**的。 |
| 读取 | `O(1)` | `O(n)` |
| 插入 | `O(n)` | `O(1)` |
| 删除 | `O(n)` | `O(1)` |
|  | 插入少，读取多，用数组。 | 插入多，读取少，用链表。 |

**实际的应用中，数组用的更多一些，因为它支持随机读取。**

## How to define a linked list❓

```c
struct node {
    typename data;    // 数据域
    node* next;    // 指针域
};
```

* 以链表是否存在头结点，又可以把链表分为**带头结点的链表**和**不带头结点的链表**。头结点一般称为 `head`，且其**数据域 `data` 不存放任何内容，而指针域 `next` 指向第一个数据域有内容的结点**（一般直接把这个结点叫作第一个结点）。

## Use `malloc` or `new` to assign memory to linked nodes

### `malloc` function 

* `malloc` 函数是C语言中 `stdlib. h` 头文件下用于**申请动态内存的函数**，其**返回类型是申请的同变量类型的指针**，其基本用法如下：

```c
typename* p = (typename*)malloc(sizeof(typename));
int* p = (int*)malloc(sizeof(int));
node* p = (node*)malloc(sizeof(node));
```

* 以需要申请的内存空间大小（即 `sizeof(node)` 为 `malloc` 函数的参数，这样 `malloc` 函数就会向内存申请一块大小为 `sizeof(node)` 的空间，并且返回指向这块空间的指针。但是此时这个指针是一个**未确定类型**的指针 `void*`，因此需要把它强制转换为 `node*` 型的指针，因此在 `malloc` 之前加上 `(node*)`。这样等号右边就得到了一个 `node*` 型的指针，并通过赋值等号把这个指针赋给 `node*` 型的指针变量 `p`，就成功申请了一块 `node` 类型大小的内存空间，即一个 `node` 型的结构体变量，并通过指针 `p` 来访问它。
* **如果申请失败，则会返回空指针 `NULL`**。一般来说，如果只是申请一个链表结点的话是不会失败的，失败一般发生在使用 `malloc` 申请了较大的动态数组，即 `int* p = (int*)malloc(1000000 * sizeof(int));` 这种情况下 `malloc` 会返回空指针 `NULL` 并赋值给`p`。因此只要是正常分配一个结点的空间，是不会失败的，当然发生死循环导致无限申请的情况除外。

### `new` operator 

* `new` 是 `C++` 中用来**申请动态空间的运算符**，其**返回类型同样是申请的同变量类型的指针**，其基本用法如下：

```cpp
typename* p = new typename;
int* p = new int;
node* p = new node;
```

* 如果申请失败，则会启动 `C++` 异常机制处理而不是返回空指针 `NULL`。和 malloc同理，如果是使用 `new` 申请了较大的动态数组，即 `int* p= new int [1000000];` 这时会发生异常，并**在没有特殊处理的情况下直接退出程序**。

### Memory leak 

* 内存泄露是指使用 `malloc` 与 `new` 开辟出来的内存空间在使用过后没有释放，导致其在程序结束之前始终占据该内存空间，这在一些较大的程序中很容易导致内存消耗过快以致最后无内存可分配。**`C/C++` 语言的设计者认为，程序员完全有能力自己控制内存的分配与释放，因此把对内存的控制操作全部交给了程序员。在使用完 `malloc` 与 `new` 开辟出来的空间后必须将其释放，否则会造成内存泄露。**

#### `free` function 

* `free` 函数是对应 `malloc` 函数的，同样是在 `stdlib.h` 头文件下。
* 其使用方法非常简单，只需要在 `free` 的参数中填写需要释放的内存空间的指针变量即可

```c
free(p);
```

* `free` 函数主要实现了两个效果：
* 释放指针变量 `p` 所指向的内存空间；
* 将指针变量 `p` 指向空地址 `NULL`。
* 在 `free` 函数执行之后，指针变量 `p` 本身并没有消失，只不过让它指向了空地址NULL，但是它原指向的内存是确实被释放了的。
* **`malloc` 函数与 `free` 函数必须成对出现，否则容易产生内存泄露。**

#### `delete` operator 

* `delete` 运算符是对应 `new` 运算符的，其使用方法和实现效果均与 `free` 相同。

```cpp
delete(p);
```

* **和 `free` 函数一样，`new` 运算符与 `delete` 运算符必须成对出现，否则会容易产生内存泄露。**
* 不过一般在考试中，分配的空间在程序结束时即被释放，因此即便不释放空间，也不会产生什么影响，并且内存大小一般也足够一道题的使用了。但是从编程习惯上，读者应养成即时释放空间的习惯。

## Basic operation of linked list 

```cpp
#include <bits/stdc++.h>
using namespace std;

struct node {
    int data;    // 数据域
    node* next;    // 指针域
};

// 创建链表
node* create(int arr[], int n) {
    node* p, * pre, * head;
    head = new node;
    head->next = NULL;
    pre = head;
    for (int i = 0; i < n; i++) {
        p = new node;
        p->data = arr[i];
        p->next = NULL;
        pre->next = p;
        pre = p;
    }
    return head;
}

// 在以 head 为头节点的链表上计数元素 x 的个数
int count(node* head, int x) {
    int cnt = 0;
    node* p = head->next;
    while (p != NULL) {
        if (p->data == x) {
            cnt++;
        }
        p = p->next;
    }
    return cnt;
}

// 把 x 插入以 head 为头结点的链表的第 pos 个位置上
void insert(node* head, int pos, int x) {
    node* p = head->next;
    for (int i = 0; i < pos - 1; i++) {
        p = p->next;    // pos - 1 是为了到插入位置的前一个结点
    }
    node* q = new node;
    q->data = x;
    q->next = p->next;
    p->next = q;
}

// 删除以 head 为头结点的链表中所有数据域为 x 的结点
void del(node* head, int x) {
    node* p = head->next;
    node* pre = head;
    while (p != NULL) {
        if (p->data == x) {
            pre->next = p->next;
            delete(p);
            p = pre->next;
        }
        else {
            pre = p;
            p = p->next;
        }
    }
}

int main() {
    int arr[] = { 5, 3, 6, 1, 2 };

    node* L = create(arr, 5);    // 5 3 6 1 2

    cout << count(L, 3) << endl;    // 1

    insert(L, 3, 4);    // 5 3 6 4 1 2

    del(L, 6);    // 5 3 4 1 2

    L = L->next;
    while (L != NULL) {
        printf("%d ", L->data);
        L = L->next;
    }
    return 0;
}
```

## Static linked list 

* 前面讲解的都是动态链表，即需要指针来建立结点之间的连接关系。而对有些问题来说，**结点的地址是比较小的整数（例如5位数的地址），这样就没有必要去建立动态链表，而应使用方便得多的静态链表。**
* 静态链表的实现原理是 `hash`，即**通过建立一个结构体数组，并令数组的下标直接表示结点的地址，来达到直接访问数组中的元素就能访问结点的效果**。另外，由于结点的访问非常方便，因此**静态链表是不需要头结点的**。静态链表结点定义的方法如下：

```cpp
struct Node {
    typename data;    // 数据域
    int next;    // 指针域
}node[size];
```

* 把结构体类型名和结构体变量名设成了不同的名字（即 `Node` 与 `node`），事实上在一般情况下它们是可以相同的，但是由于静态链表是由数组实现的，那么就有可能需要对其进行排序，这时如果结构体类型名和结构体变量名相同，sort函数就会报编译出错的问题，因此，**在使用静态链表时，尽量不要把结构体类型名和结构体变量名取成相同的名字**。

## References

* [算法笔记](https://book.douban.com/subject/26827295/)
* [图解数组和链表](https://www.cnblogs.com/jiqing9006/p/7615467.html)


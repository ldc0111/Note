[TOC]



# 顺序表

```c
#include <stdio.h>
#include <stdlib.h>
 
#define ERROR 0
#define OK 1
/*结构体*/
typedef struct Vector {
    int size, length;
    int *data;
} Vector;
/*构建*/
void init(Vector *vec, int size) {
    vec->data = (int *)malloc(sizeof(int) * size);
    vec->size = size;
    vec->length = 0;
}
/*扩展*/
void expand(Vector *vec) {
    vec->size *= 2;
    vec->data = (int *)realloc(vec->data, vec->size * sizeof(int));
}
/*插入*/
int insert(Vector *vec, int loc, int value) {
    if (loc < 0 || loc > vec->length) {
        return ERROR;
    }
    if (vec->length >= vec->size) {
        expand(vec);
    }
    for (int i = vec->length; i > loc; --i) {
        vec->data[i] = vec->data[i - 1];
    }
    vec->data[loc] = value;
    ++vec->length;
    return OK;
}
/*查找*/
int search(Vector *vec, int value) {
    for (int i = 0; i < vec->length; ++i) {
        if (vec->data[i] == value) {
            return i;
        }
    }
    return -1;
}
/*删除*/
int delete_node(Vector *vec, int index) {
    if (index < 0 || index >= vec->length) {
        return ERROR;
    }
    for (int i = index + 1; i < vec->length; ++i) {
        vec->data[i - 1] = vec->data[i];
    }
    vec->length--;
    return OK;
}
/*翻转*/
int reverse(Vector *vec, int begin, int end) {
    int temp;
    while (begin < end) {
        temp = vec->data[begin];
        vec->data[begin] = vec->data[end];
        vec->data[end] = temp;
        ++begin;
        --end;
    }
    return OK;
}
/*遍历*/
void output(Vector *vec) {
    for (int i = 0; i < vec->length; ++i) {
        if (i != 0) printf(" ");
        printf("%d", vec->data[i]);
    }
    printf("\n");
}
/*释放*/
void clear(Vector *vec) {
    free(vec->data);
    free(vec);
}
 
int main() {
    Vector *vec = (Vector *)malloc(sizeof(Vector));
    init(vec, 20);
    int n, t, a1, a2;
    char str[2][10] = {"success", "failed"};
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &t);
        switch (t) {
            case 1:
                scanf("%d%d", &a1, &a2);
                printf("%s\n", insert(a, a1, a2) == OK ? str[0] : str[1]);
                break;
            case 2:
                scanf("%d", &a1);
                printf("%s\n", delete_node(a, a1) == OK ? str[0] : str[1]);
                break;
            case 3:
                scanf("%d", &a1);
                printf("%s\n", search(a, a1) != -1 ? str[0] : str[1]);
                break;
            case 4:
                output(vec);
                break;
            default :
                printf("ERROR\n");
                break;
        }
    }
    return 0;
}
```

# 顺序表的循环左移

```c
#include <stdio.h>
#include <stdlib.h>
 
#define ERROR 0
#define OK 1
 
typedef struct {
    int size, length;
    int *data;
} Vector;
 
int init(Vector *vec, int size) {
    vec->size = size;
    vec->data = (int *)malloc(sizeof(int) * size);
    if (vec->data == NULL) {
        return ERROR;
    }
    vec->length = 0;
    return OK;
}
 
void expand(Vector *vec) {
    vec->size *= 2;
    vec->data = (int *)realloc(vec->data, vec->size * sizeof(int));
    return ;
}
 
int insert(Vector *vec, int loc, int value) {
    if (loc < 0 || loc > vec->length) {
        return ERROR;
    }
    if (vec->length == vec->size) {
        expand(vec);
    }
    for (int i = vec->length; i > loc; --i) {
        vec->data[i] = vec->data[i - 1];
    }
    vec->data[loc] = value;
    vec->length++;
    return OK;
}
 
int reverse(Vector *vec, int begin, int end) {
    int temp;
    while (begin < end) {
        temp = vec->data[begin];
        vec->data[begin] = vec->data[end];
        vec->data[end] = temp;
        ++begin;
        --end;
    }
    return OK;
}
 
void output(Vector *vec) {
    if (vec->length) {
        printf("%d", vec->data[0]);
    }
    for (int i = 1; i < vec->length; ++i) {
        printf(" %d", vec->data[i]);
    }
    printf("\n");
}
 
void clear(Vector *vec) {
    free(vec->data);
    free(vec);
}
 
int main() {
    int n, k, a;
    Vector *vec = (Vector *)malloc(sizeof(Vector));
    scanf("%d%d", &n, &k);
    init(vec, n);
    for (int i = 1; i<= n; ++i) {
        scanf("%d", &a);
        insert(vec, i - 1, a);
    }
    reverse(vec, 0, k - 1);
    reverse(vec, k, n - 1);
    reverse(vec, 0, n - 1);
    output(vec);
    clear(vec);
    return 0;
}
```



# 有序集合的运算

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
 
#define ERROR 0
#define OK 1
 
typedef struct {
    int size, length;
    int *data;
} Vector;
 
int init(Vector *vec, int size) {
    vec->size = size;
    vec->data = (int *)malloc(sizeof(int) * size);
    if (vec->data == NULL) {
        return ERROR;
    }
    vec->length = 0;
    return OK;
}
 
void expand(Vector *vec) {
    vec->size *= 2;
    vec->data = (int *)realloc(vec->data, vec->size * sizeof(int));
    return ;
}
 
int insert(Vector *vec, int loc, int value) {
    if (loc < 0 || loc > vec->length) {
        return ERROR;
    }
    if (vec->length == vec->size) {
        expand(vec);
    }
    for (int i = vec->length; i > loc; --i) {
        vec->data[i] = vec->data[i - 1];
    }
    vec->data[loc] = value;
    vec->length++;
    return OK;
}
 
void output(Vector *vec) {
    printf("%d\n", vec->length);
    if (vec->length) {
        printf("%d", vec->data[0]);
    }
    for (int i = 1; i < vec->length; ++i) {
        printf(" %d", vec->data[i]);
    }
    printf("\n");
    return ;
}
 
void merge(Vector *v1, Vector *v2, Vector *v3) {
    int i = 0, j = 0, k = 0;
    while (i < v1->length && j < v2->length) {
        if (v1->data[i] < v2->data[j]) {
            ++i;
        } else if (v1->data[i] > v2->data[j]) {
            ++j;
        } else {
            insert(v3, k++, v1->data[i]);
            ++i, ++j;
        }
    }
}
 
void clear(Vector *vec) {
    free(vec->data);
    free(vec);
}
 
int main() {
    int n, x;
    Vector *vec1 = (Vector *)malloc(sizeof(Vector));
    Vector *vec2 = (Vector *)malloc(sizeof(Vector));
    Vector *vec3 = (Vector *)malloc(sizeof(Vector));
    scanf("%d", &n);
    init(vec1, n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &x);
        insert(vec1, i, x);
    }
    scanf("%d", &n);
    init(vec2, n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &x);
        insert(vec2, i, x);
    }
    init(vec3, vec1->size + vec2->size);
    merge(vec1, vec2, vec3);
    output(vec3);
    clear(vec1);
    clear(vec2);
    clear(vec3);
    return 0;
}
```



# 稀疏多项式

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
 
#define ERROR 0
#define OK 1
 
typedef struct {
    int c, e;
} Data;
 
typedef struct {
    int size, length;
    Data *data;
} Vector;
 
int init(Vector *vec, int size) {
    vec->size = size;
    vec->data = (Data *)malloc(sizeof(Data) * size);
    if (vec->data == NULL) {
        return ERROR;
    }
    vec->length = 0;
    return OK;
}
 
void expand(Vector *vec) {
    vec->size *= 2;
    vec->data = (Data *)realloc(vec->data, vec->size * sizeof(Data));
    return ;
}
 
int insert(Vector *vec, int loc, Data value) {
    if (loc < 0 || loc > vec->length) {
        return ERROR;
    }
    if (vec->length == vec->size) {
        expand(vec);
    }
    for (int i = vec->length; i > loc; --i) {
        vec->data[i] = vec->data[i - 1];
    }
    vec->data[loc] = value;
    vec->length++;
    return OK;
}
 
void output(Vector *vec, int x) {
    int ret = 0;
    for (int i = 0; i < vec->length; ++i) {
        ret += vec->data[i].c * (int)pow(x, vec->data[i].e);
    }
    printf("%d\n", ret);
}
 
void clear(Vector *vec) {
    free(vec->data);
    free(vec);
}
 
int main() {
    Data data;
    int m, x;
    Vector *vec = (Vector *)malloc(sizeof(Vector));
    scanf("%d", &m);
    init(vec, m);
    for (int i = 0; i< m; ++i) {
        scanf("%d%d", &data.c, &data.e);
        insert(vec, i, data);
    }
    scanf("%d", &x);
    output(vec, x);
    clear(vec);
    return 0;
}
```



# 哪位同学最优秀

```c
#include <stdio.h>
#include <stdlib.h>
 
typedef struct Node{
    int data;
    struct Node *next;
}Node, *LinkedList;
 
Node insert(LinkedList head, int index, int value) {
    Node *p, ret;
    p = &ret;
    ret.data = 0;
    ret.next = head;
    while (p->next && index) {
        p = p->next;
        --index;
    }
    if (index == 0) {
        Node *node = (Node *)malloc(sizeof(Node));
        node->data = value;
        node->next = p->next;
        p->next = node;
        ret.data = 1;
    }
    return ret;
}
 
Node delete_node(LinkedList head, int index) {
    Node ret, *p, *q;
    ret.next = head;
    ret.data = 0;
    p = &ret;
    while (p->next && index) {
        p = p->next;
        --index;
    }
    if (p->next) {
        q = p->next;
        p->next = q->next;
        ret.data = 1;
        free(q);
    }
    return ret;
}
 
void output(LinkedList head) {
    int len = 0;
    Node *p = head;
    while (p) {
        ++len;
        p = p->next;
    }
    len >>= 1;
    p = head;
    while (len--) {
        p = p->next;
    }
    printf("%d\n", p->data);
}
 
void clear(LinkedList head) {
    Node *p, *q;
    p = head;
    while (p) {
        q = p->next;
        free(p);
        p = q;
    }
    return ;
}
 
int main() {
    LinkedList linkedlist = NULL;
    Node ret;
    int n, m, k;
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; ++i) {
        ret = insert(linkedlist, i, i + 1);
        if (ret.data == 0) {
            printf("ERROR\n");
        }
        linkedlist = ret.next;
    }
    for (int i = 0; i < m; ++i) {
        scanf("%d", &k);
        ret = delete(linkedlist, k - 1);
        if (ret.data == 0) {
            printf("ERROR\n");
        }
        linkedlist = ret.next;
    }
    output(linkedlist);
    clear(linkedlist);
    return 0;
}
```



# 单链表就地转置

```c
#include <stdio.h>
#include <stdlib.h>
 
typedef struct Node{
    char data;
    struct Node *next;
}Node, *LinkedList;
 
Node insert(LinkedList head, int index, char value) {
    Node *p, ret;
    p = &ret;
    ret.data = 0;
    ret.next = head;
    while (p->next && index) {
        p = p->next;
        --index;
    }
    if (index == 0) {
        Node *node = (Node *)malloc(sizeof(Node));
        node->data = value;
        node->next = p->next;
        p->next = node;
        ret.data = 1;
    }
    return ret;
}
 
LinkedList reverse(LinkedList head) {
    Node ret, *p, *q;
    ret.next = NULL;
    p = head;
    while (p) {
        q = p->next;
        p->next = ret.next;
        ret.next = p;
        p = q;
    }
    return ret.next;
}
 
void output(LinkedList head) {
    int len = 0;
    Node *p = head;
    if (p) {
        printf("%c", p->data);
        p = p->next;
    }
    while (p) {
        printf(" %c", p->data);
        p = p->next;
    }
    printf("\n");
}
 
void clear(LinkedList head) {
    Node *p, *q;
    p = head;
    while (p) {
        q = p->next;
        free(p);
        p = q;
    }
    return ;
}
 
int main() {
    LinkedList linkedlist = NULL;
    Node ret;
    int n;
    int c;
    scanf("%d\n", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%c", &c);
        if (i != n - 1) {
            scanf(" ");
        }
        ret = insert(linkedlist, i, c);
        if (ret.data == 0) {
            printf("ERROR\n");
            return 1;
        }
        linkedlist = ret.next;
    }
    linkedlist = reverse(linkedlist);
    output(linkedlist);
    clear(linkedlist);
    return 0;
}
```

# 单项循环链表变双向

```c
# include <stdio.h>
# include <stdlib.h>

typedef struct Node{
    int data;
    struct Node *next;
    struct Node *prior;
}Node, *LinkedList;

Node insert(LinkedList head, int index, int value) {
    Node *p, ret;
    int raw_index = index, len = 0;
    p = head;
    ret.data = 0;
    while (p && index) {
        p = p->next;
        --index;
        ++len;
        if (p == head) break;
    }
    if (index == 0) {
        Node *node = (Node *)malloc(sizeof(Node));
        node->data = value;
        node->next = node;
        if (p != NULL) {
            node->next = p->next;
            p->next = node;
        }
        node->prior = NULL;
        if (p == head && len == raw_index) {
            head = node;
        }
        ret.data = 1;
    }
    ret.next = head;
    return ret;
}

LinkedList build(LinkedList head) {
    Node *p, *q;
    if (head == NULL) {
        return head;
    }
    p = head;
    q = head->next;
    do {
        q->prior = p;
        p = p->next;
        q = q->next;
    } while (q != head->next);
    return head;
}

void output(LinkedList head, int m) {
    int len = 0;
    Node *p = head, *q;
    while (p->data != m) {
        p = p->next;
    }
    q = p;
    printf("%d", p->data);
    p = p->prior;
    while (p != q) {
        printf(" %d", p->data);
        p = p->prior;
    }
    printf("\n");
    return ;
}

void clear(LinkedList head) {
    Node *p, *q;
    if (head == NULL) {
        return ;
    }
    p = head->next;
    head->next = NULL;
    while (p) {
        q = p->next;
        free(p);
        p = q;
    }
    return ;
}

int main() {
    LinkedList linkedlist = NULL;
    Node ret;
    int n, m, num;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &num);
        ret = insert(linkedlist, i, num);
        if (ret.data == 0) {
            printf("ERROR\n");
            return 1;
        }
        linkedlist = ret.next;
    }
    linkedlist = build(linkedlist);
    scanf("%d", &m);
    output(linkedlist, m);
    fflush(stdout);
    clear(linkedlist);
    return 0;
}
```

# 队列

```c
#include <stdio.h>
#include <stdlib.h>
 
#define ERROR 0
#define OK 1
 
typedef struct Queue{
    int *data;
    int head, tail, length;
}Queue;
/*初始化*/
void init(Queue *q, int length) {
    q->data = (int *)malloc(sizeof(int) * length);
    q->length = length;
    q->head = 0;
    q->tail = -1;
}
/*插入*/
int push(Queue *q, int element) {
    if (q->tail + 1 >= q->length) {
        return ERROR;
    }
    q->data[++q->tail] = element;
    return OK;
}
/*遍历*/
void output(Queue *q) {
    if (!empty(q)) {
        printf("%d", q->data[q->head]);
    }
    for (int i = q->head + 1; i <= q->tail; ++i) {
        printf(" %d", q->data[i]);
    }
    printf("\n");
}
/*出队*/
int front(Queue *q) {
    return q->data[q->head];
}

void pop(Queue *q) {
    ++(q->head);
}
/*空的*/
int empty(Queue *q) {
    return q->head > q->tail;
}
/*清除*/
void clear(Queue *q) {
    free(q->data);
    free(q);
}
 
int main() {
    Queue *queue = (Queue *)malloc(sizeof(Queue));
    init(queue, 100);
    int m, n, k;
    scanf("%d", &m);
    for (int i = 0; i < m; i++) {
        scanf("%d", &k);
        push(queue, k);
    }
    scanf("%d", &n);
    while (n--) {
        pop(queue);
    }
    printf("%d\n", front(queue));
    output(queue);
    clear(queue);
    return 0;
}
```

# 循环队列

```c
#include <stdio.h>
#include <stdlib.h>

#define ERROR 0
#define OK 1

typedef struct Queue {
    int *data;
    int head, tail, length, count;
}Queue;

void init(Queue *q, int length) {
    q->data = (int *)malloc(sizeof(int) * length);
    q->length = length;
    q->head = 0;
    q->tail = -1;
    q->count = 0;
}

int push(Queue *q, int element) {
    if (q->count >= q->length) {
        return ERROR;
    }
    q->tail = (q->tail + 1) % q->length;
    q->data[q->tail] = element;
    q->count++;
    return OK;
}

void output(Queue *q) {
    int i = q->head;
    do {
        printf("%d ", q->data[i]);
        i = (i + 1) % q->length;
    } while(i != (q->tail + 1) % q->length);
    printf("\n");
}

int front(Queue *q) {
    return q->data[q->head];
}

void pop(Queue *q) {
    q->head = (q->head + 1) % q->length;
    q->count--;
}

int empty(Queue *q) {
    return q->count == 0;
}

void clear(Queue *q) {
    free(q->data);
    free(q);
}

int main() {
    Queue *q = (Queue *)malloc(sizeof(Queue));
    init(q, 100);
    for (int i = 1; i <= 10; i++) {
        push(q, i);
    }
    output(q);
    if (!empty(q)) {
        printf("%d\n", front(q));
        pop(q);        
    }
    output(q);
    clear(q);
    return 0;
}
```

# 栈

```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
 
#define ERROR 0
#define OK 1
 
typedef struct Stack {
    int *data;
    int size, top;
} Stack;
 
void init(Stack *s, int n) {
    s->data = (int *)malloc(sizeof(int) * n);
    s->size = n;
    s->top = -1;
}
 
int push(Stack *s, int e) {
    if (s->top == s->size - 1) {
        return ERROR;
    }
    s->data[++(s->top)] = e;
    return OK;
}
 
int pop(Stack *s) {
    if (!empty(s)) {
        --(s->top);
        return OK;
    }
    return ERROR;
}
 
int top(Stack *s) {
    return s->data[s->top];
}
 
int empty(Stack *s) {
    return (s->top == -1);
}
 
int precede(int opr1, int opr2) {
    int rank_a, rank_b;
    if (opr1 == '+' || opr1 == '-') {
        rank_a = 1;
    } else {
        rank_a = 2;
    }
    if (opr2 == '+' || opr2 == '-') {
        rank_b = 1;
    } else {
        rank_b = 2;
    }
    return rank_a > rank_b;
}
int operate(int opr, int a, int b) {
    switch (opr) {
        case '+' : return a + b;
        case '-' : return b - a;
        case '*' : return a * b;
        case '/' : return b / a;
    }
    return 0;
}
void calc(Stack *numbers, Stack *operators) {
    int a, b;
    a = top(numbers);
    pop(numbers);
    b = top(numbers);
    pop(numbers);
    push(numbers, operate(top(operators), a, b));
    pop(operators);
}
 
void clear(Stack *s) {
    free(s->data);
    free(s);
}
 
int main() {
    int n;
    Stack *num = (Stack *)malloc(sizeof(Stack));
    Stack *opr = (Stack *)malloc(sizeof(Stack));
    scanf("%d", &n);
    init(num, n);
    init(opr, n);
    char *buffer = (char *)malloc(n + 1);
    scanf("%s", buffer);
    for (int i = 0; i < n; i++) {
        if (isdigit(buffer[i])) {
            push(num, buffer[i] - '0');
        } else {
            while (!empty(opr) && !precede(buffer[i], top(opr))) {
                calc(num, opr);
            }
            push(opr, buffer[i]);
        }
    }
    while (!empty(opr)) {
        calc(num, opr);
    }
    printf("%d\n", top(num));
    clear(num);
    clear(opr);
    free(buffer);
    return 0;
}#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
 
#define ERROR 0
#define OK 1
 
typedef struct Stack {
    int *data;
    int size, top;
} Stack;
 
void init(Stack *s, int n) {
    s->data = (int *)malloc(sizeof(int) * n);
    s->size = n;
    s->top = -1;
}
 
int push(Stack *s, int e) {
    if (s->top == s->size - 1) {
        return ERROR;
    }
    s->data[++(s->top)] = e;
    return OK;
}
 
int pop(Stack *s) {
    if (!empty(s)) {
        --(s->top);
        return OK;
    }
    return ERROR;
}
 
int top(Stack *s) {
    return s->data[s->top];
}
 
int empty(Stack *s) {
    return (s->top == -1);
}
 
int precede(int opr1, int opr2) {
    int rank_a, rank_b;
    if (opr1 == '+' || opr1 == '-') {
        rank_a = 1;
    } else {
        rank_a = 2;
    }
    if (opr2 == '+' || opr2 == '-') {
        rank_b = 1;
    } else {
        rank_b = 2;
    }
    return rank_a > rank_b;
}
int operate(int opr, int a, int b) {
    switch (opr) {
        case '+' : return a + b;
        case '-' : return b - a;
        case '*' : return a * b;
        case '/' : return b / a;
    }
    return 0;
}
void calc(Stack *numbers, Stack *operators) {
    int a, b;
    a = top(numbers);
    pop(numbers);
    b = top(numbers);
    pop(numbers);
    push(numbers, operate(top(operators), a, b));
    pop(operators);
}
 
void clear(Stack *s) {
    free(s->data);
    free(s);
}
 
int main() {
    int n;
    Stack *num = (Stack *)malloc(sizeof(Stack));
    Stack *opr = (Stack *)malloc(sizeof(Stack));
    scanf("%d", &n);
    init(num, n);
    init(opr, n);
    char *buffer = (char *)malloc(n + 1);
    scanf("%s", buffer);
    for (int i = 0; i < n; i++) {
        if (isdigit(buffer[i])) {
            push(num, buffer[i] - '0');
        } else {
            while (!empty(opr) && !precede(buffer[i], top(opr))) {
                calc(num, opr);
            }
            push(opr, buffer[i]);
        }
    }
    while (!empty(opr)) {
        calc(num, opr);
    }
    printf("%d\n", top(num));
    clear(num);
    clear(opr);
    free(buffer);
    return 0;
}
```


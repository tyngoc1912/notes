# Queue
Hoạt động theo cơ chế FIFO
## Cài đặt bằng mảng
```cpp
int a[100000], maxN = 100000;
int n = 0;

void push(int x){
    if(n == maxN) return;
    a[n] = x;
    n++;
}

void pop(){
    if(n == 0) return;
    for(int i = 0; i < n - 1; i++){
        a[i] = a[i + 1]; //dịch trái
    }
    n--;
}

int ssz(){
    return n;
}

bool empty(){
    return n == 0;
}

int front(){
    return a[0];
}
```
## Cài đặt bằng danh sách liên kết
```cpp
struct Node{
    int data;
    Node* next;
};
typedef Node* node;

node makeNode(int x){
    node *tmp = new node;
    tmp->data = x;
    tmp->next = NULL;
    return tmp;
}

void push(node &queue, int x){
    node tmp = makeNode(x);
    if(queue == NULL){
        queue = tmp;
        return;
    }
    node find = queue;
    while(find->next != NULL) find = find->next;
    find->next = tmp;
}

void pop(node &queue){
    if(queue = NULL) return;
    node tmp = node;
    queue = queue->next;
    delete tmp;
}

int sz(node queue){
    int res = 0;
    while(queue != NULL){
        res++;
        queue = queue->next;
    }
    return res;
}

bool empty(node queue){
    return queue == NULL;
}

int front(node queue){
    return queue->data;
}

void print(node queue){
    while(queue != NULL){
        cout << queue->data << ' ';
        queue = queue->next;
    }
}
```
# Priority queue 
Như `queue` nhưng lưu được dưới dạng max heap hoặc min heap => truy xuất phần tử bé nhất hoặc lớn nhất nhanh chóng
```cpp
// max-heap
priority_queue<int> Q;                             

// min-heap
priority_queue<int, vector<int>, greater<int>> Q;
```
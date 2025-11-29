# Linked list
- Xây dựng các cấu trúc Node (data, reference) ⇒ data : bất cứ dữ liệu gì, reference : lưu địa chỉ của node tiếp theo, node cuối tham chiếu trỏ vào NULL
- Quản lý dslk = node đầu tiên : head
- Cấu trúc Node tự trỏ → tham chiếu trỏ đến địa chỉ của phần tử giống hệt nó đều là Node ⇒ mang tính đệ quy
- Gắn 1 node vào dslk ⇒ cấp phát động ⇒ khai báo con trỏ kiểu Node
## Danh sách liên kết đơn
```cpp
//Singly Linked list (Danh sách liên kết đơn)
struct Node{
    //data -> có thể bất cứ KDL gì
    int data;
    //reference -> là 1 con trỏ kiểu Node => lưu địa chỉ của Node tiếp theo nên bản thân nó phải là con trỏ kiểu Node
    Node* next; //Cấu trúc tự trỏ
};
// mỗi node trong dslk đều là cấp phát động (con trỏ kiểu node) => quản lý thông qua địa chỉ => lưu địa chỉ thằng tiếp theo
typedef Node* node;  

//Tạo 1 node mới từ thông tin đã có
node makeNode(int newData){
    //Cấp phát động 1 node 
    node tmp = new Node(); // Node* tmp = new Node();
    tmp->data = newData; //gán data
    tmp->next = NULL; //trỏ địa chỉ vào NULL -> có thể thay đổi sau khi đưa vào linked list
    return tmp;
}

//Tính số lượng pt trong dslk
int sz(node head){
    int cnt = 0;
    while(head != NULL){ //địa chỉ của node head chưa tới node cuối
        cnt++;
        head = head->next; //head->next : địa chỉ của node tiếp theo => node head sẽ nhảy sang node tiếp theo để quản lý
    }
    return cnt;
}

//In dslk
void print(node head){
    while(head != NULL){
        cout << head->data << ' '; // In giá trị data
        head = head->next; //nhảy sang node tiếp theo
    }
}

//Thêm đầu dslk
void pushFront(node &head, int newData){ //Sau khi kết thúc hàm muốn nó làm đổi dslk thì phải tham chiếu 
    node tmp = makeNode(newData);
    //Dslk rỗng hoặc dslk đã có phần tử => kt trước khi thêm
    if(head == NULL){ //DSLK rỗng
        head = tmp; // Vì head cũng là con trỏ kiểu Node nên chỉ việc gán cho nhau là đc
        return;
    }
    else{ //DSLK có nhiều pt
        //Cập nhật phần tham chiếu trỏ đến node head đầu tiên trước rồi cập nhật head = tmp
        tmp->next = head;
        head = tmp; // giá trị mới đc thêm vào đầu tiên dslk
    }
}

//Thêm cuối dslk
void pushBack(node &head, int newData){
    node tmp = makeNode(newData);
    if(head == NULL){
        head = tmp;
        return;
    }
    else{
        //Tìm đc node cuối cùng -> cho head->next = tmp
        node find = head;
        //lặp đến code cuối cùng
        while(find->next != NULL){
            find = find->next; //Gán địa chỉ cho node tiếp theo
        }
        find->next = tmp; //Cập nhật node cuối = tmp
    }
}

//Thêm giữa dslk
void insert(node &head, int newData, int pos){
    int n = size(head); //đếm số lượng pt dslk;
    if(pos < 1 || pos > n + 1) return; // Chèn các vị trí hợp lệ nằm trong dslk và vị trí bắt đầu từ 1 -> n
    if(pos == 1) pushFront(head, newData); // vị trí đầu
    else if(pos == n + 1) pushBack(head, newData); // vị trí sau vị trí n (cuối cùng)
    else{ //chèn vào giữa
        node find = head;
        for(int i = 1; i <= pos - 2; i++){ //ra đc vị trí của node trước node cần chèn
            find = find->next;
        }
        node tmp = makeNode(newData);//Tham chiếu cái node tmp này vào node cần chèn trc để giữ được cái mạch dslk
        tmp->next = find->next; // gán địa chỉ của node chỗ cần chèn vào trước
        find->next = tmp;//cập nhật node tmp vào vị trí cần chèn
    }
}

//Xóa đầu dslk
void popFront(node &head){
    if(head == NULL) return; // dslk rỗng
    node tmp = head; // tạo 1 node tmp = node head
    head = head->next; // nhảy sang node tiếp theo
    delete tmp; // xóa thẳng node tmp đang chứa node head ban đầu
}

//Xóa cuối dslk
void popBack(node &head){
    if(head == NULL) return; // dslk rỗng
    node p = head, q = NULL; // dùng kĩ thuật 2 con trỏ chạy trước và chạy sau
    while(p->next != NULL){
        q = p; // q sẽ là node kế cuối
        p = p->next; // khi p là node cuối
    }
    delete p; // xóa p là xóa node cuối
    if(q == NULL) head = NULL; //dslk chỉ có 1 pt
    else q->next = NULL; // dslk có 2 pt trở lên node q trở thành node cuối
}
//Sử dụng ý tưởng thuần túy ko dùng kĩ thuật 2 con trỏ
void popBack2(node &head){
    if(head == NULL) return;
    node tmp = head;
    if(tmp->next == NULL){
        head = NULL;
        delete tmp;
        return;
    }
    while(tmp->next->next != NULL) tmp = tmp->next;
    node last = tmp->next;
    tmp->next = NULL;
    delete last;
}

//Xóa giữa dslk
void erase(node &head, int pos){
    int n = size(head); // đếm số lượng pt trong dslk
    if(pos < 1 || pos > n) return; // vị trí ko hợp lệ
    // kĩ thuật 2 con trỏ
    node p = head, q = NULL;
    for(int i = 1; i < pos; i++){ //con trỏ p đến vị trí pos cần xóa và con trỏ q sẽ đứng trc p
        q = p;
        p = p->next;
    }
    if(q != NULL) q->next = p->next; // con trỏ q trỏ vào con trỏ đừng sau p
    else head = head->next; // xóa pt đầu tiên của dslk
    delete p;
}
//Ý tưởng thuần túy ko dùng 2 con trỏ
void erase2(node &head, int pos){
    int n = size(head);
    if(pos < 1 || pos > n + 1) return;
    if(pos == 1) popFront(head);
    else{
        node find = head;
        for(int i = 1; i <= pos - 2; i++){
            find = find->next; //đến vị trí k - 1 => node thứ k - 1
        }
        node tmp = find->next; // node thứ k 
        //Kết nối node thứ k - 1 đến node thứ k + 1
        find->next = tmp->next; 
        delete tmp; 
    }
}

//Đảo dslk
node reverse(node &head){
    node prev = NULL;
    while(head != NULL){
        node tmp = head->next;
        head->next = prev;
        prev = head;
        head = tmp;
    }
    return prev;
}

//Tìm kiếm 1 pt trong dslk
bool search(node head, int val){
    while(head != NULL){
        if(head->data == val) return true;
        head = head->next;
    }
    return false;
}

//Sắp xếp trong dslk -> selection sort
void sort(node &head){
    for(node i = head; i != NULL; i = i->next){
        node min = i;
        for(node j = i->next; j != NULL; j = j->next){
            if(min->data > j->data){
                min = j;
            }
        }
        swap(i->data, min->data);
        // int tmp = min->data;
        // min->data = i->data;
        // i->data = tmp;
    }
}

//Thêm các pt vào dslk đến khi dừng
void input(node &head){
    head = NULL;
    while(1){
        int x; cin >> x;
        if(x == -1) break; // hoặc có thể là điều kiện x là khác để dừng
        else pushBack(head, x);
    }
}
```
## Danh sách liên kết đôi
```cpp
//Doubly linked list (Danh sách liên kết đôi)
struct Node{
    int data;
    Node* next;
    Node* prev;
};
typedef Node* node;

node makeNode(int x){
    node tmp = new node;
    tmp->data = x;
    tmp->next = tmp->prev = NULL;
    return tmp;
}

void print(node head){
    while(head != NULL){
        cout << head->data << ' ';
        head = head->next;
    }
}

int sz(node head){
    int cnt = 0;
    while(head != NULL){
        cnt++;
        head = head->next;
    }
    return cnt;
}

void pushFront(node &head, int x){
    node tmp = makeNode(x);
    tmp->next = head;
    if(head != NULL){
        //Tránh bị lỗi truy cập vào dslk rỗng
        head->prev = tmp;
    }
    head = tmp;
}

void pushBack(node &head, int x){
    node tmp = makeNode(x);
    node find = head;
    //Tránh trường hợp dslk rỗng => lỗi truy cập
    if(head == NULL){
        head = tmp;
        return;
    }
    while(find->next != NULL){
        find = find->next;
    }
    find->next = tmp;
    tmp->prev = find;
}

void insert(node &head, int x, int k){
    int n = size(head);
    if(k < n || k > n + 1) return;
    if(k == 1){
        pushFront(head, x);
        return;
    }
    else{
        node find = head;
        for(int i = 1; i <= k - 1; i++){
            find = find->next;
        }
        node tmp = makeNode(x);
        tmp->next = find;
        find->prev->next = tmp;
        tmp->prev = find->prev;
        find->prev = tmp;
    }
}

void popFront(node &head){
    if(head == NULL) return; // DSLK rong
    node deleteNode = head;
    //Cho node head thanh node thu 2 trong DSLK
    head = head->next; 
    //Neu head != NULL thi tro ngược lại vào NULL
    if(head != NULL){
        head->prev = NULL;
    }
    //Giai phong vung nho
    delete deleteNode;
}

void popBack(node &head){
    if(head == NULL) return; // DSLK rong
    if(head->next == NULL){
        delete head;
        head = NULL;
    }
    else{
        node tmp = head;
        //Duyệt đến node thứ 2 từ cuối về : tmp
        while(tmp->next->next != NULL){
            tmp = tmp->next;
        }
        //Lưu lại node cuối để giải phóng
        node delNode = tmp->next;
        //Node tmp = NULL
        tmp->next = NULL;
        //Giải phóng node cuối
        delete delNode;
    }
}

void erase(node &head, int k){
    if(k < 1 || k > len(head)) return; // vi tri xoa ko hop le
    if(k == 1){
        popFront(head);
    }
    else{
        node tmp = head;
        //Duyet den node k - 1
        for(int i = 1; i <= k - 1; i++){
            tmp = tmp->next;
        }
        //Luu lai node thu k
        node delNode = tmp;
        //Cho k - 1 => tro vao k + 1
        tmp->prev->next = tmp->next;
        //Cho k + 1 => tro nguoc k - 1
        if(tmp->next != NULL)
            tmp->next->prev = tmp->prev;
        //Giai phong node thu k
        delete delNode;
    }
}
```
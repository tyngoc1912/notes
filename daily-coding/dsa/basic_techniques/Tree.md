# Tree
## Cây - đồ thị
Có n đỉnh -> n - 1 cạnh, là đồ thị liên thông và ko có chu trình
```cpp
//Kiểm tra đồ thị phải là cây hay ko => đồ thị liên thông + ko có chu trình
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
bool DFScycle(int u, int parent){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]){
            if(DFScycle(v, u)) return true;
        }
        else if(v != parent) return true;
    }
    return false;
}
void tree(){
    input();
    //có chu trình -> ko phải cây
    if(DFScycle(1, 0)) cout << "NO\n";
    else{
        //duyệt xem đồ thị có liên thông hay ko
        for(int i = 1; i <= n; i++){
            //có bất kì đỉnh nào chưa đc thăm -> ko liên thông -> ko phải dồ thị
            if(!visited[i]){
                cout << "NO\n";
                return;
            }
        }
        cout << "YES\n";
    }
}
//Tính độ cao của cây
//mảng chiều cao của các node trên cây
int he[k + 1];
void input(){
    cin >> n;
    for(int i = 0; i < n; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
//dùng mảng parent -> khi cout thì chiều cao phải trừ đi 1 để thỏa yêu cầu chiều cao bắt đầu của bài toán
void DFSheight(int u, int parent){
    visited[u] = true;
    he[u] = he[parent] + 1;
    for(int v : adj[u]){
        if(!visited[v]){
            DFSheight(v, u);
        }
    }
}
//ko dùng mảng parent
void DFSheight(int u, int parent){
    visited[u] = true;
    he[u] = parent;
    for(int v : adj[u]){
        if(!visited[v]) DFSheight(v, parent + 1);
    }
}
void height(){
    input();
    DFSheight(1, 0);
    for(int i = 1; i <= n; i++){
        cout << i << ' ' << he[i] - 1 << endl;
    }
    DFSheight(1, 0);
    for(int i = 1; i <= n; i++){
        cout << i << ' ' << he[i] << endl;
    }
} 
```
## Cây nhị phân - BT
### Lưu trữ và khởi tạo
```cpp
struct node{
    //data -> có thể là bất cứ kiểu dữ liệu gì
    int data;
    //lưu địa chỉ 2 node con bên trái bên phải
    node *left;
    node *right;
    //Constructor mặc định
    node(){}
    //Constructor => tạo cấu trúc node
    node(int x){
        data = x;
        left = right = NULL;
    }
};

//Có thể tạo hàm makeNode trả về kiểu con trỏ -> node cấp phát động thay constructor => chậm và phiền phức hơn
node* makeNode(int x){
    node* newNode = new node;
    newNode->data = x;
    newNode->left = newNode->right = NULL;
    return newNode;
}

//Tìm ra node cha để có thể tạo thêm node mới cho cây
//u : node đang xét, v : node con trái hoặc phải
void makeRoot(node *root, int u, int v, char c){
    if(c == 'L') root->left = new node(v);
    else root->right = new node(v);
}

//tìm ra node cần tìm để thêm
void insertNode(node *root, int u, int v, char c){
    if(root == NULL) return; //kết thúc lời gọi đệ quy
    if(root->data = u) makeRoot(root, u, v, c); //tìm được node để thêm bên trái hoặc phải
    else{
        insertNode(root->left, u, v, c); //tìm bên cây con bên trái
        insertNode(root->right, u, v, c); //tìm bên cây con bên phải
    }
}

//Nhập node từ input
void input(){
    node *root = NULL;
    int n; cin >> n;
    for(int i = 0; i < n; i++){
        int u, v; char c;
        cin >> u >> v >> c;
        if(root == NULL){
            root = new node(u);
            makeRoot(root, u, v, c);
        }
        else insertNode(root, u, v, c);
    }
}
```
### Duyệt cây
#### Đệ quy
```cpp
//Duyệt giữa -> left - root - right
void inOrder(node *root){
    if(root == NULL) return;
    inOrder(root->left);
    cout << root->data << ' ';
    inOrder(root->right);
}

//Duyệt trước -> root - left - right
void preOrder(node *root){
    if(root == NULL) return;
    cout << root->data << ' ';
    preOrder(root->left);
    preOrder(root->right);
}

//Duyệt sau -> left - right - root
void postOrder(node *root){
    if(root == NULL) return;
    postOrder(root->left);
    postOrder(root->right);
    cout << root->data << ' ';
}

//Duyệt theo mức
void levelOrder(node *root){
    queue<node*> q;
    q.push(root);
    while(!q.empty()){
        node* tmp = q.front();
        q.pop();
        cout << tmp->data << ' ';
        if(tmp->left != NULL) q.push(tmp->left); //thăm cây con left
        if(tmp->right != NULL) q.push(tmp->right); //thăm cây con right 
    }
}

//Duyệt xoắn ốc
void spiralOrder(node *root){
    stack<node*> st1;
    stack<node*> st2;
    //stack 1 push root
    st1.push(root);
    while(!st1.empty() || !st2.empty()){
        while(!st1.empty()){
            node* tmp = st1.top(); 
            st1.pop();
            cout << tmp->data << ' ';
            //duyệt từ trái sang phải -> stack 2 push r -> l
            if(tmp->right != NULL) st2.push(tmp->right);
            if(tmp->left != NULL) st2.push(tmp->left);
        }
        while(!st2.empty()){
            node* tmp = st2.top(); 
            st2.pop();
            cout << tmp->data << ' ';
            //duyệt từ phải sang trái -> stack 1 push l -> r
            if(tmp->left != NULL) st1.push(tmp->left);
            if(tmp->right != NULL) st1.push(tmp->right);
        }
    }
}
```
#### Không đệ quy
```cpp
// NLR
void preorder(Node* root){
    if(root == NULL) return;
    stack<Node*> nodeList;
    nodeList.push(root);
    Node* curr;
    while(!nodeList.empty()){
        curr = nodeList.top();
        cout << curr->data << " ";
        nodeList.pop();
        if(curr->right != NULL) nodeList.push(curr->right);
        if(curr->left != NULL) nodeList.push(curr->left);
    }
}

// LRN
void postorder(Node* root){
    if(root == NULL) return;
    stack<Node*> s1, s2;
    s1.push(root);
    Node* node;

    while(!s1.empty()){
        node = s1.top();
        s1.pop();
        s2.push(node);

        if(node->left) s1.push(node->left);
        if(node->right) s1.push(node->right);
    }
    while(!s2.empty()){
        node = s2.top();
        s2.pop();
        cout << node->data << " ";
    }
}

// LNR
void inorder(Node* root){
    stack<Node*> s;
    Node* curr = root;
    while(curr != NULL || s.empty() == false){
        while(curr !=  NULL){
            s.push(curr);
            curr = curr->left;
        }
        curr = s.top();
        s.pop();
        cout << curr->data << " ";
        curr = curr->right;
    }
}
```
### Kiểm tra cây 
```cpp
//Cây nhị phân đầy đủ => 0 con hoặc 2 con
bool checkFullBT(node* root){
    //0 con
    if(root == NULL) return true;
    //2 con
    if(root->left == NULL && root->right == NULL) return true;
    if(root->left != NULL && root->right != NULL){
        return checkFullBT(root->left) && checkFullBT(root->right);
    }
    //1 trong 2 con left or right có xuất hiện -> sai 
    else return false;
}

//Cây nhị phân hoàn hảo
set<int> se;
bool check = true;
void track(node* root, int cnt){
    if(root == NULL) return;
    if(root->left == NULL && root->right == NULL) se.insert(cnt);
    else check = false;
    track(root->left, cnt + 1);
    track(root->left, cnt + 1);
}
void checkPerfectBT(){
    input();
    track(root, 0);
    if(check){
        if(se.size() == 1) cout << "YES";
        else cout << "NO";
    }
    else cout << "NO";
}

//Cây nhị phân tìm kiếm
bool isValid(node* root, int l, int r){
    if(root == NULL) return true;
    if(root->data > l && root->data < r){
        return isValid(root->left, l, root->data) && isValid(root->right, root->data, r);
    }
    else return false;
}
bool checkBST(node *root){
    return isValid(root, -INT_MAX, INT_MAX);
}
//code khác
bool checkBST(node* root){
    auto isBST = [](node* root, int left, int right, auto& isValid) -> bool{
        if(root == NULL) return true;
        if(root->data > left && root->data < right){
            return isValid(root->left, l, root->data, isValid) && isValid(root->right, root->data, r, isValid);
        }
        else return false;
    };
    return isBST(root, -INT_MAX, INT_MAX, isBST);
}
```
### Thao tác khác
```cpp
//Đếm node lá
int cntLeaf(node* root){
    if(root == NULL) return 0;
    if(root->left == NULL && root->right == NULL) return 1; //node lá
    int cnt = 0;
    //đếm lần lượt các cây con bên trái và bên phải của cây
    cnt += cntLeaf(root->left);
    cnt += cntLeaf(root->right);
    return cnt;
}

//Độ cao của cây
int height(node* root){
    if(root == NULL) return -1;
    return max(height(root->left) + 1, height(root->right) + 1);
    //if(root->left == NULL && root->right == NULL) return 0;
    //return max(height(root->left), height(root->right));
}

//Check các node lá cùng mức
set<int> level;
void cntLevel(node* root, int cnt){
    if(root == NULL) return;
    //là node lá
    if(root->left == NULL && root->right == NULL) level.insert(cnt);
    //duyệt các cây con bên trái và bên phải
    cntLevel(root->left, cnt + 1);
    cntLevel(root->right, cnt + 1);
}
void checkLevel(){
    input();
    cntLevel(root, 0);
    if(level.size() == 1) cout << "YES";
    else cout << "NO";
}
//check level lá cùng mức code khác => truyền tham chiếu để maxh thay đổi và tham gia vào các lần gọi đệ quy sau
bool checkSameLevel(node* root, int h, int &maxh){
    if(root == NULL) return true;
    //node lá
    if(root->left == NULL && root->right == NULL){
        if(maxh == x){ //x là giá trị muốn truyền vào ở hàm main mà thường là 0
            maxh = h;
            return true;
        }
        else return h == maxh;
    }
    else{
        return checkSameLevel(root->left, h + 1, maxh) && checkSameLevel(root->right, h + 1, maxh);
    }
}
```
## Cây nhị phân tìm kiếm - BST
```cpp
//Tìm kiếm trên BST
bool search(node *root, int key){
    if(root == NULL) return false;
    if(root->data == key) return true;
    else if(root->data < key) return search(root->right, key);
    else return search(root->left, key);
}

//Chèn trên BST -> giữ đc cây nhị phân tìm kiếm
node* insert(node* root, int key){
    if(root == NULL) return makeNode(key);
    else if(root->data >= key){
        root->left = insert(root->left, key);
    } 
    else root->right = insert(root->right, key);
    return root;
}

//Xóa trên BST
//tìm node nhỏ nhất lớn hơn node cần xóa => cây con bên trái
node* minNode(node* root){
    node* tmp = root;
    while(tmp != NULL && tmp->next != NULL) tmp = tmp->next; //kt trước khi thực hiện để ko bị lỗi
    return tmp;
}
node* erase(node* root, int key){
    if(root == NULL) return root; //chưa có node xóa
    //tìm node cần xóa
    if(key < root->data) root->left = erase(root, key);
    else if(key > root->data) root->data = erase(root, key);
    //Tìm đúng node cần xóa
    else{
        //TH1 : node có cây con bên phải
        if(root->left == NULL){
            node* tmp = root->right;
            delete root;
            return tmp;
        }
        //TH2 : node có cây con bên trái
        if(root->right == NULL){
            node*tmp = root->left;
            delete root;
            return tmp;
        }
        //TH3 : node trung gian
        else{
            //tìm node nhỏ nhất lớn hơn node cần xóa
            node* tmp = minNode(root->right);
            root->data = tmp->data;
            root->right = erase(root->right, tmp->data);
        }
    }
    return root;
}

// LCA : Tổ tiên chung gần nhất
node *LCA(node *root, int v1, int v2){
    int small = min(v1, v2);
    int large = max(v1, v2);
    while (root != NULL) {
        if (root->data > large) // p, q belong to the left subtree
            root = root->left;
        else if (root->data < small) // p, q belong to the right subtree
            root = root->right;
        else // Now, small <= root.val <= large -> This root is the LCA between p and q
            return root;
    }
    return NULL;
}
```
## Cây Fenwick - Segment
Thay thế bài toán truy vấn và tính tổng dùng mảng cộng dồn => nhiều truy vấn
### Segment
```cpp
//Cây Segment -> cập nhật giá tr và truy vấn tổng của i phần tử trong mảngị 
int const maxN = 1e6 + 5;
int a[maxN], SEG[4 * maxN], n;
//xây cây segment
void build(int v, int l, int r){
    //SEG[v] = sum của các pt từ l -> r
    if(l == r){ //điều kiện dừng
        SEG[v] = a[l]; //node lá 
    }
    else{
        //chưa phải node lá
        int m = (l + r) / 2;
        build(2 * v, l, m); //gọi cây con bên trái
        build(2 * v + 1, m + 1, r); //cây con bên phải
        //Bài toán khác vd như tìm giá trị nhỏ nhất trong đoạn l->r thì cài đặt từ chỗ này khác đi
        SEG[v] = SEG[2 * v] + SEG[2 * v + 1]; // node cha = tổng 2 node con
    }
}

//Truy vấn -> query : O(logN)
int getSum(int v, int tLeft, int tRight, int l, int r){
    if(l > r) return 0; //truy vấn trên 1 đoạn có thể ko có giá trị
    if(l == tLeft && r == tRight) return SEG[v]; //kết thúc đệ quy và trả kq
    else{
        int tMid = (tLeft + tRight) / 2;
        //l r có thể nằm lệch so vs tMid
        return getSum(2 * v, tLeft, tMid, l, min(tMid, r)) + getSum(2 * v + 1, tMid + 1, tRight, max(tMid + 1, l), r);
    }
}

//Cập nhật giá trị update : O(logN) a[pos] = val
void update(int v, int l, int r, int pos, int val){
    if(l == r) SEG[v] = val;
    else{
        int m = (l + r) / 2;
        //lệch bên trái
        if(pos <= m) update(2 * v, l, m, pos, val);
        //lệch bên phải
        else update(2 * v + 1, m + 1, r, pos, val);
        //giá trị của cây con bên trái hoặc bên phải có thể thay đổi -> cập nhật giá trị cho cha
        SEG[v] = SEG[2 * v] + SEG[2 * v + 1];
    }
}
```
### Fenwick
```cpp
//Cây Fenwick (Binary Indexed Tree) -> cập nhật giá tr và truy vấn tổng của i phần tử trong mảngị 
int const maxN = 1e6 + 5;
int a[maxN], BIT[maxN], n;

//2 thao tác -> tạo mảng bit -> update và query
//update
void update(int pos, int val){
    //từ chỉ số pos -> chỉ số n
    for(; pos <= n; pos += pos & (-pos)) BIT[pos] += val;
    //pos & (-pos) -> lấy ra bit 1 cuối cùng của pos hiện tại và cộng số đó vào pos theo pp bù 2
}

//query -> sum các phần tử từ chỉ số 1 -> pos
int query(int pos){
    int sum = 0;
    for(; pos > 0; pos -= pos & (-pos)) sum += BIT[pos];
    return sum;
}
```
### Test
```cpp
int main(){
    cin >> n;
    //tạo mảng BIT
    for(int i = 1; i <= n; i++){
        cin >> a[i];
        update(i, a[i]);
    }
    //sum từ chỉ số 1 đến số 2
    cout << query(5) - query(1) << endl;
   
   //tạo cây Segment
    build(1, 0, n - 1);
    for(int i = 1; i <= 20; i++){
        if(SEG[i] != 0)
            cout << SEG[i] << ' ';
    }
    cout << endl;
    int l, r;
    cin >> l >> r;
    cout << getSum(1, 0, n - 1, l, r) << endl;
    update(1, 0, 3, 2, 10);
    for(int i = 1; i <= 20; i++){
        if(SEG[i] != 0)
            cout << SEG[i] << ' ';
    }
}
```
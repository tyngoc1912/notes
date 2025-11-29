# Graph
## Lưu trữ
3 loại : ma trận kề, danh sách cạnh, danh sách kề
```cpp
const int maxN = 10000;
//Số đỉnh và cạnh => duyệt số cạnh
int n, m;

//Cấu trúc dữ liệu lưu danh sách kề (Adjacency list) => mảng vector
vector<int> adj[maxN];
//Nhập danh sách cạnh sang danh sách kề
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        //Vô hướng
        adj[x].push_back(y);
        adj[y].push_back(x);
        //Có hướng
        adj[x].push_back(y);
        //Nhập vào ds cạnh
        edge.push_back({x, y});
    }
    //có thể sắp xếp sẵn sau khi nhập xong => để duyệt ds kề theo từ bé đến lớn
    for(int i = 0; i < m; i++){
        sort(adj[i].begin(), adj[i].end());
    }
    //reset mảng visited
    memset(visited, false, sizeof(visited));
}
//Xuất danh sách kề
void output(){
    //In theo thứ tự tăng dần của mỗi đỉnh
    for(int i = 1; i <= n; i++){
        //sắp xếp mỗi phần tử của mảng vector tăng dần
        sort(adj[i].begin(), adj[i].end());
        cout << i << ' ' <<  ':' << ' ';
        for(int j = 0; j < adj[i].size(); j++){
            cout << adj[i][j] << ' ';
        }
        cout << endl;
    }
    cout << endl;
}

//cấu trúc dữ liệu lưu ma trận kề 
int a[maxN + 1][maxN + 1];
//Nhập danh sách cạnh đến ma trận kề
void input(){
	cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        //vô hướng
        a[x][y] = 1;
        a[y][x] = 1;
        //có hướng 
        a[x][y] = 1;
    }
}
//Xuất ma trận kề
void output(){
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= n; j++){
            cout << a[i][j] << ' ';
        }
        cout << endl;
    }
    cout << endl;
}
```
## Disjoint set union - DSU
### Khởi tạo
```cpp
int n; //số đỉnh đồ thị
const int maxN = 1e4;
int parent[maxN + 1]; //truy vết thằng đại diện của mỗi đỉnh
int sz[maxN + 1];

//Khởi tạo DSU
void init(){
    cin >> n;
    //đánh dấu đại diện của mỗi đỉnh riêng biệt là chính nó
    for(int i = 1; i <= n; i++){
        parent[i] = i;
        sz[i] = 1;
    }

}
```
### Find
```cpp
//Chưa tối ưu
int Find(int u){
    while(u != parent[u]) u = parent[u]; //lấy cha của u gán cho u để truy vết ngược lại
    return u;
}

//Tối ưu
//Tìm đại diện của đỉnh u => i = parent[i]
int Find(int u){
    if(u == parent[u]) return u;
    else{
        int tmp = Find(parent[u]);
        parent[u] = tmp;
        return tmp;
    }
    //code gọn hơn
    else parent[u] = Find(parent[u]);
}
```
### Union
```cpp
//Chưa tối ưu
bool Union(int u, int v){
    u = Find(u); // tìm đại diện của u
    v = Find(v); // tìm đại diện của v
    if(u == v) return false; // 2 đỉnh này cùng tập hợp
    else{
        //gán đại diện cho tập hợp là phần tử nhỏ hơn
        if(u < v) parent[v] = u;
        else parent[u] = v;
        return true; // gộp đc
    }
}

// Tối ưu
//Gộp 2 đỉnh -> tìm đại diện cho 2 thằng và thay đổi đại diện
bool Union(int u, int v){
    u = Find(u); // tìm đại diện của u
    v = Find(v); // tìm đại diện của v
    if(u == v) return false; // 2 đỉnh này cùng tập hợp
    if(sz[u] < sz[v]) swap(u, v); //ưu tiên thằng lớn hơn là thằng u
    sz[u] += sz[v]; // size của u là lớn hơn v nên sẽ được đi nhập vào
    parent[v] = u; // v gọi u là cha
    return true;
}
```
### Struct DSU
```cpp
const int k = 1e4;
int n, m; 
int parent[k + 1];
int sz[k];

struct DSU{
    void init(){
        cin >> n >> m;
        for(int i = 1; i <= n; i++){
            parent[i] = i;
            sz[i] = 1;
        }
    }
    int Find(int u){
        if(u == parent[u]) return u;
        else return parent[u] = Find(parent[u]);
    }
    bool Union(int u, int v){
        u = Find(u);
        v = Find(v);
        if(u == v) return false;
        if(sz[u] < sz[v]) swap(u, v);
        sz[u] += sz[v];
        parent[v] = u;
        return true;
    }
};
```
### Ứng dụng
```cpp
//đếm số lượng tlpt -> disjoint set
int m; // số cạnh của đồ thị
int CC(){
    init();
    cin >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        Union(x, y);
    }
    int cnt = 0;
    for(int i = 1; i <= n; i++){
        //số tplt
        if(i == parent[i]) cnt++;
    }
    return cnt;
}
```

## Thuật toán đồ thị
### DFS
- DFS -> duyệt theo từng nhánh ⇒ đến đáy rồi quay lại
- Mã giả 
    ```cpp
    //Dùng danh sách kề => O(V + E) tối ưu và dễ code
    //Tìm kiếm theo chiều sâu DFS => đệ quy
    DFS(u){
        <Tham dinh u> //code
        visited[u] = true;//Đánh dấu đã thăm u
        //Duyệt danh sách kề của u
        for(int v : adj[u]){
            if(!visited[v]){
                DFS(v);
            }
        }
    }
    ```
```cpp
bool visited[maxN];
//Ds kề
void DFS(int u){
    cout << u << ' '; //in ra
    visited[u] = true; // đánh dấu đã thăm
    for(int v : adj[u]){ //duyệt danh sách kề của đỉnh u
        if(!visited[v]){
            DFS(v);
        }
    }
}

//Ma trận kề
void DFS1(int u){
    cout << u << ' ';
    visited[u] = true;
    //duyệt ds kề của đỉnh u = duyệt dòng thứ u trong ma trận kề
    for(int i = 1; i <= n; i++){ //cột 1 -> n
        if(mtx[u][i] == 1){ //xét từng cột của dòng u
            if(!visited[i]){
                DFS1(i);
            }
        }
    }
}

//Ds cạnh
void DFS2(int u){
    cout << u << ' ';
    visited[u] = true;
    //duyệt ds của kề đỉnh u = duyệt tất cả các cạnh trong ds cạnh => xét xem đỉnh đầu vs đỉnh cuối của cạnh đó nếu là u => có 1 cạnh kè với nó => gọi dfs 
    for(auto it : edge){ 
        if(it.first == u){ 
            if(!visited[it.second]){
                DFS1(it.second);
            }
        }
        if(it.second == u){
            if(!visited[it.first]){
                DFS2(it.first);
            }
        }
    }
}
```
### BFS
- BFS → duyệt theo từng hàng cùng bậc
- Mã giả
    ```cpp
    //Tìm kiếm theo chiều rộng BFS => hàng đợi
    BFS(u){
        //STEP 1: Khởi tạo
        queue = ∅; //Tạo một hàng đợi rỗng
        push(queue, u); // Đẩy đỉnh u vào hàng đợi
        visited[u] = true; // Đánh dấu đỉnh u đã được thăm
        //STEP 2: Lặp khi mà hàng đợi vẫn còn phần tử
        while (queue != ∅){
            v = queue.front(); // Lấy ra đỉnh ở đầu hàng đợi
            queue.pop(); //Xóa đỉnh khỏi đầu hàng đợi
            <Thăm đỉnh v>
        // Duyệt các đỉnh kề với v mà chưa được thăm và đẩy vào hàng đợi
            for(int x: ke[v]){
                if(!visited[x]){ // Nếu x chưa được thăm
                    push(queue, x);
                    visited[x] = true;
                }
            }
        }
    }
    ```
```cpp
bool visited[maxN];
//Ds kề
void BFS(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        //Thăm đỉnh u
        int x = q.front();
        q.pop();
        cout << x << ' ';
        //Duyệt danh sách kề của đỉnh u
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
            }
        }
    }
}
```
### Kĩ thuật di chuyển trên mảng 2 chiều
```cpp
int dx[8] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy[8] = {-1, 0, 1, -1, 1, -1, 0, 1};

void DFS(int i, int j){
    visited[i][j] = true;
    for(int k = 0; k < 8; k++){
        int i1 = i + dx[k], j1 = j + dx[k];
        if(i1 >= 1 && i1 <= n && j1 >= 1 && j1 <= m && !visited[i1][j1]){
            DFS(i1, j1);
        }
    }
}

void BFS(int i, int j){
    queue<pair<int, int>> q;
    q.push({i, j});
    visited[i][j] = true;
    while(!q.empty()){
        pair<int, int> x = q.front(); 
        q.pop();
        for(int k = 0; k < 8; k++){
            int i1 = x.first + dx[k], j1 = x.second + dx[k];
            if(i1 >= 1 && i1 <= n && j1 >= 1 && j1 <= m && !visited[i1][j1]){
                q.push({i1, j1});
                visited[i1][j1] = true;
            }
        }
    }   
}
```
### Sắp xếp Topo
```cpp
//Topological sort -> đồ thị có hướng
//sắp xếp topo => những đỉnh liên quan vs nhau thì đỉnh nào bắt buộc phải đứng trước đỉnh kia sẽ đc đứng trc(giống như môn tiên quyết ở trường ĐH) -> những thằng ko liên quan ko xét đứng đâu cũng đc
//miễn xuất hiện đường nối có hướng giữa u và v -> sắp xếp topo => u phải xếp đứng trước v -> ứng dụng stack
//ứng dụng DFS
vector<int> topo;
stack<int> st;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
    }
    memset(visited, false, sizeof(visited));
}
void DFStopo(int u){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]) DFStopo(v);
    }
    //đã duyệt xong đỉnh u -> push vào vector hoặc stack
    //topo.push_back(u);
    st.push(u);
}
void topoSort(){
    //input();
    //duyết hết tất cả các đỉnh của đồ thị
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFStopo(i);
    }
    // reverse(topo.begin(), topo.end());
    // for(int x : topo) cout << x << ' ';
    while(!st.empty()){
        int x = st.top();
        st.pop();
        cout << x << ' ';
    }

}
```
### Đồ thị liên thông
#### Kiểm tra đồ thị lt + Đếm số lượng tplt đồ thị vô hướng  
```cpp
//Mỗi lần lời gọi là 1 lần xác định tp liên thông
void DFSconnect(int u){
    visited[u] = true; 
    for(int v : adj[u]){ 
        if(!visited[v]){
            DFSconnect(v);
        }
    }
}
void BFSconnect(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        int x = q.front();
        q.pop();
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
            }
        }
    }
}
int connectivity(){
    //Tránh mỗi lần q truy vấn thì mảng chưa được reset => tùy lúc hẵn xài
    //memset(visited, false, sizeof(visited));
    int cnt = 0;
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            //chọn 1 trong 2
            DFSconnect(i);
            BFSconnect(i);
            cnt++;
        }
    }
    return cnt;
}
void connect(){
    if(connectivity() == 1) cout << "TRUE" << endl;
    else cout << "FALSE" << endl;
}
```
#### Kiểm tra tplt mạnh của đồ thị có hướng
```cpp
//Brute force -> kt tất cả các đỉnh có được thăm khi gọi dfs hoặc bfs ko
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
    }
    memset(visited, false, sizeof(visited));
}
void strongConnect(){
    input();
    for(int i = 1; i <= n; i++){
        memset(visited, false, sizeof(visited));
        DFSconnect(i); //BFSconnect(i);
        //duyệt hết tất cả các đỉnh của đồ thị để xem có được thăm hết ko
        for(int j = 1; j <= n; j++){
            if(visited[j] == false){
                cout << "NO\n";
                return;
            }
        }
    }
    cout << "YES\n";
}

//Kosaraju -> liệt kê các tplt mạnh
vector<int> tAdj[k + 1]; // dùng 1 mảng để lưu đồ thị chuyển vị -> quay ngược chiều toàn bộ các phần tử ban đầu
stack<int> st; //đẩy các phần tử khi gọi DFS vào stack như sắp xếp topo 
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        //lật ngược ds kề và lưu vào tAdjs
        tAdj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFS1(int u){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]) DFS1(v);
    }
    //sau khi gọi xong DFS1 của ds kề của u thì push các đỉnh lần lượt vào stack
    st.push(u);
}
void DFS2(int u){
    visited[u] = true;
    cout << u << ' '; //xuất giá trị của u
    //duyệt đồ thị chuyển vị 
    for(int v : tAdj[u]){
        if(!visited[v]) DFS2(v);
    }
}
void kosaraju(){
    input();
    //duyệt hết tất cả các tplt có thể có của đồ thị
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFS1(i);
    }
    //duyệt xong đã có các pt trong stack
    memset(visited, false, sizeof(visited));
    int scc = 0; //đếm số tplt mạnh
    while(!st.empty()){
        //lấy đỉnh đầu stack ra và duyệt DFS của đỉnh đó nhưng trong đồ thị transpose
        int x = st.top();
        st.pop();
        for(int v : tAdj[x]){
            if(!visited[v]){
                scc++; // tăng số tplt mạnh
                cout << scc << " : "; //liệt kê các tplt mạnh
                DFS2(v);
                cout << endl; // sau khi in xong mỗi tplt thì xuống dòng
            }
        }
    }
    cout << scc << endl;
}
//Kiểm tra đồ thị có phải liên thông mạnh => số tplt = 1 là đồ thi liên thông mạnh còn nhiều hơn thì ko phải
bool strongConntect(){
    //viết lại hàm kosaraju trả về kiểu int
    if(kosaraju() == 1) return true;
    else return false;
}

//Thuật toán Tarjan : dùng 2 mảng disc[] và low[], timer -> thể hiện thời gian thăm => tìm tplt mạnh
int low[k + 1], disc[k + 1];
int timer = 0, scc = 0;
stack<int> st;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        //adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFStarjan(int u){
    visited[u] = true;
    //đánh dấu mảng disc vs low khi thăm lần đầu
    disc[u] = low[u] = ++timer; //ko được timer++ => nó ko cập nhật timer giá trị đúng
    //đẩy u vào trong stack
    st.push(u);
    for(int v : adj[u]){
        if(!visited[v]){
            DFStarjan(v);
            //đỉnh chưa được thăm thì so sánh và cập low của u hoặc v
            low[u] = min(low[u], low[v]);
        }
        //đỉnh được thăm thì phải so sánh và cập nhật low u vs disc v
        else low[u] = min(low[u], disc[v]);
    }
    //sau khi gọi DFS và cập nhật mảng low vs disc ở mỗi đỉnh xong
    if(low[u] == disc[u]){ // đỉnh thăm sớm nhất của tp liên thông mạnh
        scc++;
        while(st.top() != u){
            cout << st.top() << ' ';
            st.pop();
        }
        cout << st.top() << ' ';
        st.pop();
        cout << endl;
    }
}
void SCC(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFStarjan(i);
    }
}
```
### Đỉnh trụ - cạnh cầu
#### Cơ bản
```cpp
//Tìm đỉnh trụ -> Articulation point(cut vertice)
//Xóa 1 đỉnh khỏi đồ thị => visited[i] = true trước khi gọi DFS hoặc BFS
int connectivity(){
    int cnt = 0;
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            DFSconnect(i);
            //BFSconnect(i);
            cnt++;
        }
    }
    return cnt;
}
void AP(){
    //đếm số tp liên thông ban đầu
    int prevCnt = connectivity();
    //Duyệt từng đỉnh để xem nếu bỏ từng đỉnh đó ra thì nó có tạo thêm tplt ko
    for(int i = 1; i <= n; i++){
            //reset lại mảng visited sau khi đếm số tplt ban đầu
        memset(visited, false, sizeof(visited));
        //loại đỉnh đó khỏi đồ thị
        visited[i] = true;
        if(prevCnt < connectivity()){ // số tplt sau khi loại đỉnh i
            //i là đỉnh trụ
            cout << i << ' ';
        }
    }
    cout << endl;
}
//Đếm số lượng cạnh cầu => mảng set và vector<pair> và thực hiện 2 thao tác
//xóa x ở danh sách kề của y và xóa y ở ds kề của x sau đó insert lại
//Viết riêng ra đừng để chồng chất nhiều hàm thì thuật toán này sai
set<int> adjList[maxN];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        edge.push_back({x, y});
        adjList[x].insert(y);
        adjList[y].insert(x);
    }
}
void bridge(){
    input();
    //đếm số lượng tplt ban đầu
    int prevCnt = connectivity();
    int cnt = 0;
    for(auto it : edge){
        int x = it.first, y = it.second;
        //loại bỏ cạnh của 2 đỉnh ra khỏi đồ thị mà ko xóa 2 đỉnh đó
        adjList[x].erase(y);
        adjList[y].erase(x);
        //làm mới lại mảng visited
        memset(visited, false, sizeof(visited));
        if(prevCnt < connectivity()){
            cnt++;
        }
        //Thêm cạnh lại vào ds để đảm bảo ds còn nguyên
        adjList[x].insert(y);
        adjList[y].insert(x);
    }
    cout << cnt << endl;
}

//Cạnh cầu ko cần dùng mảng set => đánh dấu rằng 2 đỉnh mình chuẩn bị xét là 2 đỉnh của cạnh liên thuộc cần xóa
//ko cần xóa hoặc thêm + code ngắn hơn
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        edge.push_back({x, y});
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
}
void DFSbridge(int u, int s, int t){
    visited[u] = true;
    for(int v : adj[u]){
        //kiểm tra xem 2 đỉnh đó có phải là 2 đỉnh thuộc cạnh cầu => continue
        if(u == s && v == t || u == t && v == s) continue;
        if(!visited[v]){
            DFSbridge(v, s, t);
        }
    }
}
int connect(int s, int t){
    int ans = 0;
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            ans++;
            DFS(i, s, t);
        }
    }
    return ans;
}
void bridge(){
    input();
    int cc = connect(0, 0); //truyền 0 0 để ko ảnh hưởng đến lần xét tplt ban đầu
    int cnt = 0;
    for(auto it : edge){
        int x = it.first, y = it.second;
        memset(visited, false, sizeof(visited));
        //cạnh cầu bị loại bỏ -> truyền 2 đỉnh vào
        if(cc < connect(x, y)){
            cnt++;
        }
    }
    cout << cnt << endl;
}
```
#### Nâng cao
```cpp
//Tarjan dùng để tìm đỉnh trụ : đỉnh trụ => low[v] >= disc[u] (khác đỉnh nguồn) -> u là đỉnh trụ
int low[k + 1], disc[k + 1];
int timer = 0, scc = 0;
int AP[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
    memset(AP, false, sizeof(AP));
}
void DFSap(int u, int parent){
    visited[u] = true;
    //đánh dấu mảng disc vs low khi thăm lần đầu
    disc[u] = low[u] = ++timer; //ko được timer++ => nó ko cập nhật timer giá trị đúng
    //đếm số lượng con của cây DFS nếu lớn hơn hoặc bằng 2 con từ đỉnh u thì đó là đỉnh trụ 
    int child = 0;
    for(int v : adj[u]){
        if(v == parent) continue; //nếu nó là chu trình thì bỏ qua => ko xét đến
        if(!visited[v]){
            DFSap(v, u);
            //mỗi lần gọi dfs là tăng số cây con của đỉnh u
            child++;
            //đỉnh chưa được thăm thì so sánh và cập low của u hoặc v
            low[u] = min(low[u], low[v]);
            //kiểm tra đỉnh u ko phải là đỉnh gốc và có đặc điểm là disc[u] <= low[v] => là đỉnh trụ
            if(parent != -1 && disc[u] <= low[v]) AP[u] = true;
        }
        //đỉnh được thăm thì phải so sánh và cập nhật low u vs disc v
        else low[u] = min(low[u], disc[v]);
    }
    //sau khi gọi DFS và cập nhật mảng low vs disc ở mỗi đỉnh xong
    //đỉnh u là đỉnh gốc và có số con lớn hơn 2 -> đỉnh trụ
    if(parent == -1 && child > 1) AP[u] = true; 
}
void findAP(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFSap(i, -1);
    }
    for(int i = 1; i <= n; i++){
        if(AP[i]) cout << i << ' ';
    }
}

//Tarjan dùng để tìm cạnh cầu => low[v] > disc[u] -> tránh u là đỉnh nguồn của chu trình chứa v 
int low[k + 1], disc[k + 1];
int timer = 0;
vector<pair<int, int>> bridge;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFSbridge(int u, int parent){
    visited[u] = true;
    //đánh dấu mảng disc vs low khi thăm lần đầu
    disc[u] = low[u] = ++timer; //ko được timer++ => nó ko cập nhật timer giá trị đúng
    for(int v : adj[u]){
        if(v == parent) continue; //nếu nó là chu trình thì bỏ qua => ko xét nữa
        if(!visited[v]){
            DFSbridge(v, u);
            //đỉnh chưa được thăm thì so sánh và cập low của u hoặc v
            low[u] = min(low[u], low[v]);
            //kiểm tra disc[u] < low[v] => là cạnh cầu
            if(disc[u] < low[v])  bridge.push_back({u, v});
        }
        //đỉnh được thăm thì phải so sánh và cập nhật low u vs disc v
        else low[u] = min(low[u], disc[v]);
    }
}
void findBridge(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]) DFSbridge(i, -1);
    }
    for(auto x : bridge) cout << x.first << ' ' << x.second << endl;
}
```
### Đường đi
#### Cơ bản
```cpp
//Kiểm tra đường đi 2 đỉnh -> mỗi lần gọi DFS hoặc BFS thì đánh dấu những đỉnh thăm thuộc tp liên thông nào => 2 đỉnh cùng thuộc 1 tp liên thông thì có đường đi với nhau
int ID[k + 1], cnt = 0;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
void DFSid(int u){
    visited[u] = true;
    ID[u] = cnt; 
    for(int v : adj[u]){ 
        if(!visited[v]){
            DFSid(v);
        }
    }
}
void BFSid(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        int x = q.front();
        ID[x] = cnt;
        q.pop();
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
            }
        }
    }
}
void checkPath(){
    input();
    for(int i = 1; i <= n; i++){
        if(!visited[i]){
            //đánh dấu đỉnh thuộc tplt nào
            cnt++;
            //loang ra hết tất cả các đỉnh kề của u
            DFSid(i);
            //BFSid(i);
        }
    }
    int q; cin >> q;
    while(q--){
        int x, y; cin >> x >> y;
        if(ID[x] == ID[y]) cout << "1" << ' ';
        else cout << "0" << ' ';
    }
}

//Tìm đường đi giữa 2 đỉnh trên đồ thị => dùng thêm 1 mảng cha -> parent[u] : đỉnh cha của đỉnh u
int parent[k + 1];
int s, t;

void DFSpath(int u){
    visited[u] = true;
    for(int v : adj[u]){ 
        if(!visited[v]){
            DFSpath(v);
            //v là ds kề của u -> mở rộng v => đánh dấu u là cha của v
            parent[v] = u;
        }
    }
}
void BFSpath(int u){
    queue<int> q;
    q.push(u);
    visited[u] = true;
    while(!q.empty()){
        int x = q.front();
        ID[x] = cnt;
        q.pop();
        for(int v : adj[x]){
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
                //đánh dấu x là cha của v
                parent[v] = x;
            }
        }
    }
}
//Tìm đường đi bằng DFS là đường đi bình thường
void findPath(){
        input();
    //nhập 2 đỉnh (đầu, cuối để xác định đường đi)
    cin >> s >> t;
    DFSpath(s); //BFSpath(s);
    if(!visited[t]) cout << "-1" << endl;
    else{
        //Truy vết đường đi
        vector<int> res;
        while(t != s){
            res.push_back(t);
            //đi dò ngược lại cho đến khi thấy đỉnh đầu tiên 
            t = parent[t];
        }
        res.push_back(s);
        //lật ngược vector lại vì nãy giờ lần đi thì pushback vào theo thứ tự ngược
        reverse(res.begin(), res.end());
        for(int x : res) cout << x << ' ';
        cout << endl;
    }    
}
//Trên đồ thị ko có trọng số => BFS tìm được đường đi ngắn nhất giữa 2 đỉnh
void minPath(){
    input();
    //nhập 2 đỉnh (đầu, cuối để xác định đường đi)
    cin >> s >> t;
    BFSpath(s);
    if(!visited[t]) cout << "-1" << endl;
    else{
        //Truy vết đường đi
        vector<int> res;
        while(t != s){
            res.push_back(t);
            //đi dò ngược lại cho đến khi thấy đỉnh đầu tiên 
            t = parent[t];
        }
        res.push_back(s);
        //lật ngược vector lại vì nãy giờ lần đi thì pushback vào theo thứ tự ngược
        reverse(res.begin(), res.end());
        for(int x : res) cout << x << ' ';
        cout << endl;
    }    
}
```
#### Nâng cao
```cpp
//Tìm đường đi ngắn nhất
//Thuật toán Dijkstra => 1 -> mọi đỉnh : đồ thị có trọng số ko âm
typedef pair<int, int> ii;
bool taken[k + 1];
vector<ii> adj[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y, w; cin >> x >> y >> w;
        adj[x].push_back({y, w});
        adj[y].push_back({x, w});
    }
}
void dijkstra(int s){
    //tạo mảng lưu chi phí đường đi từ đỉnh nguồn đến đỉnh u
    vector<int> d(n + 1, 1e9);
    d[s] = 0; // chi phí ban đầu là 0
    priority_queue<ii, vector<ii>, greater<ii>> q;
    q.push({0, s}); //đẩy vào hàng đợi ưu tiên {giá trị, đỉnh}
    while(!q.empty()){
        //chọn ra đỉnh u có đường ngắn nhất => relaxation
        pair<int, int> t = q.top();
        q.pop();
        int u = t.second, dist = t.first;
        if(dist > d[u]) continue; // chi phí từ đỉnh nguồn đến đỉnh u ko phải là ngắn nhất => bỏ qua
        //relax
        for(auto it : adj[u]){
            int v = it. first, w = it.second;
            //kiểm tra chi phí đi từ đỉnh s -> v lớn hơn chi phí từ s -> u -> v
            if(d[v] > d[u] + w){
                //cập nhật chi phí
                d[v] = d[u] + w;
                q.push({d[v], v});
            }
        }
    }
    for(int i = 1; i <= n; i++) cout << d[i] << ' ';
}

//Thuật toán Bellman Ford => 1 -> mọi đỉnh : đồ thị ko có chu trình âm
struct edge{
    int x, y, w;
};
vector<edge> lst;
int d[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y, w; cin >> x >> y >> w;
        lst.push_back({x, y, w});
    }
}
void bellmanFord(int s){
    //Tất cả các đỉnh trừ đỉnh nguồn đều có giá trị max
    for(int i = 1; i <= n; i++) d[i] = INT_MAX;
    d[s] = 0; // đỉnh nguồn
    //lặp n - 1 lần
    for(int i = 0; i < n - 1; i++){
        //lặp m cạnh
        for(int j = 0; j < m; j++){
            edge tmp = lst[j];
            int x = tmp.x, y = tmp.y, w = tmp.w;
            //nếu đỉnh đã cập nhật giá trị rồi
            if(d[x] < INT_MAX){ // độ thị có cạnh là trọng số âm => rất quan trọng để khỏi bị cập nhật nhầm khi chưa tìm đc đường đi ngắn nhất
                d[y] = min(d[y], d[x] + w); //cập nhật chi phí đường đi thấp hơn giống dijkstra
            }
        }
    }
    //sau khi nó kết thúc nếu đồ thị có chu trình âm => làm thêm 1 vòng for như trên
    //nếu giá trị được cập nhật thì tức là đồ thị có chu trình âm
    for(int i = 1; i <= n; i++) cout << d[i] << ' ';
}

//Floyd : All pair -> 2 đỉnh bất kì trên đồ thị => số đỉnh thường bé hơn 400
int d[k + 5][k + 5]; //khoảng cách đường đi ngắn nhất từ i tới j
void floyd(){
    for(int k = 1; k <= n; k++){
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= n; j++){
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
    int q; cin >> q;
    while(q--){
        int x, y; cin >> x >> y;
        cout << d[x][y] << endl;
    }
}
```   
### Chu trình
```cpp
//Dồ thị có chu trình => ko có thứ tự topo
//Ứng dụng BFS (Kahn) : xóa dần đỉnh => đồ thị có chu trình -> ko đầy đủ các đỉnh khi thực hiện sx kahn => ứng dụng thực hiện kt chu trình trên đồ thị có hướng
//Quan tâm đến bán bậc vào
int inDeg[k + 1]; // lưu bán bậc vào
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        //tăng số bán bậc vào cho đỉnh đồ thị => đỉnh y
        inDeg[y]++;
    }
    memset(visited, false, sizeof(visited));
}
void kahn(){
    input();
    queue<int> q;
    //duyệt tất cả các đỉnh => tìm thằng nào có bán bậc vào bằng 0 sẽ là thằng bắt đầu thuật toán
    for(int i = 1; i <= n; i++){
        //nếu bán bậc vào của đỉnh đó bằng 0 thì push vào queue
        if(inDeg[i] == 0) q.push(i);
    }
    while(!q.empty()){
        //lấy thằng đầu hàng đợi ra
        int u = q.front();
        q.pop();
        cout << u << ' ';
        //duyệt danh sách kề của u
        for(int v : adj[u]){
            //xóa đỉnh u khỏi đồ thị -> xóa cạnh nối u vs v => bán bậc vào của v bị giảm
            inDeg[v]--;
            //nếu bán bậc vào của v là 0 => push nó vào hàng đợi
            if(inDeg[v] == 0) q.push(v); 
        }
    }
}

//Kiểm tra chu trình trên đồ thị
//Vô hướng : DFS => tìm cạnh ngược (Nếu đỉnh u mở rộng ra đỉnh v đã được thăm rồi mà v ko phải cha trực tiếp của u -> u cạnh ngược)
int parent[k + 1];
bool DFS_Undirected_cycle(int u){
    //đỉnh truyền vào đc thăm
    visited[u] = true;
    for(int v : adj[u]){
        //đỉnh trong danh sách kề vs u chưa đc thăm
        if(!visited[v]){
            //cha của v là u
            parent[u] = v;
            //phải kiểm tra nhánh v của u đang xét có chu trình hay ko => ko có thì xét nhánh khác => ko được return DFS(v)
            if(DFS_Undirected_cycle(v)) return true;
        }
        //đỉnh trong ds kề u đã đc thăm => check xem nó có cha trực tiếp => ko có thì kết luận là cạnh ngược
        else if(v != parent[u]){
            return true;
        }
    }
    //duyệt hết các nhánh mà ko thấy xuất hiện -> ko có chu trình
    return false;
}
//Vô hướng : BFS
bool BFS_Undirected_cycle(int u){
    queue<int> q;
    //đỉnh truyền vào hàng đợi đc thăm
    q.push(u);
    visited[u] = true;
    //lặp cho đến khi hàng đợi rỗng
    while(!q.empty()){
        int x = q.front();  //thường là in ra x nếu duyệt
        q.pop();
        //duyệt ds kề của đỉnh u
        for(int v : adj[x]){
            //chưa được thăm
            if(!visited[v]){
                q.push(v);
                visited[v] = true;
                parent[v] = x; //đánh dấu x là cha của v
            }
            else if(v != parent[x]) return true;
        }
    }
    return false;
}
//Có thể thay bằng viết hàm void có biến toàn cục ok và check dựa vào biến ok đó 
int ok = 0;
void DFScycle(int u){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]){
            parent[v] = u;
            DFScycle(v);
        }
        else if(v != parent[u]) ok = 1;
    }
}
void cycle(){
    input();
    //duyệt tổng quát-> một đồ thị có thể có 1 tplt hoặc nhiều hơn thì chỉ cần có ít nhất 1 tplt có chu trình thì đồ thị có chu trình
    for(int i = 1; i <= n; i++){
        //xét toàn bộ các đỉnh
        if(!visited[i]) checkCycle(i);
    }
    if(ok) cout << "YES\n";
    else cout << "NO\n";
}
//Code chu trình ko cần mảng parent
bool DFScycle(int u, int parent){
    visited[u] = true;
    for(int v : adj[u]){
        if(!visited[v]){
            //kiểm tra 1 nhánh đó có chu trình ko => ko có sang nhánh khác
            if(DFScycle(v, u)) return true;
        }
        //đỉnh v đã được thăm nhưng mà v ko là cha trực tiếp của u
        if(v != parent) return true;
    }
    //ko có chu trình
    return false;
}
void cycle(){
    input();
    bool check = false; // check tp liên thông nào có chu trình vì 1 đồ thị có thể có nhiều tplt
    for(int i = 1; i <= n; i++){
        if(!visited[i] && DFScycle(i, 0)){ //giá trị khởi đầu cho 0 hoặc -1 đều đc
            check = true;
            break;
        }
    }
    if(check == true) cout << "YES\n";
    else cout << "NO\n";
}

//Có hướng : DFS chuẩn -> xét 3 màu : trắng(0)->chưa thăm, xám(1)->đang thăm, đen(2)->đã thăm
int color[k + 1];
bool DFS_Directed_cycle(int u){
    //đỉnh truyền vào đc thăm -> xám
    color[u] = 1;
    for(int v : adj[u]){
        //đỉnh trong danh sách kề vs u chưa đc thăm
        if(color[v] == 0){
            //phải kiểm tra nhánh v của u đang xét có chu trình hay ko => ko có thì xét nhánh khác => ko được return DFS(v)
            if(DFS_Directed_cycle(v)) return true;
        }
        //đỉnh trong ds kề u đã đc thăm => check xem nó có màu xám ko => có kết luận là cạnh ngược
        else if(color[v] == 1){
            //ok = 1;
            return true;
        }
    }
    //Duyệt xong 1 nhánh DFS của 1 đỉnh thì cho đỉnh ban đầu truyền màu đen
    color[u] = 2;
    //duyệt hết các nhánh mà ko thấy xuất hiện -> ko có chu trình
    return false;
}
void cycle(){
    input();
    bool check = false; // check tp liên thông nào có chu trình vì 1 đồ thị có thể có nhiều tplt
    for(int i = 1; i <= n; i++){
        if(!color[i] && DFS_Directed_cycle(i, 0)){ //giá trị khởi đầu cho 0 hoặc -1 đều đc
            check = true;
            break;
        }
    }
    if(check == true) cout << "YES\n";
    else cout << "NO\n";
}

//Check chu trình trên đồ thị có hướng => thuật toán Kahn -> nếu Kahn ko duyệt được đầy đủ các đỉnh => có chu trình
int inDeg[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        inDeg[y]++;
        //adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
bool BFS_Directed_cycle(){
    input();
    queue<int> q;
    //duyệt tất cả các đỉnh => tìm thằng nào có bán bậc vào bằng 0 sẽ là thằng bắt đầu thuật toán
    for(int i = 1; i <= n; i++){
        //nếu bán bậc vào của đỉnh đó bằng 0 thì push vào queue
        if(inDeg[i] == 0) q.push(i);
    }
    int cnt = 0;
    while(!q.empty()){
        //lấy thằng đầu hàng đợi ra
        int u = q.front();
        q.pop();
        cnt++; //đếm số lần đẩy đỉnh
        //duyệt danh sách kề của u
        for(int v : adj[u]){
            //xóa đỉnh u khỏi đồ thị -> xóa cạnh nối u vs v => bán bậc vào của v bị giảm
            inDeg[v]--;
            //nếu bán bậc vào của v là 0 => push nó vào hàng đợi
            if(inDeg[v] == 0) q.push(v); 
        }
    }
    if(cnt == n){   //đầy đủ các đỉnh => ko có chu trình
        return false;
    }
    else return true;
}
void cycle(){
    input();
    if(BFS_Directed_cycle()) cout << "YES\n";
    else cout << "NO\n";
}

//Kiểm tra chu trình âm => đồ thị có nhiều tplt thì kt hết tất cả các tplt đó
bool bellmanFordcheck(int s){
    d[s] = 0;
    for(int i = 1; i <= n - 1; i++){
        for(edge e : lst){
            if(d[e.x] < INT_MAX){
                d[e.y] = min(d[e.y], d[e.x] + e.w);
            }
        }
    }
    bool check = false;
    for(edge e : lst){
        if(d[e.x] < INT_MAX){
            if(d[e.y] > d[e.x] + e.w){
                check = true; 
                break;
            }
        }
    }
    return check;
}
bool negativeCycle(){
    fill(d + 1, d + n + 1, INT_MAX);
    bool res = false;
    for(int i = 1; i <= n; i++){
        if(d[i] == INT_MAX){
            if(bellmanFordcheck(i)){
                res = true; 
                break;
            }
        }
    }
    return res;
}
```
### Chu trình Hamilton
Thăm mỗi đỉnh 1 lần -> đồ thị Hamilton <=> bậc của các đỉnh trên đồ thị >= 2
```cpp
//Đường đi Hamilton => đi qua mỗi đỉnh 1 lần
set<int> adj[k + 1]; //dùng set để lưu ds kề mục đích để truy xuất đỉnh đầu tiên trong chu trình cho nhanh
int degree[k + 1]; // tính bậc đỉnh đồ thị
bool visited[k + 1];
int HC[k + 1]; //lưu vị trí các đường đi của Hamilton
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].insert(y);
        adj[y].insert(x);
        degree[x]++;
        degree[y]++;
    }
}
void checkCycle(){
    input();
    //kt xem đồ thị có phải là chu trình hamilton ko bậc của 1 đỉnh bất kì < n/2
    for(int i = 1; i <= n; i++){
        if(degree[i] < n / 2){
            cout << "NO\n";
            return;
        }
    }
}
void hamilton(int pos, int u){
    visited[u] = true;
    HC[pos] = u; //lưu đường đi các đỉnh tại vị trí pos trong mảng
    if(pos == n){ //đã thăm hết các đỉnh của đồ thị
        if(adj[u].find(HC[1]) != adj[u].end()){ //tìm vị trí đầu tiên trong mảng ham là đỉnh đầu tiên xuất phát có kề vs đỉnh cuối ko, nếu kề là chu trình
            HC[++pos] = HC[1];
            //vị trí pos có thể khác nên phải cân nhắc đề chạy for duyệt cho chuẩn
            for(int i = 1; i <= pos; i++){
                cout << HC[i] << ' '; //in ra đường đi chu trình hamilton 
            }
            cout << endl;
            return; //khi duyệt xong chu trình thì kết thúc luôn đỡ phải duyệt tiếp
        }
    }
    //duyệt ds kề của u
    for(int v : adj[u]){
        if(!visited[v]){
            //gọi hamilton
            hamilton(pos + 1, v);
            //trả lại trả thái ban đầu cho v => nếu nhánh này ko phải hamilton => xét nhánh khác
            visited[v] = false;
        }
    }
    visited[u] = false; 
}
```
### Chu trình Euler 
Đi qua mỗi cạnh đúng 1 lần
```cpp
set<int> adj[k + 1];
int degree[k + 1]; 
vector<int> EC;
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].insert(y);
        adj[y].insert(x);
        degree[x]++;
        degree[y]++;
    }
}
void euler(int v){
    stack<int> st;
    //đẩy đỉnh v tùy vào stack
    st.push(v);
    while(!st.empty()){
        //lấy ra phần tử đầu tiên trong stack
        int x = st.top();
        //xét ds kề của đỉnh x xem có khác rỗng ko
        if(adj[x].size() != 0){
            //lấy ra phần tử đầu trong ds kề của x
            int y = *adj[x].begin();
            //đẩy vào stack
            st.push(y);
            //xóa cặp(x, y) để nó ko đi lại nữa
            adj[x].erase(y);
            adj[y].erase(x);
        }
        else{
            //đẩy ra khỏi ngăn xếp
            st.pop();
            //bỏ đỉnh x vào mảng EC
            EC.push_back(x);
        }
    }
    //thích thì lật ngược ko thì để nguyên vẫn đc
    reverse(EC.begin(), EC.end());
    for(int x : EC) cout << x << ' ';
}
```
### Đồ thị 2 phía - lưỡng phân 
Dùng 2 màu -> vô hướng => 2 đỉnh kề nhau ko được tô cùng 1 màu
```cpp
int color[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    memset(visited, false, sizeof(visited));
}
bool DFSbipartie(int u, int parent){
    color[u] = 3 - color[parent]; // dùng 2 màu : 1 -> trắng, 2 -> đen (0 là chưa tô màu)
    for(int v : adj[u]){
        if(color[v] == 0){
            //xét các tplt của đồ thị -> ko là đồ thị liên thông
            if(!DFSbipartie(v, u)) return false; 
        }
        //màu của đỉnh giống màu của cha nó => 2 đỉnh kề nhau chung 1 màu
        else if(color[v] == color[u]) return false;
    }
    return true;    
}
void bipartie(){
    input();
    //Đồ thị có tplt mà ko 2 phía thì đồ thị đó sẽ ko phải là đồ thị 2 phía
    bool check = true;
    //cho cha của đỉnh u ban đầu có màu là 2
    color[0] = 2;
    //duyệt hết các đỉnh
    for(int i = 1; i <= n; i++){
        if(color[i] == 0){
            if(!DFSbipartie(i, 0)){
                check = false;
            }
        }
    }
    cout << check << endl;
}
```
### Tô màu đồ thị
```cpp
//Tô màu đồ thị
vector<int> adj[k + 1];
int color[k + 1];
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
}
//KT có thể tô màu c cho đỉnh u hay ko
bool check(int u, int c){
    for(int v : adj[u]){
        if(color[v] == c) return false;
    }
    return true;
}
//Đếm số lượng màu tối đa mà màu c có thể tô đc
int cntColor(int c){
    int res = 0;
    for(int i = 1; i <= n; i++){
        if(!color[i] && check(i, c)){
            color[i] = c;
            res++;
        }
    }
    return res;
}
//Duyệt màu đồ thị
void printColor(){
    input();
    int c; cin >> c;
    int cnt = 0;
    while(cnt < n){ //chưa tô màu hết các đỉnh
        cnt += cntColor(c++);
    }
    for(int i = 1; i <= n; i++){
        cout << i << ' ' << color[i] << endl;
    }
}
//Test mẫu
// 9 8
// 1 2
// 1 6
// 2 3
// 2 4
// 3 5
// 6 7
// 7 8
// 7 9
// 4 7
// 3
// 2 3
// 4 8
// 6 7 
```
### Cây khung cực tiểu - Minimum spanning tree
```cpp
//Thuật toán Prim -> ứng dụng priority queue
struct edge{
    int x, y, w;
    //w là trọng số của cạnh
};
vector<edge> lst;
typedef pair<int, int> ii;
vector<ii> adj[k + 1];
bool taken[k + 1]; // check xem nó có nằm trong MST ko (True MST-False V)
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x , y, w; cin >> x >> y >> w;
        adj[x].push_back({y, w});
        adj[y].push_back({x, w});
    }
    memset(taken, false, sizeof(taken));
}
void prim(int s){
    //tạo 1 hàng đợi ưu tiên nhưng mà là min heap -> top luôn là thằng nhỏ nhất
    priority_queue<ii, vector<ii>, greater<ii>> q;
    //nạp giá trị ban đầu s vào cây khung (MST)
    taken[s] = true;
    //duyệt ds kề của s trong V để đẩy vào MST
    for(auto it : adj[s]){
        int t = it.first; // trong cặp pair giá trị thứ 1 là đỉnh thứ 2 là trọng số
        if(!taken[t]){
            q.push({it.second, t}); // đẩy trọng số vào trước trong hàng đợi ưu tiên để nó sắp xếp theo thứ tự tăng dần
        }
    }
    ll d = 0; // đếm trọng số
    ll cnt = 0; // đếm số lượng cành có bằng n - 1 đỉnh để thỏa đk cây khung ko
    while(!q.empty()){
        //lấy ra cạnh ngắn nhất
        pair<int, int> e = q.top();
        q.pop();
        int u = e.second, w = e.first;
        if(!taken[u]){ // bắt buộc phải có dòng này lúc đầu lọc thì thỏa điều kiện 2 đỉnh thuộc 2 tập khác nhau nhưng trong quá trình đẩy vào q có khả năng nó sẽ đẩy vào sai => phải check
            ++cnt; // tăng số lượng cạnh
            d += w; // cộng trọng số của cây
            taken[u] = true;
            //duyệt ds kề của đỉnh u xem mấy đỉnh khác có trong MST ko => ko có thì đẩy vào
            for(auto it : adj[u]){
                if(!taken[it.first]){
                    q.push({it.second, it.first});
                }
            }
        }
    }
    if(cnt == n - 1) cout << d << endl;
    else cout << "NO\n";
}
//Bản đầy đủ 
//Thuật toán Prim -> ứng dụng priority queue => lưu lại cạnh
struct edge{
    int x, y, w;//w là trọng số của cạnh
};
vector<edge> MST;
typedef pair<int, int> ii;
vector<ii> adj[k + 1];
bool taken[k + 1]; // check xem nó có nằm trong MST ko (True MST-False V)
int parent[k + 1], d[k + 1]; // lưu lại cạnh của cây khung
void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x , y, w; cin >> x >> y >> w;
        adj[x].push_back({y, w});
        adj[y].push_back({x, w});
    }
    memset(taken, false, sizeof(taken));
    //Khởi tạo trọng số ban đầu cho các đỉnh là INT_MAX
    for(int i = 1; i <= n; i++) d[i] = INT_MAX;
}
void prim(int u){
    priority_queue<ii, vector<ii>, greater<ii>> q;
    int res = 0; // đếm trọng số
    q.push({0, u}); //push vào hàng đợi ưu tiên đỉnh nguồn
    while(!q.empty()){
        //lấy ra cạnh ngắn nhất
        pair<int, int> e = q.top();
        q.pop();
        int v = e.second, w = e.first;
        //trong hàng đợi ưu tiên có thể có nhiều cặp cạnh có đỉnh trùng nhau nên cần có câu lệnh này
        if(taken[v]) continue;
        res += w; // trọng số đc cộng
        taken[v] = true; // đưa vào MST
        if(u != v) MST.push_back({v, parent[v], w});
        //duyệt tất cả các đỉnh kề
        for(auto it : adj[v]){
            //it.first = đỉnh, it.second = trọng số
            if(!taken[it.first] && it.second < d[it.first]){
                q.push({it.second, it.first});
                //lưu vào mảng để xuất cạnh của cây khung
                d[it.first] = it.second; // kỉ lục ghi nhận => trọng số nhỏ nhất của tất cả các cạnh nối vs it.first
                parent[it.first] = v;
            }
        }
    }
    cout << res << endl;
    for(auto it : MST) cout << it.x << ' ' << it.y << ' ' << it.w << endl;
}

//Thuật toán Kruskal
struct edge{
    int x, y, w;
    //w là trọng số của cạnh
};
vector<edge> lst;

void input(){
    cin >> n >> m;
    for(int i = 0; i < m; i++){
        int x, y, w; cin >> x >> y >> w;
        edge e{x, y, w};
        lst.push_back(e);//nhập ds cạnh vào có trọng số
    }
}
bool cmp(edge a, edge b){
    return a.w < b.w;
}
void kruskal(){
    vector<edge> MST; //lưu ds cạnh của cây khung
    int d = 0; // đếm trọng số của cây khung cực tiểu
    sort(lst.begin(), lst.end(), cmp); //sort danh sách theo thứ tự tăng dần
    /*
    có thể dùng biểu thức lambda chỗ này
    sort(list.begin(), list.end(), [](edge a, edge b)->bool{
        return a.w < b.w;
    });
    */
    for(edge e : lst){ //duyệt từng cạnh trong ds cạnh và chọn cạnh có trọng số nhỏ nhất và ko tạo thành chu trình bỏ vào cây khung
        if(MST.size() == n - 1) break; //cây khung đã đủ
        if(Union(e.x, e.y)){ //2 đỉnh này có thể kết nối với nhau
            MST.push_back(e);
            d += e.w;    
        }
    }
    if(MST.size() < n - 1) cout << "NO\n";
    else{
        cout << d << endl;
        for(edge e : MST) cout << e.x << ' ' << e.y << ' ' << e.w << endl;
    }
}
```

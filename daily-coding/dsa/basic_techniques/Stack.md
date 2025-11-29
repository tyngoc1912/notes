# Stack
Hoạt động theo cơ chế LIFO
## Cài đặt bằng mảng
```cpp
int n = 0, st[100001];

void push(int x){
    st[n] = x;
    n++;
}

void pop(){
    if(n >= 1) n--;
    else return;
}

int top(){
    return st[n - 1];
}

int sz(){
    return n;
}
```
## Cài đặt bằng danh sách liên kết
```cpp
struct Node{
    int data;
    Node* next;
    Node(int x){
        this->data = x;
        this->next = NULL;
    }
};
typedef Node* node;

node makeNode(int x){
    node *tmp = new node;
    tmp->data = x;
    tmp->next = NULL;
    return tmp;
}

void push(node &top, int x){
    node tmp = new node;
    if(top == NULL){
        top = tmp;
        return;
    }
    tmp->next = top;
    top = tmp;
}

void pop(node &top){
    if(top == NULL) return;
    else{
        node tmp = top;
        top = tmp->next;
        delete tmp;
    }
}

int top(node top){
    if(top != NULL) return top->data; 
}

int sz(node top){
    int res = 0;
    while(top != NULL){
        res++;
        top = top->next;
    }
    return res;
}
```
## Ký pháp ba lan 
### Biểu thức trung tố sang tiền tố - Khó nhất
```cpp
int priority(char c){
    if(c == '^') return 3;
    else if(c == '*' || c == '/') return 2;
    else if(c == '+' || c == '-') return 1;
    return 0;
}
void convert(string s){
    //string lưu kq
    string res = "";
    //dùng stack để thực hiện liên quan những bài này
    stack<char> st; // lưu các toán tử và toán hạng
    //duyệt từ đầu đến cuối string
    for(int i = 0; i < s.length(); i++){
        if(isalpha(s[i])) res += s[i]; //là toán hạng push vào res
        else if(s[i] == '(') st.push(s[i]); // là dấu mở ngoặc push vào stack là mốc để thực hiện vòng lặp
        else if(s[i] == ')'){ 
            // bắt đầu lặp để thực hiện push vào cho đến khi gặp dấu mở ngoặc tương ứng
            while(!st.empty() && st.top() != '('){
                res += st.top(); // đẩy các toán tử trong stack vào res
                st.pop(); 
            }
            st.pop(); //xóa nốt dấu mở ngoặc
        }
        else{ // xử lí các toán tử
            while(!st.empty() && priority(s[i]) <= priority(st.top())){ // để thực hiện phép tính các phép tính có độ ưu tiên lớn còn nếu ngang nhau sẽ thực hiện từ trái sang phải
                res += st.top();
                st.pop();
            }
            st.push(s[i]); //đưa kí tự có độ ưu tiên thấp vào
        }
    }
    //thực hiện hết vòng lặp thì còn những toán tử nào trong stack thì pop hết ra
    while(!st.empty()){
        res += st.top();
        st.pop();
    }
    cout << res << endl;
} 
```
### Biểu thức tiền tố sang trung tố - Duyệt từ cuối về đầu
```cpp
void convert(string s){
    stack<string> st; //stack string để lưu kq
    for(int i = s.size() - 1; i >= 0; i--){
        if(isalpha(s[i])) st.push(string(1, s[i])); //hàm string(số kí tự, kí tự) -> biến đổi char thành string vs số lượng string là số kí tự truyền vào như copy lại
        else{
            string op1 = st.top();
            st.pop();
            string op2 = st.top();
            st.pop();
            string expression = '(' + op1 + s[i] + op2 + ')';
            //đẩy lại vào stack -> nguyên cụm (A+B)
            st.push(expression);
        }
    }
    cout << st.top() << endl;
} 
```
### Biểu thức tiền tố sang hậu tố - Duyệt từ cuối về đầu
```cpp
void convert(string s){
    stack<string> st; //stack string để lưu kq
    for(int i = s.size() - 1; i >= 0; i--){
        if(isalpha(s[i])) st.push(string(1, s[i])); //hàm string(số kí tự, kí tự) -> biến đổi char thành string vs số lượng string là số kí tự truyền vào như copy lại
        else{
            string op1 = st.top();
            st.pop();
            string op2 = st.top();
            st.pop();
            string expression = op1 + op2 + s[i];
            //đẩy lại vào stack -> nguyên cụm (A+B)
            st.push(expression);
        }
    }
    cout << st.top() << endl;
} 
```
### Biểu thức hậu tố sang tiền tố - Duyệt từ đầu tới cuối
```cpp
void convert(string s){
    stack<string> st; //stack string để lưu kq
    for(int i = 0; i < s.size(); i++){
        if(isalpha(s[i])) st.push(string(1, s[i])); //hàm string(số kí tự, kí tự) -> biến đổi char thành string vs số lượng string là số kí tự truyền vào như copy lại
        else{
            string op1 = st.top();
            st.pop();
            string op2 = st.top();
            st.pop();
            string expression = s[i] + op2 + op1;
            //đẩy lại vào stack -> nguyên cụm (A+B)
            st.push(expression);
        }
    }
    cout << st.top() << endl;
} 
```
## Kiểm tra ngoặc tổng quát
```cpp
bool isValid(string s){
    stack<char> st;
    for(char x : s){
        if(x == '(' || x == '{' || x == '[') st.push(x);
        else if(st.empty()) return false;
        else{
            if((x == ')' && st.top() != '(') || (x == '}' && st.top() != '{') || (x == ']' && st.top() != '[')) return false;
            st.pop();
        }
    }
    return st.empty();
}
```
## Tính toán các biểu thức
```cpp
int priority(char c){
    if(c == '*' || c == '/') return 2;
    else if(c == '+' || c == '-') return 1;
    return 0;
}

ll compute(ll a,ll b, char c){
    if(c == '+') return a + b;
    else if(c == '-') return a - b;
    else if(c == '*') return a * b;
    else return a/b;
}
void convert(string s){
    stack<char> st1;
    stack<ll> st2;
    for(int i = 0; i < s.size(); i++){
        if(s[i] == '('){
            st1.push(s[i]);
        }
        else if(isdigit(s[i])){
            ll tmp = 0;
            while(i < s.size() && isdigit(s[i])){
                tmp = tmp * 10 + s[i] - '0'; 
                ++i;
            }
            --i; //rất quan trong vì khi kết thúc vòng while thì i nó đã nhảy sang kí tự kế tiếp rồi phải lùi lại để tăng i lên lại sẽ đúng vị trí 
            st2.push(tmp);
        }
        else if(s[i] == ')'){
            while(!st1.empty() && st1.top() != '('){
                ll op1 = st2.top();
                st2.pop();
                ll op2 = st2.top();
                st2.pop();
                st2.push(compute(op2, op1, st1.top())); //tính toán từ toán hạng đứng trước là 2 rồi tới 1
                st1.pop();
            }
            st1.pop();
        }
        else{ //toán tử
            while(!st1.empty() && priority(s[i]) <= priority(st1.top())){
                ll op1 = st2.top();
                st2.pop();
                ll op2 = st2.top();
                st2.pop();
                st2.push(compute(op2, op1, st1.top())); 
                st1.pop();
            }
            st1.push(s[i]); //đẩy kí tự độ ưu tiên thấp vào stack
        }
    }
    //trong stack vẫn còn kí tự thì pop hết ra
    while(!st1.empty()){
        ll op1 = st2.top();
        st2.pop();
        ll op2 = st2.top();
        st2.pop();
        st2.push(compute(op2, op1, st1.top())); 
        st1.pop();
    }
    cout << st2.top() << endl;
}
void solve(){
    int q; cin >> q;
    while(q--){
        string s; cin >> s;
        convert(s);
    }
}
```
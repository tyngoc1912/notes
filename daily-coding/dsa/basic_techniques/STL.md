# STL
## Functions of STL
Import
```cpp
#include <algorithm>
#include <functional> // if using lambda
```
### [`max`](https://en.cppreference.com/w/cpp/algorithm/max)
```cpp
cout << min(10, 20) << endl; // tương tự với max()
cout << max({10, 20, 30, 50}) << endl; // tương tự vs min()
```
### [`max_element`](https://en.cppreference.com/w/cpp/algorithm/max_element)
```cpp
//trả về con trỏ và iterator đến phần tử => dùng * để giải tham chiếu
cout << *max_element(a, a + n) << endl;
cout << *min_element(v.begin() + 3, v.begin() + 5) << endl;
//lambda function
auto it_abs_max = max_element(v.begin(), v.end(), [](int a, int b){ return abs(a) < abs(b); });
```
### `accumulate`
```cpp
//accumulate => tính tổng
int sum = accumulate(a, a + n, 0); //giống như khởi tạo s = 0 -> đi cộng vào
cout << sum << endl;
cout << accumulate(v.begin(), v.end(), 0) << endl;
```
### `swap`
```cpp
//swap => hoán vị giá trị 2 phần tử 
int x = 100, y = 200;
cout << x << ' ' << y << endl;
swap(x, y);
cout << x << ' ' << y << endl;
```
### `find`
```cpp
//find() tìm 1 phần tử có xuất hiện trong mảng, vector, set hay ko => trả về iterator hoặc con trỏ
if(find(a, a + n, 5) != a + n) cout << "FOUND" << endl;
else cout << "NOT FOUND" << endl;
if(find(v.begin(), v.end(), 4) != v.end()) cout << "FOUND" << endl;
else cout << "NOT FOUND" << endl;
```
### `memset`
```cpp
//memset() => gán tất cả các phần tử trong mảng, mảng 2 chiều cho 0 hoặc -1
int b[n];
memset(b, 0, sizeof(a)); 
for(int x : b) cout << x << ' ';
cout << endl;
```
### `fill`
```cpp
//fill() => gán tất cả các phần tử trong mảng, vector cho 1 giá trị bất kì
vector<int> vi(5);
fill(begin(vi), end(vi), 200);
for(int x : vi) cout << x << ' ';
cout << endl;
```
### `merge`
```cpp
//merge => trộn 2 dãy tăng dần vào 1 dãy
vector<int> res(10);
vector<int> c = {1, 2, 3, 6, 8};
vector<int> d = {2, 4, 7, 10, 11};
merge(c.begin(), c.end(), d.begin(), d.end(), res.begin());
for(int x : res) cout << x << ' ';
cout << endl;
```
### `reverse`
```cpp
//reverse() => lật ngược mảng hoặc vector hoặc string
string s = "ngocty";
reverse(s.begin(), s.end());
cout << s << endl;
```
### [`sort`](https://en.cppreference.com/w/cpp/algorithm/sort) and[`stable_sort`](https://en.cppreference.com/w/cpp/algorithm/)
```cpp
//mảng
sort(a, a + n);                 // Sorts ascending (default)
sort(a, a + n, less<int>());    // Sorts descending
sort(a, a + n, greater<int>()); // Sorts descending
sort(a + x, a + y + 1); // sắp xếp trên [x,y] theo thứ tự tăng dần
stable_sort(a, a + n);          // Stable sorts ascending
//comparator
bool cmp(int a, int b){
    return abs(a) < abs(b);
}
stable_sort(a, a + n, cmp);
//lambda
stable_sort(a, a + n, [](int a, int b){
    return abs(a) < abs(b);
});
//vector
sort(vi.begin(), vi.end()); // sắp xếp trên cả vector
sort(vi.begin(), vi.end(), greater<int>()); // sorts descending
sort(vi.begin() + x, vi.begin() + y + 1); // sắp xếp trên [x, y] của vector
```
### [`binary_search`](https://en.cppreference.com/w/cpp/algorithm/binary_search)
```cpp
//sort trước khi dùng
//mảng 
int n; cin >> n;
int a[n];
for(int &x : a) cin >> x;
int x; cin >> x;
if(binary_search(a, a + n, x)) cout << 1 << endl;
else cout << 0 << endl;

//vector
vector<int> v;
for(int &x : a){
    cin >> x;
    v.push_back(x);
}
bool found_4 = binary_search(v.begin(), v.end(), 4);  //=> true
bool found_3 = binary_search(v.begin(), v.end(), 3);  //=> false
```
### [`lower_bound`](https://en.cppreference.com/w/cpp/algorithm/lower_bound)
Tìm vị trí đầu tiên của phần tử >= x trong mảng, vector sắp xếp tăng dần **CÓ THỨ TỰ**
```cpp
auto it = lower_bound(v.begin(), v.end(), x);  //=> iterator poiting at index position x
cout << *it << endl; 

it--; it += k;
cout << *it << endl; // phần tử < x

if(it == v.end()) cout << "NO\n"; //không tìm thấy
```
### [`upper_bound`](https://en.cppreference.com/w/cpp/algorithm/upper_bound)
Tìm vị trí đầu tiên của phần tử > x trong mảng, vector sắp xếp tăng dần **CÓ THỨ TỰ**
```cpp
it = upper_bound(a, a + n, x);  //=> iterator poiting at index position x
cout << *it << endl;

it--;
cout << *it << endl; // phần tử <= x

if(it == a + n) cout << "No\n"; // không tìm thấy
```
### Note `lower_bound` and `upper_bound`
- 2 hàm trên trả về con trỏ nếu là mảng, và iterator nếu là vector => tính chỉ số : `auto it = lower_bound(a, a + n, x) - a/a.end()`
- Nếu ko tìm thấy phần tử thỏa điều kiện thì trả về con trỏ `a + n` hoặc `a.end()`
- Cũng áp dụng được cho **set với map** nhưng phải dùng hàm `distance(se.begin(), it)` để tính chỉ số **(khoảng cách của phần tử tìm đc vs phần tử đầu tiên)**

### Sets function
Đều phải sort trước khi sử dụng => các phép trong tập hợp
- `set_union`
- `set_intersection`
- `set_difference`
- `set_symmetric_difference`
```cpp
//set_union tương tự vs các hàm set_ khác
int z[] = {1, 2, 3, 4, 5};
int t[] = {1, 3, 4, 9, 10};
vector<int> u(11);
auto it = set_union(z, z + 5, t, t + 5, u.begin()); // tương tự những cái còn lại
// phải resize vector u đi vì sau khi giao hợp hiệu thì vector u kích thước sẽ thay đổi chứ ko như ban đầu
u.resize(it - u.begin());
for(auto x : u) cout << x << ' ';
cout << endl;
```
### [`std::copy`, `std::copy_if`](https://en.cppreference.com/w/cpp/algorithm/copy)
```cpp
vector<int> v1{1,2,3,4,5};
vector<int> v2(5);
copy(v1.begin(), v1.end(), v2.begin());
//v1 == v2

vector<int> v_geq3{3,4,5};
vector<int> v3(3);
copy_if(v1.begin(), v1.end(), v3.begin(), [](int i){ return i >= 3;});
//v3 == v_geq3
```
### [`std::count`, `std::count_if`](https://en.cppreference.com/w/cpp/algorithm/count)
```cpp
vector<int> v{1,2,1,3,1,4,1,5,1,6};
cout << count(v.begin(), v.end(), 1);  //5
cout << count_if(v.begin(), v.end(), [](int i){ return i >= 3; });  //4
```
### [`std::equal_range`](https://en.cppreference.com/w/cpp/algorithm/equal_range)
```cpp
vector<pair<int, char>> v_ic = { {1,'A'}, {2,'B'}, {2,'C'}, {2,'D'}, {4,'G'}, {3,'F'} };
pair<int, char> value = {2, '?'};
auto [it_begin, it_end] = equal_range(v_ic.begin(), v_ic.end(), value);
auto it_lower = lower_bound(v_ic.begin(), v_ic.end(), value);
auto it_upper = upper_bound(v_ic.begin(), v_ic.end(), value);
//it_begin == it_lower
//it_end == it_upper
```
### [`std::remove`, `std::remove_if`](https://en.cppreference.com/w/cpp/algorithm/remove)
```cpp
string str1 = "Hello   World!";
str1.erase(remove(str1.begin(), str1.end(), ' '), str1.end());
//str1 == "HelloWorld!"

string str2 = " Hello \t World! \n";
str2.erase(remove(str2.begin(), str2.end(), [](char x){ return isspace(x); }), str2.end());
//str2 == "HelloWorld!"
```
## Iterator
- Các hàm 
    - `v.begin()` : phần tử đầu
    - `v.rbegin()` : phần tử cuối
    - `v.end()` : sau phần tử cuối
    - `v.rend()` : trước phần tử đầu
- Dùng iterator để duyệt set, map, vector, pair
    - [x; y) = (v.begin() + x, v.begin() + y);
    - [x; y] = (v.begin() + x, v.begin() + y + 1);
    - v[x] = v.begin() + x 
``` cpp
for(int i = 0; i < vi.size(); i++) cin >> v[i] ;
for(int &x : vi) cin >> x;
for(int i = 0; i < vi.size(); i++) cout << v[i] ;
for(int x : vi) cout << x; // iterator thông minh và ko cần *it để truy cập giá trị
for(vector<int>::iterator it = vi.begin(); it ≠ vi.end(); it++) cout << *it;
vector<int>::iterator it = auto it  //auto thay cho mọi kiểu dữ liệu
```
## Pair
- Khai báo : pair<kiểu_dữ_liệu, kiểu_dữ_liệu>; 
    - `pair<int, int> pi;`   
    - `pair<pair<bool, char>, string> pi2;` 
- Nhập và xuất pair :
``` cpp
cin >> a. first >> a.second  // pi = {100, 200} = make_pair(100, 200);
for(int i = 0; i < n; i++){
    cin >> a[i].first >> a[i].second; 
    cout << a[i].first << a[i].second;
}
```
## Vector
- Khai báo và nhập : 
    ```cpp
    vector<int> vi; 
    vector <pi> vii; 

    for(int i = 0; i < n; i++) {
        int temp; cin >> temp; 
        vi.push_back(temp);
    }
    // khai báo vector có n pt như mảng
    const n = 1e6;
    vector<int> a(n); 
    for(int i = 0; i < n; i++){
        cin >> a[i]; // hoặc dùng push_back() vẫn đc 
    }
    // gán giá trị sẵn cho vector 
    vector<kiểu dữ liệu> a(n, giá trị); // n pt có giá trị bằng giá trị gán
    vector<int> a(n, 6);
    ```
- Các hàm :
    - `vi.push_back()` : thêm vào sau vector và truy cập phần tử như mảng
    - `vi.size()` : cho biết số lượng phần tử vector
    - `vi.pop_back()` : xóa phần tử ở cuối vector
    - `vi.erase(vị trí)` : xóa phần tử thông qua iterator `vi.erase(vi.begin() + x)` 
    - `vi.erase(it.begin() + x, it.begin() + y + 1)` : xóa 1 đoạn [x, y] 
    - `vi.insert(vị trí, giá trị)` : thêm phần tử vào vị trí nào đó `vi.insert(vi.begin() + x, k)`
    - `vi.clear()` : xóa toàn bộ phần tử trong vector `vi.clear();` : `vi.size() = 0`
- Ứng dụng :
    ```cpp
    int mark[1000001] = {0};
    //In các phần tử khác nhau theo thứ tự xuất hiện
    //Code trâu
    int n; cin >> n;
    int a[n];
    for(int &x : a) cin >> x;
    for(int i = 0; i < n; i++){
        bool check = true;
        for(int j = 0; j < i; j++){
            if(a[i] == a[j]){
                check = false;
                break;
            }
        }
        if(check) cout << a[i] << ' ';
    }
    cout << endl;

    //Đếm phân phối -> nhanh
    for(int i = 0; i < n; i++){
        if(mark[a[i]] == 0){
            cout << a[i] << " ";
            mark[a[i]] = 1;
        }
    }
    cout << endl;

    //Trộn 2 mảng đã sx vào 1 mảng + tìm giao và hợp giữa 2 mảng
    int m, n; cin >> m >> n;
    int c[m], d[n];
    vector<int> v;
    for(int &x : c) cin >> x;
    for(int &x : d) cin >> x;
    int i = 0, j = 0;
    vector<int> giao, hop;
    while(i < m && j < n){
        if(c[i] == d[j]){
            v.push_back(c[i]);
            hop.push_back(c[i]);
            giao.push_back(c[i]);
            i++; j++;
        }
        else if(c[i] > d[j]){
            v.push_back(d[j]);
            hop.push_back(d[j]);
            j++;
        }
        else{
            v.push_back(c[i]);
            hop.push_back(c[i]);
            i++;
        }
    }
    while(i < m) v.push_back(c[i++]); hop.push_back(c[i++]);
    while(j < n) v.push_back(d[j++]); hop.push_back(d[j++]);

    for(int i = 0; i < v.size(); i++){
        cout << v[i] << ' ';
    }
    for(int i = 0; i < hop.size(); i++){
        cout << hop[i] << ' ';
    }
    for(int i = 0; i < giao.size(); i++){
        cout << giao[i] << ' ';
    }
    ```
## Set
### set
- Khai báo : set<kiểu_dữ_liêu> tên_biến  `set<int> se`
- Các hàm :
    - `se.insert(giá trị)` : thêm phần tử vào set  `se.insert(3)`
    - `se.size()` : số lượng phần tử trong set
    - `se.find(x)`: tìm phần tử trong set trả về iterator trỏ tới phần tử trong set
        - `auto it = se.find(3);`
        - `vector<int>::iterator it = se.find(3);`
    - `se.count(x)` : đếm số phần tử xuất hiện trong set cho ra giá trị là 0 hoặc 1
    - `se.erase(x)` : xóa phần tử trong set
### multiset
- Khai báo : multiset<kiểu_dữ_liệu> tên_biến `multiset<long long> se;`
- Các hàm giống set trừ hàm find :
    - `se.find(x)` : trả về iterator của phần tử đầu tiên nếu các phần tử trùng nhau
### unordered_set
- Khai báo : unordered_set<kiểu_dữ_liệu> tên_biến `unordered_set<string> se;` 
- Các hàm giống set
## Map
### map
- Khai báo : map<kiểu dữ liệu, kiểu dữ liệu> tên_biến `map<int, int> mp;`
- Truy xuất phần tử : mp[key]  `mp[5];`
- Duyệt map :
    - **for each** : `for(pair<KDL, KDL> x : mp){ cout << x.first << x.second)` 
    - **iterator** : tương tự như set, vector NHƯNG chú ý (*it).first
    - Có thể thay `(*it).first = it → first`
- Các hàm :
    - `mp.insert({x, y})` : thêm 1 cặp phần tử `mp.insert({1, 5}) == map[1] = 5;`
    - `mp[key] = value` : có thể thay đổi giá trị của key đã cho trước
    - `mp[key]++` : tăng giá trị của value có trước lên 1 còn nếu chưa có key với value trong map thì tự động thêm vào và cho `mp[key] = 1`
    - `mp.size()` : số lượng cặp pt
    - `mp.find(x)` : tìm key có nằm trong map hay không và trả về iterator như vector hoặc set
    - `mp.count(x)` : đếm xem key trong map xuất hiện bao nhiêu lần
    - `mp.erase(x)` : xóa cả key và value và xóa thông qua key
### multimap
- Giống map
- **KO HỖ TRỢ mp[key] = value**  không truy cập và gán
### unordered_map 
- Giống map
- Ứng dụng hash_table : Truy xuất O(1)
### Ứng dụng map 
``` cpp
//Đếm tần suất xuất hiện của các ptu trong mảng
//Đếm theo thứ tự tăng dần
map<ll, int> mp;
int n; cin >> n;
ll a[n];
for(int i = 0; i < n; i++){
    cin >> a[i];
    mp[a[i]]++;
}
//Duyệt bình thường bằng iterator
for(auto it = mp.begin(); it != mp.end(); it++){
    cout << (*it).first << " " << (*it).second << endl;
}
//Duyệt bằng for each
// bản thân it là 1 pair trong map rồi chứ ko phải iterator nữa nên ko cần (*it) -> trỏ đến 1 pair trong map nữa rồi mới (*it).first
for(auto it : mp){
    cout << it.first << ' ' << it.second << endl;
}

//Đếm theo thứ tự xuất hiện của mảng
map<ll, int> mp;
int n; cin >> n;
ll a[n];
for(int i = 0; i < n; i++){
    cin >> a[i];
    mp[a[i]]++;
}
for(int i = 0; i < n; i++){
    if(mp[a[i]] != 0){
        cout << a[i] << ' ' << mp[a[i]] << endl;
        mp[a[i]] = 0;
    }
}
```
## List
- Khai báo : list<kiểu_dữ_liệu> tên_biến;
    ```cpp
    list<int> lst;              
    list<int> lst({1, 2, 3});   // Initialize with items {1, 2, 3}
    list<int> lst = {1, 2, 3};  // Initialize with items {1, 2, 3}
    ```
- Các hàm
    - `lst.empty()` : Checks if list is empty
    - `lst.size()` : Returns size of list
    - `lst.front()` : Get head item
    - `lst.back()` : Get tail item
    - `lst.clear()` : Clear all contents of list
    - `lst.push_back(item)` : Appends item to the rear
    - `lst.push_front(item)` : Prepends item at head
    - `lst.insert(it, item)` : Inserts item before iterator position
    - `lst.pop_back()` : Removes last item
    - `lst.reverse()` : Reverses the entire list
    - `lst.remove(v)` : Removes all elements equal to v
    - `lst.remove_if([](int x){ return x > 10; })` : Removes all elements greater than 10
    - [Splice](http://www.cplusplus.com/reference/list/list/splice/)
    ```cpp
    list<int> lst1 = {1, 2, 3};
    list<int> lst2 = {4, 5, 6};
    lst1.splice(lst1.end(), lst2); // O(1)
    list<int> lst3 = {1, 2, 3, 4, 5, 6};
    //lst1 == lst3
    ```
## Stack
- Khai báo : stack<kiểu_dữ_liệu> tên_biến; `stack<int> st`;
- Các hàm 
    - `st.empty()` : Checks if stack is empty
    - `st.size()` : Returns current size on stack
    - `st.top()` : Returns the topmost element
    - `st.push(item)` : Push item to top of stack
    - `st.pop(item)` : Pop item off top of stack
    - `st1.swap(st2)` : Swap two values of two stacks
## Queue 
- Khai báo : queue<kiểu_dữ_liệu> tên_biến; `queue<int> q;`
- Các hàm 
    - `q.empty()` : Checks if queue is empty
    - `q.size()` : Returns current size on queue
    - `q.front()` : Returns front item of queue
    - `q.back()` : Returns rear item of queue
    - `q.push(item)` : Enqueue item to rear of queue
    - `q.pop(item)` : Dequeue item from front of queue
    - `print_queue(q)`

## Deque
- Khai báo : deque<kiểu_dữ_liệu> tên_biến  `deque<int> deq;`
- Các hàm
    - `deq.empty()` : Checks if deque is empty
    - `deq.size()` : Returns current size on deque
    - `deq.front()` : Returns front item of deque
    - `deq.back()` : Returns rear item of deque
    - `deq.at(i)` : Returns the item at position i in O(1)
    - `deq.push_front(item)` : Push item to front of deque
    - `deq.pop_front(item)` : Pop item from front of deque
    - `deq.push_back(item)` : Inject item at rear of deque
    - `deq.pop_back(item)` : Eject item at rear of deque
## Priority_queue
- Khai báo 
    ```cpp
    // Creates a max-heap
    priority_queue<int> pq;                             

    // Creates a min-heap
    priority_queue<int, vector<int>, greater<int>> pq;  

    // Initialize max-heap with {1,2,3} | O(N)
    priority_queue<int> pq(less<int>(), {1,2,3});       

    // Initialize max-heap with vector | O(N)
    vector<int> vect = {1,2,3};
    priority_queue<int> pq(less<int>(), vect);          

    // comparator
    bool cmp(int lhs, int rhs) {
        return lhs < rhs; // This entails a max heap
    }
    priority_queue<int, vector<int>, cmp> pq;
    ```
- Các hàm
    - `pq.empty()`
    - `pq.size()`
    - `pq.top()`
    - `pq.push(item)` : Add
    - `pq.pop()` : Remove
    - `print_queue(pq)`

## String
- Khai báo : string tên_biến `string s = ”Noi dung”` (trong dấu ngoặc kép lưu 1 chuỗi kí tự)
- Nhập :
    - `cin >> s` : xâu không có dấu cách `s = "Python";`
    - `getline(cin, s, <có thể thêm kí tự phân cách khác>)` : xâu có dấu cách
    - `cin.ignore()` : xóa dấu cách của cin trước khi dùng getline
- Các hàm : 
    - `s.size()` hoặc `s.length()` : trả số lượng kí tự trong xâu s
    - `s.front() == s[0]` : kí tự đầu tiên
    - `s.back() == s[s.size() - 1]` → kí tự cuối cùng
    - `s.compare(t)` nếu s == t thì 0; nếu s > t thì 1; nếu s < t thì -1;
    - `+` : concate nối 2 xâu lại và cũng có thể nối xâu với kí tự
    - `isdigit(char c)` : kiểm tra chữ số
    - `islower(char c)` : kiểm tra chữ in thường
    - `isupper(char c)` : kiểm tra in hoa
    - `isalpha(char c)` : kiểm tra chữ cái
    - `int tolower(char c)` : chuyển thành chữ in thường mã ASCII ép kiểu char
    - `int toupper(char c)` : chuyển thành chữ in hoa mã ASCII ép kiểu char ta thường dùng `s[i] = toupper(s[i])` => gán lại cho s[i]
    - `stoi (string to integer)` : đổi string sang số int `int t = stoi(s);`
    - `stoll (string to ll)` : chuyển string sang long long `ll a = stoll(s);`
    - `to_string()` : đổi số sang string `string s = to_string(t);`
    - `stringstream ss(s)` : dùng để tách các xâu có nhiều dấu cách hoặc xâu con có các kí tự phân biệt
        ```cpp
        string s = "hoc lap trinh   Python";
        string tmp;
        stringstream ss(s);
        while(ss >> tmp){
            //code
        }
- Ứng dụng :
    ```cpp
    //Chuyển đổi chuỗi sang full in hoa
    void toUpper(string &s){
        for(int i = 0; i < n; i++){
            s[i] = toupper(s[i]);
        }
    }

    //Chuyển đổi chuỗi sang full in thường
    void toLower(string &s){
        for(int i = 0; i < n; i++){
            s[i] = tolower(s[i]);
        }
    }

    //Đối vs các số lớn có nhiều chữ số (>= 10^6 chữ số) lưu vào string
    void solve(string s){
        cin >> s;
        int sum = 0;
        for(char x : s){
            sum += (int) x; // sai vì nó sẽ cộng mã ASCII của x vào sum chứ ko phải số cần tính
            sum += x - '0'; //Lấy mã ASCII của char x trừ đi mã ASCII 0 sẽ ra số tách ra từ xâu
        }
        cout << sum;
    }

    //Tách chuỗi nhỏ trong xâu -> áp dụng tách số vẫn được nhưng phải convert qua
    string s; getline(cin, s);
    stringstream ss(s); //khởi tạo stringstream tách chuỗi trong xâu
    string word;
    vector<string> vi;
    //while(getline(ss, word, ts3)) ts3 : là kí hiệu sẽ ngắt luồng cin
    while(ss >> word){ // luồn nhập liên tục cứ tới dấu cách là ngưng
        vi.push_back(word);
    }
    //có thể làm bất cứ thứ gì với vector<string>
    sort(vi.begin(), vi.end());  
    ```
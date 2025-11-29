# String 
## Xử lý xâu căn bản
### Tổng chữ số của số nguyên lớn
```cpp
ll sumOfNum(string s){
    ll sum = 0;
    for(int i = 0; i < s.size(); i++){
        sum += s[i] - '0';
    }
    return sum;
}
```
### Kiểm tra xâu con t trong xâu s
```cpp
bool checkSubString(string s, string t){
    if(s.find(t) != string::npos) return true;
    else return false;
}
```
### In hoa
```cpp
string Upper(string s){
	for(int i = 0; i < s.size(); i++){
		s[i] = toupper(s[i]);
	}
	return s;
}
// Cách khác
void upper(string &s){
    for(int i = 0; i < s.size(); i++){
        s[i] = toupper(s[i]);
    }
}
```
### In thường
```cpp
string Lower(string s){
	for(int i = 0; i < s.size(); i++){
		s[i] = tolower(s[i]);
	}
	return s;
}
// Cách khác
void lower(string &s){
    for(int i = 0; i < s.size(); i++){
        s[i] = tolower(s[i]);
    }
}
```
### Kiểm tra in hoa và in thường
```cpp
void check(char c){
	if(c >= 65 && c <= 90){
		cout << "UPCASE : ";
		cout << (char)(c + 32) << endl;
    } 
	else if(97 <= c <= 122){
		cout << "DOWNCASE : ";
		cout << (char)(c - 32);
	} 
}
```
### Chuẩn hóa ngày tháng năm sinh 
- Theo form dd/mm/yyyy => ứng dụng để viết comparator so sánh tuổi theo thứ tự từ điển
```cpp
void stringMod(string &s){
    if(s[2] != '/') s = "0" + s;
    if(s[5] != '/') s.insert(3, "0");
}
```
## Phân loại kí tự
```cpp
void solve1(){
    string s;
    getline(cin, s);
    int num = 0, character = 0, special = 0;
    for(int i = 0; i < s.size(); i++){
        if(isdigit(s[i])) num++;
        else if(isalpha(s[i])) character++;
        else special++;
    }
    cout << character << " " << num << " " << special << endl;
}
```

### Đếm tần xuất xuất hiện của các từ trong xâu
```cpp
void solve3(){
    string s;
    getline(cin, s);
    //cin.ignore();
    //C1
    int cnt[256] = {0};
    for(int i = 0; i < s.size(); i++){
        cnt[s[i]]++;
    }
    //chỉ duyệt 0 - 255 hoặc bé hơn 256 nếu dùng <= 256 sẽ sai ngay
    for(int i = 0; i < 256; i++){
        if(cnt[i] != 0){
            cout << (char)i << " " << cnt[i] << endl;
        }
    }
    cout << endl;
    for(int i = 0; i < s.size(); i++){
        if(cnt[s[i]] != 0){
            cout << s[i] << " " << cnt[s[i]] << endl;
            cnt[s[i]] = 0;
        }  
    }
    cout << endl;
    //C2
    map<char, int> mp;
    for(auto x : s) mp[x]++;
    for(auto it : mp) cout << it.first << ' ' << it.second << endl;
    cout << endl;
    for(auto x : s){
        if(mp[x] != 0){
            cout << x << " " << mp[x] << endl;
            mp[x] = 0;
        }
    }
    cout << endl;
}
```
### Tần suất xuất hiện nhiều nhất, ít nhất
- Kí tự có số lần xuất hiện nhiều nhất và thứ tự xuất hiện ít nhất nếu trùng tần xuất thì in theo thứ tự từ điển lớn nhất
```cpp
bool cmp(pair<char, int> a, pair<char, int> b){
    if(a.second != b.second) return a.second < b.second;
    else return a.first < b.first; 
}
void solve4(){
    string s;
    cin >> s;

    map<char, int> mp;
    for(auto x : s) mp[x]++;

    vector<pair<char, int>> v;
    for(auto it : mp) v.push_back(it);
    sort(v.begin(), v.end(), cmp);
    
    for(auto x : v) cout << x.first << " " << x.second << endl;
    cout << endl;

    cout << v[v.size() - 1].first << " " << v[v.size() - 1].second << endl;
    char res = v[0].first; 
    int cnt = v[0].second;
    for(int i = 1; i < s.size(); i++){
        if(cnt == v[i].second) res = v[i].first;
    } 
    cout << res << " " << cnt << endl;
}
```
### Kí tự xuất hiện ở cả 2 xâu và xuất hiện 1 trong 2 xâu
```cpp
void solve5(){
    string s; cin >> s;
    string t; cin >> t;
    set<char> se1, se2, se3;
    for(int i = 0; i < s.size(); i++){
        se1.insert(s[i]);
        se2.insert(s[i]);
    }
    for(int i = 0; i < t.size(); i++){
        if(se1.count(t[i]) != 0) se3.insert(t[i]);
        else se2.insert(t[i]);
    }
    for(auto x : se3) cout << x;
    cout << endl;
    for(auto x : se2) cout << x;
    cout << endl;
}
```
### Kí tự chỉ xuất hiện trong xâu 1 mà ko có trong xâu 2 và ngược lại
```cpp
void solve6(){
    string s1; cin >> s1;
    string s2; cin >> s2;
    set<char> se1, se2;
    for(auto x : s1) se1.insert(x);
    for(auto x : s2) se2.insert(x);
    for(auto x : se1){
        if(!se2.count(x)) cout << x;
    }
    cout << endl;
    for(auto x : se2){
        if(!se1.count(x)) cout << x;
    }
    cout << endl;
}
```
### Xâu đối xứng
```cpp
void solve7(){
    string s; cin >> s;
    string t = "";
    for(int i = s.size() - 1; i >= 0; i--){
        t += s[i];
    }
    if(t == s) cout << "YES\n";
    else cout << "NO\n";
}
```
### Kiểm tra kí tự 
```cpp
void solve8(){
    char c; cin >> c;

   //in ra chữ cái liền sau
    if(c == 'z') cout << 'a' << endl;
    else cout << (char)(c + 1) << endl;

   //in ra chữ liền sau nhưng luôn luôn là chữ thường
    if(c == 'z' || c == 'Z') cout << 'a' << endl;
    else if(c >= 'A' && c <= 'Z') cout << (char)(c + 33) << endl;
    else cout << (char)(c + 1);
}
```
### Kiểm tra xâu có đầy đủ 26 kí tự (Xâu Pangram)
```cpp
void solve9(){
    string s; cin >> s;
    set<char> se;

    for(int i = 0; i < s.size(); i++) s[i] = tolower(s[i]); 
    for(auto x : s) se.insert(x);

    if(se.size() == 26) cout << "YES\n";
    else cout << "NO\n";
}
```
### Đếm số lượng từ
```cpp 
void solve10(){
    string s; 
    getline(cin, s); 
    cin.ignore();
    stringstream ss(s);
    string w;
    int cnt = 0;
    while(ss >> w){
        cnt++;
    }
    cout << cnt << endl;
}
```
### Liệt kê các từ khác nhau trong xâu
```cpp
void solve11(){
    string s; 
    getline(cin, s);
    set<string> se;
    vector<string> v;
    map<string, int> mp;
    stringstream ss(s);
    string w;
    while(ss >> w){
        //C1
        if(!se.count(w)) v.push_back(w);
        se.insert(w);
        //C2
        mp[w]++;
        v.push_back(w);
        se.insert(w);
    }
    //C1
    for(auto it : se) cout << it << " "; cout << endl;
    for(auto x : v) cout << x << ' ';

    //C2
    for(auto x : se) cout << x << " "; cout << endl;
    for(auto x : v){
        if(mp[x] != 0){
            cout << x << " ";
            mp[x] = 0;
        }
    }
}
```
### Sắp xếp xâu theo thứ tự từ điển và độ dài xâu
```cpp
void solve12(){
    string s; 
    getline(cin, s);
    vector<string> v;
    stringstream ss(s);
    string w;
    while(ss >> w) v.push_back(w);

    sort(v.begin(), v.end());
    for(auto x : v) cout << x << " "; 
    cout << endl;
    sort(v.begin(), v.end(), [](string a, string b)-> bool {
        if(a.size() != b.size()) return a.size() < b.size();
        else return a < b;
    });
    for(auto x : v) cout << x << ' ';
}
```
### Sắp xếp xâu đối xứng khác nhau theo độ dài và theo thứ tự xuất hiện
```cpp
bool palindrome(string s){
    string t = s;
    reverse(t.begin(), t.end());
    return s == t;
}
void solve13(){
    string s; 
    getline(cin, s);
    vector<string> v;
    set<string> se;
    stringstream ss(s);
    string w;
    while(ss >> w){
        if(!se.count(w) && palindrome(w)){
            v.push_back(w);
            se.insert(w);
        }
    }
    stable_sort(v.begin(), v.end(), [](string a, string b)-> bool {
        return a.size() < b.size();
    });
    for(auto x : v) cout << x << ' ';
}
```
### Tần xuất các từ trong xâu
```cpp
void solve14(){
    string s; 
    getline(cin, s);
    map<string, int> mp;
    vector<string> v;
    stringstream ss(s);
    string w;
    while(ss >> w){
        mp[w]++;
        v.push_back(w);
    }
    for(auto x : mp) cout << x.first << " " << x.second << endl;
    cout << endl;
    for(auto x : v){
        if(mp[x] != 0){
            cout << x << ' ' << mp[x] << endl;
            mp[x] = 0;
        }
    }
}
```
### Từ xuất hiện nhiều nhất, ít nhất
```cpp
void solve15(){
    string s; 
    getline(cin, s);
    map<string, int> mp;
    vector<pair<string, int>> v;
    stringstream ss(s);
    string w;
    while(ss >> w) mp[w]++;
    for(auto x : mp) v.push_back(x);
    sort(v.begin(), v.end(), [](pair<string, int> a, pair<string, int> b)-> bool {
        if(a.second != b.second) return a.second < b.second;
        else return a.first < b.first;
    });
    
    cout << v[v.size() - 1].first << " " << v[v.size() - 1].second << endl;
    string res = v[0].first;
    int cnt = v[0].second;
    for(int i = 1; i < v.size(); i++){
        if(v[i].second == cnt) res = v[i].first;
    }
    cout << res << " " << cnt << endl;
}
```
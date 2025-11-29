# Greedy
- Tham lam: Tự build lên từ những kiến thức tự nhiên (constructive) -> Sắp xếp trước khi thực hiện
    - Ứng viên: Tập dữ liệu đề bài => Suy ra lời giải cho bài toán
    - Hàm lựa chọn: lựa chọn ứng viên tốt nhất
    - Hàm khả thi: quyết định xem ứng viên có được chọn hay không
    - Hàm mục tiêu: xác định xem giá trị của lời giải chưa hoàn chỉnh của từng bước là tốt nhất chưa
    - Hàm đánh giá: xác định xem lời giải đã được hoàn thành hay chưa
## Coin problem
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int n; cin >> n;
    // Sắp xếp trước
    int a[10] = {1000, 500, 200, 100, 50, 20, 10, 5, 2, 1};
    int res = 0;
    for(int i = 0; i < 10; i++){
        res += n / a[i];
        n %= a[i];
    }
    cout << res << endl;
}
```
## Scheduling
```cpp
// Xếp lịch diễn
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int n; cin >> n;
    pair<int, int> a[n];
    for(auto &x : a){
        cin >> x.first >> x.second;
    }
    sort(a, a + n, [](auto a, auto b){
        if(a.first != b.first) return a.second < b.second;
        else return a.first < b.first;
    });
    int cnt = 1, endTime = a[0].second;
    for(int i = 1; i < n; i++){
        if(a[i].first > endTime){
            cnt++;
            endTime = a[i].second;
        }   
    }
    cout << cnt << endl;
}
```
## Nối dây
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int t; cin >> t;
    while(t--){
        // Dùng Min heap
        int n; cin >> n;
        priority_queue<int, vector<int>, greater<int>> q;
        for(int i = 0; i < n; i++){
            int x; cin >> x;
            q.push(x);
        }
        ll res = 0;
        while(q.size() > 1){
            int x = q.top();
            q.pop();
            int y = q.top();
            q.pop();
            res += x + y;
            q.push(x + y);
        }
        cout << res << endl;

        // Dùng multiset
        int n; cin >> n;
        multiset<int> se;
        for(int i = 0; i < n; i++){
            int x; cin >> x;
            se.insert(x);
        }
        ll res = 0;
        while(se.size() > 1){
            auto i = *se.begin();
            se.erase(se.begin());
            auto j = *se.begin();
            se.erase(se.begin());
            res += i + j;
            se.insert(i + j);
        }
        cout << res << endl;
    }
}
```
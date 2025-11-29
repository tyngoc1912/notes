# Một chiều
## Prefix sum
Tính tổng từ l --> k : dp[r] - d[l - 1] / dp[r] nếu l = 0
```cpp
int n; cin >> n;
int a[n]; 
ll dp[n];
for(int i = 0; i < n; i++){
    cin >> a[i];
    if(i == 0) dp[i] = a[i];
    else dp[i] = dp[i - 1] + a[i];
}
int q; cin >> q;
while(q--){
    int l, r; 
    cin >> l >> r; 
    --l; --r; // nếu chỉ số l bắt đầu tính từ 1
    if(l == 0) cout << dp[r];
    else cout << dp[r] - dp[l - 1];
    }
```
## Difference array
- Update phần tử l->r lên k đơn vị : d[l] += k và d[r + 1] -= k
- Tính mảng cộng dồn của mảng hiệu -> quay lại mảng gốc ban đầu
```cpp
ll d[n + 5]; // tránh bị lỗi khi đó là chỉ số cuối cùng
for(int i = 0; i < n; i++){
    if(i == 0) d[i] = a[i];
    else d[i] = a[i - 1] - a[i]
}
int q; cin >> q;
while(q--){
    int l, r, k; 
    cin >> l >> r >> k; 
    d[l] += k;
    d[r + 1] -= k;
}
for(int i = 0; i < n; i++){
    if(i == 0) dp[i] = d[i];
    else dp[i] = dp[i - 1] + d[i];
    cout << dp[i] << " ";
}
```
## Sliding window
```cpp
ll sum = 0;
for(int i = 0; i < k; i++){
    sum += a[i];
}
ll res = sum, pos = 0;
for(int i = 1; i <= n - k; i++){
    sum = sum - a[i - 1] + a[i + k - 1];
    if(sum > res){ //nếu có dấu bằng là cập nhật dãy cuối cùng, còn ko có là dãy đầu 
        res = sum;
        pos = i;
    }
}
cout << res << endl;
for(int i = 0; i < k; i++) cout << a[pos + i] << " ";
```
## Two pointer
- In số phần tử ít hơn của mỗi phần tử ở dãy 1 so với dãy 2
    ```cpp
    int n, m; cin >> n >> m;
    int a[n], b[m];
    for(int &x : a) cin >> x;
    for(int &x : b) cin >> x;
    int i = 0, j = 0;
    while(i < n && j < m){
        if(a[i] < b[j]) i++;
        else{
            cout << i << " ";
            j++;
        }
    }
    while(j < m){
        cout << n << ' ';
        j++;
    }

    //C2
    int j = 0;
    for(int i = 0; i < m; i++){
        while(j < n && a[j] < b[i]) j++;
        cout << j << ' '; 
    }
    ```
- Tìm số cặp của các phần tử giống nhau ở 2 dãy
    ```cpp
    int n, m; cin >> n >> m;
    int a[n], b[m];
    for(int &x : a) cin >> x;
    for(int &x : b) cin >> x;
    int i = 0, j = 0;
    ll cnt = 0;
    while(i < n && j < m){
        if(a[i] == b[j]){
            int x = a[i], d1 = 0, d2 = 0;
            while(a[i] == x){
                d1++;
                i++;
            }
            while(b[j] == x){
                d2++;
                j++;
            }
            cnt += 1ll * d1 * d2;
        }
        else if(a[i] < b[j]) i++;
        else j++;
    }
    cout << cnt << endl;
    ```
- In ra vị trí 2 phần tử có tổng bằng k
    ```cpp
    int n, k; cin >> n >> k;
    int a[n];
    for(int &x : a) cin >> x;
    int i = 0, j = n - 1;
    while(i <= j){
        ll sum = a[i] + a[j];
        if(sum == k){
            cout << i + 1 << " " << j + 1 << endl;
            return 0;
        }
        else if(sum < k) i++;
        else j--;
    }
    cout << "IMPOSSIBLE\n";
    ```
- Đêm các cặp phần tử có tổng bằng k
    ```cpp
    int n, k; cin >> n >> k;
    int a[n];
    for(int &x : a) cin >> x;
    int i = 0, j = n - 1;
    ll cnt = 0;
    while(i <= j){
        ll sum = a[i] + a[j];
        if(sum == k){
            int x1 = a[i], x2 = a[j], d1 = 0, d2 = 0;
            while(a[i] == x1){
                d1++; i++;
            }
            while(a[j] == x2){
                d2++; j--;
            }
            if(x1 == x2) cnt += 1ll * d1 * (d1 - 1) / 2;
            else cnt += 1ll * d1 * d2;
        }
        else if(sum < k) i++;
        else j--;
    }
    cout << cnt << endl;
    ```
- Đêm 3 phần tử có tổng bằng k
    ```cpp
    int n, k; cin >> n >> s;
    int a[n];
    for(int &x : a) cin >> x;
    ll cnt = 0;
    for(int i = 0; i < n; i++){
        int l = i + 1, r = n - 1;
        int k = s - a[i];
        while(l < r){
            ll sum = a[l] + a[r];
            if(sum == k){
                int x1 = a[l], x2 = a[r], d1 = 0, d2 = 0; 
                while(a[l] == x1){
                    d1++; l++;
                }
                while(a[r] == x2){
                    d2++; r--;
                }
                if(x1 == x2) cnt += 1ll * d1 * (d1 - 1) / 2;
                else cnt += 1ll * d1 * d2;
            }
            else if(sum < k) l++;
            else r--;
        }
    }
    cout << cnt << endl;
    ```
# Hai chiều
## Kỹ thuật duyệt các ô liền kề
- Duyệt ô liền kề -> dùng 2 mảng hoặc 1 pair để lưu lượng thay đổi khi di chuyển sang các ô ở cột và hàng khác nhau
  - move[4] -> duyệt 4 ô xung quanh (chung cạnh)
  ```cpp
  int dx[4] = {-1, 0, 0, 1};
  int dy[4] = {0, -1, 1, 0};
  pair<int, int> move4[4] = {{-1, 0}, {0, -1}, {1, 0}, {0, 1}};
  ```
  - move[8] -> duyệt 8 ô xung quanh (chung đỉnh)
  ```cpp
  int dx[8] = {-1, -1, -1, 0, 0, 1, 1, 1};
  int dy[8] = {-1, 0, 1, -1, 1, -1, 0, 1};
  pair<int, int> move8[8] = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
  ```
  - knightmove[8] -> duyệt 8 ô là nước đi của con mã
  ```cpp
  int dx[8] = {-2, -2, -1, -1, +1, +1, +2, +2};
  int dy[8] = {-1, +1, -2, +2, -2, +2, -1, +1};
  pair<int, int> knigntMove[8] = {{-2, -1}, {-2, 1}, {-1, -2}, {-1, 2}, {1, -2}, {1, 2}, {2, -1}, {2, 1}};
  ```
- Ứng dụng :
    ```cpp
    for(int i = 1; i < n; i++){
        for(int j = 1; j < n; j++){
            ll sum = a[i][j];
            for(int k = 0; k < 8; k++){
                int i1 = i + dx[k];
                int j1 = j + dy[k];
                sum += a[i1][j1];
            }
            res = max(res, sum); 
        }
    }
    cout << res << endl;
    ```
## Prefix sum
```cpp
pre[n + 5][n + 5];
for(int i = 1; i <= n; i++){
    for(int j = 1; j <= m; j++){
        pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + a[i][j];
    }
}    
```
## Kỹ thuật loang
```cpp
//Loang
void spread(int i, int j){
    a[i][j] = 0; //đánh dấu đã thăm
    for(int k = 0; k < 4; k++){
        int i1 = i + dx[k];
        int j1 = j + dy[k];
        if(i1 >= 0 && i1 < n && j1 >= 0 && j1 < m && a[i1][j1] == 1){
            spread(i1, j1);
        }
    }
}
int main(){
    //Count island => Kĩ thuật Loang
    int cnt = 0;
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(a[i][j] == 1){
                cnt++;
                spread(i, j);
            }
        }
    }
    cout << cnt << endl;
}
```
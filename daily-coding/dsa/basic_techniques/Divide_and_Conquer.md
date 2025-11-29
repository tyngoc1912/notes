# Divide and Conquer
## Xâu fibo
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll f[100];
char fiboFind(ll n, ll k){
    if(n == 1) return 'A';
    if(n == 2) return 'B';
    // xâu thứ n - 2 vị trí k
    if(k <= f[n - 2]) return fiboFind(n - 2, k);
    // xâu thứ n - 1 vị trí k - f[n - 1]
    else return fiboFind(n - 1, k - f[n - 2]);
}
int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    // So sánh chỉ số k cần tìm thuộc xâu fibo thứ n - 2 hoặc n - 1 rồi gọi đệ quy tới
    f[0] = 0;
    f[1] = 1;
    for(int i = 2; i <= 92; i++) f[i] = f[i - 1] + f[i - 2];
    ll n, k; cin >> n >> k;
    cout << fiboFind(n, k) << endl;
}
```
## Số Fibonacci thứ N
- N rất lớn không dùng QHD được vì n = 10^10
- Lũy thừa nhị phân ma trận ma trận 
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int mod = 1000000007;
struct mtx{
    ll a[2][2];
    friend mtx operator * (mtx x, mtx y){
        mtx res;
        for(int i = 0; i < 2; i++){
            for(int j = 0; j < 2; j++){
                res.a[i][j] = 0;
                for(int k = 0; k < 2; k++){
                    res.a[i][j] += (x.a[i][k] * y.a[k][j]);
                    res.a[i][j] %= mod;
                }
            }
        }
        return res;
    }
};
mtx binPow(mtx x, ll n){
    // Đệ quy
    if(n == 1) return x;
    mtx X = binPow(x, n / 2);
    if(n % 2 == 1) return X * X * x;
    else return X * X;

    // Ko đệ quy  
    // ma trận đơn vị
    mtx res;
    res.a[0][0] = 1;
    res.a[0][1] = 0;
    res.a[1][0] = 0;
    res.a[1][1] = 1;
    while(n){
        if(n % 2 == 1) res = res * x;
        x = x * x;
        n /= 2;
    }
    return res;
}

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    mtx x;
    x.a[0][0] = 1;
    x.a[0][1] = 1;
    x.a[1][0] = 1;
    x.a[1][1] = 0;
    ll n; cin >> n;
    mtx res = binPow(x, n);
    cout << res.a[0][1] << endl;
}
```
## Đếm dãy số
- Kỹ thuật star and bar => tính lũy thừa nhị phân với 2^(n-1)
- Không dùng sinh với quay lui được vì n = 10^12
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll binpow(ll a, ll b){
    if(b == 0) return 1;
    ll X = binpow(a, b / 2);
    if(b % 2 == 0) return X * X;
    else return a * X * X;
}

int main(){
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); 

    int n; cin >> n;
    cout << binpow(2, n - 1) << endl;
}
```
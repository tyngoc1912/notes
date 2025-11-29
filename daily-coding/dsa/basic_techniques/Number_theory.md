# Number theory  
## Kiểm tra số nguyên tố
```cpp 
bool prime(ll n){
	if(n < 2) return false;
	for(int i = 2; i <= sqrt(n); i++){
		if(n % i == 0) return false;
	}
	return true;
}
```
## Phân tích thừa số nguyên tố
```cpp
ll factorize(ll n){
    for(int i = 2; i <= sqrt(n); i++){
	    //ước đầu tiên khác 1 mà n chia hết => số nguyên tố nhỏ nhất
        if(n % i == 0) return i;
    }
}
// in theo format 
void factorize2(ll n){
    if(n == 1){
        cout << 0 << " " << 1 << endl;
        return;
    }
    else{
        int total = 1;
        for(int i = 2; i <= sqrt(n); i++){
            int cnt = 0;
            while(n % i == 0){
                cnt++;
                n /= i;
            }
            cout << i << "^" << cnt;
            if(n > 1) cout << " * ";
            total *= cnt + 1;
        }
        if(n > 1) cout << n << "^1";
        cout << " " << total << endl;
    }
}

// in theo format
void factorize3(ll n){
    for(int i = 2; i <= sqrt(n); i++){
        if(n % i == 0){
            int cnt = 0;
            while(n % i == 0){
                cnt++;
                n /= i;
            }
            if(cnt > 1) cout << i << "^" << cnt << " ";
            else cout << i << " ";
        }
    }
    if(n > 1){
        cout << n << endl;
    }
}
```
## Sàng số nguyên tố 
- Kiểm tra nhiều lần trong 1 khoảng n<=10^7
    ```cpp
    //Để sàng đc <= n số -> tạo mảng cỡ n + 1 phần tử
    const int k = 100000; 
    bool nt[k + 1];
    void sang(){
        //cho tất cả các giá trị bằng true
        for(int i = 0; i <= k; i++){
            nt[i] = true;
        }
        //cho số 0 và 1 false
        nt[0] = nt[1] = false;

        //kt số nào là nguyên tố
        for(int i = 2; i <= sqrt(k); i++){
            if(nt[i]){
                //loại tất cả các bội < n của nó
                for(int j = i * i; j <= k; j += i){ 
                // j phải bắt đầu từ bình phương của số nguyên tố sau đó tăng bước nhảy lên i lần
                    nt[j] = false;
                }
            }
        }
    }
    //code gọn hơn
    const int MAXN = 1e7 + 9;
    bool prime[MAXN];
    void sieve(){
        memset(prime, true, sizeof(prime));
        prime[0] = prime[1] = false;
        for(int i = 2; i <= sqrt(MAXN); i++){
            if(prime[i]){
                for(int j = i * i; j <= MAXN; j += i){
                    prime[j] = false;
                }
            }
        }
    }
    ```
- Ứng dụng sàng số nguyên tố :
    - Sàng số nguyên tố trong đoạn [a,b]
    ```cpp  
    void sang(ll l, ll r){
        int p[r - l + 1];       
        for(ll i = 0; i <= r; i++) p[i] = 1;
        for(ll i = 2; i <= sqrt(r); i++){
            // bội nhỏ nhất >= L của i : L + i - 1 / i * i
            for(ll j = max(i * i, (l + i - 1) / i * i); j <= r; j += i){
                // lưu và kiểm tra chỉ số j - l => dịch chuyển chỉ số của phần tử ra
                p[j - l] = 0;
                
            }
        }
        //duyệt
        for(ll i = max(2ll, l); i <= r; i++){
            if(p[i - l]) cout << i << " ";
        }
    }
    ```
    - Kiểm tra số đẹp trong đoạn [a,b]
    ```cpp
    const int MAXN = 1e7 + 1;
    int nt[MAXN], dep[MAXN];
    void sang(){
        memset(nt, 1, sizeof(nt));
        nt[0] = nt[1] = 0;
        for(int i = 2; i <= sqrt(MAXN); i++){
            if(nt[i]){
                //i là số nguyên tố
                //số đẹp là bội của nguyên tố và chia hết cho bình phương nguyên tố đó
                int tmp = i * i;
                //k là bội của tmp
                for(int k = 1; k * tmp <= MAXN; k++){
                    dep[k * tmp] = 1;
                }
                for(int j = i * i; j <= MAXN; j += i){
                    nt[j] = 0;
                }
            }
        }
    }
    void check(int a, int b){
        sang();
        for(int i = a; i <= b; i++){
            if(dep[i]) cout << i << ' ';
        }
    }
    ```
    - Sinh ra các số hoàn hảo nhanh
    ```cpp
    // nếu p là số nt thì 2^p - 1 cũng là số nt => (2^p-1)*(2^(p-1)) là số hh
    bool nt(int n){
        for(int i = 2; i <= sqrt(n); i++){
            if(n % i == 0) return false;
        }
        return n > 1;
    }
    ll hh[10];
    int cnt = 0;
    void perfect(){
        for(int p = 2; p <= 32; p++){
            if(nt(p)){
                int tmp = (int)pow(2, p) - 1;
                if(nt(tmp)){
                    hh[cnt] = 1ll * (int)pow(2, p - 1) * tmp;
                    ++cnt;
                }
            }
        }
    }
    ```
    - Sàng phi hàm Euler
    ```cpp
    int phi[1000001];
    void init(){
        for(int i = 1; i <= 1000000; i++) phi[i] = i;
        for(int i = 2; i <= 1000000; i++){
            if(phi[i] == i){ //i là số nt
                phi[i] = i - 1; // các số nt <= n
                for(int j = i * 2; j <= 1000000; j += i){
                    phi[j] = phi[j] - phi[j] / i;
                }
            }
        }
    }
    ```
    - Sàng ước số nguyên tố nhỏ nhất 
    ```cpp
    int prime[1000001];
    void sang(){
        for(int i = 1; i <= 1000000; i++) prime[i] = i; // coi mỗi số là 1 ước SNT nhỏ nhất của chính nó
        for(int i = 2; i <= sqrt(1000000); i++){
            if(prime[i] == i){ // i là SNT
                for(int j = i * i; j <= 1000000; j += i){ // duyệt bội của i
                    if(prime[j] == j){//tránh bị trùng vd như 12 = 2^2 * 3 -> cập nhập số 2 ko cập nhập só 3
                        prime[j] = i;// ước NT nhỏ nhất của j là i
                    }
                }
            }
        }
    }
    ```

## Kiểm tra số hoàn hảo 
- Trong miền long long có 8 số hoàn hảo 
```cpp
bool perfect(ll n){
    ll s = 1;
    for (int i = 2; i <= sqrt(n); i++){
        if (n % i == 0){
            s += i;
            if (i != n / i) s += n / i;
        }
    }
    return s == n;
}
```
## Kiểm tra số đối xứng 
```cpp
bool palindrome(ll n){
	if(n < 10) return true;
	int temp = 0, res = n;
	while(n != 0){
		temp = temp * 10 + n % 10;
		n /= 10;
	}
	if(temp == res) return true;
	else return false;
}
```
## Kiểm tra số chính phương 
```cpp
bool square(ll n){
	ll c = sqrt(n);
	if(n == c * c) return true;
	else return false;
}
```
## Số Fibonacci 
- Chỉ có 93 số đầu là lưu được với giá trị long long
- Đệ quy :
    ```cpp
    ll fibo(int n){ //0 1 1 2 3 5 8 ...
        if(n == 1 || n == 0) return n;
        else return fibo(n - 1) + fibo(n - 2); 
    }
    // Cách khác
    ll fi(int n){ // 1 1 2 3 5 8 13 ...
        if(n == 1) return 0;
        if(n == 2) return 1;
        return fi(n - 1) + fi(n - 2);
    }
    ```
- Khởi tạo mảng 93 số Fibo :
    ```cpp
    ll f[100]; 
    void fibo(){
        f[0] = 0;
        f[1] = 1;
        for(int i = 2; i <= 92; i++){
            f[i] = f[i - 1] + f[i - 2];
        }
        //for(int i = 0; i <= 92; i++) cout << f[i]<< ' ';
    }
    ```
- Kiểm tra số fibonacci -> sinh 93 số đầu -> kt số đã cho có trong mảng fibo[n] không
    ```cpp
    bool checkFibo1(ll n){
        for(int i = 0; i <= 92; i++){
            if(n == f[i]) return true;
        }
        return false;
    }
    ```
- Kiểm tra số Fibonacci không dùng mảng
    ```cpp
    bool checkFibo2(ll n){
        if(n == 1 || n == 0) return true;
        ll f1 = 0, f2 = 1;
        for(int i = 0; i <= 92; i++){
            ll fn = f1 + f2;
            if(fn == n) return true;
            f1 = f2;
            f2 = fn;
        }
        return false;
    }
    ```
## UCLN và BCNN
- UCLN Đệ quy :
    ```cpp
    ll gcd(ll a, ll b){
        if(b == 0) return a;
        else return gcd(b, a % b);
    }
    ```
- UCLN Không đệ quy :
    ```cpp
    ll gcd2(ll a, ll b){
        ll r;
        while(b != 0){
            r = a % b;
            a = b;
            b = r;
        }
        return a;
    }
    ```
- BCNN :
    ```cpp        
    ll lcm(ll a, ll b){
        return a / gcd(a, b) * b; //tránh bị tràn số
    }
    ```
- Hàm có sẵn trong C++ :
    - `__gcd(a, b);`
    - `__lcm(a, b);`

- Ứng dung: 
    - GCD và LCM của các phần tử trong mảng
    ```cpp
    void GCDandLCM(ll a[], int n){
        ll res = 0, ans = 1;
        for(int i = 0; i < n; i++){
            res = gcd(res, a[i]);
            ans = lcm(ans, a[i]);
        }
        cout << res << endl << ans << endl;
    }
    ```
    - Tính số nguyên tố cùng nhau -> 2 số có ucln là 1
    ```cpp
    bool co_prime(int a, int b){ 
        if(gcd(a, b) == 1) return true;
        else return false;
    }
    ```
## Phi hàm Euler:
```cpp 
ll phi(ll n){
	ll res = n;
	for(int i = 2; i <= sqrt(n); i++){
		if(n % i == 0){ // i là thừa số nguyên tố của n
			res -= res / i; //(n - n/p)
		    while(n % i == 0) n /= i; // loại bỏ những thừa số nguyên tố giống nó
		}
	}
	if(n > 1) res -= res / n; //thừa số nguyên tố cuối cùng
	return res;
}
```
## Tổ hợp
- Đệ quy :
    ```cpp
    ll C(int n, int k){ // nCk CT truy hồi: C(n, k)=C(n - 1, k - 1) + C(n - 1, k)
        if(k == 0 || k == n) return 1;
        return C(n - 1, k - 1) + C(n - 1, k);
    }
    ```
- Ứng dụng CT: nCk = nC(n - k) = n*(n-1)*(n-2)*...*(n-k+1)/1*2*3*...*k
    ```cpp
    ll toHop(ll n, ll k){ 
        ll res = 1;
        k = min(k, n - k);
        for(int i = 1; i <= k; i++){
            res *= (n - i + 1);
            res /= i;
        }
        /*
        ll res = 1;
        k = min(k, n - k);
        for(int i = 0; i < k; i++){
            res *= (n - i);
            res /= (i + 1);
        }
        */
        return res;
    }
    ```
- Dùng Quy hoạch động -> số lớn được
    ```cpp
    ll toHop2(ll n, ll k){
        //C[i][j] tổ hợp chập j của i 
        ll C[n + 1][k + 1];
        for(int i = 0; i <= n; i++){//hàng
            for(int j = 0; j <= i; j++){//cột
                if(j == 0 || j == i) C[i][j] = 1;
                else C[i][j] = C[i - 1][j] + C[i - 1][j - 1];
            }
        }
    }
    ```
## Lũy thừa nhị phân: 
- Không đệ quy :
    ```cpp
    ll binPow(ll a, ll b){
        ll res = 1;
        while(b){
            if(b % 2 == 1) res *= a;
            //lũy thừa của bit xét lên
            a *= a;
            //loại bỏ bit đó đi
            b /= 2;
        }
        return res; //Tính lũy thừa thì dùng hàm này ko cần dùng hàm khác
    }
    // chia dư
    ll powMOD(ll a, ll b){
        ll res = 1;
        while(b){
            if(b % 2 == 1){
                res = ((res % MOD) * (a % MOD)) % MOD;
            }
            a = ((a % MOD) * (a % MOD)) % MOD;
            b /= 2;
        }
        return res; 
    }
    ```
- Đệ quy :
    ```cpp
    ll du(ll a, ll b){
        return ((a % MOD) * (b % MOD)) % MOD;
    }

    ll binpow(ll a, ll b){ // Lũy thừa nhị phân
        if(b == 0) return 1;
        ll x = binpow(a, b / 2);
        if(b % 2 == 0) return du(x, x); // a^b/2 * a^b/2
        else{
            ll res = du(x, x);
            return du(res, a); // a^b/2 * a^b/2 * a;
        }
    }
    ```
## Giai thừa chia dư
- Không đệ quy :
    ```cpp
    ll gtMOD(int n){
        ll res = 1;
        for(int i = 1; i <= n; i++){
            res = ((res % MOD) * (i % MOD)) % MOD;
        }
        return res;
    }
    ```
- Đệ quy :
    ```cpp
    ll gt(int n){
        if(n == 0) return 1;
        else return ((n % MOD) * (gt(n - 1) % MOD)) % MOD;
    }
    ```
## Công thức Legendre
```cpp
// Ứng dụng 
// Tính số ước d(n) = (e1 + 1)(e2 + 1)...(ek + 1) ek là bậc thừa số nguyên tố
// Tính tích của các ước của n p(n) = n ^(d(n) / 2) 
int degree(int n, int k){
    int res = 0;
    for(int i = k; i <= n; i *= k){
        res += n / i;
    }
    return res;
}
```
## Đồng dư (Modular Arithmetic)
- Công thức cơ bản :
    - (*a* − *b*) % *c* =[(*a* % *c*)−(*b* % *c*)+*c*] % *c*
    - (*a* + *b*) % *c* =[(*a* % *c*)+(*b* % *c*)] % *c*.
    - (*a* × *b*) % *c* =[(*a* % *c*)×(*b* % *c*)] % *c*.
    - *(a^m)* % *c* =[(*a* % *c*)^m] % *c*.
- Công thức nâng cao :
    - *(a / b) % c = [(a % c) x (b^-1 % c)] % c*
    - **b^-1** : nghịch đảo modulo → giải thuật Euclid mở rộng + nhỏ Fermat
    - Cài đặt Euclid :
    ```cpp
    int extended_gcd(int a, int b, int& x, int& y){
        if(b == 0){
            x = 1;
            y = 0;
            return a;
        }
        int x1, y1;
        int d = extended_gcd(b, a % b, x1, y1);
        //công thức
        x = y1;
        y = x1 - y1 * (a / b);
        return d;
    }
    int euclid_inverse(int a, int m){
        int x, y;
        int d = extended_gcd(a, m, x, y);
        if(d != 1){
            return -1;
        }
        else{
            //đảm bảo cho nghịch đảo module ko bị âm
            x = (x % m + m) % m;
            return x;
        }
    }

    // Cài đặt khác
    int x, y, ucln;
    void extended_gcd(int a, int b){
        if(b == 0){
            x = 1;
            y = 0;
            ucln = a;
        }
        else{
            extended_gcd(b, a % b);
            // áp dụng công thức
            int x1 = x;
            x = y; //y1
            y = x1 - a / b * y; //y1
        }
    }
    // O(log(max(a, m)))
    // (A / B) % M = (A * B^-1) % M => B^-1 nghịch đảo modulo A dưới Modulo M
    // ĐK nghịch đảo modulo gcd(A, M) = 1 và B thuộc [1, M-1]
    int modular_inverse(int a, int m){
        extended_gcd(a, m);
        return (x % m + m) % m;
    }
    ```
    - Cài đặt Fermat :
    ```cpp 
    ll powMod(ll a, ll b, ll c){
        ll res = 1;
        while(b){
            if(b % 2){
                res *= a; 
                res %= c;
            }
            a *= a;
            a %= c;
            b /= 2;
        }
        return res;
    }
    ll fermat_inverse(ll a, ll p){
        //a^-1 - a^(p - 2) % p = 1
        return powMod(a, p - 2, p);
    }
    ```
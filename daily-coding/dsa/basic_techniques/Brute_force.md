# Brute force 
Vét cạn -> xét mọi cấu hình, trường hợp cụ thế => Solution
## Generation
- Bài toán liệt kê : Xác định cấu hình đầu tiên và cấu hình cuối cùng + Thuật toán sinh
- Mã giả :
   ```cpp
    <Khởi tạo cấu hình đầu tiên>
    while(<Chưa phải cấu hình cuối cùng>){
    		<Xử lý cấu hình hiện tại>
    		<Sinh ra cấu hình kế tiếp>
    }
    ```
### Sinh nhị phân có độ dài n
- Bản chất sinh ra cấu hình nhị phân kế tiếp : cộng thêm 1 vào cấu hình hiện tại → có thể hiểu là dịch bit sang trái liên tục gặp bit 1 đổi thành 0 + gặp bit 0 lần đầu tiên thì dừng và viết lại các phần trước xuống lại nếu còn
- Dùng mảng → lưu dãy bit của cấu hình số đang xét và thường số phần tử của mảng đó ko quá 100 (10 < n < 15 bit → a[100])
- Liệt kê các tập con có n phần tử => liệt kê xâu nhị phân
- Chia mảng/ tập hợp thành 2 phần → xâu nhị phân ⇒ gom bit 1 thành 1 tập con và bit 0 thành 1 tập con (mỗi tập là 1 xâu nhị phân)

```cpp
//Sinh tất cả các xâu nhị phân có độ dài n
int n, a[100], check; // n độ dài bit nhị phân, mảng chứa các bit nhị phân, check có phải cấu hình cuối chưa

void khoiTao(){
    //Sinh ra xâu nhị phân đầu tiên toàn bộ là 0
    for(int i = 1; i <= n; i++){ 
        a[i] = 0;
    }
}

void sinh(){
    //bắt đầu từ bit cuối cùng n dịch qua
    int i = n;
    while(i >= 1 && a[i] == 1){ //chừng nào còn thỏa điều kiện là các bit = 1 và chưa hết xâu nhị phân
        a[i--] = 0; //Đổi các bit đó sang 0
    }
    if(i == 0){ //cấu hình cuối toàn là 1 thì chạy hết xâu
        check = 0; // dừng chương trình
    }
    else a[i] = 1; // bật bit 0 lần đầu tiên lên là 1
}

int main(){
    cin >> n;
    check = 1;
    khoiTao();//Tạo cấu hình đầu tiên
    while(check){
        for(int i = 1; i <= n; i++) cout << a[i]; //in cấu hình đầu tiên
        cout << endl; // sau mỗi cấu hình thì xuống dòng
        sinh(); // sinh ra các cấu hình tiếp theo 
    }
}
```
### Sinh tổ hợp chập k của n phần tử
- Cấu hình đầu tiên là k số nhỏ nhất trong n số **VD n = 5, k = 3 → 123**
- Cấu hình cuối cùng là k số lớn nhất trong n số **VD n = 5, k = 3 → 345 có công thức : n - k + i**
- Sinh ra tổ hợp tiếp theo phải lớn hơn tổ hợp hiện tại nhưng phải bé nhất theo thứ tự từ điển nên khi gặp bit đầu tiên chưa đạt max **n - k + i** thì tăng bit đó lên 1 đơn vị và xét kể từ bit đó về sau mỗi bit j tương ứng sẽ +1 đơn vị lên

```cpp
//Sinh tổ hợp chập k của n pt
int n, k, a[100], check;
void khoiTao(){
    for(int i = 1; i <= k; i++) a[i] = i; //k bit bé nhất
}

void sinh(){
    int i = k;
    while(i >= 1 && a[i] == n - k + i){
        i--; // Nếu mỗi bit đạt giá trị max r thì dịch qua bit tiếp theo
    }
    if(i == 0) check = 0; //Đạt cấu hình cuối
    else{
        a[i]++; //bit đầu tiên chưa đạt max tăng 1 đv
        for(int j = i + 1; j <= k; j++){ //xét từ bit thứ i+1 đến bit thứ k
            a[j] = a[j - 1] + 1; //giá trị từ sau pt i+1 tăng 1 đv
        }
    }
}

int main(){
    cin >> n >> k;
    check = 1;
    khoiTao();
    while(check){
        for(int i = 1; i <= k; i++) cout << a[i];
        cout << endl;
        sinh();
    }
}
```
### Sinh hoán vị
- Số có n chữ số sinh ra 1 số lớn hơn số đó và là nhỏ nhất có thể
- Điều kiện là **ít nhất trong 1 số có n chữ số** có **1 cặp số sao cho số đứng trước < số đứng sau** ⇒ hoán vị 2 chữ số
- Xét từ cuối về → tìm 1 số a[i] < a[i + 1] ⇒ xét đoạn [i+1, n] tìm đc a[j]_min > a[i] ⇒ sort đoạn đó tạo dãy tăng dần
- Sinh hoán vị có ứng dụng rất nhiều → có 2 hàm có sẵn :
    - `next_permuation()` : mảng, vector, string ⇒ true hoặc false
        ```cpp
        // sinh ra tất cả cấu hình bằng hàm có sẵn
        do{
            cout << s << endl;
        }while(next_permutation(s.begin(), s.end())
        ``` 
    - `prev_permutation()` : sinh ra hoán vị ngược từ cuối về đầu cú pháp giống `next_permutation()` **VD: 54321 → 12345**

```cpp
//Sinh hoán vị
int n, k, a[100], check;

void khoiTao(){
    //Khởi tạo dãy đầu tiên
    for(int i = 1; i <= n; i++) a[i] = i;
}

void sinh(){
    int i = n - 1; //sau n ko có pt nên phải xét từ n - 1
    while(i >= 1 && a[i] > a[i + 1]){ //Xét pt trc > pt sau
        i--;
    }
    if(i == 0) check = 0; //cấu hình cuối
    else{
        // đi tìm a[j] trong i + 1 -> n, > a[i] và nhỏ nhất có thể
        // trong đoạn i + 1 -> n tạo thành dãy giảm dần
        int j = n;
        while(a[i] > a[j]) j--; //xét từ pt cuối trở lên vì là dãy giảm dần nên pt đầu tiên lớn hơn a[i] là pt nhỏ nhất
        swap(a[i], a[j]); //đổi chỗ 2 pt cho nhau
        reverse(a + i + 1, a + n + 1); //Đảo ngược dãy trên đoạn từ i + 1 -> n => dãu tăng dần
    }
}

int main(){
    cin >> n;
    check = 1;
    khoiTao();
    while(check){
        for(int i = 1; i <= n; i++){
            cout << a[i];
        }
        cout << endl;
        sinh();
    }
}
```    
### Sinh phân hoạch
- Biểu diễn **n = tổng các số tự nhiên nhỏ hơn hoặc bằng n** → sinh ra các cách biểu diễn **(sinh hoặc quay lui)** , đếm các cách biểu diễn **(quy hoạch động)**
- Cấu hình đầu là n, cấu hình cuối là tổng n số 1 với nhau
- Xét từ bit cuối nếu là 1 thì dịch sang tiếp đến khi nào gặp số i có thể giảm đc 1 đơn vị → xem trước số i còn bao nhiêu số thì gán số lượng số còn lại cho biến dem → lấy tổng các số còn lại đó + (i - 1) = temp → lấy n - temp = kq thì lấy kq % (i-1) = q => biểu diễn số lượng bit kết quả q lần

```cpp
//Sinh phân hoạch 
int n, a[100], check, dem;//đếm số hạng của mỗi lần sinh ra cấu hình mới

void khoiTao(){
    dem = 1;//số lượng pt là 1
    a[1] = n;
}

void sinh(){
    int i = dem; //bắt đầu sinh từ số hạng cuối
    while(i >= 1 && a[i] == 1){ // nếu các bit là số 1 thì bỏ qua đến số có thể giảm 1 dv đc
        i--;
    }
    if(i == 0) check = 0; //cấu hình cuối full số hạng 1
    else{
        a[i]--;
        int thieu = dem - i + 1; // vì dem = số lần bỏ qua các số 1 mà trc đó giảm thêm 1 đv của a[i] nên phải +1
        dem = i; //cập nhật lại số phần tử hiện tại
        int q = thieu / a[i]; // tìm xem thiếu gấp bao nhiêu lần a[i] để biểu diễn q lần số a[i] ở vế sau
        int r = thieu % a[i]; // tương tự như trên nhưng mà là bit còn sót lại khi ko còn biểu diễn bằng a[i] đc nữa
        if(q != 0){
            for(int j = 1; j <= q; j++){ //biểu diễn q lần a[i]
                dem++; 
                a[dem] = a[i];
            }
        }
        if(r != 0){
            dem++; 
            a[dem] = r;
        }
    }
}

int main(){
    cin >> n;
    check = 1;
    khoiTao();
    while(check){
        for(int i = 1; i <= dem; i++) cout << a[i] << ' ';
        cout << endl;
        sinh();
    }
}
```
### Mã Gray
#### Chuyển từ mã nhị phân sang mã Gray
- Thuật toán 
    - bit đầu tiên của mã Gray và mã nhị phân là giống nhau
    - các bit còn lại ở vị trí i của mã Gray có được bằng cách XOR 2 bit thứ i và i - 1 của xâu nhị phân
```cpp
void convert(string s){
    cout << s[0];
    for(int i = 1; i < s.size(); i++){
        //XOR bit s[i], s[i - 1]
        if(s[i] == s[i - 1]) cout << 0;
        else cout << 1;
    }
    cout << endl;
}
```
#### Chuyển từ mã Gray sang mã nhị phân
- Thuận toán
    - bit đầu tiên của mã Gray và mã nhị phân là giống nhau
    - các bit thứ i còn lại có được bằng cách:
        - nếu bit thứ i của mã Gray là 0 thì bit thứ i của mã nhị phân là copy của bit thứ i - 1 của mã nhị phân
        - Ngược lại, bit thứ i của mã nhị phân là lật ngược của bit thứ i của bit thứ i - 1 của mã nhị phân
```cpp
void convert(string s){
    string res = "";
    res += s[0];
    for(int i = 1; i < s.size(); i++){
        if(s[i] == '1'){
            if(res[i - 1] == '0') res += "1";
            else res += "0";
        }
        else res += res[i - 1];
    }
    cout << res << endl;
}
```
## Backtracking
- Quay lui → đệ quy + bài toán liệt kê mà ko biết cấu hình cuối cùng hoặc đầu tiên + ko biết thuật toán sinh ra cấu hình kế tiếp ⇒ thử mọi TH => Solution 
- Tính số lần gọi đệ quy bằng chia thành các nhánh của số các giá trị mà mỗi thành phần có thể nhận đc
- Chú ý mỗi nhánh nó sẽ thực hiện đi sâu nhất có thể khi mà ko được thì nó mới chẻ sang nhánh tiếp theo rồi tiếp tục dò ⇒ không thực hiện đồng thời 2 nhánh
- Mã giả :
    ```cpp
    X = {x1, x2, x3,... xn} -> cần xây dựng X gồm các pt
    Try(int i){
    	//i gán các giá trị cho thành phần xây dựng nên X -> thử cho đến khi thỏa
    	for(j = <khả năng 1>; j <= <khả năng m>; j++){
            x[i] = j;
            if(i == n){ //xây dựng đc đên xn
                <In kq> //cấu hình cuối
            }
            else Try(i + 1)
    	}
    }
    ```
- Mã giả tổng quát
    ```cpp
    X = {x1, x2, x3,... xn} -> cần xây dựng X gồm các pt
    Try(int i){
    	//i gán các giá trị cho thành phần xây dựng nên X -> thử cho đến khi thỏa
    	for(j = <khả năng 1>; j <= <khả năng m>; j++){
            if(<có thể gán j cho X[i]>){
                x[i] = j;
                <Ghi nhận j đã đc sử dụng>
                if(i == n){ //xây dựng đc đên xn
                    <In kq> //cấu hình cuối
                }
                else Try(i + 1);
                <Backtrack>
                <bỏ ghi nhận đã sử dụng cho các nhánh khác thử tiếp giá trị j>
            }
    	}
    }
    ```
### Sinh xâu nhị phân có độ dài n
```cpp
//Sinh xâu nhị phân có n bit -> Backtracking
int n, X[100];

void Try(int i){
    //đi xây dựng bit thứ i cho xâu nhị phân
    for(int j = 0; j <= 1; j++){ //Xâu nhị phân chỉ có 2 khả năng là 0 vs 1
        X[i] = j; //bit thứ i = khả năng j
        if(i == n){ //bit cuối cùng
            for(int i = 1; i <= n; i++) cout << X[i]; //in xâu nhị phân
            cout << endl;
        } 
        else{
            Try(i + 1); // chưa phải bit cuối thì gọi đệ quy bit tiếp theo
        }
    }
}

int main(){
    cin >> n;
    Try(1);
}
```
### Sinh tổ hợp châp k của n 
```cpp
//Sinh tổ hợp chập k của n pt
int n, k, X[100]; 
//Giới hạn được khả năng của i theo công thức X[i - 1] + 1 < X[i] <= n - k + i
//vì cấu hình tổ hợp chập k của n thì giá trị đứng sau > đứng trc -> X[i - 1] <= X[i]
//nhưng vì mảng X[100] là lưu toàn cục nên giá trị ban đầu là 0 phải + 1
void Try(int i){
    for(int j = X[i - 1] + 1; j <= n - k + i; j++){
        X[i] = j;
        if(i == k){
            for(int i = 1; i <= k; i++) cout << X[i];
            cout << endl;
        }
        else Try(i + 1);
    }
}

int main(){
    cin >> n >> k;
    Try(1);
}
```
### Sinh hoán vị
```cpp
//Sinh hoán vị -> khả năng nhận được sẽ là từ 1 -> n ko giới hạn
int n, X[100];
//Làm như v thì sẽ bị lắp giá trị vì i thử hết mọi giá trị có thể của j nên lặp giá trị

// Hoán vị lặp
void Try(int i){
    for(int j = 1; j <= n; j++){
        X[i] = j;
        if(i == n){
            for(int i = 1; i <= n; i++) cout << X[i];
            cout << endl;
        }
        else Try(i + 1);
    }
}

//Hoán vị ko lặp
bool used[100]; // KT giá trị i đã đc sử dụng chưa

void Try(int i){
    for(int j = 1; j <= n; j++){
        if(used[j] == false){ //Check giá trị j được gán cho các pt X[1] -> X[i-1]
            X[i] = j; // Giá trị j đã đc sử dụng
            used[j] = true; // Báo cho các lời đệ quy ở sau rằng giá trị j đã được sử dụng
            if(i == n){
                for(int i = 1; i <= n; i++) cout << X[i];
                cout << endl;	
            }
            else Try(i + 1);
            //Backtrack : khi đi hết nhánh mà gán giá trị j cho X[i] -> trả lại giá trị j cho các nhánh khác sử dụng
            used[j] = false;
        }
    }
}

int main(){
    cin >> n;
    Try(1);
}
```
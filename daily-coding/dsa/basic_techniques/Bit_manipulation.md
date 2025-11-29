# Bit manipulation
## Bitwise
- Biểu diễn nhị phân : dùng unsigned int vì không có dấu xét bit cho dễ
- Ứng dụng phép AND : `b & 1 = b % 2 == 1` vì khi AND 1 số bất kì với 1 thì kết quả chỉ bằng 1 khi số đó có bit cuối là 1 ⇒ số lẻ
- Ứng dụng phép XOR : Áp dụng cho 1 mảng chỉ có duy nhất 1 phần tử có số lần xuất hiện là lẻ. Tìm phần tử đó ⇒ vì các số giống nhau XOR với nhau đều bằng 0 
    ```cpp
    //Cho 1 mảng chỉ có duy nhất 1 pt có số lần xuất hiện là lẻ
        int n; cin >> n;
        int a[n];
        for(int i = 0; i < n; i++) cin >> a[i];
        int res = 0;
        for(int x : a) res ^= x;
        cout << res;
    ```
- Dịch bit :
    - **left shift <<** : dịch trái k bit = nhân 2^k.  VD : 1 << k → 1 * 2^k
    - **right shift >>** : dịch phải k bit = chia 2^k. VD : 1 >> k -> 1 / 2^k
## Bitmasks
- Ứng dụng bitmasks : Sinh tập con có n phần tử : 
    ```cpp
    //Sinh ra tập con có n phần tử (N*2^N -> như giải thuật sinh)
    void binGenerate(int a[], int n){
        for(int i = 0; i < (1 << n); i++){ //Duyệt các số i từ 0 -> 2^n - 1
            //Dựa vào cấu hình nhị phân của i để suy ra cấu hình 1 tập con tương ứng
            // 0 = 000 : {∅}
            // 1 = 001 : {9} ...
            for(int j = 0; j < n; j++){ //Duyệt các bit từ 0 -> n - 1 (cấu hình i)
                if(i & (1 << j)){ // Kiếm tra thử bit thứ j của i được bật là 1 ko bằng cách i & 2^j
                    cout << a[j] << ' ';
                }
            }
            cout << endl;
        }
    }
    ```
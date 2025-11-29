# Sort 
## Trong thư viện <algorithm>
- `sort(a, a + n)` : sắp xếp cả mảng theo thứ tự tăng dần
- `sort(a, a + n, greater<kiểu_dữ_liệu>)` : sắp xếp cả mảng theo thứ tự giảm dần tương tự với vector
- `sort(a + x, a + y + 1)` : sắp xếp trên [x,y] theo thứ tự tăng dần
- `sort(vi.begin(), vi.end())` : sắp xếp trên cả vector
- `sort(vi.begin() + x, vi.begin() + y + 1)` : sắp xếp trên [x, y] của vector
- Ứng dụng một số bài toán :
    ```cpp
    // Cho 1 mảng số nguyên, in ra các phần tử trong mảng theo thứ tự tuần suất xuất hiện giảm dần
    // Đối vs các pt có cùng tần suất thì pt nào nhỏ hơn thì in ra trước
    #include <bits/stdc++.h>
    using namespace std;
    using ll = long long;

    bool cmp(pair<int, int> a, pair<int, int> b){
        if(a.second != b.second) return a.second > b.second;
        else return a.first < b.first;
    }

    int main(){
        freopen("input.txt", "r", stdin);
        freopen("output.txt", "w", stdout);
        ios::sync_with_stdio(false); 
        cin.tie(nullptr); cout.tie(nullptr);

        map<int, int> mp;
        int n; cin >> n;
        int a[n];
        for(int i = 0; i < n; i++){
            cin >> a[i];
            mp[a[i]]++;
        }
        vector<pair<int, int>> vi;
        for(auto x : mp) vi.push_back({x.first, x.second});
        sort(vi.begin(), vi.end(), cmp);
        for(auto x : vi) cout << x.first << ' ' << x.second << endl;

    }

    //Sắp xếp theo trị tuyệt đối tăng dần, nếu số có cùng trị tuyệt đối thì in ra theo thứ tự xuất hiện ban đầu
    #include <bits/stdc++.h>
    using namespace std;
    using ll = long long;

    bool cmp(int a, int b){
        return abs(a) < abs(b);
    }

    int main(){
        freopen("input.txt", "r", stdin);
        freopen("output.txt", "w", stdout);
        ios::sync_with_stdio(false); 
        cin.tie(nullptr); cout.tie(nullptr);
        //sử dụng stable_sort
        int n; cin >> n;
        int a[n];
        for(int &x : a) cin >> x;
        stable_sort(a, a + n, cmp);
        for(int x : a) cout << x << ' ';

        //ko dùng stable_sort
        int n; cin >> n;
        //mảng các pair
        pair<int, int> a[n];
        //gán giá trị cho first và chỉ số cho second -> những số cùng TTD -> sx theo chỉ số đánh dấu
        for(int i = 0; i < n; i++){
            cin >> a[i].first;
            a[i].second = i;
        }
        sort(a, a + n, cmp);
        for(auto x : a) cout << x.first << ' ';
    }
    ```

## Sort cơ bản 
### Selection sort
- Cài đặt :
    ```cpp
    // Gán chỉ số min
    void selection_sort(int a[], int n){
        for(int i = 0; i < n - 1; i++){
            //Giả sử chỉ số ở phần tử i là nhỏ nhất
            int min = i;
            for(int j = i + 1; j < n; j++){
                //Duyệt thấy thằng nào nhỏ hơn min thì cập nhật chỉ số min = j
                if(a[min] > a[j]) min = j;
            }
            //Đổi chô 2 phần tử cho nhau
            swap(a[i], a[min]);
            cout << "Buoc thu " << i + 1 << ": ";
            for(int i = 0; i < n; i++) cout << a[i] << " ";
        }
        cout << endl;
        for(int i = 0; i < n; i++) cout << a[i] << ' ';
    }
    ```
### Insertion sort
- Cài đặt :
    ```cpp 
    //Chèn những phần tử nằm sai vị trí vào vị trí đúng của nó -> xét 1 phần tử và những phần tử đứng trước nó xem thử nó đứng đúng thứ tự chưa
    void insertion_sort(int a[], int n){
        for(int i = 1; i < n; i++){
            //Lấy ra phần tử ở chỉ số i và vị trí trước phần tử trước nó
            int x = a[i], pos = i - 1;
            while(pos >= 0 && a[pos] > x){
                //Dịch sang phải để chừa chỗ để chèn x vào vị trí pos
                a[pos + 1] = a[pos];
                pos--;
            }
            //out vòng while có 2 trường hợp : chạy hết chỉ số pos (pos = -1) hoặc x > a[pos] => chèn sau vị trí của a[pos]
            a[pos + 1] = x;
            cout << "Buoc thu " << i << ": ";
            for(int i = 0; i < n; i++) cout << a[i] << " ";
        }
        cout << endl;
        for(int i = 0; i < n; i++) cout << a[i] << " ";
    }
    ```
### Counting sort
- Cài đặt :
    ```cpp
    // Cách 1
    int cnt[10000001] = {0};
    void counting_sort(int a[], int n){ 
        int min_val = INT_MAX, max_val = INT_MIN;
        for(int i = 0; i < n; i++){
            cnt[a[i]]++;
            max_val = max(max_val, a[i]);
            min_val = min(min_val, a[i]);
        }
        for(int i = min_val; i <= max_val; i++){
            if(cnt[i] != 0){
                while(cnt[i]--) cout << i << " ";
            }
        }
    }

    // Cách 2
    const int maxn = 1e6 + 5;
    int b[maxn] = {0};
    void countingSort(int a[], int n, int c[]){
        for(int i = 0; i < n; i++) b[a[i]]++;
        for(int i = 1; i <= maxn; i++) b[i] += b[i - 1];
        for(int i = n - 1; i >= 0; i--){
            b[a[i]]--;
            c[b[a[i]]] = a[i];
        }
    }
    ```
- Ứng dụng Counting sort : Frequency theo thứ tự xuất hiện của các phần tử trong mảng
    ```cpp
    int cnt[10000001] = {0};
    int used[10000001] = {0};
    void freq(int a[], int n){
        for(int i = 0; i < n; i++) cnt[a[i]]++;    
        for(int i = 0; i < n; i++){
            if(used[a[i]] == 0){
                cout << a[i] << " " << cnt[a[i]] << endl;
                used[a[i]] = 1;
            } 
        }
    }
    ```
### Interchange và Bubble Sort
```cpp
//Thấy thằng nào nhỏ hơn số đang xét là đổi chổ luôn
void interchange_sort(int a[], int n){
    for(int i = 0; i < n - 1; i++){
        for(int j = i + 1; j < n; j++){
            if(a[i] > a[j]) swap(a[i], a[j]);
        }
        cout << "Buoc thu " << i + 1 << ": ";
        for(int i = 0; i < n; i++) cout << a[i] << " ";
    }
    cout << endl;
    for(int i = 0; i < n; i++) cout << a[i] << ' ';
}

//Đưa phần tử lớn nhất nổi về sau cùng -> xét 1 cặp a[j] và a[j + 1] ứng vs mỗi chỉ số i
void bubble_sort(int a[], int n){
    for(int i = 0; i < n - 1; i++){
        for(int j = 0; j < n - i - 1; j++){
            if(a[j] > a[j + 1]) swap(a[j], a[j + 1]);
        }
        cout << "Buoc thu " << i + 1 << ": ";
        for(int i = 0; i < n; i++) cout << a[i] << " ";
    }
    cout << endl;
    for(int i = 0; i < n; i++) cout << a[i] << " ";
}
```
## Sort nâng cao
### Merge sort
- Dùng khi cần ổn định, sắp xếp dữ liệu lớn, hoặc khi dữ liệu không nằm hoàn toàn trong bộ nhớ
- Cài đặt : 
    - Dùng vector :
    ```cpp
    //Trộn 2 dãy con đã sắp xếp vào 1 dãy : dãy 1 [l, m], dãy 2 [m + 1, r]
    void merge(int a[], int l, int m, int r){
        //copy nội dung ra 2 mảng con 1 và 2
        vector<int> x(a + l, a + m + 1); //mảng con 1
        vector<int> y(a + m + 1, a + r + 1); // mảng con 2
        //Trộn vào mảng a thành 1 dãy tăng dần bắt đầu từ chỉ số l
        int i = 0, j = 0;
        while(i < x.size() && j < y.size()){
            if(x[i] <= y[j]){
                a[l] = x[i];
                l++; i++;
            }
            else{
                a[l] = y[j];
                l++; j++;
            }
        }
        //Trộn các phần tử còn dư (nếu có) của mỗi mảng vào mảng a
        while(i < x.size()){
            a[l] = x[i];
            l++; i++;
        }
        while(j < y.size()){
            a[l] = y[j]; 
            l++; j++;
        }
        //vì chỉ số left chỉ truyền tham trị nên sau vòng lặp while thì chỉ số left ko đổi
    }
    //Cài đặt hàm mergeSort theo đệ quy => chia 2 dãy con và sắp xếp 2 dãy con đó theo thứ tự tăng dần => trộn 2 dãy đó vào 1 dãy
    void merge_sort(int a[], int l, int r){
        //Chia dãy thành các dãy con bằng đệ quy và sắp xếp theo thứ tự tăng dần
        if(l >= r) return;
        int m = (l + r) / 2;
        merge_sort(a, l, m);
        merge_sort(a, m + 1, r);
        //Trộn 2 dãy vào 1 dãy
        merge(a, l, m, r);
    }
    void print(int a[], int n){
        for(int i = 0; i < n; i++) cout << a[i] << ' ';
        cout << endl;
    }
    ```
    - Dùng mảng :
    ```cpp
    void merge(int a[], int l, int m, int r){
        int cnt = l;
        int n1 = m - l + 1, n2 = r - m;
        int x[n1], y[n2];
        for(int j = l; j <= m; j++) x[j - l] = a[j];
        for(int j = m + 1; j <= r; j++) y[j - m - 1] = a[j];
        int i = 0, j = 0;
        while(i < n1 && j < n2){
            if(x[i] <= y[j])
                a[cnt++] = x[i++];
            else
                a[cnt++] = y[j++];
        }
        while(i < n1) a[cnt++] = x[i++];
        while(j < n2) a[cnt++] = y[j++];
    }
    void merge_sort(int a[], int l, int r){
        if(l < r){
            int m = (l + r) / 2;
            merge_sort(a, l, m);
            merge_sort(a, m + 1, r);
            merge(a, l, m, r);
        }
    }
    void print(int a[], int n){
        for(int i = 0; i < n; i++) cout << a[i] << ' ';
        cout << endl;
    }
    ```
- Ứng dụng :
    - Đếm số lượng cặp nghịch thế (Count inversion)
        - Brute force (O(n^2))
        ```cpp
        int count_inversion(int a[], int n){
            int dem = 0;
            for(int i = 0; i < n; i++){
                for(int j = i + 1; j < n; j++){
                    if(a[i] > a[j]) dem++;
                }
            }
            return dem;
        }
        ```
        - Merge sort (O(NlogN))
        ```cpp
        int merge(int a[], int l, int m, int r){
            //sao chép nội dung 2 dãy con
            vector<int> x(a + l, a + m + 1);
            vector<int> y(a + m + 1, a + r + 1);
            //trộn 2 dãy con thành 1 dãy từ chỉ số l
            int i = 0, j = 0;
            //Đếm số lượng cặp nghịch thế a[i] > a[j]
            int cnt = 0;
            while(i < x.size() && j < y.size()){
                if(x[i] <= y[j]){
                    //ko tạo cặp nghịch thế
                    a[l] = x[i];
                    l++; i++;
                }
                else{
                    // tạo cặp nghịch thế => số cặp nghịch thế sẽ tính từ chỉ số i đến x.size()
                    cnt += x.size() - i;
                    a[l] = y[j];
                    l++; j++;
                }
            }
            while(i < x.size()){
                a[l] = x[i];
                l++; i++;
            }
            while(j < y.size()){
                a[l] = y[j];
                l++; j++;
            }
            return cnt;
        }
        int mergeSort(int a[], int l, int r){
            int dem = 0;
            if(l < r){
                int m = (l + r) / 2;
                //count inversion = số lượng cặp nghịch thế đoạn bên trái + số lượng cặp nghịch thế đoạn bên phải + số lượng cặp nghịch thế ở phần tử bên trái và bên phải (trộn 2 dãy con thành 1 dãy)
                dem += mergeSort(a, l, m);
                dem += mergeSort(a, m + 1, r);
                dem += merge(a, l, m, r);
            }
            return dem;
        }
        ```   
    - In mergeSort theo format và liệt kê theo từng bước để biết rõ cách nó hoạt động
         ```cpp
        #define vi vector<int>
        #define vvi vector<vector<int>>
        #define p pair<int, int>
        
        stringstream ss;
        
        void input(vi &container) {
            for (int i = 0; i < container.size(); i++) {
                cin >> container[i];
            }
        }
        
        void saveChangeContainer(vi &container, int left, int right) {
            for (int i = 0; i < container.size(); i++) {
                if (i == left) {
                    ss << "[ " << container[i] << ' ';
                } else if (i == right) {
                    ss << container[i] << " ] ";
                } else {
                    ss << container[i] << ' ';
                }
            }
            ss << '\n';
        }
        
        void merge(vi &container, int left, int right, int partition) {
            vi vectorLeft(container.begin() + left, container.begin() + partition + 1);
            vi vectorRight(container.begin() + partition + 1, container.begin() + right + 1);
        
            int i = 0, j = 0, indexInContainer = left;
            while (i < vectorLeft.size() && j < vectorRight.size()) {
                if (vectorLeft[i] <= vectorRight[j]) {
                    container[indexInContainer] = vectorLeft[i];
                    i++; indexInContainer++;
                } else {
                    container[indexInContainer] = vectorRight[j];
                    j++; indexInContainer++;
                }
            }
        
            while (i < vectorLeft.size()) {
                container[indexInContainer] = vectorLeft[i];
                i++; indexInContainer++;
            }
        
            while (j < vectorRight.size()) {
                container[indexInContainer] = vectorRight[j];
                j++; indexInContainer++;
            }
        }
        
        void mergeSort(vi &container, int left, int right) {
            if (left >= right) return;
        
            int partition = (left + right) / 2;
        
            mergeSort(container, left, partition);
            mergeSort(container, partition + 1, right);
        
            merge(container, left, right, partition);
            saveChangeContainer(container, left, right);
        }
        
        int main() {
            int n;
            cin >> n;
            vi container(n);
            input(container);
            mergeSort(container, 0, container.size() - 1);
            cout << ss.str();
        }
        ```
        - Natural Merge Sort : sort theo đường chạy
        ```cpp
        // Find runs
        vector<vector<int>> find_runs(int a[], int n){
            vector<vector<int>> runs;
            vector<int> curr;
            curr.push_back(a[0]);
            for(int i = 1; i < n; i++){
                if(a[i] >= a[i - 1]){ // sắp xếp tăng dần hoặc giảm dần sửa chỗ này
                    curr.push_back(a[i]);
                }
                else{
                    runs.push_back(curr);
                    curr.clear();
                    curr.push_back(a[i]);
                }
            }
            runs.push_back(curr);
            return runs;
        }
        // Merge
        vector<int> merge(vector<int> &x, vector<int> &y){
            vector<int> res;
            int i = 0, j = 0;
            while(i < x.size() && j < y.size()){
                if(x[i] <= y[j]){ //sắp xếp tăng dần hoặc giảm dần sửa chỗ này
                    res.push_back(x[i]);
                    i++;
                }
                else{
                    res.push_back(y[j]);
                    j++;
                }
            }
            while(i < x.size()){
                res.push_back(x[i]);
                i++;
            }
            while(j < y.size()){
                res.push_back(y[j]);
                j++;
            }
            return res;
        }
        void natural_merge_sort(int a[], int n, vector<int> &res){
            vector<vector<int>> runs = find_runs(a, n);
            while(runs.size() > 1){
                vector<vector<int>> merge_run;
                for(int i = 0; i < runs.size(); i += 2){
                    if(i + 1 < runs.size()){
                        merge_run.push_back(merge(runs[i], runs[i + 1]));
                    }
                    else merge_run.push_back(runs[i]);
                }
                runs = merge_run;
            }
            res = runs[0];
        }
        ```
### Heap sort
- Dùng khi muốn đảm bảo thời gian sắp xếp 𝑂(𝑛log𝑛)O(nlogn) trong mọi trường hợp và cần ít bộ nhớ phụ.
- node gốc : 0 
- mỗi node cha : (i - 1) / 2
- node con bên phải : 2i + 1 
- node con bên trái : 2i + 2
- Tạo Max heap : Heapify
    ```cpp
    void heapify(int a[], int n, int i){
        //xem node đang xét là lớn nhất
        int largest = i; 
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        //kiểm tra node con bên trái và bên phải vẫn thỏa chỉ số trong mảng và nếu nó lớn hơn node cha thì gán chỉ số
        if(l < n && a[l] > a[largest]) largest = l;
        if(r < n && a[r] > a[largest]) largest = r;
        if(largest != i){
            // nếu node i ko phải max thì swap vs node largest
            swap(a[i], a[largest]);
            //tiếp tục gọi đệ quy đến node chỉ số largest 
            heapify(a, n, largest); 
        }
    }
    void buildHeap(int a[], int n){
        //gọi heapify với mọi node ko phải node lá => Node cha đầu tiên i = (i - 1) / 2
        for(int i = n / 2 - 1; i >= 0; i++) heapify(a, n, i);
    }
    ```
- Cài đặt 
    ```cpp
    //Heap sort
    //Tạo Max heap -> Heapify
    void heapify(int a[], int n, int i){
        int largest = i; //xem node đang xét là lớn nhất
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        //kiểm tra node con bên trái và bên phải vẫn thỏa chỉ số trong mảng và nếu nó lớn hơn node cha thì gán chỉ số
        if(l < n && a[l] > a[largest]) largest = l;
        if(r < n && a[r] > a[largest]) largest = r;
        if(largest != i){
            // nếu node i ko phải max thì swap vs node largest
            swap(a[i], a[largest]); 
            //tiếp tục gọi đệ quy đến node chỉ số largest
            heapify(a, n, largest); 
        }
    }
    //swap(a[0], a[n - 1])
    //Heapify lại từ cái node gốc sau khi swap phần tử lớn nhất vs phần tử cuối trong đoạn từ 0 đến n - 2
    void heap_sort(int a[], int n){
        //tạo heapify
        //gọi heapify với mọi node ko phải node lá => Node cha đầu tiên i = i / 2 - 1
        for(int i = n / 2 - 1; i >= 0; i--) heapify(a, n, i);
        // xét từ cuối đến đầu
        for(int i = n - 1; i >= 0; i--){  
            // đổi chỗ phần tử lớn nhất
            swap(a[0], a[i]); 
            // tạo max heap với số lượng phần tử là i vì mỗi lần swap sẽ ko xét đến thằng cuối cùng nữa
            heapify(a, i, 0); 
        }
    }
    ```
- Code khác 
    ```cpp
    void heapify(int a[], int k, int n){
        int j = 2 * k + 1;
        while(j < n){
            if (j + 1 < n){
                if (a[j] < a[j + 1]) j = j + 1;
            }
            if(a[k] >= a[j]) return;
            swap(a[k], a[j]);
            k = j; 
            j = 2 * k + 1;
        }
    }
    void buildHeap(int a[], int n){
        int i = (n - 1) / 2;
        while(i >= 0){
            heapify(a, i, n);
            i--;
        }
    }
    void heapsort(int a[], int n){
        buildHeap(a, n);
        while(n > 0){
            n = n - 1;
            swap(a[0], a[n]);
            heapify(a, 0, n);
        }
    }
    ```
### Quick sort
- Dùng khi cần hiệu suất cao, in-place và dữ liệu ngẫu nhiên, nhưng cần chú ý chọn pivot tốt
- Hoare Partition :
    ```cpp
    //Tìm cặp nghịch thế (a[i] > a[j]) i = l - 1, j = r + 1 -> swap cặp đó luôn
    int partitionHoare(int a[], int l, int r){
        int pivot = a[l]; // phần tử đầu
        int i = l, j = r;
        while(1){
            while(a[i] < pivot) i++;
            while(a[j] > pivot) j--;
            // kết thúc vòng while là 1 cặp nghịch thế, nếu 2 phần tử này vẫn còn thỏa điều kiện thì swap luôn
            if(i < j){ 
                swap(a[i], a[j]);
                i++; j--;
            }
            // j lúc này ko phải nằm giữa dãy nữa mà là 1 phần tử của dãy con bên trái để xét tiếp đệ quy 
            else return j; 
        }
    }
    void quick_sort_Hoare(int a[], int l, int r){
        if(l >= r) return;
        int p = partitionHoare(a, l, r); // phân hoạch 2 dãy con
        quick_sort_Hoare(a, l, p); // phần tử p phải thuộc dãy con trái
        quick_sort_Hoare(a, p + 1, r); // dãy con bên phải
    }
    ```
- Lomuto Partition :
    ```cpp
    //Nếu dãy đã xếp tăng dần thì ko tối ưu : Chọn pivot = r -> duyệt từ l đến r - 1 -> nếu phần tử nào < pivot thì swap, còn > pivot thì bỏ qua
    int partitionLomuto(int a[], int l, int r){
        int pivot = a[r]; // phần tử ngoài cùng
        int i = l - 1; // biến i 1 trong 2 biến dùng để chạy và hoán đổi vị trí của 2 phần tử
        for(int j = l; j < r; j++){
            if(a[j] <= pivot){
                i++;
                swap(a[i], a[j]);
            }
        }
        //đưa chốt về giữa
        i++;
        swap(a[i], a[r]);
        return i; // vị trí chốt sau khi phân hoạch
    }
    void quick_sort_Lomuto(int a[], int l, int r){
        if(l >= r) return;
        //phân hoạch thành 2 dãy con
        int p = partitionLomuto(a, l, r);
        //đệ quy dãy bên trái
        quick_sort_Lomuto(a, l, p - 1);
        //đệ quy dãy bên phải
        quick_sort_Lomuto(a, p + 1, r);
    } 
    ```
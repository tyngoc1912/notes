# Search  
## Linear Search
- Cài đặt :    
    ```cpp
    bool linearSearch(int a[], int n int x){
        for(int i = 0; i < n; i++){
            if(a[i] == x) return true;
        }
        return false;
    }
    ```
## Binary Search 
- Cài đặt :    
    ```cpp
    //áp dụng vs mảng đã sx tăng dần
    bool binarySearch(int a[], int n, int x){
        int l = 0, r = n - 1;
        while(l <= r){
            int m = (l + r)/2;
            if(a[m] == x) return true; //tìm thấy
            else if(a[m] < x) l = m + 1; //tìm bên phải mid
            else r = m - 1; // tìm bên trái mid
        }
        return false; // ko tìm thấy 
    }
    ``` 
- Tìm kiếm nội suy phát triển từ Binary Search
    ```cpp
    //áp dụng vs mảng đã sx tăng dần
    int interSearch(int a[], int n, int x){
        int l = 0, r = n - 1;
        while(l <= r){
            int m = l + ((r - l) * (x - a[l]) / (a[r] - a[l]));
            if(a[m] == x) return m; //tìm thấy
            else if(a[m] < x) l = m + 1; //tìm bên phải mid
            else r = m - 1; // tìm bên trái mid
        }
        return -1; // ko tìm thấy 
    }
    ```    
- Ứng dụng Binary Search :
    - Cho 1 mảng đã sắp xếp tăng dần tìm xem số lượng phần tử = x xuất hiện trong mảng
    - **Dùng 2 hàm tìm first pos và last pos kết hợp công thức total = last - first + 1**
    - Chú ý trường hợp `l = -1 và r = -1` 
        ```cpp
        if(l != -1) total = last - first + 1;
        else total = 0;
        ```
    - Tìm vị trí đầu tiên của x trong mảng đã sắp xếp tăng dần
    ```cpp
    int firstPos(int a[], int n, int x){
        int res = -1, l = 0, r = n - 1;
        while(l <= r){
            int m = (l + r) / 2;
            if(a[m] == x){
                res = m;
                r = m - 1; // tiếp tục tìm kiếm bên trái mid xem còn x ko
            }
            else if(a[m] < x) l = m + 1; // tìm kiếm bên phải mid
            else r = m - 1;
        }
        return res;
    }
    ```
    - Tìm vị trí cuối cùng của x trong mảng đã sắp xếp tăng dần
    ```cpp
    int lastPos(int a[], int n, int x){
        int res = -1, l = 0, r = n - 1;
        while(l <= r){
            int m = (l + r) / 2;
            if(a[m] == x){
                res = m;
                l = m + 1; //tiếp tục tìm bên phải mid
            }
            else if(a[m] > x) r = m - 1; //tìm bên trái mid
            else l = m + 1;
        }
        return res;
    }
    ```
    - Tìm vị trí đầu tiên của phần tử >= x
    ```cpp
    int lowerBound(int a[], int n, int x){
        int l = 0, r = n - 1, res = -1;
        while(l <= r){
            int m = (l + r) / 2;
            if(a[m] >= x){
                res = m;
                r = m - 1;
            }
            else l = m + 1;
        }
        return res;
    }
    ```
    - Tìm vị trí đầu tiên của pt > x
    ```cpp
    int upperBound(int a[], int n, int x){
        int l = 0, r = n - 1, res = -1;
        while(l <= r){
            int m = (l + r) / 2;
            if(a[m] > x){
                res = m;
                 r = m - 1;
            }
            else l = m + 1;
        }
        return res;
    }
    ```


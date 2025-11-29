# Recursion (Đệ quy)
## Đổi sang hệ nhị phân
```cpp
void binConvert(ll n){
	if(n == 0) return;
	convert(n / 2);
	cout << n % 2;
}

int main(){
	ll n; cin >> n;
	if(n == 0) cout << 0 << endl;
	convert(n);
}
```
## Đổi sang hệ hexadecimal
```cpp 
void hexConvert(ll n){
	if(n == 0) return;
	convert(n / 16);
	int r = n % 16; // r = (0, 15)
	if(r < 10) cout << r;
	else cout << (char)(r + 55); // A B C D ... = 65 66 67 68 ...
}
```
## Dãy số
### In từ trái sang phải
```cpp
void inphai(ll n){
	if(n < 10){
		cout << n << ' '; 
        return;
	}
	inphai(n / 10);
	cout << n % 10 << " ";
}
```
### In từ phải sang trái
```cpp
void intrai(ll n){
	if(n < 10){
		cout << n << ' '; 
        return;
	}
	cout << n % 10 << " ";
	intrai(n / 10);
}
```
### Tổng chữ số
- Đệ quy :
	```cpp
	ll sum_digit(ll n){
		if(n < 10) return n;
		else return n % 10 + sum_digit(n / 10);
	}
	```
- Không đệ quy :
	```cpp
	ll sum = 0;
	int digit(ll n){
		int dem = 0;
		if(n == 0) return 1;
		while(n){
			dem++;
			sum += n % 10;
			n /= 10;
		}
		return dem;
	}
	```
### Đếm chữ số
```cpp
ll count_digit(ll n){
	if(n < 10) return 1;
	else return 1 + count_digit(n / 10);
} 
```
### Chữ số đầu tiền trong dãy số
```cpp
ll first_num(ll n){
	if(n < 10) return n;
	else return first_num(n / 10);
}
```
### Chữ số lớn nhất
```cpp
ll max_num(ll n){
	if(n < 10) return n;
	else return max(n % 10, max_num(n / 10));
}
// Cách khác
void fmax(ll n){
 	if(n < 10) cout << n;
 	int max1 = 0;
 	if(n % 10 > max1){
 		max1 = n % 10;
 		fmax(n / 10);
 	}
 	cout << max1 << endl;
}
```
### Chữ số nhỏ nhất
```cpp
ll min_num(ll n){
	if(n < 10) return n;
	else return min(n % 10, min_num(n / 10));
}
```
### Tổng chữ số chẵn
```cpp
ll even_sum(ll n){
	if(n < 10){
		if(n % 2 == 0) return n;
		else return 0;
	}
	else{
		if(n % 10 % 2 == 0) return n % 10 + even_sum(n / 10);
		else return even_sum(n / 10);
	}
}
```
### Tổng chữ số lẻ
```cpp
ll odd_sum(ll n){
	if(n < 10){
		if(n % 2 != 0) return n;
		else return 0;
	}
	else{
		if(n % 10 % 2 != 0) return n % 10 + odd_sum(n / 10);
		else return odd_sum(n / 10);
	}
}
```
### Số toàn chẵn
- Không đệ quy :
	```cpp
	bool evenCheck(int n){
		while(n != 0){
			if(n % 2 != 0) return false;
			else n /= 10;
		}
		return true;
	}
	```
- Đệ quy :
	```cpp
	bool even_check(ll n){
		if(n < 10){
			if(n % 2 == 0) return true;
			else return false;
		}
		else{
			if(n % 10 % 2 != 0) return false;
			return even_check(n / 10);
		}
	}
	```
### Số toàn lẻ
- Không đệ quy :
	```cpp
	bool oddCheck(int n){
		while(n != 0){
			if(n % 2 == 0) return false;
			else n /= 10;
		}
		return true;
	}
	```
- Đệ quy :
	```cpp
	bool odd_check(ll n){
		if(n < 10){
			if(n % 2 == 0) return false;
			else return true;
		}
		else{
			if(n % 10 % 2 == 0) return false;
			return odd_check(n / 10);
		}
	}
	```
### Các bài toán tính tổng chữ số và công thức rút gọn
```cpp
// S = 1+2+3+..+n --> S = n(n + 1)/2
ll sum1(int n){
	if(n == 1) return 1;
	else return n + sum1(n - 1);
}

// S = 1^2+2^2+3^2+4^2+..+n^2 --> S = n(n + 1)(2n + 1)/6
ll sum2(int n){
	if(n == 1) return 1;
	else return n * n + sum2(n - 1);
}

//S = 1^3+2^3+3^3+..+n^3 --> S = (n * (n + 1)/2)^2
ll sum3(int n){
	if(n == 1) return 1;
	else return n * n * n + sum3(n - 1);
}

//S=-1+2-3+4-5+6+..+(-1)^n * n --> S = n/2 (n chẵn) = (n - 1)/2 - n (n lẻ)
ll sum4(int n){
	if(n == 1) return -1;
	if(n % 2 == 0) return n + sum4(n - 1);
	else return -n + sum4(n - 1);
}

//S = 1/1 + 1/2 + 1/3 + ... + 1/n
double sum5(int n){
	if(n == 1) return 1;
	return 1.0 / n + sum5(n - 1);
}

//Tổng chia hết cho 3 có thể dùng CT 3ll*m(m+1)/2 vs m=n
ll sum6(int n){
	ll s = 0;
	for(int i = 3; i <= n; i += 3){
      s += i;
   }
}
```
### Kiểm tra số tăng dần (luôn tăng)
```cpp
bool increase(int n){
    int r = n % 10;
    while(n != 0){
        n /= 10;
        if(n % 10 > r) return false;
        r = n % 10;
    }
    return true;
}
```
### Kiểm tra số giảm dần (luôn giảm)
```cpp
bool decrease(int n){
    int r = n % 10;
    while(n != 0){
        n /= 10;
        if(n % 10 < r) return false;
        r = n % 10;
    }
    return true;
}
```
## Mảng
### Mảng đối xứng
```cpp
bool dx(int a[], int l, int r){
	if(l > r) return true;
	if(a[l] != a[r]) return false;
	return dx(a, l + 1, r - 1);
}
```
### In từ trái qua bên phải mảng
```cpp
void intrai(int a[], int n){
	//Cách 1
	if(n == 1){
		cout << a[0] << " ";
		return;
	}
	//Cách 2:
	//if(n == 0) return;
	intrai(a, n - 1);
	cout << a[n - 1] << ' ';
}
```
### In từ phải sang trái mảng
```cpp
void in2(int a[], int n){
	// if(n == 1){
	// 	cout << a[0] << " ";
	// 	return;
	// }
	if(n == 0) return;
	cout << a[n - 1] << ' ';
	in2(a, n - 1);
}
```
### Kiểm tra mảng toàn phần tử chẵn
```cpp
bool check(int a[], int n){
	if(n == 1){
		if(a[0] % 2 != 0) return false;
		else return true;
	}
	else{
		if(a[n - 1] % 2 != 0) return false;
		else return check(a, n - 1);
	}
}
```
### Kiểm tra mảng luôn tăng dần
```cpp
bool increase_check(int a[], int n){
	if(n == 1) return true;
	if(a[n - 1] <= a[n - 2]) return false;
	return increase_check(a, n - 1);
}
```